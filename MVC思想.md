##### MVC各模块功能

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200929175315939.png" alt="image-20200929175315939" style="zoom: 67%;" />

```
MVC是一种软件工程中的一种软件架构模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller），即为MVC。
	控制器Controller：控制器即是控制请求的处理逻辑，对请求进行处理，负责请 求转发。
	视图View：视图即是用户看到并与之交互的界面，比如HTML（静态资源），JSP（动态资源）等等。
	模型Model：模型代表着一种企业规范，就是业务流程/状态的处理以及业务规则的规定。
```



##### 项目结构

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200929175344407.png" alt="image-20200929175344407" style="zoom: 67%;" />

```
实际上上述系统的分包实现先按照三层架构垒起来的：
	三层架构是从整个业务应用角度对程序的划分，其分层逻辑来源于“高内聚，低耦合”的思想。
各层的定义 ：
	表现层（Web层  控制器controller  视图view）：通俗说就是用户所能看到的直观的界面。其作用就是接收用户提交的请求数据，以及将程序对用户请求所产生的响应数据反馈给用户。目的就是为用户提供可交互的操作界面。
	业务逻辑层（模型model）：简单讲就是“具体问题，具体分析”。它根据用户的不同请求而做出不同响应的处理。可以说是对数据层的一种整合方式。
	数据访问层（持久化  模型model）：它只是提供对数据库操作的多种途径。
流程：根据积木成型的图纸（表现层），我们会设计该怎样去搭建（业务逻辑层），然后就开始取积木（数据访问层）进行搭建，当我们完成设计流程的时候，积木也就成型了。
总结：JavaWeb项目的设计整体遵循三层架构的分包实现，web部分的功能实现又按照了软件设计模式中的另外一种重要的模式来实现的：MVC模式。
开发商（控制器controller）②④
客户（客户）
工程队（模型model）③
楼（视图view）①⑤
```

------

### 														  															代码实现

#### 1.用mvc实现用户的登录注册功能

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20201010103950150.png" alt="image-20201010103950150" style="zoom:50%;" />

##### com.mvc.dao

###### BaseDao.java

```javascript
package com.mvc.dao;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import com.mvc.util.JDBCUtils;
public class BaseDao<T> {
	/**
	 * 返回单个对象
	 * @param <T>
	 * @param sql
	 * @param clazz
	 * @param params
	 *        如果没有参数的情况下，就设定数组为空 Object[] params = {}
	 * @return  返回
	 */
	public <T> T get(String sql,Class<T> clazz,Object[] params) {
		T obj = null;
		Connection con = null;
		try {
			con = JDBCUtils.getConnection();
			QueryRunner qRunner = new QueryRunner();
			obj = qRunner.query(con, sql,new BeanHandler<T>(clazz),params);
			
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return obj;
	}
	
	/**
	 * 返回单个对象 带有事务的
	 * @param <T>
	 * @param sql
	 * @param clazz
	 * @param params
	 *        如果没有参数的情况下，就设定数组为空 Object[] params = {}
	 * @return  返回
	 */
	public <T> T get(Connection con,String sql,Class<T> clazz,Object[] params) {
		T obj = null;
		
		try {
			//事务处理我们需要手动去获取con对象，那么在这里就不需要去获取con对象了 直接使用参数中的con对象
			
			QueryRunner qRunner = new QueryRunner();
			obj = qRunner.query(con, sql,new BeanHandler<T>(clazz),params);
			
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return obj;
	}
	/**
	 * 返回多条数据
	 * @param T
	 * @param sql
	 * @param clazz
	 * @param params 
	 *        如果没有参数就设定为 Object[] params = {}
	 * @return
	 */
	public <T> List<T> query(String sql,Class<T> clazz,Object[] params){
		List beans = null;
		Connection con = null;
		try {
			con = JDBCUtils.getConnection();
			QueryRunner qRunner = new QueryRunner();
			beans = (List)qRunner.query(con, sql, new BeanListHandler<T>(clazz), params);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			JDBCUtils.closeConnection();
		}
		return beans;
	}
	/**
	 * 返回增删改是否成功
	 * @param sql
	 * @param params
	 * @return
	 */
	public boolean update(String sql,Object[] params) {
		Connection con = null;
		//定义一个返回值
		boolean flag = false;
		try {
			con = JDBCUtils.getConnection();
			QueryRunner qRunner = new QueryRunner();
			int i = qRunner.update(con,sql, params);
			if(i>0) {
				flag = true;
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return flag;
	}
	/**
	 * 返回增删改是否成功 带有事务的
	 * @param sql
	 * @param params
	 * @return
	 * @throws SQLException 
	 */
	public boolean update(Connection con,String sql,Object[] params) throws SQLException  {
		//定义一个返回值
		boolean flag = false;
		
			//事务处理我们需要手动去获取con对象，那么在这里就不需要去获取con对象了 直接使用参数中的con对象
			QueryRunner qRunner = new QueryRunner();
			int i = qRunner.update(con,sql, params);
			if(i>0) {
				flag = true;
			}
		
		return flag;
	}
	/**
	 * 返回批量操作的，需要用到事物
	 * @param con
	 * @param sql
	 * @param params
	 * @return
	 * @throws SQLException
	 * 
	 */
	public boolean batchUpdate(Connection con,String sql,Object[][] params) throws SQLException {
		QueryRunner qRunner = new QueryRunner();
		//事务处理我们需要手动去获取con对象，那么在这里就不需要去获取con对象了 直接使用参数中的con对象
		int result = 0;//用来接收操作结果的
		boolean flag = false;
		result = qRunner.batch(con, sql, params).length;
		if(result > 0) {
			flag= true;
		}
		return flag;
	}
	/**
	 * 返回统计单值的
	 * @param sql
	 * @param params
	 * @return
	 */
	public long getCount(String sql,Object[] params) {
		long count = 0L;
		Connection con = null;
		try {
			con = JDBCUtils.getConnection();
			QueryRunner qRunner = new QueryRunner();
			count = (long)qRunner.query(con, sql,new ScalarHandler(),params);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return count;
	}
	/**
	 * 返回主键的，通常是执行insert语句时 返回当前的主键
	 * @param sql
	 * @param params
	 * @return
	 */
	public Object getCurrentKey(String sql,Object[] params) {
		Connection con = null;
		Object key = 0;
		try {
			con = JDBCUtils.getConnection();
			QueryRunner qRunner = new QueryRunner();
			key = qRunner.insert(con, sql,new ScalarHandler(1),params);
			
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return key;
		
	}
	/**
	 * 返回主键的，通常是执行insert语句时 返回当前的主键 带有事务的
	 * @param sql
	 * @param params
	 * @return
	 * @throws SQLException 
	 */
	public Object getCurrentKey(Connection con,String sql,Object[] params) throws SQLException {
		Object key = 0;
		
			QueryRunner qRunner = new QueryRunner();
			key = qRunner.insert(con, sql,new ScalarHandler(1),params);
		
		return key;
	}
}
```

###### AdminDao.java

```javascript
package com.mvc.dao;
import com.mvc.po.Admin;
public class AdminDao extends BaseDao<Admin>{
}
```

##### com.mvc.po

###### Admin.java

```javascript
package com.mvc.po;

public class Admin {
		private int id;
		private String name;
		private String pwd;
		private String sex;
		public int getId() {
			return id;
		}
		public void setId(int id) {
			this.id = id;
		}
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public String getPwd() {
			return pwd;
		}
		public void setPwd(String pwd) {
			this.pwd = pwd;
		}
		public String getSex() {
			return sex;
		}
		public void setSex(String sex) {
			this.sex = sex;
		}
		public Admin() {
			super();
		}
		public Admin(int id, String name, String sex) {
			super();
			this.id = id;
			this.name = name;
			this.sex = sex;
		}
}
```

##### com.mvc.service

###### AdminService.java

```javascript
package com.mvc.service;
import java.util.List;
import com.mvc.po.Admin;
public interface AdminService {
	public Admin login(String name,String pwd);
	public List<Admin> findAll();
	public boolean register(String name,String sex, String pwd);
}
```

###### AdminServiceIMPL.java

```javascript
package com.mvc.service;

import java.util.List;

import com.mvc.dao.AdminDao;
import com.mvc.po.Admin;

public class AdminServiceIMPL implements AdminService{
//实例DAO
	AdminDao adminDao=new AdminDao();
	@Override
	public Admin login(String name, String pwd) {
		// TODO Auto-generated method stub
		String sql="select * from admin where name=? and pwd=?";
		//数组参数
		Object[] params= {name,pwd};
		return adminDao.get(sql, Admin.class, params);
	}
	@Override
	public List<Admin> findAll() {
		// TODO Auto-generated method stub
		String sql="select * from admin";
		Object[] params= {};
		return adminDao.query(sql, Admin.class, params);
	}
	@Override
	public boolean register(String name, String sex, String pwd) {
		// TODO Auto-generated method stub
		String sql="insert into admin (name,sex,pwd) values(?,?,?)";
		Object[] params= {name,sex,pwd};
		return adminDao.update(sql, params);
	}
}
```

##### com.mvc.servlet

###### LoginServlet.java

```javascript
package com.mvc.servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

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
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=UTF-8");
		//获取请求的是什么方法
		String method=request.getParameter("method");
		if(method.equals("login")) {
			this.login(request,response);
		}else if(method.equals("findAll")) {
			this.findAll(request,response);
		}
		
		
		
		/*String username=request.getParameter("username");
		String password=request.getParameter("password");
		AdminService adminservice=new AdminServiceIMPL();
		if(adminservice.login(username, password)==null) {
			
		}*/
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
		if(admin!=null) {
			//登陆成功
			//response.sendRedirect("jsp/Success.jsp");
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

###### RegisterServlet.java

```javascript
package com.mvc.servlet;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.mvc.po.Admin;
import com.mvc.service.AdminService;
import com.mvc.service.AdminServiceIMPL;

/**
 * Servlet implementation class RegisterServlet
 */
@WebServlet("/RegisterServlet")
public class RegisterServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
    AdminService adminservice =new AdminServiceIMPL();
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
		// TODO Auto-generated method stub
		doPost(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=UTF-8");
		String name=request.getParameter("username");
		String sex=request.getParameter("sex");
		String pwd=request.getParameter("password");
		if(adminservice.register(name, sex, pwd)) {
			findAll(request,response);
			System.out.println(sex);
		}else {
			request.getRequestDispatcher("jsp/Register.jsp").forward(request, response);
		}
		
	}
	private void findAll(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		List<Admin> list=adminservice.findAll();
		request.setAttribute("list", list);
		request.getRequestDispatcher("jsp/Success.jsp").forward(request, response);
	}
}
```

##### util

###### *JDBCUtils.java

```javascript
package com.mvc.util;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import org.apache.commons.dbutils.DbUtils;

public class JDBCUtils {
	private static String url="jdbc:mysql://localhost:3306/admin";
	private static String username = "root";
	private static String password = "123456";
	private static String driver = "com.mysql.jdbc.Driver";
	//线程池
	private static ThreadLocal<Connection> tl = new ThreadLocal<Connection>();
	//注册驱动
	static {
		boolean result = DbUtils.loadDriver(driver);
		if(result == false) {
			//手动抛出异常
			try {
				throw new SQLException("加载驱动失败！");
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
	}
	//获取连接对象connection
	public static Connection getConnection() {
		Connection con = tl.get();//先从线程池中获取
		if(con==null) {
			try {
				con = DriverManager.getConnection(url,username,password);
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		tl.set(con);
		return con;
	}
	//开启事务
	public static void startTransaction() {
		Connection con = getConnection();
		try {
			con.setAutoCommit(false);//false代表的是不自动提交事务，true代表自动提交事务 这个时候我们自己手动提交事务
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	//提交事务
	public static void commit() {
		Connection con = getConnection();
		try {
			con.commit();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}
	//回滚事务
	public static void rollback() {
		Connection con = getConnection();
		try {
			con.rollback();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	//关闭资源  
	//关闭connection 并且同时移除线程池中的connection
	public static void closeConnection() {
		close(getConnection());
		tl.remove();
		
	}
	private static void close(Connection connection) {
		// TODO Auto-generated method stub
		if(connection !=null) {
			try {
				connection.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	//关闭preparedstatement
	public static void closePst(PreparedStatement pst) {
		if(pst!=null) {
			try {
				pst.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	//关闭resultSet
	public static void closeRest(ResultSet rest) {
		if(rest !=null) {
			try {
				rest.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	//测试
	public static void main(String[] args) {
		System.out.println(getConnection());
	}
}
```

##### jsp

###### MainPage.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<center>
		<h1>欢迎使用本系统</h1>
		<h3>如果您已有账号请去前去登陆<a href="Login.jsp">登陆</a></h3>
		<h4>如果您没有账号请去前去注册<a href="Register.jsp">注册</a></h4>
	</center>
</body>
</html>
```

###### *Login.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <!-- 获取当前项目的虚拟目录 项目路径 -->
    <%String path=request.getContextPath(); %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<form action="<%=path %>/LoginServlet" method="post">
	<center>
		<p color="red">登陆界面</p>
		<input type="hidden" name="method" value="login"/>
		用户名：<input type="text" name="name"/>
		密码：<input type="password" name="pwd"/>
		<input type="submit" value="登陆"/>
		<input type="reset" value="重置"/>
	</center>
</form>
</body>
</html>
//此处需要注意的是form表单的action路径
```

###### Register.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%String path=request.getContextPath(); %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<form action="<%=path %>/RegisterServlet" method="post">
	<center>
		<h1>
			<p color="red">注册界面</p>
		</h1>
		用户名：<input type="text" name="username"/><br/>
		性别：<input type="radio" name="sex" checked="checked" value="男"/>男
		<input type="radio" name="sex" value="女"/>女<br/>
		密码：<input type="password" name="password"/><br/>
		<input type="submit" value="注册"/>
		<input type="reset" value="重置"/>
	</center>
</form>
</body>
</html>
```

###### *Success.jsp

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ page import="java.util.*" %>
    <%@ page import="com.mvc.po.Admin" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<center>
		<h1 color="red">登陆成功</h1>
		<!-- 将request域中的集合获取出来 -->
		<% List<Admin> list =(List)request.getAttribute("list"); %>
		<table border="1" bordercolor="red" color="blue">
			<tr>
				<td>编号</td>
				<td>姓名</td>
				<td>性别</td>
			</tr>
			<% for(Admin a : list){ %>
			<tr>
				<td><%=a.getId() %></td>
				<td><%=a.getName() %></td>
				<td><%=a.getSex() %></td>
			</tr>
			<%} %>
		</table>
	</center>
</body>
</html>
```

###### 程序所需jar包

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20201010110036151.png" alt="image-20201010110036151" style="zoom:50%;" />