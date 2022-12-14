![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"을 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 웹과 JSP 프로그래밍 이해하기

### 인터넷과 웹의 개요

인터넷

- 컴퓨터가 서로 연결되어 TCP/IP라는 통신 프로토콜을 이용하여 정보를 주고 받는 전 세계의 컴퓨터 네트워크

웹

- 인터넷에 연결된 컴퓨터들을 통해 사람들이 정보를 공유할 수 있는 정보 공간
- 월드 와이드 웹(world wide web)의 줄임말

웹의 동작 원리

- 웹은 기본적으로 클라이언트/서버 방식으로 동작
- 기본적으로 http 통신 (요청/응답)

가장 널리 쓰이는 웹 서버

- 아파치
- 톰캣 (아파치 재단에서 배포)
  - 아파치 소프트웨어재단(Apache Software Foundation)에서 개발한 웹 어플리케이션 서버
  - 자바로 만들어진 웹 페이지를 구동하기 위한 엔진
- IIS (윈도우에서만 동작)

### 정적 웹 페이지와 동적 웹 페이지

정적 웹 페이지

- 컴퓨터에 저장된 텍스트 파일을 그대로 보는 것
- 서버: 이미 준비된 HTML 문서를 그대로 전달
  ![](https://i.imgur.com/Efryckf.png)

동적 웹 페이지

- 저장된 내용을 다른 변수로 가공 처리하여 보는 것
- PHP(Personal Home Page), ASP(Active Server Page), JSP(Java Server Page)
- 서버: 사용자 요청에 맞게 정제된 HTML 문서를 전달
  ![](https://i.imgur.com/iMmEeYf.png)

### 웹 프로그래밍과 JSP(JavaServer Pages)

웹 프로그래밍 언어

- 클라이언트 측 실행 언어와 서버 측 실행 언어로 구분
- 자바를 기반으로 하는 **JSP는 서버 측 웹 프로그래밍 언어**
  - JSP: HTML내에 자바 코드를 삽입하여 웹 서버에서 동적으로 웹 페이지를 생성하여 웹 브라우저에 돌려주는 **서버 사이드 스크립트 언어**

JSP 특징

- JSP는 서블릿 기술의 확장(서블릿의 모든 기능을 사용할 수 있다.)
- JSP는 유지 관리가 용이
- JSP는 빠른 개발이 가능
- JSP로 개발하면 코드 길이를 줄일 수 있음

### JSP 페이지의 처리 과정

![](https://i.imgur.com/NhGLduG.png)

JSP 컨테이너에서 아래와 같은 동작들이 수행된다.

1. JSP 요청 (Hello.jsp)
2. 번역: 서블릿 프로그램 (Hello_jsp.java)
3. 컴파일: 서블릿 클래스 (Hello_jsp.class 바이트 코드)

### JSP 생명주기

각 단계별 함수 중요.

![](https://i.imgur.com/aQgWill.png)
![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"을 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 스크립트 태그

### 스크립트 태그

- <% ... %> 사용
- JSP 페이지가 서블릿 프로그램에서 서블릿 클래스로 변환할 때
  - JSP 컨테이너가 자바 코드가 삽입되어 있는 스크립트 태그를 처리하고 나머지는 HTML 코드나 일반 텍스트로 간주

자바 서블릿: 자바를 사용해서 웹 페이지를 동적으로 생성하는 서버 프로그램

![](https://i.imgur.com/wlFf5OQ.png)

**선언문 태그(선언부)**: <%! ... ; %> 자바 변수와 메소드 정의 (jspService() 메소드 외부에 배치가 되어있다.)

- 변수: 전역 변수로 선언한다
- 메소드: 전역 메소드로 사용한다
- 변수와 메소드가 전역으로 사용되기 때문에 순서는 상관없다.
  **스크립틀릿 태그(처리부)**: <% ... ; %> 자바 로직 코드 작성
  **표현문 태그(출력부)**: <%= ... %> 선언문의 메소드를 호출하여 문자열 형태로 출력
  **주석**: <%-- ... --%>

![](https://i.imgur.com/tX0QMCn.png)

![](https://i.imgur.com/Pg9Fxdc.png)
JSP 생명주기의 (4)실행 단계에서 jspService()를 호출하여 스크립틀릿 태그(처리부)와 표현문 태그(출력부)를 처리한다.

**예시)**

![](https://i.imgur.com/0VRAn3w.png)
아래와 같이 변환
![](https://i.imgur.com/PNh39UE.png)

## tip) 한빛 아카데미 예제 소스로 실습 진행하기

## 스크립틀릿 태그의 기능과 사용법

**스크립틀릿 태그**

- 자바 코드로 이루어진 로직 부분을 표현
- out 객체를 사용하지 않고도 쉽게 HTML 응답을 만들어냄
- **지역 변수를 선언할 수 있다.(메소드는 선언할 수 없다.)**
- \_jspService() 메소드 내부에 배치된다.

![](https://i.imgur.com/t1AaYTO.png)
![](https://i.imgur.com/pwbOG5x.png)

## 표현문 태그의 기능과 사용법

**표현문 태그**

- 웹 브라우저에 출력할 부분을 표현
- 표현문 태그에 숫자, 문자, 불린(Boolean) 등의 기본 데이터 타입과 **자바 객체 타입도 사용 가능**
- **각 행을 세미콜론으로 종료할 수 없음**
  - 출력부의 내용이 \_jspService() 메소드의 out.println()의 매개변수로 들어가기 때문이다. (즉, 출력부 자체가 자바코드가 아니다.)
    ![](https://i.imgur.com/dcawj8o.png)

tip) html 주석 vs jsp 주석
html 주석처리는 서블릿으로 변환할 때 주석이 무시되지 않고 모두 컴파일 되지만, 페이지에서 렌더링을 안하는 것(?)
jsp 주석 처리는 서블릿으로 변환할 때 주석처리된 부분은 모두 무시한다.

### bootstrap css 적용하기

![](https://i.imgur.com/TbLW0fT.png)
![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"을 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 디렉티브 태그

### 디렉티브 태그

- JSP 페이지를 어떻게 처리할 것인지를 설정하는 태그
- JSP페이지가 서블릿 프로그램에서 서블릿 클래스로 변환할 때
  - JSP 페이지와 관련된 정보를 JSP컨테이너에 지시하는 메시지

page: <%@ page... %> JSP 페이지에 대한 정보를 설정합니다.
include: <%@ include... %> JSP 페이지의 특정 영역에 다른 문서를 포함합니다.
taglib: <%@ taglib... %> JSP 페이지에서 사용할 태그 라이브러리를 설정합니다.

![](https://i.imgur.com/xG1o3nj.png)

### page 디렉티브 태그

- 현재 JSP 페이지에 대한 정보를 설정하는 태그
- JSP 페이지의 어디에서든 선언할 수 있지만 일바적으로 JSP 페이지의 최상단에 선언하는 것을 권장
  <%@ page 속성1="값1" [속성2="값" ... ] %> // <%와 @사이에 공백이 없어야 한다.>

**page 디렉티브 태그의 속성**

isErrorPage: 현재 JSP 페이지가 오류 페이지인지 여부를 설정한다. (defalut: false)
isELignored: 현재 JSP 페이지의 표현 언어(EL) 지원 여부를 설정한다. (defalut: false)
isScriptingEnabled: 현재 JSP 페이지의 스크립트 태그 사용 여부를 설정한다.

![](https://i.imgur.com/bK0KsnQ.png)

ex) page 디렉티브 태그 사용법 예제
![](https://i.imgur.com/kO7a4s4.png)
![](https://i.imgur.com/Upy9eNZ.png)

- contentType="charset=EUC-KR": 서버에서는 EUC-KR로 인코딩하여 웹 브라우저로 전송한다.
- pageEncoding="UTF-8": JSP파일은 UTF-8로 인코딩이 된다.

Language 속성: jsp 페이지에서 사용할 프로그래밍 언어를 설정(default: java)
contentType 속성: 현재 JSP 페이지의 콘텐츠 유형(MIME-type)을 설정하는데 사용(default: text/html)
![](https://i.imgur.com/LSLXcLB.png)
![](https://i.imgur.com/8e5UQ46.png)
pageEncoding 속성: 현재 JSP 페이지의 문자 인코딩 유형을 설정하는 데 사용. 문자 인코딩 유형의 기본 값은 ISO-8859-1

- msword에서 실행하게 된다.
  ![](https://i.imgur.com/NyZSoOn.png)
  위 그림과 같이하면 contentType, pageEncoding 두 속성을 같은 의미로 사용할 수 있다

import 속성

- 현재 JSP 페이지에서 사용할 자바 클래스를 설정하는 데 사용
- 둘 이상의 자바 클래스를 포함하는 경우 쉼표(,)로 구분하여 연속해서 여러 개의 자바 클래스를 설정
- 또는 여러 개의 자바 클래스를 각각 별도로 설정할 수도 있음

![](https://i.imgur.com/zFTv2VZ.png)

session 속성

- 현재 JSP 페이지의 **HTTP 세션 사용 여부를 설정**하는 데 사용
- 세션은 일반적으로 웹 애플리케이션이 실행되는 동안 사용자가 웹 애플리케이션의 데이터를 가져와 확인할 수 있는 권한을 부여받기 위해 사용함
  - 예를 들어, 사용자가 은행 계좌에 로그인하여 로그아웃(세션 만료)할 때까지 모든 데이터에 접근할 수 있음
- JSP 페이지에 대한 세션을 유지하려면 세션 속성을 true로 함
- 기본 값: 세션을 자동으로 사용하는 true
  - 만약 session 속성 값을 false로 설정할 경우, 해당 JSP 페이지에서 내장 객체인 session 변수를 사용할 수 없다는 의미이므로 해당 페이지에 대해 세션을 유지 관리할 수 없음

buffer 속성

- 현재 JSP 페이지의 출력 버퍼 크기를 설정하는 데 사용
- 속성 값: none과 '버퍼 크기'로 설정 - 버퍼 크기: 출력 버퍼에 먼저 기록한 후 웹 브라우저로 보냄
  ![](https://i.imgur.com/lXiQ6JY.png)

tip) buffer 사용 이유: 작은 데이터를 여러번 전송하는 것보다, 데이터를 모아서 한번에 보내는 것이 효율적이기 때문

autoFlush 속성

- 출력 버퍼를 자동으로 비울것인지 여부

isThreadSafe 속성

- 멀티스레드 처리 여부

info 속성

- JSP 페이지 설명을 위한 설정 (**주석문의 기능과 같음**)

errorPage 속성

- 이동할 오류 페이지 ex) MyErrorPage.jsp 설정
- 웹 서버가 기본 제공하는 오류 페이지를 사용하지 않고 따로 오류페이지를 설정하는 것
  ![](https://i.imgur.com/aIhp2Fq.png)

ex) 오류 발생 예제 (null.toString() 불가능)
![](https://i.imgur.com/EKK7BKP.png)

isErrorPage 속성

- 현재 JSP 페이지가 오류 페이지인지 여부를 설정하는데 사용함
- 기본값은 false, 예외처리를 위한 내장 객체인 exception 변수를 사용할 수 없음
- 속성값은 true로 설정하면, 현재 jsp 페이지는 오류페이지가 됨
- 만약 다른 jsp 페이지에서 오류가 발생하면, 호출되는 오류페이지는 true가 설정된 페이지가 됨

tip) MIME(Multipurpose Internet Mail Extensions)

- 전자우편을 위한 인터넷 표준 포맷. 현재는 웹을 통해서 여러 형태의 파일 전달을 위해 사용

tip) 표현언어(표현식) 사용법

- ${2+5} // 7 출력
- \${2+5} // "${2+5}" 출력

![](https://i.imgur.com/kO7a4s4.png)
![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"을 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 액션 태그

### 액션 태그

- 서버나 클라이언트에게 어떤 행동을 하도록 명령하는 태그
- JSP 페이지에서 페이지와 페이지 사이 제어
- 다른 페이지의 실행 결과 내용을 현재 페이지에 포함
- 자바빈즈(JavaBeans) 등의 다양한 기능 제공
  - 자바빈즈(JavaBeans): 자바로 작성된 **소프트웨어 컴포넌트**
- <%...%>와 같은 스크립트 태그의 형식을 따르지 않고, XML 형식 <jsp: .../>을 사용한다.

#### **forward 액션태그: <jsp:forward .../>**

- 다른 페이지로의 이동과 같은 페이지 흐름을 제어한다.
- JSP 컨테이너는 현재 JSP 페이지에서 forward 액션 태그를 만나면 그 전까지 출력 버퍼에 저장되어 있던 내용을 모두 삭제하고, forward 액션 태그에 설정된 페이지로 프로그램의 제어가 이동
- page 속성 값: 이동할 페이지의 외부 파일명(같은 디렉토리면 파일명만, 다르면 전체 URL 또는 상대 경로)
  ![](https://i.imgur.com/Mwwf6Dr.png)

ex) forward 액션 태그 사용 예제
![](https://i.imgur.com/g0EGJdz.png)
"이 파일은 first.jsp입니다"는 출력되지 않은 것을 볼 수 있다. (버퍼를 비우기 때문)
![](https://i.imgur.com/B8g9RnK.png)

tip) forword액션 태그 사용 시 주의점
![](https://i.imgur.com/HJvyFYm.png)

액션 태그를 사용하는 이유: 자바 코드의 삽입을 되도록 최소화하여 유지 보수를 효율적으로 하는 것이 목적이다.

#### **include 액션 태그: <jsp:include .../> 외부 페이지의 내용을 포함하거나 페이지를 모듈화한다.**

- include 디렉티브 태그처럼 현재 JSP 페이지의 특정 영역에 외부 파일의 내용을 포함하는 태그
- 현재 JSP 페이지에 포함할 수 있는 외부 파일은 HTML, JSP, 서블릿 페이지 등
- page 속성: 현재 JSP 페이지 내에 **포함할 내용**을 가진 외부 파일명
- flush 속성(default: false): 설정한 외부 파일로 제어가 이동할 때, 출력 버퍼에 저장한 결과를 처리(true면 제어가 이동할 때 모두 비운다. -> 삭제가 아닌 비우는 것)
  - 헤더정보도 같이 전송되기 때문에 웹 브라우저에 전송되고 나면 헤더 정보가 반영되지 않아 문제가 될 수 있다.
- include 액션태그가 forward 액션태그와 다른 점은 포함된 외부 파일이 실행된 후 현재 JSP 페이지로 제어를 반환한다는 것
  ![](https://i.imgur.com/zaxaUIF.png)
  제어권: first.jsp -> second.jsp -> first.jsp
  ("Java Server Page" 출력 위치를 보면 이해가 쉽다.)

**tip) include 액션 태그 vs include 디렉티브 태그**
![](https://i.imgur.com/XJKAfJ2.png)

- include 액션 태그는 포함될 페이지의 결과가 원래 페이지의 결과와 **합쳐져서** 보인다.
- include 디렉티브 태그는 포함될 파일의 **소스코드를 복사하여 붙여넣기** 함. 조각코드 삽입개념. 단순하게 다른 페이지의 내용이 텍스트로 포함된다.
  ![](https://i.imgur.com/KEq6nHv.png)

#### param 액션 태그: <jsp:param .../> <jsp:forward>, <jsp:include>, <jsp:plugin> 태그에 인자를 추가한다.

- 현재 JSP 페이지에서 다른 페이지에 정보를 전달하는 태그
- 이 태그는 단독으로 사용되지 못하며 <jsp:forward>나 <jsp:include> 태그의 내부에 사용
- 다른 페이지에 여러 개의 정보를 전송해야 할 때는 다중의 param 액션 태그 사용
  ![](https://i.imgur.com/KTAT9Pm.png)

![](https://i.imgur.com/Q9rVXVp.png)

#### 그 외 태그

- useBean: <jsp:useBean .../> JSP 페이지에 자바빈즈를 설정한다.
- setProperty: <jsp:setProperty .../> 자바빈즈의 프로퍼티 값을 설정한다.
- getProperty: <jsp:getProperty .../> 자바빈즈의 프로퍼티 값을 얻어온다.
- plugin: <jsp:plugin .../> 웹 브라우저에 자바 애플릿을 실행한다. 자바 플러그인에 대한 OBJECT 또는 EMBED 태그를 만드는 브라우저별 코드를 생성한다.
- element: <jsp:element .../> 동적 XML요소를 설정한다.
- attribute: <jsp:attribute .../> 동적으로 정의된 XML 요소의 속성을 설정한다.
- body: <jsp:body .../> 동적으로 정의된 XML 요소의 몸체를 설정한다.
- text: <jsp:text .../> JSP 페이지 및 문서에서 템플릿 텍스트를 작성한다.

![](https://i.imgur.com/4lZJJGD.png)
![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 자바빈즈 액션 태그

### 자바빈즈 액션 태그

자바빈즈

- 동적 컨텐츠 개발을 위해 자바 코드를 사용하여 자바 클래스로 로직을 작성하는 방법
- JSP 페이지에서 화면을 표현하기 위한 계산식이나 자료의 처리를 담당하는 **자바코드를 따로 분리하여 작성하는 것이 자바빈즈**
- 따라서, JSP 페이지는 HTML과 같이 쉽고 간단한 코드만으로 구성된다.

![](https://i.imgur.com/GXXqG9X.png)

#### 자바빈즈를 작성할 때 규칙

- 자바 클래스는 java.io.Serializable 인터페이스를 구현해야 함
- 인수가 없는 기본 생성자가 있어야 함
- 모든 멤버 변수인 프로퍼티는 private 접근 지정자로 설정해야 함
- 모든 멤버 변수인 프로퍼티는 getter/setter() 메소드가 존재해야 함
  - getter() 메소드는 멤버 변수에 저장된 값을 가져올 수 있는 메소드이고, setter() 메소드는 멤버 변수에 값을 저장할 수 있는 메소드
- getter/setter 메소드: 직접 접근하는 것을 방지한다.

java.io.Serialiable 인터페이스는 생략 가능하나 자바빈즈 규약에 명시된 내용으로, **자바빈즈에 저장된 프로퍼티를 포함한 채로 파일 시스템에 저장되거나 네트워크로 전송**될 수 있도록 **객체 직렬화**를 제공해야 하므로 implement해야한다.

![](https://i.imgur.com/irPtx81.png)

자바빈즈는 JSP 페이지에서 useBeans, setProperty, getProperty 등의 자바빈즈 액션 태그와 스크립트 태그에 자바 코드와 같이 사용할 수 있다. 또한 폼 페이지의 입력 데이터나 HTML 페이지에서 넘어오는 데이터를 쉽게 자바빈즈 객체로 저장할 수 있다.

### useBeans 액션 태그

- JSP 페이지에서 자바빈즈를 사용하기 위해 **실제 자바 클래스를 선언하고 초기화**하는 태그
- id 속성과 scope 속성을 바탕으로 자바빈즈의 객체를 검색하고, 객체가 발견되지 않으면 빈 객체를 생성
  ![](https://i.imgur.com/i9rZkMN.png)

- id: 자바빈즈를 식별하기 위한 이름
- class: 패키지 이름을 포함한 자바빈즈 이름. 자바빈즈는 인수가 없는 기존 생성자가 있어야 하며 추상 클래스를 사용할 수 없다
- scope: 자바빈즈가 지정되는 영역을 설정한다. page(기본값), request, session, application 중 하나의 값을 사용한다.

### 객체 범위 종류

웹 애플리케이션에는 4개의 객체 범위가 존재한다.

**page 영역**

- 한 번의 클라이언트 요청이 오면, 하나의 JSP 페이지가 응답된다.

**request 영역**

- 요청을 받아서 응답하기까지 객체가 유효한 영역
- **Servlet에서 forward 또는 include를 사용하면, request 요청 객체가 공유되어서 request 영역이 된다.**

**session 영역**

- 하나의 브라우저 당 1개의 session 객체가 생성된다.
- 즉, 같은 브라우저 내에서 요청되는 페이지들은 같은 객체를 공유하게 되는데, 이를 세션 영역이라 한다.

**application 영역**

- 하나의 애플리케이션 당 1개의 application 객체가 생성된다.
- 즉, 같은 애플리케이션 내에서 요청되는 페이지들은 같은 객체를 공유하게 되는데 이를 애플리케이션 영역이라 한다.

scope 범위 **page < request < session < application** 순

![](https://i.imgur.com/pH7ONFh.png)

**DAO: Date Access Object.**

- DB의 data에 접근하는 트랜잭션 객체

**DTO: Data Transfer Object.**

- 계층간 데이터 교환을 위한 자바 빈즈.
- DB레코드의 data를 매핑하기 위한 데이터 객체.
- DTO는 로직을 가지지 않으며 getter, setter 메소드만 가지는 클래스다.
- setter를 활용하므로 가변 속성

**VO: Value Object**

- Read Only 속성을 가짐
  ![](https://i.imgur.com/8EcGqQu.png)

tip) 자바 네이밍 룰

일반적 관례

- 클래스 이름은 대문자의 명사로 시작
- 메서드 이름은 소문자의 동사
- 변수는 소문자의 명사
- 상수는 대문자의 명사

올바른 예

- 클래스 이름: ClassName
- 메서드 이름: getValue, get_Value
- 변수 이름: value
- 상수 이름: CONSTANT_VALUE

공통 규칙
공통적으로 카멜 표기법을 사용한다

- 기본적으로 변수명을 모두 소문자로 쓰고 여러 단어가 이어지는 경우 첫단어를 제외하고 각 단어의 첫글자만 대문자로 지정한다
- ex) camelCase, memberInsert

약어는 최대한 쓰지 않으며 풀네임을 사용한다

- 예외) id, pw

반의어는 반드시 대응되는 개념으로 사용한다

- ex) start/finish, insert/deleter, first/last
  ![](https://i.imgur.com/60nvlYr.png)
  ![](https://i.imgur.com/GNLxZwn.png)
  ![](https://i.imgur.com/F7sqesR.png)
  ![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## JSP 웹 프로그래밍 기본 구조

**JSP 웹 프로그래밍 기본 구조**

- 한 파일에서 모든 작업을 할 수 없기 때문에 요청을 하는 페이지와 요청을 처리해서 응답하는 페이지로 분리함

**요청 페이지**: 요청을 입력받는 페이지로 로그인이나 회원가입 페이지가 포함된다

- html 또는 jsp 파일

**응답 페이지**: 해당 요청을 처리한 뒤 요청 페이지로 응답을 보내는 페이지

- jsp 파일

이클립스에서 작업시, 파일 우클릭 -> Open With -> Web Page Editor를 사용하면 작업 내용을 실시간으로 확인할 수 있다.

## JSP 내장 객체

- JSP 페이지에서 사용할 수 있도록 JSP 컨테이너에 미리 정의된 객체
- JSP 페이지가 서블릿 프로그램을 번역될 때 JSP 컨테이너가 자동으로 내장 객체를 멤버 변수, 메소드 매개변수 등의 각종 참조 변수(객체)로 포함
- JSP 페이지에 별도의 import 문 없이 자유롭게 사용 가능
- 스크립틀릿 태그나 표현문 태그에 선언을 하거나 객체를 생성하지 않고도 직접 호출하여 사용 가능

![](https://i.imgur.com/jrK49ww.png)
![](https://i.imgur.com/1X09Orn.png)

### 내장 객체의 종류

request: javax.servlet.http.HttpServletRequest

- 웹 브라우저의 HTTP 요청 정보를 저장한다.
- 웹 브라우저에서 JSP 페이지로 전달되는 정보의 모임
- HTTP 헤더와 HTTP 바디로 구성
- 웹 컨테이너가 요청된 HTTP 메시지를 통해 HttpServletRequest 객체를 얻어낸다.
- JSP 페이지에서는 HttpServletRequest 객체를 request 객체명으로 사용

response: javax.servlet.http.HttpServletResponsse

- 웹 브라우저의 HTTP 요청에 대한 응답 정보를 저장한다.

out: javax.servlet.jsp.jsp.jspWriter

- JSP 페이지에 출력할 내용을 담고 있는 출력 스트림이다.

session: javax.servlet.http.HttpSession

- 웹 브라우저의 정보를 유지하기 위한 세션 정보를 저장한다.

application: javax.servlet.ServletContext

- 웹 애플리케이션의 콘텍스트 정보를 저장한다.
  PageContext: javax.servlet.jsp.PageContext
- JSP 페이지의 정보를 저장한다.

page: java.lang.Object

- JSP 페이지를 구현한 자바 클래스로 JSP 페이지 자체를 나타낸다.

config: javax.servlet.ServletConfig

- JSP 페이지의 설정 정보를 저장한다.

exception: java.lang.Throwable

- JSP 페이지의 예외 발생을 처리한다.

![](https://i.imgur.com/ODGe5nd.png)

## request 내장 객체의 기능과 사용법

### request 내장 객체

- JSP 페이지에서 가장 많이 사용되는 기본 내장 객체
- 웹 브라우저에서 서버의 JSP 페이지로 전달하는 정보를 저장
  - 폼 페이지로부터 입력된 데이터를 전달하는 요청 파라미터 값을 JSP 페이지로 가져 옴
- JSP 컨테이너는 웹 브라우저에서 서버로 전달되는 정보를 처리하기 위해 javax,servlet.http.HttpServletRequest 객체 타입의 request 내장 객체를 사용하여 사용자의 요구 사항을 얻어 냄

### 요청 파라미터 관련 메소드

요청 파라미터는 사용자가 폼 페이지에 데이터를 입력한 후 서버에 전송할 때 전달되는 폼 페이지의 입력된 정보 형태를 말함
요청 파라미터는 <name=value>형식으로 웹 브라우저에서 서버의 JSP 페이지로 전송

### 요청 파라미터 관련 메소드의 종류

getParameter(String name): String 반환, **요청 파라미터 이름이 name인 값**을 전달받는다. 요청 파라미터 값이 없으면 null을 반환한다.
getParameterValues(String name): String[] 반환, **모든 요청 파라미터 이름이 name인 값을 배열 형태**로 전달받는다. 요청 파라미터 값이 없으면 null을 반환한다.
getParameterNames(): java.util.Enumeration 반환, **모든 요청 파라미터의 이름과 값을 Enumeration 객체 타입**으로 전달받는다.
getParameterMap(): java.util.Map 반환, 모든 **요청 파라미터의 이름과 값을 Map 객체 타입으로 전달**받는다. [Map 객체 타입은 (요청 파라미터 이름, 값) 형식으로 구성된다.]

- ex)request 내장 객체로 폼 페이지로부터 아이디와 비밀번호를 전송받아 출력하기

![](https://i.imgur.com/JyrYhQW.png)
![](https://i.imgur.com/3cIfmdF.png)

### 요청 HTTP 헤더 관련 메소드

- 웹 브라우저는 HTTP 헤더에 부가적인 벙보를 담아 서버로 전송

### 요청 HTTP 헤더 관련 메소드의 종류

getHeader(String name): String 반환, 설정한 name의 헤더 값을 가져온다.
getHeaders(String name): Enumeration 반환, 설정한 name의 헤더 목록 값을 가져온다.
getHeaderNames(): Enumeration 반환, 모든 헤더 이름을 가져온다.
getIntHeader(String name): int 반환, 설정한 name의 헤더 값을 정수로 가져온다.
getDateHearder(String name): long 반환, 설정한 name의 값을 시간 값으로 가져온다.
getCookies(): javax.servlet.http.Cookie 반환, 모든 쿠키 값을 가져온다.

ex) request 내장 객체 사용 예: 요청 HTTP 헤더 정보 값 출력하기

![](https://i.imgur.com/izbiTH3.png)

ex) request 내장 객체로 모든 HTTP 헤더 정보 값 출력하기
![](https://i.imgur.com/hudssZU.png)

### 웹 브라우저/서버 관련 메소드

![](https://i.imgur.com/ogvZS0c.png)
![](https://i.imgur.com/SDPrluJ.png)

ex) request 내장 객체로 모든 웹 브라우저 및 서버 정보 값 출력하기
![](https://i.imgur.com/XOawoMx.png)

## response 내장 객체의 기능과 사용법

### response 내장 객체

- 사용자의 요청을 처리한 결과를 서버에서 웹 브라우저로 전달하는 정보를 저장하고 서버는 응답 헤더와 요청 처리 결과 데이터를 웹 브라우저로 보냄
- JSP 컨테이너는 서버에서 웹 브라우저로 응답하는 정보를 처리하기 위해 javax.servlet.http.HttpServelteResponse 객체 타입의 response 내장 객체를 사용하여 사용자의 요청에 응답

### 페이지 이동 관련 메소드

**페이지 이동 = 리다이렉션(redirection)**

- 사용자가 새로운 페이지를 요청할 때와 같이 페이지를 강제로 이동하는 것
- 서버는 웹 브라우저에 다른 페이지로 강제 이동하도록 response 재장 객체의 리다이렉션 메소드를 제공
- 페이지 이동 시에는 문자 인코딩을 알맞게 설정해야 함

**forward와의 차이**

- 요청을해서 A라는 페이지로 이동해주세요, 하는데 요청자에게 block이 되서 정보를 제한적으로 이동(내가 갖고 있는 페이지에서만 이동이 가능)
- direct를 하게되면 내가 갖고 있지 않더라도 요청자에게 정보를 공개 적으로 제공(내가 갖고 있지 않더라도 페이지 이동 가능)

![](https://i.imgur.com/83lH8Xx.png)

### 페이지 이동 관련 메소드의 종류

sendRedirect(String url): void 반환, 설정한 URL 페이지로 강제 이동한다.
![](https://i.imgur.com/zJWuP5q.png)

ex) responsse 내장 객체로 페이지 이동하기
![](https://i.imgur.com/q79cdjA.png)
![](https://i.imgur.com/0JUNjBU.png)
![](https://i.imgur.com/ZsF3K98.png)

### 응답 HTTP 헤더 관련 메소드

- 응답 HTTP 헤더 관련 메소드는 서버가 웹 브라우저에 응답하는 정보에 헤더를 추가하는 기능을 제공
- 헤더 정보에는 주로 서버에 대한 정보가 저장되어 있음

### 응답 HTTP 헤더 관련 메소드의 종류

![](https://i.imgur.com/Q0TLmeg.png)
![](https://i.imgur.com/KkQUd9h.png)

### 응답 콘텐츠 관련 메소드

- response 내장 객체는 웹 브라우저로 응답하기 위해 MIME 유형, 문자 인코딩, 오류 메시지, 상태 코드 등을 설정하고 가져오는 응답 콘텐츠 관련 메소드 제공

### 응답 콘텐츠 관련 메소드의 종류

![](https://i.imgur.com/1R0utuM.png)
![](https://i.imgur.com/ggFi7F0.png)
ex) response 내장 객체로 오류 응답 코드와 오류 메시지 보내기
![](https://i.imgur.com/v7PVaht.png)

## out 내장 객체의 기능과 사용법

### out 내장 객체

- 웹 브라우저에 데이터를 전송하는 출력 스트림 객체
- JSP 컨테이너는 JSP 페이지에 사용되는 모든 표현문 태그와 HTML, 일반텍스트 등을 out 내장 객체를 통해 웹 브라우저에 그대로 전달
- 스크립틀릿 태그에 사용하여 단순히 값을 출려가는 표현문 태그(<%= ...%>)와 같은 결과를 얻을 수 있음

### out 내장 객체 메소드의 종류

![](https://i.imgur.com/nse2L3A.png)

ex) out 내장 객체 사용 예
![](https://i.imgur.com/asVftZL.png)

ex) out 내장 객체로 오늘의 날짜 및 시간 출력하기
![](https://i.imgur.com/GVtRNhp.png)

ex) out 내장 객체로 폼페이지에서 아이디와 비밀번호를 전송받아 출력하기

![](https://i.imgur.com/JGn1tL8.png)
![](https://i.imgur.com/qo6Dsor.png)
![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 폼 처리의 개요

### 폼(form)

- 사용자가 웹 브라우저를 통해 입력된 모든 데이터를 한번에 웹 서버로 전송하는 양식
  - 전송한 데이터는 웹 서버가 처리하고 처리 결과에 따라 다른 웹페이지를 보여줌
- 사용자와 웹 애플리케이션이 상호 작용하는 중요한 기술 중 하나
- 사용자가 어떤 내용을 원하는지, 사용자의 요구 사항이 무엇인지 파악할 때 가장 많이 사용하는 웹 애플리케이션 필수적인 요소

### 폼 데이터 처리 과정

![](https://i.imgur.com/1Pg295q.png)

### 폼을 구성하는 태그의 종류

![](https://images.velog.io/images/ansalstmd/post/7d79b331-c808-4116-a7b8-95c6b61fe873/image.png)

## form 태그의 기능과 사용법

### form 태그

- 사용자가 다양한 정보를 입력하고 서로 전달할 때 사용하는 태그
- 단독으로 쓰이지 않고 사용자가 다양한 정보를 입력하는 양식을 포함하는 최상위 태그

![](https://i.imgur.com/yZdlBJU.png)

    - 속성을 이용하여 폼 데이터를 전송할 때 어디로 보낼지, 어떤 방식으로 보낼지 설정
    - form 태그의 모든 속성은 필수가 아니라 선택적으로 사용

![](https://i.imgur.com/z1DODmQ.png)
ex) form 태그 사용 예
![](https://i.imgur.com/gDRKAuR.png)

## input 태그의 기능과 사용법

### input 태그

사용자가 텍스트 입력이나 선택 등을 다양하게 할 수 있도록 공간을 만드는 태그
종료 태그 없이 단독으로 사용할 수 있음
![](https://i.imgur.com/qIpwXyb.png)

- input 태그의 기본 속성

![](https://i.imgur.com/zQgfV0x.png)

ex) input 태그 사용 예
![](https://i.imgur.com/XGbK9sr.png)

ex) form 태그와 input 태그로 간단한 회원 가입 양식 만들기
![](https://i.imgur.com/vhQw2q7.png)

## select 태그의 기능과 사용법

### select 태그

여러 개의 항목이 나타나는 목록 상자에서 항목을 선택하는 태그
시작 태그와 종료 태그가 있으며, 리스트 박스에 여러 항목을 추가 삽입하기위해 반드시 option 태그를 포함해야 함
![](https://i.imgur.com/gaVigFW.png)

- select 태그의 속성
  ![](https://i.imgur.com/DtDkLRv.png)
- option 태그의 속성
  ![](https://i.imgur.com/Oy7QYqo.png)
  ex) select 태그 사용 예
  ![](https://i.imgur.com/uJ4sYwF.png)

ex) select 태그로 [예제 6-1] 회원 가입 양식의 연락처 수정하기
![](https://i.imgur.com/OtNm3qZ.png)

## textarea 태그의 기능과 사용법

### textarea 태그

- 여러 줄의 텍스트를 입력할 수 있는 태그
- 기본 값은 <textarea>와 </textarea> 태그 사이에 설정
- 입력 폼 안에 사용된 태그와 띄어쓰기가 그대로 출력됨
  ![](https://i.imgur.com/IGQt8fk.png)
- textarea 태그의 속성
  ![](https://i.imgur.com/wGItWi6.png)

ex) textarea 태그 사용 예
![](https://i.imgur.com/9NwHpj6.png)

ex) textarea 태그로 [예제 6-2]의 회원 가입 양식에 가입 인사 추가하기

![](https://i.imgur.com/srocfUg.png)

## 폼 데이터 처리하기

### 요청 파라미터의 값 받기

- request 내장 객체는 웹 브라우저가 서버로 보낸 요청에 대한 다양한 정보를 담고 있어 getParameter() 메소드를 이용하여 요청 파라미터의 값을 얻을 수 있음

![](https://i.imgur.com/DHaQ4b0.png)
![](https://i.imgur.com/Y7nM1al.png)

ex) [예제 6-3]의 회원 가입 양식에서 폼 데이터 전송받기
![](https://i.imgur.com/tXXjXOU.png)
![](https://i.imgur.com/6hf4gU4.png)

ex) [예제 6-4]의 회원 가입 양식에서 폼 데이터 전송받기

![](https://i.imgur.com/2yi9OIw.png)
![](https://i.imgur.com/xgGSNd6.png)
![](https://i.imgur.com/QCLa9zx.png)

### 요청 파라미터의 전체 값 받기

- 요청 파라미터를 설정하지 않아도 모든 값을 전달 받을 수 있음
- 텍스트 박스, 라이도 버튼, 드롭다운 박스와 같은 다양한 유형에 대해 한 번에 폼 데이터를 전달 받을 수 있음

### 폼 데이터의 일괄 처리 메소드

![](https://i.imgur.com/nuHQD0D.png)
![](https://i.imgur.com/V2Vh5Hb.png)

ex)
![](https://i.imgur.com/2FnLeUy.png)
![](https://i.imgur.com/f2i9CCD.png)
![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 파일 업로드(file upload)

### 파일 업로드

- 웹 브라우저에서 서버로 파일을 전송하여 서버에 저장하는 것
- 서버로 업로드할 수 있는 파일
  - 텍스트 파일, 바이너리 파일, 이미지 파일, 문서 등 다양한 유형이 있음
- 웹 브라우저에서 서버로 파일을 전송하기 위해 JSP 페이지에 폼 태그를 사용
  함
- 전송된 파일을 서버에 저장하기 위해 오픈 라이브러리를 이용해야 함

### 파일 업로드를 위한 JSP 페이지

- 웹 브라우저에서 서버로 파일을 전송하기 위해 JSP 페이지에 폼 태그를 작성할 때 몇 가지 중요한 규칙
  ![](https://i.imgur.com/BQOJwEM.png)

- form 태그의 method 속성은 반드시 POST 방식으로 설정
- form 태그의 enctype 속성은 반드시 multipart/form-data로 설정
- form 태그의 action 속성은 파일 업로드를 처리할 JSP 파일로 설정
- 파일 업로드를 위해 input 태그의 type 속성을 file로 설정
  − 만약 여러 파일을 업로드하려면 2개 이상의 input 태그를 사용하고 name 속성에 서로 다른 값을 설정

ex) 파일 업로드를 위한 폼 태그 사용 예
![](https://i.imgur.com/m59zm00.png)

### 파일 업로드 처리 방법

- 단순한 자바 코드로 작성하여 처리할 수 없어 오픈 라이브러리인 cos.jar나 commonsfileupload.jar를 사용해야 함
  ![](https://i.imgur.com/cAFtZXz.png)

### MultipartRequest

- 웹 페이지에서 서버로 업로드되는 파일 자체만 다루는 클래스
- 웹 브라우저가 전송한 multipart/form-data 유형과 POST 방식의 요청 파라미터 등을 분석한 후 일반 데이터와 파일 데이터를 구분하여 파일 데이터에 접근
- 한글 인코딩 값을 얻기 쉽고, 서버의 파일 저장 폴더에 동일한 파일명이 있으면 파일명을 자동으로 변경
- 오픈 라이브러리 cos.jar를 배포 사이트에서 직접 다운로드해서 사용
  - 배포 사이트: http://www.servlets.com/cos/
  - 다운로드 파일: cos-22.05.zip
  - JSP 페이지에 page 디렉티브 태그의 import 속성을 사용하여 패키지 com.oreilly.servlet.\*을 설정
    ![](https://i.imgur.com/hkaOMIU.png)

### MultipartRequest 클래스 생성

![](https://i.imgur.com/Te9tAEv.png)

### MultipartRequest 생성자의 매개변수

![](https://i.imgur.com/8QhwUsM.png)

ex) MultipartRequest 클래스 생성 예
![](https://i.imgur.com/59O0vY2.png)

### MultipartRequest 메소드

- 웹 브라우저에서 전송되는 요청 파라미터 중
- 일반 데이터는 getParameter( ) 메소드로 값을 받음
- 파일의 경우 getFileNames( ) 메소드를 이용하여 데이터를 받음

### MultipartRequest 메소드의 종류

![](https://i.imgur.com/59O0vY2.png)

ex) MultipartRequest 클래스의 메소드 사용 예: 요청 파라미터 정보 출력하기
![](https://i.imgur.com/59O0vY2.png)

- 오픈 라이브러리 cos.jar 파일을 다운로드하여 /WebContent/WEB-INF/lib/ 폴더에 추가

![](https://i.imgur.com/oriu0rb.png)
![](https://i.imgur.com/mFXPqz1.png)
![](https://i.imgur.com/kan1gxU.png)
![](https://i.imgur.com/tag9Cqr.png)
![](https://i.imgur.com/bIlNRGC.png)
