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
