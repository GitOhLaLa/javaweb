##### Ajax

```
AJAX：
	1. 概念： ASynchronous JavaScript And XML	异步的JavaScript 和 XML
		1. 异步和同步：客户端和服务器端相互通信的基础上
			* 客户端必须等待服务器端的响应。在等待的期间客户端不能做其他操作。
			* 客户端不需要等待服务器端的响应。在服务器处理请求的过程中，客户端可以进行其他的操作。
		Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。 [1] 
		通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
		传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。
		提升用户的体验
	2. 实现方式：
		1. 原生的JS实现方式（了解）
	//1.创建核心对象
	var xmlhttp;
	if (window.XMLHttpRequest) {// code for IE7+, Firefox, Chrome, Opera, Safari
	xmlhttp=new XMLHttpRequest();
	} else{
// code for IE6, IE5
	 xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
		
//2. 建立连接
  /*
参数：
1. 请求方式：GET、POST
* get方式，请求参数在URL后边拼接。send方法为空参
 * post方式，请求参数在send方法中定义
2. 请求的URL：
3. 同步或异步请求：true（异步）或 false（同步）*/
		            xmlhttp.open("GET","ajaxServlet?username=tom",true);
		
	//3.发送请求
	xmlhttp.send();
	//4.接受并处理来自服务器的响应结果
	//获取方式 ：xmlhttp.responseText
	//什么时候获取？当服务器响应成功后再获取
	//当xmlhttp对象的就绪状态改变时，触发事件onreadystatechange。
	xmlhttp.onreadystatechange=function() {
		//判断readyState就绪状态是否为4，判断status响应状态码是否为200
	 if (xmlhttp.readyState==4 && xmlhttp.status==200)
{
	 //获取服务器的响应结果
	var responseText = xmlhttp.responseText;
	 alert(responseText);
   }
}
		2. JQeury实现方式
			1. $.ajax()
* 语法：$.ajax({键值对});
				$.ajax({
	url:"ajaxServlet1111" , // 请求路径
	type:"POST" , //请求方式
	//data: "username=jack&age=23",//请求参数
	data:{"username":"jack","age":23},
	 success:function (data) {
	alert(data);
	},//响应成功后的回调函数
	error:function () {
	alert("出错啦...")
},//表示如果请求响应出现错误，会执行的回调函数
 dataType:"text"//设置接受到的响应数据的格式
 });
			2. $.get()：发送get请求
				* 语法：$.get(url, [data], [callback], [type])
					* 参数：
						* url：请求路径
						* data：请求参数
						* callback：回调函数
						* type：响应结果的类型
			3. $.post()：发送post请求
				* 语法：$.post(url, [data], [callback], [type])
					* 参数：
						* url：请求路径
						* data：请求参数
						* callback：回调函数
						* type：响应结果的类型
```

##### JSON

```
JSON：
	1. 概念： JavaScript Object Notation		JavaScript对象表示法
		Person p = new Person();
		p.setName("张三");
		p.setAge(23);
		p.setGender("男");
		var p = {"name":"张三","age":23,"gender":"男"};
		* json现在多用于存储和交换文本信息的语法
		* 进行数据的传输
		* JSON 比 XML 更小、更快，更易解析。
	2. 语法：
		1. 基本规则
			* 数据在名称/值对中：json数据是由键值对构成的
				* 键用引号(单双都行)引起来，也可以不使用引号
				* 值得取值类型：
					1. 数字（整数或浮点数）
					2. 字符串（在双引号中）
					3. 逻辑值（true 或 false）
					4. 数组（在方括号中）	{"persons":[{},{}]}
					5. 对象（在花括号中） {"address":{"province"："陕西"....}}
					6. null
			* 数据由逗号分隔：多个键值对由逗号分隔
			* 花括号保存对象：使用{}定义json 格式
			* 方括号保存数组：[]
		2. 获取数据:
			1. json对象.键名
			2. json对象["键名"]
			3. 数组对象[索引]
			4. 遍历
				//1.定义基本格式
var person = {"name": "张三", age: 23, 'gender': true};		
var ps = [{"name": "张三", "age": 23, "gender": true},
	{"name": "李四", "age": 24, "gender": true},
	{"name": "王五", "age": 25, "gender": false}];
//获取person对象中所有的键和值
//for in 循环
/* for(var key in person){
//这样的方式获取不行。因为相当于  person."name"
//alert(key + ":" + person.key);
alert(key+":"+person[key]);
}*/
	//获取ps中的所有值
	for (var i = 0; i < ps.length; i++) {
	var p = ps[i];
	for(var key in p){
	 alert(key+":"+p[key]);
 }
}
		3. JSON数据和Java对象的相互转换
			* JSON解析器：
				* 常见的解析器：Jsonlib，Gson，fastjson，jackson
			1. JSON转为Java对象
				1. 导入jackson的相关jar包
				2. 创建Jackson核心对象 ObjectMapper
				3. 调用ObjectMapper的相关方法进行转换
					1. readValue(json字符串数据,Class)
			2. Java对象转换JSON
				1. 使用步骤：
					1. 导入jackson的相关jar包
					2. 创建Jackson核心对象 ObjectMapper
					3. 调用ObjectMapper的相关方法进行转换
						1. 转换方法：
							* writeValue(参数1，obj):
								参数1：
									File：将obj对象转换为JSON字符串，并保存到指定的文件中
									Writer：将obj对象转换为JSON字符串，并将json数据填充到字符输出流中
									OutputStream：将obj对象转换为JSON字符串，并将json数据填充到字节输出流中
						* writeValueAsString(obj):将对象转为json字符串
						2. 注解：
							1. @JsonIgnore：排除属性。
							2. @JsonFormat：属性值得格式化
								* @JsonFormat(pattern = "yyyy-MM-dd")
						3. 复杂java对象转换
							1. List：数组
							2. Map：对象格式一致
```

##### 什么是Ajax?

```
	Ajax ，是指一种创建交互式、快速动态网页应用的网页开发技术，无需重新加载整个网页的情况下，能够更新部分网页的技术。
	通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
	使用Ajax的最大优点，就是能在不更新整个页面的前提下维护数据。这使得Web应用程序更为迅捷地回应用户动作，并避免了在网络上发送那些没有改变的信息。
	Ajax是与服务器交换数据并更新部分网页的技术，在不重新加载整个网页的情况下。
```

##### 实现Ajax的步骤

```javascript
1.创建一个异步对象
	var xmlhttp=new XMLHttpRequest();
2.设置请求方式和请求地址
	xmlhttp.open(method,url,async);
	/*规定请求的类型、URL 以及是否异步处理请求。
	method：请求的类型；GET 或 POST;
	url：文件在服务器上的位置;
	async：true（异步）或 false（同步）*/
	xmlhttp.open("GET","test1.txt",true);
3.发送请求
	xmlhttp.send(string);
	//将请求发送到服务器。string：仅用于 POST 请求
4.监听状态的变化
5.处理返回的结果
	xmlhttp.onreadystatechange=function(ev2){
    	//只要状态改变，就会调用这个方法
   	 	//处理返回的结果
    	console.log("接收服务器返回的数据");
	}
	/*readyState 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。
	0: 请求未初始化
	1: 服务器连接已建立
	2: 请求已接收
	3: 请求处理中
	4: 请求已完成，且响应已就绪
	status 
	200: "OK"
	404: 未找到页面*/
```

------

### 															代码实现		

##### 1.使用ajax判断用户是否存在											

```javascript
使用ajax判断用户名是否存在
第一步：创建jsp页面
		<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

</head>
<body>
<input type="text" name="name" id="name"/>
<span id="span"></span>
<br>
<input type="button" id="btn" value="点击"/>

</body>
</html>
第二步：创建Servlet
		package com.ajax.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.ajax.po.User;
import com.ajax.service.IUserService;
import com.ajax.service.UserServiceIMPL;
import com.alibaba.fastjson.JSONObject;

/**
 * Servlet implementation class CheckServlet
 */
@WebServlet("/CheckServlet")
public class CheckServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
    private IUserService userService = new UserServiceIMPL();
    /**
     * @see HttpServlet#HttpServlet()
     */
    public CheckServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html;charset=utf-8");
		String name = request.getParameter("name");
		User user = userService.getName(name);
		if(user != null ){
			JSONObject json = new JSONObject();
			json.put("name", "用户名已存在！");
			PrintWriter out = response.getWriter();
			out.print(json);
		}else{
			JSONObject json = new JSONObject();
			json.put("name", "用户名可以使用！");
			PrintWriter out = response.getWriter();
			out.print(json);
		}
	}

}

第三步：编写ajax
		<script type="text/javascript" src="js/jquery.min.js"></script>
<script type="text/javascript">
		$(function(){
			$("#btn").click(function(){
				var name = $("#name").val();
					$.ajax({
						url:"/ServletAjax0310/CheckServlet",
						type:"post",
						//使用json数据格式传递参数
						data:{"name":name},
						dataType:"json",
						success:function(data){
							$("#span").append("<font color='red''>"+data.name+"</font>");					
							}
					})
				})
		})
</script>
```

##### 2.使用ajax执行异步请求并返回JSON数据

```javascript
使用ajax执行异步请求并返回JSON数据
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

 <button id="btn">点击</button>
 <button id="jsonBtn">测试json数据</button>

</body>
</html>
	第二步：创建servlet
		package com.servlet.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.servlet.po.User;
import com.servlet.service.IUserService;
import com.servlet.service.UserServiceIMPL;

import net.sf.json.JSONArray;



/**
 * Servlet implementation class AjaxServlet
 */
@WebServlet("/AjaxServlet")
public class AjaxServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public AjaxServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html;charset=utf-8");
		String name = request.getParameter("id");
		PrintWriter out = response.getWriter();
		out.print("获取到的参数是："+name+"又给你返回回来了！");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String name = request.getParameter("name");
		String sex = request.getParameter("sex");
		String age = request.getParameter("age");
		List<User> list = new ArrayList<User>();
		User user = new User();
		user.setId(1);
		user.setName("张三");
		user.setPassword("123456");
		User user1 = new User();
		user1.setId(2);
		user1.setName("张三");
		user1.setPassword("123456");
		User user2 = new User();
		user2.setId(3);
		user2.setName("张三");
		user2.setPassword("123456");
		User user3 = new User();
		user3.setId(4);
		user3.setName("张三");
		user3.setPassword("123456");
		list.add(user);
		list.add(user1);
		list.add(user2);
		list.add(user3);
		IUserService userService = new UserServiceIMPL();
		List<User> listUser = userService.findAll();
		
		JSONArray json = JSONArray.fromObject(listUser);
		PrintWriter out = response.getWriter();
		out.print(json.toString());
	}

}

	第三步：编写ajax
		<script type="text/javaScript" src="js/jquery.min.js"></script>
<script type="text/javascript">
	
	$(function(){
		alert("12334567545342");
		$("#btn").click(function(){
			$.ajax({
				url:"AjaxServlet",//访问的路径
				type:"get",//请求的方式
				data:"id=1",//传递的参数，携带的参数
				dataType:"text",//响应的数据类型是什么 返回的数据类型
				async:"true",//设置当前ajax是否执行异步请求，只有两个参数 true/false ajax默认执行的就是异步请求
				//contentType:"",//设置编码 发送消息到服务器的编码
				success:function(data){ // success代表执行成功后 的回调函数
					alert(data);
				}
				/* ,
				error:function(){   //执行失败了  回调函数 
					
				} */
			})
		});
		$("#jsonBtn").click(function(){
			$.ajax({
				url:"AjaxServlet",
				type:"post",
				data:{"name":"张三","sex":"男","age":"18"},
				dataType:"JSON",
				success:function(data){
					for(var key in data){
						//alert(data[key].id+"+"+data[key].name+"+"+data[key].password);
						$("#tab").append("<tr><td>"+data[key].id+"</td><td>"+
										  data[key].name+"</td><td>"+data[key].password+
										 "</td></tr>")
					}
				}
			})
		})
</script>
```

