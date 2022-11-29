# EL(Expression language)

![image](https://user-images.githubusercontent.com/62749021/204466569-6a21bd80-8880-4917-882e-962194d3e21b.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/204468447-e81d7404-ac44-44df-8534-b9c3152b9071.png)

<br>

```java
package com.newlecture.web;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

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
		
		//${result}
		request.setAttribute("result", result);
		
		//${names[0]}
		String[] names = {"newlec","Dragon"};
		request.setAttribute("names",names);

		//${notice.title }
		Map<String, Object> notice = new HashMap<String, Object>();
		notice.put("id", 1);
		notice.put("title","EL GOOD");
		request.setAttribute("notice",notice);
		
		//redirect
		//forward
		RequestDispatcher dispatcher = 
				request.getRequestDispatcher("spag.jsp");
		dispatcher.forward(request, response);
	}
}
```
## EL의 데이터 저장소
저장 객체에서 값을 추출하는 순서

```
page -> request -> session -> application
```
```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html>
<html>
<head>
<%
pageContext.setAttribute("aa", "hello"); //어트리뷰트가 없어도 페이지 객체에 저장된 값을 ${aa}를 통해 호출 가능하다.
%>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%=request.getAttribute("result") %>입니다.
	${result}
	${names[0]}
	${notice.title }
	${aa }
</body>
</html>
```

![image](https://user-images.githubusercontent.com/62749021/204470306-48904b40-fc17-47f2-bc5b-e1815901bcb3.png)

<br>
