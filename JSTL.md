# JSTL(JSP Standard Tag Library)

JSTL은 크게 5가지의 태그 라이브러리를 제공한다.
Core = 제어의 행위    
Formating = 값 출력 시 형식    
Functions = 문자열 조작에 필요한 함수 라이브러리    
SQL , XML 이 추가로 있으나 SQL 과 XML은 사용하지 MVC 모델을 위해 사용하지 않는다.
    
태그 라이브러리

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
prefix를 통해 접두사를 c 로 설정하고, <c:tag > 를 통해 사용한다.
ex) <c:forEach> , <c:url> , <c:set> etc

```

```java
<c:forEach> 문의 경우 옵션으로 begin과 end를 갖는다.

<c:forEach var="n" items="${list}" begin="1" end="3" varStatus="st">
   begin = 1번 '인덱스'부터
   end = 3번 '인덱스' 까지 forEach문을 통하여 출력한다. 
   varStatus = forEach문이 반복할 때 관리되는 상태 값을 사용할 수 있게 하는 속성
</c:forEach>
```

![image](https://user-images.githubusercontent.com/62749021/205440412-f7546281-f79f-4b99-be1f-8d45952eda72.png)

```java
<c:set var="page" value="${(param.p==null)?1:param.p}" />
	<c:set var="startNum" value="${page-(page-1)%5}" />
	
	<ul class="-list- center">
		<c:forEach var="i" begin="0" end="4"  >
		<li><a class="-text- orange bold" href="?p=${startNum+1}&t=&q=" >${startNum+i}</a></li>
		</c:forEach>
	</ul>
 ```
