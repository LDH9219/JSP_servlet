# GET 요청과 쿼리 스트링

## GET 요청
사용자의 가장 기본이 되는 ```GET ```요청
무엇을 달라고 하는 요청에는 옵션이 있을 수 있다.
반복 횟수를 사용자로부터 입력 받으려면 입력 폼을 준비해야 한다.
```HTML5
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div>
		<form action="hi">
			<div>
				<label>"안녕 Servlet을 몇 번 듣고 싶으세요?</label>
			</div>
			<div>
				<input type = "text" name="cnt" />
				<input type = "submit" value="출력" />
			</div>
		</form>
	</div>
</body>
</html>
```

## 쿼리 스트링

```
http://localhost/hello
http://localhost/hello?cnt=3 // [?cnt=3]= 쿼리스트링
```

쿼리 스트링을 이용한 반복문 작성

```java
public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
  response.setCharacterEncoding("UTF-8");
  response.setContentType("text/html; charset=UTF-8");
  
  PrintWriter out = response.getWriter();
  
  int cnt = Integer.parseInt(request.getParameter("cnt"));
  
  for(int i = 0; i<cnt; i++){
  out.println("안녕 Servlet <br />");
  }
}
```
전달되는 입력값의 형태
```java
http://...hello?cnt=3 --> "3" 문자열 3
http://...hello?cnt= --> ""
http://...hello? --> null
http://...hello --> null
```



쿼리값을 전달하지 않았기 때문에 HTTP 상태 500 - 내부 서버 오류 발생
쿼리값을 전달하는 ```?cnt= ```를 지정할 경우 정상적으로 출력됨
이 경우 기본값을 설정하면 오류출력 X

## 기본값
```java
@WebServlet("/hi")
String temp = request.getParameter("cnt"); //cnt 로 설정된 파라미터에 들어온 값을 String 형 변수 temp에 저장
int cnt = 0; //int형 변수 cnt를 0으로 초기화
if(temp != null && !temp.equals("")) // if(temp != null) 일 경우 null 값의 경우에만 해당하고 "" 공백처리가 불가능하기 때문에 &&연산으로 동시검사
  cnt = Integer.parseInt(temp); //temp로 받아온 값은 정수가 아닌 문자열이기 때문에 Integer.parseInt를 통해 정수로 변환.
```
```HTML5
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	환영합니다. <br>
	<a href="hi"> 반갑습니다. </a><br>
	<a href="hi?cnt=3">반갑습니다. HTML href 속성으로도 입력값을 정할 수 있다. </a><br>
</body>
</html>
```
