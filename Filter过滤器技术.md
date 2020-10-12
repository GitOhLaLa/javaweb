##### 总结

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200929175012854.png" alt="image-20200929175012854" style="zoom: 50%;" />

```
	过滤器实际上就是对web资源进行拦截，做一些处理后再交给下一个过滤器或servlet处理，通常都是用来拦截request进行处理的，也可以对返回的response进行拦截处理
```

##### Filter过滤器

```
Filter：过滤器
	1. 概念：
		* web中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。
		* 过滤器的作用：
			* 一般用于完成通用的操作。如：登录验证、统一编码处理、敏感字符过滤...
	2. 快速入门：
		1. 步骤：
			1. 定义一个类，实现接口Filter
			2. 复写方法
			3. 配置拦截路径
				1. web.xml
				2. 注解
	3. 过滤器细节：
		1. web.xml配置	
			<filter>
		        <filter-name>demo1</filter-name>
		        <filter-class>cn.itcast.web.filter.FilterDemo1</filter-class>
		    </filter>
		    <filter-mapping>
		        <filter-name>demo1</filter-name>
			<!-- 拦截路径 -->
		        <url-pattern>/*</url-pattern>
		    </filter-mapping>
		2. 过滤器执行流程
			1. 执行过滤器
			2. 执行放行后的资源
			3. 回来执行过滤器放行代码下边的代码
		3. 过滤器生命周期方法
			1. init:在服务器启动后，会创建Filter对象，然后调用init方法。只执行一次。用于加载资源
			2. doFilter:每一次请求被拦截资源时，会执行。执行多次
			3. destroy:在服务器关闭后，Filter对象被销毁。如果服务器是正常关闭，则会执行destroy方法。只执行一次。用于释放资源
		4. 过滤器配置详解
			* 拦截路径配置：
				1. 具体资源路径： /index.jsp   只有访问index.jsp资源时，过滤器才会被执行
				2. 拦截目录： /user/*	访问/user下的所有资源时，过滤器都会被执行
				3. 后缀名拦截： *.jsp		访问所有后缀名为jsp资源时，过滤器都会被执行
				4. 拦截所有资源：/*		访问所有资源时，过滤器都会被执行
			* 拦截方式配置：资源被访问的方式
				* 注解配置：
					* 设置dispatcherTypes属性
						1. REQUEST：默认值。浏览器直接请求资源
						2. FORWARD：转发访问资源
						3. INCLUDE：包含访问资源
						4. ERROR：错误跳转资源
						5. ASYNC：异步访问资源
				* web.xml配置
					* 设置<dispatcher></dispatcher>标签即可
				
		5. 过滤器链(配置多个过滤器)
			* 执行顺序：如果有两个过滤器：过滤器1和过滤器2
				1. 过滤器1
				2. 过滤器2
				3. 资源执行
				4. 过滤器2
				5. 过滤器1 
		* 过滤器先后顺序问题：
			1. 注解配置：按照类名的字符串比较规则比较，值小的先执行
				* 如： AFilter 和 BFilter，AFilter就先执行了。
			2. web.xml配置： <filter-mapping>谁定义在上边，谁先执行
```

------

### 																案例

#### 案例一	编码过滤器

```java
编码过滤器
第一步：创建filter类实现Filter接口
		package com.servlet.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet Filter implementation class EncodingFilter
 */
@WebFilter("/EncodingFilter")
public class EncodingFilter implements Filter {
	private String defaultEncode ;

    /**
     * Default constructor. 
     */
    public EncodingFilter() {
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see Filter#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	}

	/**
	 * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// TODO Auto-generated method stub
		
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
		
	}

}

第二步：配置WEB项目的主配置文件web.xml
		  <!-- 拦截编码的过滤器 -->
  <filter>
  		
  		<filter-name>EncodingFilter</filter-name>
  		<filter-class>com.servlet.filter.EncodingFilter</filter-class>
  		<!-- 设置当前编码过滤器初始参数 为 UTF-8 -->
  		<init-param>
  			<param-name>encoding</param-name>
  			<param-value>UTF-8</param-value>
  		</init-param>
  </filter>
  <filter-mapping>
  		<filter-name>EncodingFilter</filter-name>
  		<url-pattern>/*</url-pattern>
  </filter-mapping>
第三步：编写filter类中的doFilter()方法和init方法
		public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// TODO Auto-generated method stub
		//转换参数类型
		HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse resp = (HttpServletResponse) response;
		//获取请求方式
		String method = req.getMethod();
		//判断请求方式是什么，然后让其编码变为设置的编码
		if(method.equals("POST")) {
			req.setCharacterEncoding(defaultEncode);
			response.setContentType("text/html;charset="+defaultEncode);
		}else {
			//get请求，对于服务器是tomcat8.0（包含）及以上版本的则不需要去设置编码，
			//如果在8.0版本一下的则需要去设置编码
//			EncodeRequest er = new EncodeRequest(req);
//			req=er;
		}
		chain.doFilter(req, response);
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
		//获取出配置好的初始化参数
		defaultEncode = fConfig.getInitParameter("encoding");
		System.out.println("编码过滤器初始化成功"+defaultEncode);
	}
```

#### 案例二	访问拦截过滤器

```java
访问拦截过滤器
第一步：创建filter类实现Filter接口
		package com.servlet.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;




public class UserFilter implements Filter{

	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
			throws IOException, ServletException {
		// TODO Auto-generated method stub
		
		
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub
		System.out.println("filter 被初始化成功！");
	}

}

第二步：配置WEB项目的主配置文件web.xml
		<!-- 拦截请求的过滤器 -->
  <filter>
  	<filter-name>UserFilter</filter-name>
  	<filter-class>com.servlet.filter.UserFilter</filter-class>
  </filter>
  <filter-mapping>
  		<filter-name>UserFilter</filter-name>
  		<url-pattern>/cookie.jsp</url-pattern>
  </filter-mapping>
第三步：编写filter类中的doFilter()方法
		@Override
	public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
			throws IOException, ServletException {
		// TODO Auto-generated method stub
		//转换参数类型
		HttpServletRequest request = (HttpServletRequest)req;
		HttpServletResponse response = (HttpServletResponse) resp;
		//获取session对象
		HttpSession session = request.getSession();
		//获取session中的值
		User user = (User) session.getAttribute("user");
		//获取请求的路径  符合条件就需要放行  否则永远都被拦下了
		//获取的是项目名后面的地址
		String path = request.getServletPath();
		//判断 是否有值   有值就放行否则就拦截
		if(user!=null||path.equals("/LoginServlet")) {
			chain.doFilter(req, resp);
		}else {
			//去登录界面
			request.getRequestDispatcher("login.jsp").forward(request, response);
		}
		
	}
```

#### 案例三 访问拦截和编码过滤

##### com.mvc.filter

###### *AdminFilter.java

```java
package com.mvc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.mvc.po.Admin;

public class AdminFilter implements Filter{
	//销毁过滤器方法
	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		System.out.println("验证登陆的过滤器被销毁了");
	}
	//拦截过滤的方法
	@Override
	public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
			throws IOException, ServletException {
		// TODO Auto-generated method stub
		//转换参数类型req 和resp的
		HttpServletRequest request=(HttpServletRequest) req;
		HttpServletResponse response=(HttpServletResponse) resp;
		//换取用户是否已经登录  直接获取session中的信息  如果session中有信息说明已经登录，否则未登录
		HttpSession session=request.getSession();
		//传递的就是一个字符串
		Admin admin=(Admin) session.getAttribute("admin");
		//如果传递的是一组对象，那么就获取对象即可  Admin admin=(Admin)session.getAttribute("admin")
		
		//再次获取你的请求路径，获取的是项目名称后面的路径
		String path=request.getServletPath();
		//判断从session中获取到的内容是否存在 同时判断获取到的路径是否符合我门放行的路径
		if(admin!=null||path.equals("/LoginServlet")) {
			//否则说明已经登录 ，那么就放行
			chain.doFilter(req, resp);
		}else {
			//说明没有登录 那么就让他去登录界面
			request.getRequestDispatcher("/jsp/Login.jsp").forward(request, response);
		}
	}
	//初始化方法
	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub
		System.out.println("验证登录的过滤器初始化成功");
	}
}
```

###### EncodeRequest.java

```java
package com.mvc.filter;

import java.io.UnsupportedEncodingException;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;

public class EncodeRequest extends HttpServletRequestWrapper{
	HttpServletRequest request=null;
	public EncodeRequest(HttpServletRequest request) {
		super(request);
		this.request=request;
		// TODO Auto-generated constructor stub
	}
@Override
	public String getParameter(String name) {
		String val=request.getParameter(name);
		try {
			val=new String(val.getBytes("ISO-8859-1"),"UTF-8");
		} catch (UnsupportedEncodingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return val;
	}
}
```

###### *EncodingFilter.java

```java
package com.mvc.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
//设置编码的过滤器
public class EncodingFilter implements Filter{
	private String defaultEcode;
	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void doFilter(ServletRequest req, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		// TODO Auto-generated method stub
		//转换类型
		HttpServletRequest request=(HttpServletRequest) req;
		String method=request.getMethod();
		if(method.equals("POST")) {
			//如果是POST请求，那么就需要转换编码格式
			request.setCharacterEncoding(defaultEcode);
			response.setContentType("text/html;charset="+defaultEcode);
		}else {
			EncodeRequest er=new EncodeRequest(request);
			request=er;
		}
		//如果你是tomcat8.0以下的需要去修改编码，否则不需要
		chain.doFilter(req, response);
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// TODO Auto-generated method stub
		System.out.println("过滤器被初始化");
		//获取WEB.XML中设置的初始化参数
		defaultEcode=filterConfig.getInitParameter("encoding");
	}
}
```

##### com.mvc.servlet

###### LoginServlet.java

```java
package com.mvc.servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.mvc.dao.AdminDao;
import com.mvc.dao.BaseDao;
import com.mvc.po.Admin;
import com.mvc.service.AdminService;
import com.mvc.service.AdminServiceIMPL;
import com.mvc.util.JDBCUtils;
import com.mysql.jdbc.Connection;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	
    /**
     * @see HttpServlet#HttpServlet()
     */
    public LoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }
    AdminService adminservice=new AdminServiceIMPL();
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doPost(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//设置编码集
		//request.setCharacterEncoding("utf-8");
		//response.setContentType("text/html;charset=UTF-8");
		//获取请求的是什么方法
		String method=request.getParameter("method");
		if(method.equals("login")) {
			this.login(request,response);
		}else if(method.equals("findAll")) {
			this.findAll(request,response);
		}else if(method.equals("toUpdate")) {
			this.toUpdate(request,response);
		}else if(method.equals("update")) {
			this.update(request, response);
		}else if(method.equals("Delete")) {
			this.delete(request,response);
		}else if(method.equals("add")) {
			this.add(request,response);
		}
		
		
		/*String username=request.getParameter("username");
		String password=request.getParameter("password");
		AdminService adminservice=new AdminServiceIMPL();
		if(adminservice.login(username, password)==null) {
			
		}*/
	}

	private void add(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String name=request.getParameter("name");
		String sex=request.getParameter("sex");
		String pwd=request.getParameter("pwd");
		String card=request.getParameter("card");
		String intime=request.getParameter("intime");
		Admin admin=new Admin(name,pwd,sex,card,intime);
		boolean result=adminservice.add(admin);
		String msg;
		if(result) {
			msg="添加成功";
		}else {
			msg="添加失败";
		}
		request.setAttribute("msg",msg);
		findAll(request,response);
	}

	private void delete(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String sid=request.getParameter("id");
		int id =Integer.parseInt(sid);
		boolean result=adminservice.delete(id);
		String msg;
		if(result) {
			msg="删除成功";
		}else {
			msg="删除失败";
		}
		request.setAttribute("msg",msg);
		findAll(request,response);
	}

	private void update(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String name=request.getParameter("name");
		String sex=request.getParameter("sex");
		String sid=request.getParameter("id");
		int id =Integer.parseInt(sid);
		Admin admin=new Admin(id,name,sex);
		String msg;
		boolean result=adminservice.update(admin);
		if(result) {
			msg="操作成功";
		}else {
			msg="操作失败";
		}
		request.setAttribute("msg", msg);
		findAll(request,response);
	}

	private void toUpdate(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String sid=request.getParameter("id");
		int id =Integer.parseInt(sid);
		Admin admin=adminservice.toUpdate(id);
		request.setAttribute("admin", admin);
		request.getRequestDispatcher("jsp/update.jsp").forward(request, response);
	}

	private void findAll(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		List<Admin> list=adminservice.findAll();
		request.setAttribute("list", list);
		request.getRequestDispatcher("jsp/Success.jsp").forward(request, response);
	}

	private void login(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
		// TODO Auto-generated method stub
		//获取请求的参数
		String name=request.getParameter("name");
		String pwd=request.getParameter("pwd");
		//调用业务层的方法
		Admin admin=adminservice.login(name, pwd);
		String path=request.getServletPath();
		if(admin!=null||path.equals("/LoginServlet")) {
			//登陆成功
			//response.sendRedirect("jsp/Success.jsp");
			//登陆成功后，将登陆者的信息存放到cookie当中
			//创建cookie对象,并且将登陆者的信息传递进去
			Cookie cookie =new Cookie("name",name);
			//将cookie存放到response当中 响应
			response.addCookie(cookie);
			//使用seesion
			//1.获取到seesion
			HttpSession session=request.getSession();
			//2.将登录者的信息存放到session中
			session.setAttribute("admin", admin);
			
			findAll(request,response);
		}else {
			//登陆失败,留在登陆界面
			request.getRequestDispatcher("jsp/Login.jsp").forward(request, response);
			System.out.println("登陆失败");
			System.out.println(name);
			System.out.println(pwd);
			System.out.println(admin);
		}
	}
}
```

##### jsp

###### Success.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<center>
		
		<c:choose>
			<c:when test="${!empty (sessionScope.admin.name)}">
		<h1 color="red">
			登陆成功<br>
			session欢迎您${sessionScope.admin.name}<br>
			cookie欢迎您${cookie.name.value}
		</h1>	
		<!-- 将request域中的集合获取出来 -->
		<a href="jsp/Add.jsp">添加</a>
		<table border="1" bordercolor="red" color="blue">
			<tr>
				<td>编号</td>
				<td>姓名</td>
				<td>性别</td>
				<td>工号</td>
				<td>入厂时间</td>
				<td>操作</td>
			</tr>
			<c:forEach items="${list}" var="user">
			<tr>
				<td>${user.id}</td>
				<td>${user.name}</td>
				<td>${user.sex}</td>
				<td>${user.card}</td>
				<td>${user.intime}</td>
				<td>
					<a href="LoginServlet?method=toUpdate&id=${user.id}">修改</a>
					<a href="LoginServlet?method=Delete&id=${user.id}">删除</a>
				</td>
			</tr>
			</c:forEach>
		</table>
		<font color="red">
			${msg}
		</font>
		</c:when>
		<c:when test="${empty(sessionScope.admin.name)}">
			<a href="jsp/Login.jsp">登录</a>
		</c:when>
		</c:choose>
	</center>
</body>
</html>
```





