# POST 요청
사용자로부터의 입력이 많을 경우 쿼리스트링을 통한 URL의 길이제약으로 인하여 POST를 사용한다.<br>
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
		request.setCharacterEncoding("UTF-8");
		/*
		UTF-8의 경우 2byte를 1문자로 인식해서 출력하지만
		웹서버에서의 인코딩은 1byte를 1문자로 인식하기 때문에 request.setCharacterEncoding();을 필수로 해줘야 한다.
		*/
		
		  PrintWriter out = response.getWriter();
		  
		  String title = request.getParameter("title");
		  String content = request.getParameter("content");
		  
		  out.println(title);
		  out.println(content);
	}
}
```

# 서블릿 필터
인코딩 되는 방식을 서블릿에서 매번 수정해야될 필요 없이 서블릿 필터를 사용하면 인코딩 형식을 지정해줄 수 있다.

web.xml에 추가해서 사용하거나 어노테이션을 이용한다.
```java
package com.newlecture.web.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request
			, ServletResponse response
			, FilterChain chain)
			throws IOException, ServletException {
		
		request.setCharacterEncoding("UTF-8");

		chain.doFilter(request,  response);

	}

}
```
