---
title: java-web基础功能（三）
date: 2018-10-28 10:49:38
tags: basic
categories: java
---

# 验证码登录验证功能

登录验证功能，首先说一下为什么要有这一个功能，如果没有这个功能，那么别人可能会无限测试你的账号密码，盗取你的账号信息，防止暴力破解，说成人话就是账号更安全。但是实际开发中，验证码功能也是有插件的，所以如果你不是专门在开发验证码功能的话，其实是不必自己写的，只要你学会将别人封装好的拿过来用就可以了，所以在这里我也就用了一个很简单的功能实现了。

（1）在上次登录功能的基础上，再加上一个input标签输入验证码，加上一个img标签，通过checkImg这个Servlet来控制显示的验证码。  

```html
<form action="/WEB13/login" method="post">
	用户名：<input type="text" name="username"><br/>
	密码：<input type="password" name="password"><br/>
	验证码：<input type="text" name="username"><img onclick="changeImg(this)" src="/WEB14/checkImg"><br/>
	<input type="submit" value="登录"><br/>
</form>
```
同样，from表单的数据提交到LoginServlet中进行用户名和密码的验证。另外，由于要加上一个点击图片可以更换验证码，这个功能可以用JS来实现，绑定事件changeImg。js代码如下：

```js
<script type="text/javascript">
	window.onload = function(){		
	}
//点击重新刷新验证码的方法
//单纯封装一个方法不用写进window.onload中，直接写功能代码才写进window.onload之中
	function changeImg(obj){
		obj.src="/WEB14/checkImg?time="+new Date().getTime();
	}	
</script>
```

（2）准备一个验证码的随机生成库，这里我使用的是成语验证码，随机从提前准备好的成语库，然后导入到WEB-INF目录下面，文件格式是txt格式的，内容大概如下所示：

![Markdown](http://i2.bvimg.com/645823/05a2922eb2cb43ab.png)

（3）编写CheckImgServlet。

```java
package com.Dddgle.utils;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.image.BufferedImage;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CheckImage
 */
@WebServlet("/checkImage")
public class CheckImage extends HttpServlet {
	private static final long serialVersionUID = 1L;
	// 集合中保存所有成语
	private List<String> words = new ArrayList<String>();

	@Override
	public void init() throws ServletException {
		// 初始化阶段，读取new_words.txt
		// web工程中读取 文件，必须使用绝对磁盘路径
		String path = getServletContext().getRealPath("/WEB-INF/new_words.txt");
		try {
			BufferedReader reader = new BufferedReader(new FileReader(path));
			String line;
			while ((line = reader.readLine()) != null) {
				words.add(line);
			}
			reader.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 禁止缓存
		// response.setHeader("Cache-Control", "no-cache");
		// response.setHeader("Pragma", "no-cache");
		// response.setDateHeader("Expires", -1);

		int width = 120;
		int height = 30;

		// 步骤一 绘制一张内存中图片
		BufferedImage bufferedImage = new BufferedImage(width, height,
				BufferedImage.TYPE_INT_RGB);

		// 步骤二 图片绘制背景颜色 ---通过绘图对象
		Graphics graphics = bufferedImage.getGraphics();// 得到画图对象 --- 画笔
		// 绘制任何图形之前 都必须指定一个颜色
		graphics.setColor(getRandColor(200, 250));
		graphics.fillRect(0, 0, width, height);

		// 步骤三 绘制边框
		graphics.setColor(Color.WHITE);
		graphics.drawRect(0, 0, width - 1, height - 1);

		// 步骤四 四个随机数字
		Graphics2D graphics2d = (Graphics2D) graphics;
		// 设置输出字体
		graphics2d.setFont(new Font("宋体", Font.BOLD, 18));

		Random random = new Random();// 生成随机数
		int index = random.nextInt(words.size());
		String word = words.get(index);// 获得成语

		// 定义x坐标
		int x = 10;
		for (int i = 0; i < word.length(); i++) {
			// 随机颜色
			graphics2d.setColor(new Color(20 + random.nextInt(110), 20 + random
					.nextInt(110), 20 + random.nextInt(110)));
			// 旋转 -30 --- 30度
			int jiaodu = random.nextInt(60) - 30;
			// 换算弧度
			double theta = jiaodu * Math.PI / 180;

			// 获得字母数字
			char c = word.charAt(i);

			// 将c 输出到图片
			graphics2d.rotate(theta, x, 20);
			graphics2d.drawString(String.valueOf(c), x, 20);
			graphics2d.rotate(-theta, x, 20);
			x += 30;
		}

		// 将验证码内容保存session
		request.getSession().setAttribute("checkcode_session", word);

		// 步骤五 绘制干扰线
		graphics.setColor(getRandColor(160, 200));
		int x1;
		int x2;
		int y1;
		int y2;
		for (int i = 0; i < 30; i++) {
			x1 = random.nextInt(width);
			x2 = random.nextInt(12);
			y1 = random.nextInt(height);
			y2 = random.nextInt(12);
			graphics.drawLine(x1, y1, x1 + x2, x2 + y2);
		}
		// 将上面图片输出到浏览器 ImageIO
		graphics.dispose();// 释放资源
		
		//将图片写到response.getOutputStream()中
		ImageIO.write(bufferedImage, "jpg", response.getOutputStream());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
	/**
	 * 取其某一范围的color
	 * 
	 * @param fc
	 *            int 范围参数1
	 * @param bc
	 *            int 范围参数2
	 * @return Color
	 */
	private Color getRandColor(int fc, int bc) {
		// 取其随机颜色
		Random random = new Random();
		if (fc > 255) {
			fc = 255;
		}
		if (bc > 255) {
			bc = 255;
		}
		int r = fc + random.nextInt(bc - fc);
		int g = fc + random.nextInt(bc - fc);
		int b = fc + random.nextInt(bc - fc);
		return new Color(r, g, b);
	}
}

```

大概的代码就是这样，如果需要修改显示效果的话也可以自己去改，但是实际上我觉得是没有必要的。。。。

（4）测试结果：

![Markdown](http://i2.bvimg.com/645823/499ead86c6d27a5f.png)

这里的页面和我上面的代码会有点不一样，因为我自己加了样式。输入用户名zhangsan，密码123，然后验证码三长两短，只有验证码输入正确，才能将from表单的数据提交，这时候如果用户信息正确，就可以跳转到index.jsp了。但是如果输入验证码错误的话，这里应该要提示验证码输入错误，这一个需要使用异步Ajax来实现，后面会继续完善此功能。