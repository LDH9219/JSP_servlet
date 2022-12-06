# 서비스 레이어 분리하기

## Notice

![image](https://user-images.githubusercontent.com/62749021/205894664-0adfb368-a1af-46af-9c64-97c8629133d6.png)

공지사항 페이지 요청 = ```getNoticeList()```
다음, 이전 및 번호 페이지 요청 = ```getNoticeList(int page)```
제목, 작성자 를 통해 검색 요청 = ```getNoticeList(String field, String query, int page)```
총 공지사항의 페이지 세기 = ```getNoticeCount(), getNoticeCount(String field, String query)```

## NoticeDetail

![image](https://user-images.githubusercontent.com/62749021/205894951-96086172-12b0-480d-a3b6-97cbd0173efa.png)

페이지 요청 = ```getNotice(id)```
다음 글 요청 = ```getNextNotice(id)```
이전 글 요청 = ```getPrevNotice(id)```

서비스 페이지 분리(NoticeService.java)

### Service

```java

```

### Controller

#### ListController

```java

```

***

#### DetailController

```java

```

### View

#### list.jsp

```java

```

***

#### detail.jsp
```java

```
