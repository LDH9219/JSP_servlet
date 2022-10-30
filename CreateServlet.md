# 서블릿 코드 작성과 컴파일

나나(임의의 이름) 서블릿 작성
메모장, jdk, 톰캣 사용

```java
import javax.servlet.*;
import javax,servlet.http.*;
import java.io.*;

public class Nana extends HttpServlet{
  pulbic void service(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    System.out.println("Hello Servlet");
  }
}
```
