![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"을 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

**관계형 데이터베이스를 관리하는 DBMS에서 사용하는 언어는?**
SQL

**DDL, DML, DCL 각각에 대해 설명하여라.**
DDL은 데이터 정의 언어로 CREATE, ALTER, DROP 등이 있다.
DML은 데이터 조작 언어로 INSERT, SELECT, DELETE 등이 있다.
DCL은 데이터 제어 언어로 GRANT, REVOKE, COMMIT, ROLLBACK 등이 있다.

**JDBC(java database connectivity)와 JSP 연동 과정에 대해서 서술하시오.**

1. java.sql.\* 패키지 임포트
2. JDBC 드라이버 로딩
3. connection 객체 생성
4. 쿼리문 객체 생성(Statement, preparedStatement, CallableStatement 등)
5. 쿼리 실행
6. 쿼리 실행의 결과 값 사용
7. 사용된 객체 종료(ResultSet, Statement/preparedStatement/CallableStatement, Connection 등)

**Statement 객체란?**
정적인 쿼리에 사용되는 객체로 하나의 쿼리를 사용하고 나면 더는 사용할 수 없다.
하나의 쿼리를 끝내면 반드시 close()를 사용하여 객체를 해제해야 한다. (해제하지 않으면, 페이지가 다른 작업을 수행하는 동안 멈추지 않기 때문)

**Statement 객체의 메소드 종류에 대해 서술하시오.**
executeQuery(String sql): ResultSet 객체를 반환하고, SELECT 문을 실행할 때 사용한다.
executeUpdate(String sql): 삽입, 수정, 삭제에 관련된 SQL문을 실행하는데 사용한다.
close(): Statement 객체를 반환할 때 사용한다.

예제) executeQuery(String sql) 사용법 - 데이터 조회

```java
<%
    Connection conn = null;
    // 생략
    Statement stmt = conn.createStatement()
    String sql = "SELECT * FROM Member WHERE id = '1'";
    ResultSet rs = stmt.executeQuery(sql);
    stmt.close();
%>
```

문제) "SELECT \* FROM Member WHERE id = '1'"과 같은 SQL문을 실행하는 코드를 작성하시오

```java
<%
    Connection conn = null;
    Statement stmt = conn.CreateStatement();
    String sql = "SELECT * FROM Member WHERE id = '1'";
    ResultSet rs = stmt.executeQuery(sql);
    stmt.close()
%>
```

예제) executeUpdate(String sql) 사용법 - 데이터 삽입, 수정, 삭제

```java
<%
    Connection conn = null;
    // 생략
    Statement stmt = conn.createStatement();
    String sql = "INSERT INTO Member(id, name, passwd) VALUES ('1','홍길순','1234');"
    // String sql = "UPDATE Member SET name = '관리자' WHERE id = '1'"
    // UPDATE 테이블이름 SET(문) 수정할 열 WHERE 조건

    // String sql = "DELETE FROM Member WHERE id = '1'"
    ResultSet rs = stmt.executeUpdate(sql);
    stmt.close()
%>
```

## 37~41 한번 더 보기

**PreparedStatement 객체란?**
정적인 쿼리에 사용되는 Statement와는 다르게 PreparedStatement 객체는 동적인 쿼리에 사용된다.
PreparedStatement 객체는 하나의 객체로 여러 번의 쿼리를 실행할 수 있고, 동일한 쿼리문을 특정 값만 바꾸어서 여러 번 실행해야 할 때 유용

**PreparedStatement 객체의 메소드 종류에 대해 서술하시오.**
PreparedStatement 객체의 메서드도 Statement 객체의 메서드와 같다.
executeQuery(String sql): ResultSet 객체를 반환하고, SELECT 문을 실행할 때 사용한다.
executeUpdate(String sql): 삽입, 수정, 삭제에 관련된 SQL문을 실행하는데 사용한다.
close(): Statement 객체를 반환할 때 사용한다.

예제) executeQuery(String sql) 사용법 - 데이터 조회

```java
<%
    Connection conn = null;
    String sql = "SELECT * FROM Member WHERE id = ?"
    Statement pstmt = conn.prepareStatement(sql);
    prstmt.setString(1,"1");
    ResultSet rs = pstmt.executeQuery(sql)
    // 생략

%>
```

예제) executeUpdate() 사용법 - INSERT 쿼리문

```java
<%
    Connection conn = null;
    String sql = "INSERT INTO Member(id, name, passwd) VALUES (?,?,?)";
    Statement pstmt = conn.prepareStatement(sql);
    pstmt.setString(1,"1");
    pstmt.setString(2,"홍길순");
    pstmt.setString(3,"1234");
    pstmt.executeUpdate();
    // 생략
    pstmt.close
%>
```

문제) executeUpdate() 사용법을 서술하시오 - UPDATE, DELETE

```java
<%
    Connection conn = null;
    String sql = "UPDATE Member SET name=? WHERE id=?"
    PreparedStatement pstmt = conn.prepareStatement(sql);
    pstmt.setString(1,"1")
    pstmt.setString(2,"관리자")
    pstmt.executeUpdate();
    // 생략
    pstmt.close();
%>
```

```java
<%
    Connection conn = null;
    String sql = "DELETE FROM Member WHERE id=?";
    Statement pstmt = conn.prepareStatement(sql)
    pstmt.setString(1,"1")
    pstmt.executeUpdate()
    // 생략
    pstmt.close();
%>
```

## 46 ~ 48인데, 특히 48 아래 쪽 close

**ResultSet 객체란?**
Statement 또는 PreparedStatement 객체로 SELECT 문을 사용하여 얻어온 레 코드 값을 테이블 형태로 가진 객체

## 52~68 한번 코딩해보기

**web.xml 파일을 이용한 예외 처리 방법을 서술하시오.**
먼저 web.xml 파일은 /WEB-INF/폴더에 있어야 한다.

web.xml 예시)

```xml
<error-page>
<error-code> 오류 코드에 맞는 예외처리 </error-code>
<exception-type> 자바 예외 유형의 정규화된 클래스 이름 설정 </exception-type>
<location> 오류 페이지의 URL을 설정하는데 사용합니다. </location>
</error-page>
```

**오류 코드에 대해 아는 대로 서술하라.**
200 - 요청이 정상적으로 처리됩니다.
307 - 임시로 페이지가 리다이렉트됩니다.
400 - 클라이언트의 요청이 잘못된 구문으로 구성됩니다.
401 - 접근이 허용되지 않습니다.
404 - 지정된 URL을 처리하기 위한 자원이 존재하지 않습니다.
405 - 요청된 메소드가 허용되지 않습니다.
500 - 서버 내부의 에러입니다.(JSP에서 예외가 발생하는 경우)
503 - 서버가 일시적으로 서비스를 제공할 수 없습니다.(서버 과부하나 보수 중인 경우)

web.xml 예시) 에러 코드로 오류 페이지 호출하기

```xml
<web-app...>
<error-page>
<error-code>404</error-code>
<location>errorCode_404.jsp</location>
</error-page>

<error-page>
<error-code>500</error-code>
<location>errorCode_500.jsp</location>
</error-page>

<error-page>
<error-code>503</error-code>
<location>/ch11/errorCode_503.jsp</location>
</error-page>
</web-app...>
```

web.xml 예시) 에러 타입(유형)으로 오류 페이지 호출하기

```xml
<error-page>
<exception-type>java.lang.NullPointerException</exception-type>
<location>/errorNullPointer.jsp</location>
</error-page>
```

**try-catch-finally를 이용한 예외 처리에 대해 설명하시오.**

```java
try{
// 예외가 발생할 수 있는 실행문
}
catch(){ // 처리할 예외 유형 설정
// 예외 발생시 처리할 실행문
}
finally{
// 예외와 상관없이 무조건 실행되는 문장(생략 가능)
}
```

## 86 한번 보기

**RequestDispatcher 객체에 대해 설명하시오.**
클라이언트로부터 최초에 들어온 요청을 JSP/Servlet 내에서 원하는 자원으로 요청을 넘기는 역할을 수행하거나, 특정 자원에 처리를 요청하고 처리 결과를 얻어 오는 기능을 수행하는 클래스

사용법)
RequestDispatcher dispatcher = request.getRequestDispatcher("error.jsp")
dispatcher(request, response)

**JSP 예외 처리 기법 3가지에 대해 설명하시오.**

1. web.xml 파일을 통한 예외처리

```java
<error-page>
<error-code></error-code>|
<exception-type></exception-type>
<location></location>
</error-page>
```

2. <%@ page errorPage='에러 URL 주소' %> 와 <%@ page isErrorPage='true' %>을 혼합하여 사용한다.

<%@ page errorPage='에러 URL 주소' %>는 작동하는 페이지에,
<%@ page isErrorPage='true' %>는 오류 페이지에 삽입하여 명시하는 역할

3. try, catch, finally를 사용한다.
   catch문에 아래와 같이 삽입한다.

```java
RequestDispatcher dispatcher = request.getRequestDispatcher("에러 URL 주소");
dispatcher.forward(request,response);
```

**유효성 검사란?**
사용자가 폼 페이지에서 입력한 데이터 값이 서버로 전송되기 전에 특정 규칙에 맞게 입력되었는지 검증하는 것
부적합한 데이터를 입력하면 다시 폼 페이지로 되돌려 사용자에게 오류가 있음을 알려준다.

ex) 폼 페이지에서 나이를 입력할 때 숫자를 인식하는 검사, 회원 가입시 아이디 중복 검사, 로그인 인증 시 아이디와 비밀번호 검사, IP 패킷 검사 등

ex) 핸들러 함수 등록 예제

```java
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Validation</title>
</head>
<script type="text/javascript">
    function checkform(){
        alert("아이디 : "+document.loginForm.id.value + "\n" + "비밀번호 : " + document.loginForm.passwd.value);
    }
</script>

<body>
    <form name="loginForm">
        <p> 아이디: <input type="text" name="id">
        <p> 비밀번호: <input type="password" name="passwd">
        <p> <input type="submit" value="전송" onclick="checkform">
</body>

```

**유효성 검사 2가지**
기본 유효성 검사: 데이터 값의 존재 유무 검사
데이터 형식 유효성 검사: 데이터가 특정 패턴에 적합한지 검사

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script>
	function checkLogin() {
		var form = document.loginForm;
		if(form.id.value=="") {
			alert("아이디를 입력해주세요");
			form.id.focus();
			return false;
		} else if (form.passwd.value == "") {
			alert("비밀번호를 입력해주세요");
			form.passwd.focus();
			return false;
		}
		form.submit();
	}
</script>
</head>
<body>
	<form name="loginForm" action="validation02_process.jsp" method="post">
		<p> 아이디 : <input type="text" name = "id"></p>
		<p> 비밀번호 : <input type="password" name = "passwd"></p>
		<p><input type="button" value="전송" onclick="checkLogin()"></p>
	</form>
</body>
</html>
```

**중요한 구문**
form.passwd.value == ""
alert("비밀번호를 입력해주세요");
form.passwd.focus();

## 유효성 검사 웹 마켓 부분 꼭 보기

**JSTL이란?**
JSP 표준 태그 라이브러리의 약어
자신만의 태그를 추가할 수 있는 기능을 제공한다.

ex) 사용자의 로케일에 따라 특정 날짜와 시간 형식을 표현하는 방법에 대해서 설명하세요.

```jsp
<%
    Locale locale = request.getLocale();
    String displayLanguage = locale.getDisplayLanguage();
    String language = locale.getLanguage();
    String displayCountry = locale.getDisplayCountry
%>
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.text.NumberFormat" %>
<%@page import="java.text.DateFormat"%>
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h3>현재 로케일의 국가, 날짜, 통화</h3>
	<%
		Locale locale = request.getLocale();
		Date currentDate = new Date();
		DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.FULL, locale);
		NumberFormat numberFormat = NumberFormat.getNumberInstance(locale);
	%>
	<p> 국가 : <%=locale.getDisplayCountry() %></p>
	<p> 날짜 : <%=dateFormat.format(currentDate) %></p>
	<p> 숫자(12345.67) : <%=numberFormat.format(12345.67) %></p>
</body>
</html>
```

**JSTL fmt 태그를 이용한 다국어 처리**
<fmt:setLocale value="ko">
<fmt:setLocale value="ja">

<fmt:requestEncoding value="euc-kr">

**선언적 시큐리티 처리**

String user_id = (String) session.getAttribute("userID");
String user_pw = (String) session.getAttribute("userPW");

쿠키의 동작과정

1. 쿠키 생성: 웹 서버에서 쿠키를 생성한다. 생성된 쿠키는 응답 데이터에 함께 저장되어 웹 브라우저에 전송된다.
2. 쿠키 저장: 웹 브라우저는 응답 데이터에 포함된 쿠키를 쿠키 저장소에 보관한다.
3. 쿠키 전송: 웹 브라우저는 한 번 저장된 쿠키를 요청이 있을 때마다 웹 서버에 전송한다. 웹 서버는 웹 브라우저가 전송한 쿠키를 사용하여 필요한 작업을 수행할 수 있다.

<role rolename="tomcat"/>
<role rolename="role1"/>
<user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
<user username="both" password="<must-be-changed>" roles="tomcat, role1"/>
<user username="role1" password="<must-be-changed>" roles="role1"/>
<role rolename="manager"/>
<user username="admin" password="admin1234" roles="manager">

<security-role>
    <role-name>역할 이름</role-name>
</security-role>
ddl create, altor, drop
dml insert, select, delete
dcl revoke, grant, commit, rollback

```java

```

```java

```

```java

```

```java

```

```java

```

```java

```

```java

```

```java

```

```java

```

```java

```
