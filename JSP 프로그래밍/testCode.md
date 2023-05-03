**사용자의 오컬에 따라 특정 날짜와 시간 형식을 표현하는 방법**

```java
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
```

---

**JSTL fmt 태그를 이용한 다국어 처리**
<fmt:setLocale value="ko">
<fmt:setLocale value="ja">

<fmt:requestEncoding value="euc-kr">
