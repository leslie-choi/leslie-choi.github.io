---
title: java-web基础功能（一）
date: 2018-10-22 16:59:35
tags: basic
categories: java
---

# 实现用户登录

思路：实现用户登录方法的实质，就是从表单传递两个参数，username还有password，然后通过get或者post方式提交到LoginServlet,对传进来的两个参数进行具体的判断，判断之后调用一个自己定义的User Login的方法，从数据库中查找对应的参数是否和数据库里面的数据一致，然后返回结果。  

前期准备：  

（1）在数据库中建立一张user表，因为只需要实现最基础的功能，所以里面我只设置了 三个最普通的字段，这张表我是放在demo数据库里面的。如图所示： 

![Markdown](http://i1.bvimg.com/645823/aa49de553b428259.png)

(2)导入相应的jar包，连接数据库的时候需要用到，这里我使用的时c3p0连接池：

![Markdown](http://i1.bvimg.com/645823/4d809dd5f325700c.png)

（3）配置C3P0-config.xml文件（放在src目录下面），连接到本地的数据库，demo就是我实现功能所需要的数据库的名称，具体的代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	<default-config>
		<property name="user">root</property>
		<property name="password">root</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="jdbcUrl">jdbc:mysql:///demo</property>
	</default-config> 
</c3p0-config> 
```



代码实现功能：

(1)写一个前端页面login.jsp，在这里我就不加样式了，所以页面会很简陋，但是还是能达到自己想要的功能

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>Insert title here</title>
</head>
<body>
	<form action="/Demo/login" method="post">
		<input name="username" type="text" placeholder="please input your username">
		<input name="password" type="password" placeholder="please input your password">
		<input type="submit" value="submmit">
	</form>
</body>
</html>

```

action的值是将表单的数据传递到哪个servlet进行处理，这里我传递到demo项目下的LoginServlet，LoginServlet对应映射的是login，表单使用的方式是post提交的方式。然后三个input标签，最后一个的type值必须是submit，这样我们才可以将表单中的数据提交并处理。

（2)对user对象进行封装，将需要的参数封装成一个user类，这里我多封装了几个参数，因为后续工程可能会使用到。  

```java
package com.Demo.domain;

public class User {
	private String uid;
	private String username;
	private String password;
	private String name;
	private String email;
	private String sex;
	public String getUid() {
		return uid;
	}
	public void setUid(String uid) {
		this.uid = uid;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}

	@Override
	public String toString() {
		return "User [uid=" + uid + ", username=" + username + ", password=" + password + ", name=" + name + ", email="
				+ email + ", sex=" + sex + "]";
	}	
}

```





（3）这里使用到一个工具类，DataSourceUtils，来获取数据库的连接。大概的注释已经标注在里面了，具体的功能实现其实你也可以不必去深究，会复制粘贴这个工具类就可以了hhh。

```java
package com.Demo.utils;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.sql.DataSource;

import com.mchange.v2.c3p0.ComboPooledDataSource;

public class DataSourceUtils {

	private static DataSource dataSource = new ComboPooledDataSource();

	private static ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

	// 直接可以获取一个连接池
	public static DataSource getDataSource() {
		return dataSource;
	}
	
	public static Connection getConnection() throws SQLException{
		return dataSource.getConnection();
	}

	// 获取连接对象
	public static Connection getCurrentConnection() throws SQLException {

		Connection con = tl.get();
		if (con == null) {
			con = dataSource.getConnection();
			tl.set(con);
		}
		return con;
	}

	// 开启事务
	public static void startTransaction() throws SQLException {
		Connection con = getCurrentConnection();
		if (con != null) {
			con.setAutoCommit(false);
		}
	}

	// 事务回滚
	public static void rollback() throws SQLException {
		Connection con = getCurrentConnection();
		if (con != null) {
			con.rollback();
		}
	}

	// 提交并且 关闭资源及从ThreadLocall中释放
	public static void commitAndRelease() throws SQLException {
		Connection con = getCurrentConnection();
		if (con != null) {
			con.commit(); // 事务提交
			con.close();// 关闭资源
			tl.remove();// 从线程绑定中移除
		}
	}

	// 关闭资源方法
	public static void closeConnection() throws SQLException {
		Connection con = getCurrentConnection();
		if (con != null) {
			con.close();
		}
	}
	public static void closeStatement(Statement st) throws SQLException {
		if (st != null) {
			st.close();
		}
	}

	public static void closeResultSet(ResultSet rs) throws SQLException {
		if (rs != null) {
			rs.close();
		}
	}
}
```

（4）编写LoginServlet，获取数据并使用判断的方法。

```java
package com.Demo.service;

import java.io.IOException;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

import com.Demo.utils.DataSourceUtils;
import com.Demo.domain.User;
/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public LoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("UTF-8");
		
		//1、获取用户名和密码
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		//2、调用一个业务方法进行该用户的查询
		User login = null;
		try {
			login = login(username,password);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		//3、通过user是否为null判断用户名和密码是否正确
		if(login!=null) {
			//不为空则说明登录成功，重定向index.jsp
			response.sendRedirect("/Demo/index.jsp");
		}else {
			//密码或者用户名错误，回到当前的login.jsp
			request.setAttribute("loginInfo", "用户名或者密码错误");
			request.getRequestDispatcher("/login.jsp").forward(request, response);
		}
	
	}

		public User login(String username,String password) throws SQLException{
			QueryRunner runner = new QueryRunner(DataSourceUtils.getDataSource());
			String sql = "select * from user where username=? and password=?";
			User user = runner.query(sql, new BeanHandler<User>(User.class),username ,password);
		return user;
}
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```

其实这里应该使用三层架构的方法，即web层，service层还有dao层，但是由于业务比较简单所以我把service层和web层结合到了一起，这个后续还会再讲到，所以暂时不管了hhh。

（4)编写UserLogin的方法，从数据库中查找数据进行判断。

```java
package com.Demo.dao;

import java.sql.SQLException;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import com.Demo.domain.User;
import com.Demo.utils.DataSourceUtils;

public class UserLogin {
	public User login(String username,String password) throws SQLException{
		QueryRunner runner = new QueryRunner(DataSourceUtils.getDataSource());
		String sql = "select * from user where username=? and password=?";
		User user = runner.query(sql, new BeanHandler<User>(User.class),username ,password);
	return user;
	}
}
```

（5）向user表中填入数据，这里我自己定义了一条数据，username=zhangsan，password=123，然后部署到tomcat上运行login.jsp，查看效果

![Markdown](http://i1.bvimg.com/645823/d4ff8a3844a8489f.png)

![Markdown](http://i1.bvimg.com/645823/f7ad560369cde48c.png)

如果能够跳转到index.jsp，就说明你成功了~