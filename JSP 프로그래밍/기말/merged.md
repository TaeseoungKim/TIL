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

```java

```

```java

```
