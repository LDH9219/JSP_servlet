# 어노테이션 URL 매핑
web.xml을 수정하지 않고 ```@WebServlet("/Nana")``` 를 통해 URL 매핑이 가능하다.

```java
package com.newlecture.web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hi") //Annotation : 클래스나 메서드에 붙여지는 주석
public class Nana extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		response.setCharacterEncoding("UTF-8"); // UTF-8 인코딩 형식으로 송출
		response.setContentType("text/html; charset=UTF-8"); //응답 헤더에 Content Type을 첨부해서 브라우저가 인코딩 방식 인지 후 인코딩 내용 표시
		
		PrintWriter out = response.getWriter();
		for(int i=0; i<100; i++) {
			out.println((i+1)+":안녕 Servlet! <br />");
		}
	}
}
