# JSTL(JSP Standard Tag Library)

참고 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=imf4&logNo=220654812087

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

# JSTL:if

```java
<c:forEach> 문의 경우 옵션으로 begin과 end를 갖는다.

<c:forEach var="n" items="${list}" begin="1" end="3" varStatus="st">
   begin = 1번 '인덱스'부터
   end = 3번 '인덱스' 까지 forEach문을 통하여 출력한다. 
   varStatus = forEach문이 반복할 때 관리되는 상태 값을 사용할 수 있게 하는 속성
</c:forEach>
```
forEach문의 속성

![image](https://user-images.githubusercontent.com/62749021/205440412-f7546281-f79f-4b99-be1f-8d45952eda72.png)

다음은 list.jsp 파일 내용 중 Pager 번호를 ```<c:set>```과 ```<c:forEach>```문을 사용하여 만드는 예시이다.
```java
<c:set var="page" value="${(param.p==null)?1:param.p}" />
	<c:set var="startNum" value="${page-(page-1)%5}" />
	
	<ul class="-list- center">
		<c:forEach var="i" begin="0" end="4"  >
		<li><a class="-text- orange bold" href="?p=${startNum+1}&t=&q=" >${startNum+i}</a></li>
		</c:forEach>
	</ul>
 ```

# JSTL:if

위 Pager 코드에 Pager 이전/ 다음번호 기능을 추가하기 위하여 ```<c:if> </c:if>``` JSTL 을 사용한다.

```java
	<c:set var="page" value="${(param.p==null)?1:param.p}" />
	<c:set var="startNum" value="${page-(page-1)%5}" />
	<c:set var="lastNum" value="23" />
	<%-- 현재 마지막 레코드를 임의로 지정한 상태. 레코드는 DB에 의존하기 때문에 JSTL로 완성할 수 없어 임의로 지정하였다. --%>
	
	
	<div>
		
		<c:if test="${startNum>1}">
			<a class="btn btn-prev" href="?p=${startNum-1}&t=&q=" >이전</a>
		</c:if>
		
		<c:if test="${startNum<=1}">
			<span class="btn btn-prev" onclick="alert('이전 페이지가 없습니다.');">이전</span>
		</c:if>
		
	</div>
	
	
	<ul class="-list- center">
		<c:forEach var="i" begin="0" end="4"  >
		<li><a class="-text- orange bold" href="?p=${startNum+1}&t=&q=" >${startNum+i}</a></li>
		</c:forEach>
	</ul>
	
	<div>
			<c:if test="${startNum+5 < lastNum }">
				<a href="?p=${startNum+5}&t=&q=" class="btn btn-next">다음</a>
			</c:if>
			
			<c:if test="${startNum+5 >= lastNum }">
				<span class="btn btn-next" onclick="alert('다음 페이지가 없습니다.');">다음</span>
			</c:if>
	</div>
```

# JSTL:forTokens
문자열을 분리하여 출력하는 JSTL 이다. ```StringTokenizer```와 유사하다.
```delims=""``` 속성을 통해서 분리할 지점을 선택할 수 있다.

아래의 예시에서는 첨부파일들을 ```forTokens```를 이용하여 분리한 후, ```a``` 태그를 이용하여 링크를 삽입한다.

```java
<tr>
	<th>첨부파일</th>
		<td colspan="3" style="text-align:left;text-indent:10px;">
		
		<c:forTokens var="fileName" items="${n.files}" delims="," varStatus="st" >
			<a href="${fileName}"> ${fileName} </a>
				<c:if test="${!st.last}">
				/
				</c:if>
				</c:forTokens>							
		</td>
</tr>
```

# JSTL:format
참고 : https://jamesyleather.tistory.com/358    
    
날짜, 시간,  형식을 변경하는 태그이다.       

## format 날짜 형식

format JSTL 을 사용하기 위해서는 아래와 같은 코드블럭이 필요하다.
```java
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```
    
주석표시가 붙은 문장이 JSTL 포맷 태그를 이용하여 날짜 형식을 지정하여 출력하는 문장이다.

list.jsp
```java
<c:forEach var="n" items="${list}">	
	<tr>
		<td>${n.id }</td>
		<td class="title indent text-align-left"><a href="detail?id=${n.id}">${n.title}</a></td>
		<td>${n.writerId}</td>
		<td> <fmt:formatDate value="${n.regdate}" pattern="yyyy-MM-dd" /> </td> <%-- 이 문장 --%>
		<td>${n.hit}</td>
	</tr> 
</c:forEach>
```

## format 숫자형식
```java
<fmt:formatNumber value="${n.hit}"/>
```
조회수로 설정한 hit 가 3자리마다 ,로 끊어지는 format 형식이다. 추가적인 pattern이 있다. 위의 링크 참고.

# JSTL:functions
참고 : https://cofs.tistory.com/262
