# java-web基础功能（二）----实现用户注册

思路：同登录的思路大同小异。首先写一个注册表单，将你注册需要的数据写进去，同样因为懒，所以在这里的功能我只需用username还有password，其他邮箱生日性别什么的就不加进去了。然后将注册表单的数据传递到RegisterServlet，获取到表单的数据，然后再将这些数据存储到你的数据库，下次登录就可以使用了。

前期准备：和实现用户登录功能前期准备一样。不再赘述。

代码实现： 

(1)前端页面代码，在这里我同样不加样式。

register.jsp代码如下：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>Insert title here</title>
</head>
<body>
	<form action="/Demo/login" method="post">
		<input name="username" type="text" placeholder="please input your username">
		<input name="password" type="password" placeholder="please input your password">
		<input type="submit" value="注册">
	</form>
</body>
</html>
```

（2）因为其他步骤在用户登录功能中已经完成了，所以剩下的只需要从前端页面接收参数，传递给register.servlet，然后再将数据存进本地数据库就可以了。（注意：在这里我们考虑中文乱码只是针对表单是以post方式提交的，get方式暂时不考虑）

register.servlet代码如下：

```java
package com.Demo.service;

import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.sql.SQLException;
import java.util.Map;
import java.util.UUID;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.beanutils.BeanUtils;
import org.apache.commons.dbutils.QueryRunner;

import com.Demo.domain.User;
import com.Demo.utils.DataSourceUtils;

/**
 * Servlet implementation class RegisterServlet
 */
@WebServlet("/register")
public class RegisterServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public RegisterServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//设置request的编码---只适合post方式
		request.setCharacterEncoding("UTF-8");			
		//使用BeanUtils进行自动映射封装
		//BeanUtils工作原理：将map中的数据 根据key与实体的属性的对应关系封装
		//只要key的名字与实体的属性 的名字一样 就自动封装到实体中
		Map<String, String[]> properties = request.getParameterMap();
		User user = new User();
		try {
			BeanUtils.populate(user, properties);
		} catch (IllegalAccessException | InvocationTargetException e) {
			((Throwable) e).printStackTrace();
		}

		//现在这个位置 user对象已经封装好了
		//手动封装uid----uuid---随机不重复的字符串32位--java代码生成后是36位
		user.setUid(UUID.randomUUID().toString());

		//3、将参数传递给一个业务操作方法
		try {
			regist(user);
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		//4、认为注册成功跳转到登录页面
		response.sendRedirect(request.getContextPath()+"/login.jsp");				
	}
	//注册的方法
	public void regist(User user) throws SQLException{
		//操作数据库
		QueryRunner runner = new QueryRunner(DataSourceUtils.getDataSource());
		String sql = "insert into user values(?,?,?)";		
		runner.update(sql,user.getUid(),user.getUsername(),user.getPassword());
		System.out.println("注册成功");
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

(3)测试注册功能。

![Markdown](http://i1.bvimg.com/645823/621a3a90911d7151.png)

这个时候就可以到数据库看有没有这一条数据了

![Markdown](http://i1.bvimg.com/645823/76839752170790b0.png)

显示这一条数据的时候，就说明已经注册成功了，这时候会自动跳转到login.jsp，可以测试一下刚刚注册的账号。

![Markdown](http://i1.bvimg.com/645823/bb297bde8516a387.png)

跳转到index.jsp，说明登录成功，奈斯啊~ 

![Markdown](http://i1.bvimg.com/645823/7e36c8f3d789324e.png)