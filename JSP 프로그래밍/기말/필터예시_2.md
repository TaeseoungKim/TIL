**String id = request.getParameter("id");**
**String param1 = filterConfig.getInitParameter("param1");**

    <security-role>
    	<role-name>guest</role-name>
    </security-role>
    <security-constraint>
    	<web-resource-collection>
    		<web-resource-name>JSPBook</web-resource-name>
    		<url-pattern>/Book/addBook.jsp</url-pattern>
    		<http-method>GET</http-method>
    	</web-resource-collection>
    	<auth-constraint>
    		<description></description>
    		<role-name>guest</role-name>
    	</auth-constraint>
    </security-constraint>

    <filter>
    	<filter-name>Filter02_2</filter-name>
    	<filter-class>ch12.com.filter.LogFileFilter</filter-class>
    	<init-param>
    		<param-name>filename</param-name>
    		**<param-value>c:\\logs\\monitor.log</param-value>**
    	</init-param>
    </filter>

```java
package ch12.com.filter;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class InitParamFilter implements Filter{
@Override
public void init(FilterConfig filterConfig) throws ServletException {
// TODO Auto-generated method stub
System.out.println("Filter02 초기화...");
this.filterConfig = filterConfig;
}
@Override
public void doFilter(ServletRequest request, ServletResponse response,
FilterChain filterChain) throws IOException, ServletException {
// TODO Auto-generated method stub
System.out.println("Filter02 수행...");
String id = request.getParameter("id");
String passwd = request.getParameter("passwd");
String param1 = filterConfig.getInitParameter("param1");
String param2 = filterConfig.getInitParameter("param2");
String message;
response.setCharacterEncoding("UTF-8");
response.setContentType("text/html; charset=UTF-8");
PrintWriter writer = response.getWriter();
if(id.equals(param1)&&passwd.equals(param2))
message = "로그인 성공했습니다";
else
message = "로그인 실패했습니다";
writer.println(message);
filterChain.doFilter(request, response);
}
private FilterConfig filterConfig = null;
@Override
public void destroy() {
// TODO Auto-generated method stub
System.out.println("Filter02 해제");
}

}

```

```jsp
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
	<security-role>
		<role-name>guest</role-name>
	</security-role>
	<security-constraint>
		<web-resource-collection>
			<web-resource-name>JSPBook</web-resource-name>
			<url-pattern>/Book/addBook.jsp</url-pattern>
			<http-method>GET</http-method>
		</web-resource-collection>
		<auth-constraint>
			<description></description>
			<role-name>guest</role-name>
		</auth-constraint>
	</security-constraint>
	<login-config>
		<auth-method>FORM</auth-method>
		<form-login-config>
			<form-login-page>/Book/login.jsp</form-login-page>
			<form-error-page>/Book/login_failed.jsp</form-error-page>
		</form-login-config>
	</login-config>

	<error-page>
		<exception-type>java.lang.Exception</exception-type>
		<location>/ch11/exceptionType_error.jsp</location>
	</error-page>

	<filter>
		<filter-name>Filter01</filter-name>
		<filter-class>ch12.com.filter.AuthenFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>Filter01</filter-name>
		<url-pattern>/ch12/filter01_process.jsp</url-pattern>
	</filter-mapping>

	<filter>
		<filter-name>Filter02</filter-name>
		<filter-class>ch12.com.filter.InitParamFilter</filter-class>
		<init-param>
			<param-name>param1</param-name>
			<param-value>admin</param-value>
		</init-param>
		<init-param>
			<param-name>param2</param-name>
			<param-value>1234</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>Filter02</filter-name>
		<url-pattern>/ch12/filter02_process.jsp</url-pattern>
	</filter-mapping>
</web-app>
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form method="post" action="filter02_process.jsp">
		<p> 아이디 : <input type="text" name="id"></p>
		<p> 비밀번호 : <input type="password" name="passwd"></p>
		<p><input type="submit" value="전송"></p>
	</form>
</body>
</html>
```

````jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
		String id = request.getParameter("id");
		String passwd = request.getParameter("passwd");
	%>
	<p> 입력된 id 값 : <%=id %></p>
	<p> 입력된 pw 값 : <%=passwd %></p>
</body>
</html>
```


**써보기**

```jsp
<filter>
	<filter-name>Filter02</filter-name>
	<filter-class>ch12.com.filter.InitParamFilter</filter-class>

	<init-param>
		<param-name>param1</param-name>
		<param-value>admin</param-value>
	</init-param>

	<init-param>
		<param-name>param2</param-name>
		<param-value>1234</param-value>
	</init-param>
</filter>

<filter-mapping>
	<filter-name>Filter02</filter-name>
	<url-pattern>/ch12/filter02_process.jsp</url-pattern>
</filter-mapping>
```
````
