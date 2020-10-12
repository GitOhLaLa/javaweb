##### EL表达式和JSTL标签的区别

```javascript
EL表达式代表的是数据。JSTL代表的是逻辑
EL标签是用来显示出数据的，JSTL是标准标签库能够实现页面中逻辑判断和迭代显示，当EL表达式和JSTL标签结合使用的时候JSP页面就在没有所谓的JSP语法<%%>这一代码了
```

##### EL表达式

```javascript
EL表达式
	1. 概念：Expression Language 表达式语言
	2. 作用：替换和简化jsp页面中java代码的编写
	3. 语法：${表达式}
	4. 注意：
		* jsp默认支持el表达式的。如果要忽略el表达式
			1. 设置jsp中page指令中：isELIgnored="true" 忽略当前jsp页面中所有的el表达式
			2. \${表达式} ：忽略当前这个el表达式
	5. 使用：
		1. 运算：
			* 运算符：
				1. 算数运算符： + - * /(div) %(mod)
				2. 比较运算符： > < >= <= == !=
				3. 逻辑运算符： &&(and) ||(or) !(not)
				4. 空运算符： empty
					* 功能：用于判断字符串、集合、数组对象是否为null或者长度是否为0
					* ${empty list}:判断字符串、集合、数组对象是否为null或者长度为0
					* ${not empty str}:表示判断字符串、集合、数组对象是否不为null 并且 长度>0
		2. 获取值
			1. el表达式只能从域对象中获取值
			2. 语法：
				1. ${域名称.键名}：从指定域中获取指定键的值
					* 域名称：
						1. pageScope		--> pageContext
						2. requestScope 	--> request
						3. sessionScope 	--> session
						4. applicationScope --> application（ServletContext）
					* 举例：在request域中存储了name=张三
					* 获取：${requestScope.name}
				2. ${键名}：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。
				3. 获取对象、List集合、Map集合的值
					1. 对象：${域名称.键名.属性名}
						* 本质上会去调用对象的getter方法
					2. List集合：${域名称.键名[索引]}
					3. Map集合：
						* ${域名称.键名.key名称}
						* ${域名称.键名["key名称"]}
		3. 隐式对象：
			* el表达式中有11个隐式对象
	隐含对象				类型										说明
PageContext			javax.servlet.ServletContext		表示此JSP的PageContext
PageScope			java.util.Map						取得Page范围的属性名称所对应的值
RequestScope		java.util.Map						取得Request范围的属性名称所对应的值
sessionScope		java.util.Map						取得Session范围的属性名称所对应的值
applicationScope	java.util.Map						取得Application范围的属性名称所对应的值
param				java.util.Map				ServletRequest.getParameter(String name)。回传String类型的值
paramValues			java.util.Map		ServletRequest.getParameterValues(String name)。回传String[]类型的值
header				java.util.Map				ServletRequest.getHeader(String name)。回传String类型的值
headerValues		java.util.Map				ServletRequest.getHeaders(String name)。回传String[]类型的值
cookie				java.util.Map				HttpServletRequest.getCookies()
initParam			java.util.Map			ServletContext.getInitParameter(String name)。回传String类型的值
* pageContext：
	* 获取jsp其他八个内置对象
		* ${pageContext.request.contextPath}：动态获取虚拟目录
```

##### JSTL表达式

```javascript
 JSTL
	1. 概念：JavaServer Pages Tag Library  JSP标准标签库
		* 是由Apache组织提供的开源的免费的jsp标签		<标签>
	2. 作用：用于简化和替换jsp页面上的java代码		
	3. 使用步骤：
		1.下载jstl标签的jar包
		2. 导入jstl相关jar包
		3. 引入标签库：taglib指令：  <%@ taglib %>
		4. 使用标签
	4. 常用的JSTL标签
		1. if:相当于java代码的if语句
			1. 属性：
				            * test 必须属性，接受boolean表达式
				                * 如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容
				                * 一般情况下，test属性值会结合el表达式一起使用
			2. 注意：
				* c:if标签没有else情况，想要else情况，则可以在定义一个c:if标签
		2. choose:相当于java代码的switch语句
			1. 使用choose标签声明 相当于switch声明
			2. 使用when标签做判断    相当于case
			3. 使用otherwise标签做其他情况的声明  相当于default
		3. foreach:相当于java代码的for语句
```

##### 所需jar包

![image-20201010154718493](C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20201010154718493.png)

------

### 																案例

#### 1.通过EL表达式在页面中展示数据

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20201010101830124.png" alt="image-20201010101830124" style="zoom: 67%;" />

```javascript
第一步：创建jsp页面
	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<table border="1" bordercolor="red">
		<tr>
			<td>编号</td>
			<td>姓名</td>
			<td>密码</td>
		</tr>
		<tr>
			<td>1</td>
			<td>张三</td>
			<td>123456</td>
		</tr>
	</table>
</body>
</html>
第二步：创建UserServlet
	package com.servlet.LoginServlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.servlet.po.User;
import com.servlet.service.IUserService;
import com.servlet.service.UserServiceIMPL;

/**
 * Servlet implementation class UserServlet
 */
@WebServlet("/UserServlet")
public class UserServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       IUserService userService = new UserServiceIMPL();
    /**
     * @see HttpServlet#HttpServlet()
     */
    public UserServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		User user = userService.getUser();
		request.setAttribute("user", user);
		request.getRequestDispatcher("user.jsp").forward(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
第三步：在jsp页面中使用EL表达式将数据展示出来
	<table border="1" bordercolor="red">
		<tr>
			<td>编号</td>
			<td>姓名</td>
			<td>密码</td>
		</tr>
		<tr>
			<td>${user.id }</td>
			<td>${user.name }</td>
			<td>${user.password }</td>
		</tr>
	</table>
```

#### 2.通过JSTL标签在页面中展示数据

<img src="C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20201010102121821.png" alt="image-20201010102121821" style="zoom: 50%;" />

```javascript
第一步：更改JSP页面引入JSTL标签
	<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
第二步：更改Servlet中的方法体
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		List<User> list = userService.findAll();
		request.setAttribute("list", list);
		request.getRequestDispatcher("user.jsp").forward(request, response);
	}
第三步：更改JSP页面中显示数据的部分
		<table border="1" bordercolor="red">
		<tr>
			<td>编号</td>
			<td>姓名</td>
			<td>密码</td>
		</tr>
		<c:forEach items="${list }" var="user">
		<tr>
			<td>${user.id }</td>
			<td>${user.name }</td>
			<td>${user.password }</td>
		</tr>
		</c:forEach>
	</table>
```

#### 3.EL表达式和JSTL结合使用实现员工信息增删改查

##### com.mvc.dao

###### AdminDao.java

```java
package com.mvc.dao;
import com.mvc.po.Admin;
public class AdminDao extends BaseDao<Admin>{
}
```

###### BaseDao.java

```java
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

##### com.mvc.po

###### Admin.java

```java
package com.mvc.po;

public class Admin {
		private int id;
		private String name;
		private String pwd;
		private String sex;
		private String card;
		private String intime;
		public String getCard() {
			return card;
		}
		public void setCard(String card) {
			this.card = card;
		}
		public String getIntime() {
			return intime;
		}
		public void setIntime(String intime) {
			this.intime = intime;
		}
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
		public Admin(String name, String pwd, String sex,String card,String intime) {
			super();
			this.name = name;
			this.pwd = pwd;
			this.sex = sex;
			this.card=card;
			this.intime=intime;
		}
}
```

##### com.mvc.service

###### AdminService.java

```java
package com.mvc.service;
import java.util.List;
import com.mvc.po.Admin;
public interface AdminService {
	//根据用户名和密码查询数据库语句，用于匹配
	public Admin login(String name,String pwd);
	//查询出数据库admin中所有用户信息
	public List<Admin> findAll();
	//注册方法，向数据库添加注册信息
	public boolean register(String name,String sex, String pwd,String card,String intime);
	//去修改的方法 根据id查询出当前这一条的数据  跳转到update.jsp页面
	public Admin toUpdate(int id);
	//执行更新数据库语句
	public boolean update(Admin admin);
	//执行删除数据库语句
	public boolean delete(int id);
	//执行添加数据库语句
	public boolean add(Admin admin);
}
```

###### AdminServiceIMPL.java

```java
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
	public boolean register(String name, String sex, String pwd,String card,String intime) {
		// TODO Auto-generated method stub
		String sql="insert into admin (name,sex,pwd,card,intime) values(?,?,?,?,?)";
		Object[] params= {name,sex,pwd,card,intime};
		return adminDao.update(sql, params);
	}
	@Override
	public Admin toUpdate(int id) {
		// TODO Auto-generated method stub
		String sql="select * from admin where id=?";
		Object[] params= {id};
		return adminDao.get(sql,Admin.class, params);
	}
	@Override
	public boolean update(Admin admin) {
		// TODO Auto-generated method stub
		String sql="update admin set name=?,sex=? where id=?";
		Object[] params= {admin.getName(),admin.getSex(),admin.getId()};
		return adminDao.update(sql, params);
	}
	@Override
	public boolean delete(int id) {
		// TODO Auto-generated method stub
		String sql="delete from admin where id=?";
		Object[] params= {id};
		return adminDao.update(sql, params);
	}
	@Override
	public boolean add(Admin admin) {
		// TODO Auto-generated method stub
		String sql="insert into admin (name,sex,pwd,card,intime) values(?,?,?,?,?)";
		Object[] params= {admin.getName(),admin.getSex(),admin.getPwd(),admin.getCard(),admin.getIntime()};
		return adminDao.update(sql, params);
	}
}
```

##### com.mvc.servlet

###### *LoginServlet.java

```java
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

```java
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
		String card=request.getParameter("card");
		String intime=request.getParameter("intime");
		if(adminservice.register(name, sex, pwd,card,intime)) {
			findAll(request,response);
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

##### com.mvc.util

###### JDBCUtils.java

```java
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

```jsp
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
	<!-- 使用java代码获取 -->
	这是通过java代码获取request域中的值<%out.print(request.getAttribute("msg")); %>
</body>
</html>
```

###### Login.jsp

```jsp
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
```

###### Register.jsp

```jsp
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
		卡号:<input type="text" name="card"/><br>
		入场时间：<input type="text" name="intime"/><br>
		<input type="submit" value="注册"/>
		<input type="reset" value="重置"/>
	</center>
</form>
</body>
</html>
```

###### Add.jsp

```jsp
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
<center>
	<h1>添加界面</h1>
	<form action="<%=path%>/LoginServlet?method=add" method="post">
		姓名：<input type=" text" name ="name"/><br>
		性别：<input type="radio" name="sex" value="男" checked="checked"/>男
		<input tpye="radio" name="sex" value="女"/>女<br>
		密码：<input type="password" name ="pwd"/>
		工号：<input tpye="text" name="card" />
		入场时间：<input type="text" name="intime"/>
		<input type ="submit" value="添加"/>
	</form>
	</center>
</body>
</html>
```

###### update.jsp

```jsp
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
	<h1>修改界面</h1>
	<form action="<%=path%>/LoginServlet?method=update" method="post">
		<!-- 修改的时候表中ID字段是否可以给客户进行修改    不可以     将ID的字段设置为隐藏 -->
		<input type="hidden" name="id" value="${admin.id}"/><br>
		姓名：<input type="text" name="name" value="${admin.name}"/>
		性别：<input type="text" name="sex" value="${admin.sex}"/>
		<input type="submit" value="修改"/>
	</form>
</body>
</html>
```

###### *Success.jsp

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
		<h1 color="red">登陆成功</h1>
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
	</center>
</body>
</html>
```

