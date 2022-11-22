# Get
```
GET 메서드는 클라이언트에서 서버로 어떠한 리소스로부터 정보를 요청하기 위해 사용되는 메서드
```

데이터를 읽거나(Read), 검색(Retrieve)할 때 사용되는 메서드.

Get은 요청을 전송할 때 URL 주소 끝에 파라미터로 포함되어 전송되며, 이 부분을 쿼리 스트링이라고 한다.
GET 요청은 오로지 데이터를 읽을 때만 사용되고 수정할 때는 사용하지 않는다.

## Get 요청에 대한 기타 참고 사항
GET은 불필요한 요청을 제한하기 위해 요청이 캐시될 수 있다. <br>
파라미터에 내용이 노출되기 때문에 민감한 데이터를 다룰 때 GET 요청을 사용해서는 안된다.<br>
GET 요청은 브라우저 기록에 남는다.<br>
GET 요청을 북마크에 추가할 수 있다.<br>
GET 요청에는 데이터 길이 제한이 존재한다.<br>
GET 요청은 성공 시 HTTP 응답코드를 XML, JSON 뿐만 아니라 여러 데이터(html, txt등) 여러 형식의 데이터와 함께 반환한다.<br>
GET 요청은 연산을 여러번 적용하더라도 결과가 달라지지 않는다.<br>

# POST
```
POST 메서드는 리소스를 생성/ 업데이트하기 위해 서버에 데이터를 보내는 데 사용된다.
```

GET 과 달리 전송할 데이터를 HTTP 메세지의 body에 담아서 전송한다.
body의 타입은 요청 헤더의 ```Content-Type```의 요청 데이터의 타입 표시에 따라 결정된다.

HTTP 메세지의 body는 길이의 제한 없이 데이터를 전송할 수 있다.

```POST```요청도 크롬의 개발자 도구, Fiddler 와 같은 툴로 요청 내용을 확인할 수 있기 때문에 민감한 데이터의 경우에 반드시 암호화해 전송해야 한다.

## POST 요청에 대한 기타 참고 사항
POST 요청은 캐시되지 않는다.<br>
POST 요청은 브라우저 기록에 남아 있지 않는다.<br>
POST 요청을 북마크에 추가할 수 없다.<br>
POST 요청에는 데이터 길이에 대한 제한이 없다.<br>
POST 요청 중 자원 생성은 201(Created) HTTP 응답 코드를 반환한다.<br>
POST 요청은 GET과는 다르게 연산을 여러번 적용할 경우 반환값이 달라질 수 있다.<br>

# GET과 POST의 차이점
![image](https://user-images.githubusercontent.com/62749021/202998292-fe7a1941-1400-45f7-8e27-8d5c8bd7d16e.png)

<br>

![image](https://user-images.githubusercontent.com/62749021/202998332-6f8a64c4-f23a-4330-bbb2-96e9fb49ba6f.png)