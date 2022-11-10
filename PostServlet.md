# POST 요청
사용자로부터의 입력이 많을 경우 쿼리스트링을 통한 URL의 길이제약으로 인하여 POST를 사용한다
보안적인 요소로 인하여 POST를 사용한다.

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div>
		<form action="notice-reg" method="post">
			<div>
				<label> 제목 : </label> <input name = "title" type="text">
			</div>
			<div>
				<label> 내용 : </label>
				<textarea name = "content"></textarea>
			</div>
			<div>
				<input type = "submit" value="등록" />
			</div>
		</form>
	</div>
</body>
</html>
```
```java
package com.newlecture.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/notice-reg") //Annotation : 클래스나 메서드에 붙여지는 주석
public class NoticeReg extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		
		  PrintWriter out = response.getWriter();
		  
		  String title = request.getParameter("title");
		  String content = request.getParameter("content");
		  
		  out.println(title);
		  out.println(content);
	}
}
```
