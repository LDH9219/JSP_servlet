# JSP

## 코드 블록
코드블록의 종류
1. 출력코드
```java
환영합니다 // add.jsp

↓
//add_jsp.java

public void_jspService(final javx.servlet.http.HttpServletRequest,...)
  throws java.io.IOException, javax.servlet.ServletException
  {
    out.print("환영합니다");
  }
```
2. 코드블록
```java
//add.jsp
<%
y = x + 3; 
%>

↓
//add_jsp.java

public void_jspService(final javx.servlet.http.HttpServletRequest,...)
  throws java.io.IOException, javax.servlet.ServletException
  {
    y = x + 3;
  }
```
3. 코드블록
```java
//add.jsp
y의 값은 : <% out.print(y) 

y의 값은 : <%= y %> 

↓
//add_jsp.java

public void_jspService(final javx.servlet.http.HttpServletRequest,...)
  throws java.io.IOException, javax.servlet.ServletException
  {
    out.write("y의 값은 : ");
    out.print(y);
  }
```


4. 선언부
```java
//add.jsp
<%!
public int sum(int a, int b)
{
  return a + b;
}
%>

//느낌표가 없을 경우 메서드 내부 메서드 정의로 인한 오류 발생
```

5. 초기 설정을 위한 Page 지시자
```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>

```

***

## JSP 내장객체
JSP 내장 객체
변수를 선언할때 Jasper에 의해 생성된 코드블록과 Duplicate 되지 않도록 주의해야 한다.

<br>

![image](https://user-images.githubusercontent.com/62749021/204281070-5ce70eb3-6904-41f1-be28-50883e13a95c.png)

<br>

***

### 내장 객체 request : HttpServletRequest
<br>

![image](https://user-images.githubusercontent.com/62749021/204281525-0838f3ea-c9a7-4b93-828a-d0efac5454c2.png)

![image](https://user-images.githubusercontent.com/62749021/204281758-83285c77-89bd-43d5-b792-b54d099d9e87.png)

<br>

***

### 내장 객체 session : javax.servlet.http.HttpSession
<br>

![image](https://user-images.githubusercontent.com/62749021/204282014-d2ffaffd-a85d-4585-bedf-2a8d59a37832.png)

<br>

***

### 내장 객체 application : javax.servlet.ServletContext
<br>

![image](https://user-images.githubusercontent.com/62749021/204282128-2a451299-9e83-48de-8bd8-e4d788faf93c.png)
