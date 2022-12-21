# 서비스 레이어 분리하기
목차
1. 구현 목표
2. 오늘 구현
3. 작동원리 및 순서
4. 어려웠던 점
5. 해결방법

***

## NoticeDetail

![image](https://user-images.githubusercontent.com/62749021/205894951-96086172-12b0-480d-a3b6-97cbd0173efa.png)

페이지 요청 = ```getNotice(id)```
다음 글 요청 = ```getNextNotice(id)```
이전 글 요청 = ```getPrevNotice(id)```

서비스 페이지 분리(NoticeService.java)

***

## 구현 목표

![image](https://user-images.githubusercontent.com/62749021/205894664-0adfb368-a1af-46af-9c64-97c8629133d6.png)

```
DB에서 단순히 Notice를 읽어올 뿐만 아니라

하단의 번호 바를 이용해 공지사항 페이지를 요청하거나 = getNoticeList()
다음, 이전 번호 페이지를 요청하거나 = getNoticeList(int page)
제목, 작성자를 통해 검색을 요청하거나 = getNoticeList(String field, String query, int page)
공지사항의 총 개수를 통해 페이지를 카운팅 하는 기능

상기에 기술한 기능들을 NoticeService.java에 추가한다.
```

## 작동원리

### Service

```java
//NoticeService.java
package com.newlecture.web.service;

import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

import com.newlecture.web.entity.Notice; //notice 에 필요한 
import com.newlecture.web.entity.NoticeView;

public class NoticeService {
	public int removeNoticeAll(int[] ids){
		
		return 0;
	}
	
	public int pubNoticeAll(int[] ids){
		
		return 0;		
	}
	
	public int insertNotice(Notice notice){
		int result = 0;
		
		String sql = "INSERT INTO NOTICE(TITLE, CONTENT, WRITER_ID, PUB) VALUES(?,?,?,?)";
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			st.setString(1, notice.getTitle());
			st.setString(2, notice.getContent());
			st.setString(3, notice.getWriterId());
			st.setBoolean(4,notice.getPub());
			
			result = st.executeUpdate(); // update = insert , update, delete 에 사용한다.

			
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		

		
		return result;	
	}

	public int deleteNotice(int id){
		
		return 0;
	}
	
	public int updateNotice(Notice notice){
		
		return 0;
	}
	List<Notice>getNoticeNewestList(){
		
		return null;
	}
	
	
	
	public List<NoticeView> getNoticeList(){
		
		return getNoticeList("title","",1);
	}
	
	public List<NoticeView> getNoticeList(int page){
		
		return getNoticeList("title","",page);
	}
	
	public List<NoticeView> getNoticeList(
			String field /* TITLE, WRITER_ID */,
			String query, /* A */
			int page){
		
		List<NoticeView> list = new ArrayList<>();
		
		String sql = "SELECT * FROM(" +
				"    SELECT ROWNUM NUM, N. * " +
				"    FROM (SELECT * FROM NOTICE_VIEW WHERE "+field+" LIKE ? ORDER BY REGDATE DESC) N "+
				") " +
				" WHERE NUM BETWEEN ? AND ?";
				
		// 1, 11, 21, 31 -> an = 1+(page-1)*10
		// 10, 20, 30, 40 -> page*10
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			
			st.setString(1, "%"+query+"%");
			st.setInt(2, 1+(page-1)*10);
			st.setInt(3, page*10);
			
			ResultSet rs = st.executeQuery();

			while(rs.next()){
				int id = rs.getInt("ID");
				String title = rs.getString("TITLE");
				String writerId = rs.getString("WRITER_ID");
				Date regdate = rs.getDate("REGDATE");
				int hit = rs.getInt("HIT");
				String files = rs.getString("FILES");
				//String content = rs.getString("CONTENT");
				int cmtCount = rs.getInt("CMT_COUNT");
				boolean pub = rs.getBoolean("PUB");
				
				NoticeView notice = new NoticeView(
						id,
						title,
						writerId,
						regdate,
						hit,
						files,
						pub,
						//content,
						cmtCount
				);
				list.add(notice);	
			}
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return list;
	}
	
	public int getNoticeCount() {
		
		return getNoticeCount("title","");
	}
	
	public int getNoticeCount(String field, String query) {
		
		int count = 0;
		
		String sql = "SELECT COUNT(ID) COUNT FROM ("
				+ "    SELECT ROWNUM NUM, N.* "
				+ "    FROM (SELECT * FROM NOTICE WHERE "+field+" LIKE ? ORDER BY REGDATE DESC) N  "
				+ ") ";
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			
			st.setString(1, "%"+query+"%");
			
			ResultSet rs = st.executeQuery();
			
			if(rs.next())
				count = rs.getInt("count");
			
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		
		return count;
	}
	
	public Notice getNotice(int id) {
		
		Notice notice = null;
		
		String sql = "SELECT * FROM NOTICE WHERE ID=?";

		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			
			st.setInt(1, id);
			
			ResultSet rs = st.executeQuery();

			if(rs.next()){
				int nid = rs.getInt("ID");
				String title = rs.getString("TITLE");
				String writerId = rs.getString("WRITER_ID");
				Date regdate = rs.getDate("REGDATE");
				int hit = rs.getInt("HIT");
				String files = rs.getString("FILES");
				String content = rs.getString("CONTENT");
				boolean pub = rs.getBoolean("PUB");
				
				notice = new Notice(
						nid,
						title,
						writerId,
						regdate,
						hit,
						files,
						content,
						pub
				);	
			}
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return notice;
	}
	
	public Notice getNextNotice(int id) {
		Notice notice = null;
		
		String sql = "SELECT * FROM NOTICE "
				+ "WHERE ID = ( "
				+ "    SELECT ID FROM NOTICE "
				+ "    WHERE REGDATE > (SELECT REGDATE FROM NOTICE WHERE ID=?) "
				+ "    AND ROWNUM = 1 "
				+ ")";
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			
			st.setInt(1, id);
			
			ResultSet rs = st.executeQuery();

			if(rs.next()){
				int nid = rs.getInt("ID");
				String title = rs.getString("TITLE");
				String writerId = rs.getString("WRITER_ID");
				Date regdate = rs.getDate("REGDATE");
				int hit = rs.getInt("HIT");
				String files = rs.getString("FILES");
				String content = rs.getString("CONTENT");
				boolean pub = rs.getBoolean("PUB");
				
				notice = new Notice(
						nid,
						title,
						writerId,
						regdate,
						hit,
						files,
						content,
						pub
				);	
			}
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return null;
	}
	
	public Notice getPrevNotice(int id) {
		Notice notice = null;
		
		String sql = "SELECT ID FROM (SELECT * FROM NOTICE ORDER BY REGDATE DESC) " +
				" WHERE REGDATE < (SELECT REGDATE FROM NOTICE WHERE ID=?) " +
				" AND ROWNUM = 1";
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			PreparedStatement st = con.prepareStatement(sql);
			
			st.setInt(1, id);
			
			ResultSet rs = st.executeQuery();

			if(rs.next()){
				int nid = rs.getInt("ID");
				String title = rs.getString("TITLE");
				String writerId = rs.getString("WRITER_ID");
				Date regdate = rs.getDate("REGDATE");
				int hit = rs.getInt("HIT");
				String files = rs.getString("FILES");
				String content = rs.getString("CONTENT");
				boolean pub = rs.getBoolean("PUB");
				
				notice = new Notice(
						nid,
						title,
						writerId,
						regdate,
						hit,
						files,
						content,
						pub
				);	
			}
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return null;
	}

	public int deleteNoticeAll(int[] ids) {
		int result = 0;
		
		String params = "";
		for(int i=0; i<ids.length; i++) {
			params += ids[i];
			
			if(i <= ids.length-1)
				params +=",";
		}
		String sql = "DELETE NOTICE WHERE ID IN ("+params+")";
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String userid = "NEWLEC";
		String passwd = "8327";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,userid,passwd);
			Statement st = con.createStatement();
			
			result = st.executeUpdate(sql); // update = insert , update, delete 에 사용한다.

			
			st.close();
			con.close();
			
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		

		
		return result;
	}
}
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
