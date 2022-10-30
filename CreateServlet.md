# 서블릿 코드 작성과 컴파일

## 나나(임의의 이름) 서블릿 작성

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class Nana extends HttpServlet{
  public void service(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    System.out.println("Hello Servlet");
  }
}
```

## 컴파일(톰캣 10에서는 cmd 컴파일 오류 발생)
1. jsp 폴더 이동
2. javac -cp <참조 경로 설정> <컴파일할 자바파일>
```cmd
cd jsp
javac -cp C:\Tools\apache-tomcat-9.0.68\lib\servlet-api.jar Nana.java
```

### 톰캣 webappas\ROOT\WEB-INF
이 웹 인포메이션 디렉터리는 외부에서 접근이 불가능하다.

### web.xml이란?
Web Application의 Deployment Descriptor(환경파일 : 배포서술자, DD파일)로서 XML 형식의 파일
모든 Web application은 반드시 하나의 web.xml 파일을 가져야 함
위치 : WEB-INF 폴더 아래

# 서블릿 설정
```HTML
<servlet> : 서블릿 객체 설정
<servlet-name> : 객체의 이름 </servlet-name>
<servlet-class> : 객체를 생성할 클래스 </servlet-class>
</servlet>
```

## web.xml 수정
```HTML
<servlet>
	<servlet-name>na</servlet-name>
	<servlet-class>Nana</servlet-class> //패키지 명이 존재할 경우 이름 앞에 같이 적는다. ex)newlecNana
  </servlet>
  
  <servlet-mapping>
	<servlet-name>na</servlet-name>
	<url-pattern>/hello</url-pattern> //hello URL 이 오면 na servlet을 실행, 위의 Nana가 실행되게 된다. 매핑
  </servlet-mapping>
```

실행 결과
localhost:8080/hello = 백지 페이지
Hello Servlet = 톰캣 콘솔 출력

## Servlet 문자열 출력하기 
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class Nana extends HttpServlet{
  public void service(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    System.out.println("Hello Servlet");
    
    /*
    OutputStream os = response.getOutputStream();
    PrintStream out = new PrintStream(os, true); //true 옵션은 버퍼가 전부 차는것을 기다리지 않고 전송.
    
    PrintWriter out = response.getWriter(); //다국어일 경우 PrintWriter가 기본.
    */
    
    out.println("HELLO SERVLET?");
  }
}
```


