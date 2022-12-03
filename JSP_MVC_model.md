# JSP MVC model

## 일반적인 JSP 프로그래머가 구현하는 방식의 코드
코드블록을 쪼개지 않고 쓴다.

```java
/*---입력코드---*/
<%
String num_ = request.getParameter("n");
%>

/*---출력코드---*/
<<% if(num%2==0){%>
	짝수입니다.
	<% }
	else{
		%>
	홀수입니다.
	<%} %>
```

위의 코드처럼 출력코드의 코드블록이 쪼개지도록 사용하는 것보단 아래와 같이 사용하는것이 바람직하고 깔끔하며 스파게티 코드를 생성하지 않는다.

```java
/*---입력코드---*/
<%
String num_ = request.getParameter("n");
...
if(num%2 == 0)
  model = "짝수";
else
  model = "홀수";
  %>

/*---출력코드---*/
<%=model %> 입니다.
```
***

## 출력을 가볍게 만들기 위한 코드 작성 방식 MVC model
```
출력 데이터 : Model
입력과 제어를 담당 : Controller [자바코드]
출력 담당 : View [HTML 코드]
```
자바 코드와 뷰는 분리시키는 것이 중요하다.

### MVC model 1
컨트롤러와 뷰가 물리적으로 분리되지 않은 방식
 
### MVC model 2
컨트롤러와 뷰가 물리적으로 분리된 방식
Dispatcher를 집중화 한 후의 모델

```java
package com.newlecture.web;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/spag")
public class Spag extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest request,
			HttpServletResponse response) throws ServletException,
	IOException {
		int num = 0;
		String num_ = request.getParameter("n");
		if(num_!=null && !num_.equals(""))
			num = Integer.parseInt(num_);
		
		String result;
		
		if(num%2==0)
			result = "짝수";
		else
			result = "홀수";
		
		//redirect
		//forward
		RequestDispatcher dispatcher = 
				request.getRequestDispatcher("spag.jsp");
		dispatcher.forward(request, response);
	}
}

```
```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%= request.getAttribute("result") %>입니다.
</body>
</html>
```

***

# JSP MVC Model 1
MVC 모델 1에서는 JAVA(Controller) | HTML(View) 코드를 분리해서 사용한다. 분리한 코드들의 가독성을 높임과 동시에 재사용 시 편의성이 개선된다.    


detail.jsp
```java
<%@page import="java.sql.Date"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.Statement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" %>

<%
int id = Integer.parseInt(request.getParameter("id"));

String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
String sql = "SELECT * FROM NOTICE WHERE ID=?";
String userid = "NEWLEC";
String passwd = "8327";

Class.forName("oracle.jdbc.driver.OracleDriver");
Connection con = DriverManager.getConnection(url,userid,passwd);
PreparedStatement st = con.prepareStatement(sql);
st.setInt(1, id);

ResultSet rs = st.executeQuery();
rs.next();

String title = rs.getString("TITLE");
String writerId = rs.getString("WRITER_ID");
Date regdate = rs.getDate("REGDATE");
int hit = rs.getInt("HIT");
String files = rs.getString("FILES");
String content = rs.getString("CONTENT");

rs.close();
st.close();
con.close();
%>

<div class="margin-top first">
	<h3 class="hidden">공지사항 내용</h3>
		<table class="table">
			<tbody>
				<tr>
					<th>제목</th>
					<td class="text-align-left text-indent text-strong text-orange" colspan="3"><%= title %></td>
				</tr>
				<tr>
					<th>작성일</th>
					<td class="text-align-left text-indent" colspan="3"><%= regdate %>	</td>
				</tr>
				<tr>
					<th>작성자</th>
					<td><%=writerId %></td>
					<th>조회수</th>
					<td><%=hit %></td>
				</tr>
				<tr>
					<th>첨부파일</th>
					<td colspan="3"><%=files %></td>
				</tr>
				<tr class="content">
					<td colspan="4"><%=content %><div><br></div><div>현재 진행중인 스프링 DI 8강까지의 예제입니다.</div><div><br></div><div><a href="http://www.newlecture.com/resource/spring2.zip"><b><u><font size="5" color="#dd8a00">예제 다운로드하기</font></u></b></a></div><div><br></div><div><br></div></td>
				</tr>
			</tbody>
		</table>
</div>
```

***

# JSP MVC model 2
View 와 Controller를 물리적으로 나누는 방식

![image](https://user-images.githubusercontent.com/62749021/205253949-31baa2d3-fe8e-42a3-b27b-30ab9d1757b0.png)

<br>

```java
//Controller
String title = rs.getString("TITLE");

request.setAttribute("title", title);

//View
request.getAttribute("title")
```
위와 같은 방식으로도 가능하지만, View 단에 적어야 할 것들이 많아지므로 하지 않는다.     

Entity, 즉 불러와야 할 정보들로 이루어진 객체파일에 생성자 또는 게터와 세터 또는 생성자를 사용하여 좀 더 구조화 된 형태로 사용한다.

Notice.java = Notice Entity 파일    
NoticeDetailController = 외부 컨트롤러    
detail.jsp = View    

```java
//package com.newlecture.web.entity / Notice.java / Notice에서 사용될 객체들을 담은 파일
package com.newlecture.web.entity;

import java.sql.Date;

public class Notice {
	private int id;
	private String title;
	private String writerId;
	private Date regdate;
	private int hit;
	private String files;
	private String content;
	
	public Notice() {
		// TODO Auto-generated constructor stub
	}
	
	public Notice(int id, String title, String writerId, Date regdate, int hit, String files, String content) {

		this.id = id;
		this.title = title;
		this.writerId = writerId;
		this.regdate = regdate;
		this.hit = hit;
		this.files = files;
		this.content = content;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getWriterId() {
		return writerId;
	}

	public void setWriterId(String writerId) {
		this.writerId = writerId;
	}

	public Date getRegdate() {
		return regdate;
	}

	public void setRegdate(Date regdate) {
		this.regdate = regdate;
	}

	public int getHit() {
		return hit;
	}

	public void setHit(int hit) {
		this.hit = hit;
	}

	public String getFiles() {
		return files;
	}

	public void setFiles(String files) {
		this.files = files;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	@Override // 출력 테스트용 toString()
	public String toString() {
		return "Notice [id=" + id + ", title=" + title + ", writerId=" + writerId + ", regdate=" + regdate + ", hit="
				+ hit + ", files=" + files + ", content=" + content + "]";
	}
}
```
```java
package com.newlecture.web.controller;

import java.io.IOException;
import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.newlecture.web.entity.Notice;

@WebServlet("/notice/detail")
public class NoticeDetailController extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest request,
			HttpServletResponse response) throws ServletException,
	IOException {
		
		int id = Integer.parseInt(request.getParameter("id"));

		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "SELECT * FROM NOTICE WHERE ID=?";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			st.setInt(1, id);

			ResultSet rs = st.executeQuery();
			rs.next();

			String title = rs.getString("TITLE");
			String writerId = rs.getString("WRITER_ID");
			Date regdate = rs.getDate("REGDATE");
			int hit = rs.getInt("HIT");
			String files = rs.getString("FILES");
			String content = rs.getString("CONTENT");
			
			Notice notice = new Notice(
								id,
								title,
								writerId,
								regdate,
								hit,
								files,
								content
					);
			
			request.setAttribute("n",notice);
			
			/* View 에서 rs.getAttribute("title) 와 같은 방법을 사용할 경우 아래와 같이 진행해도 무방.
			request.setAttribute("title",title);
			request.setAttribute("writerId",writerId);
			request.setAttribute("regdate",regdate);
			request.setAttribute("hit",hit);
			request.setAttribute("files",files);
			request.setAttribute("content",content);
			*/
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		//redirect
		
		//forward
		request
		.getRequestDispatcher("/WEB-INF/view/notice/detail.jsp")
		.forward(request, response);
	}
}

*** 
//view 단 / EL 사용법을 통하여 출력한다. ${ Controller 생성자.읽어올 객체}
<div class="margin-top first">
		<h3 class="hidden">공지사항 내용</h3>
		<table class="table">
			<tbody>
				<tr>
					<th>제목</th>
					<td class="text-align-left text-indent text-strong text-orange" colspan="3">${n.title}</td>
				</tr>
				<tr>
					<th>작성일</th>
					<td class="text-align-left text-indent" colspan="3"> ${n.regdate}	</td>
				</tr>
				<tr>
					<th>작성자</th>
					<td>${n.writerId}</td>
					<th>조회수</th>
					<td>${n.hit}</td>
				</tr>
				<tr>
					<th>첨부파일</th>
					<td colspan="3">${n.files}</td>
				</tr>
				<tr class="content">
					<td colspan="4">${n.content}<div><br></div><div>현재 진행중인 스프링 DI 8강까지의 예제입니다.</div><div><br></div><div><a href="http://www.newlecture.com/resource/spring2.zip"><b><u><font size="5" color="#dd8a00">예제 다운로드하기</font></u></b></a></div><div><br></div><div><br></div></td>
				</tr>
			</tbody>
		</table>
</div>
```

위의 예제들은 NoticeDetail에 관한 예제이므로 반복출력을 요하는 Notice 에 대한 예제를 아래에 추가한다.

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"  %>

<%-- 					<%
					List<Notice> list = (List<Notice>)request.getAttribute("list");
					for(Notice n : list){ 
 						pageContext.setAttribute("n",n);
						%> --%>
					
					<%-- var="n" 은 pageContext.setAttribute, items = "${list}를 통해 forEach 태그를 사용한다. --%>
					<c:forEach var="n" items="${list}">	
					<tr>
						<td>${n.id }</td>
						<td class="title indent text-align-left"><a href="detail?id=${n.id}">${n.title}</a></td>
						<td>${n.writerId }</td>
						<td>${n.regdate }</td>
						<td>${n.hit }</td>
					</tr> 
					</c:forEach>
<%-- 				<% } %> --%>
