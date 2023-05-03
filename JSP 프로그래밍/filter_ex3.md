필터는 임포트 문을 잘 보자
파라미터 = 서블릿 리퀘스트, 서블릿 리스폰스, 필터체인
**filterChain.doFilter(request, response);**

**public void destroy()**

    <filter>
    	<filter-name>Filter01</filter-name>
    	<filter-class>ch12.com.filter.AuthenFilter</filter-class>
    </filter>
    <filter-mapping>
    	<filter-name>Filter01</filter-name>
    	<url-pattern>/ch12/filter01_process.jsp</url-pattern>
    </filter-mapping>

**filter-name은 필터가 시작할 jsp이름을 걸어준다.**
**filter-class는 필터(java) 경로를 걸어주고,**
**filter-mapping의 url-pattern은 필터가 완료될 jsp 경로를 걸어준다.**

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

public class AuthenFilter implements Filter{

    @Override
    public void init(FilterConfig arg0) throws ServletException {
    	System.out.println("Filter01 초기화...");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
    		FilterChain filterChain) throws IOException, ServletException {
    	System.out.println("Filter01.jsp 수행...");
    	String name = request.getParameter("name");

    	if(name == null || name.equals("")) {
    		response.setCharacterEncoding("UTF-8");
    		response.setContentType("text/html charset=UTF-8");
    		PrintWriter writer = response.getWriter();
    		String message = "입력된 name 값은 null 입니다";
    		writer.println(message);
    		return;
    	}
    	filterChain.doFilter(request, response);
    }

    @Override
    public void destroy() {
    	System.out.println("Filter01 해제...");
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
	<form method="post" action="filter01_process.jsp">
		<p> 이름 : <input type="text" name="name"></p>
		<p><input type="submit" value="전송"></p>
	</form>
</body>
</html>
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
	<%
		String name =request.getParameter("name");
	%>
	<p>입력된 name 값 : <%=name %></p>
</body>
</html>
```
