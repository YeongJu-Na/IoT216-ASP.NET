# IoT216-ASP.NET MVC
# 1. 실습
### Lect 1: ASP.NET WebForms
- Label1.Text = TextBox1.Text;  //입력 도중에는 변화 없음 --> 엔터 눌러야
- Login폼 만들기
    - id, password받을 텍스트 박스 생성→ pw : textbox mode를 password로 
    - 이전에 만든 MyLibrary 참조 추가- SQLDB클래스 이용해 db연결, 테이블 정보와 일치여부 확인
  - 현재 폼을 default로 default.aspx--> Home.aspx, Login--> Default 
  - Response.Redirect("~/Home");

### Lect 2,3: ASP.NET MVC - 로그인 페이지
- Login 페이지(액션) 만들기
  - view>home우클릭 >추가-레이아웃이 있는 mvc 5 뷰 페이지(Login.cshtml)
  - > 이대로 실행하면 리소스 없다고 나옴
    - mvc는 세 컴포넌트 모두 맞아야함 즉, view만 만들었기때문에 이 뷰를 처리하는 컨트롤러가 없어서 발생(해당 액션에 대한 컨트롤러의 메소드가 없음)
  - 해당 액션에 대한 메소드 생성하기
    - Controller> homecontroller에서 다른 메소드랑 비슷하게 쓰고 빌드
    - id, pw받을 텍스트박스 추가 → Login.cshtml
    - → 도구상자에서 Input(Text)를 드래그앤 드롭, name 지정(페이지 내 변수명)
    - → Input(submit) 버튼 추가
  - 텍스트박스에 입력된 값 가져오기
    - homecontroller의 해당 메소드(Login)내에서 Request.QueryString[텍스트박스name];
    - → 이 값은 화면 표시 요청 시, id/pwd 제출 시(submit) 가져와짐
- 로그인 페이지 초기화면으로 설정
  - login.cshtml에서 <div class> 다음<form asp-controller=”Home” asp-action=”Login”>
  - App_Start폴더 >RouteConfig.cs >routes.maproute >default의 controller와 action 시작페이지(Home, Login)로
    - ⇒ 실행 결과 주소창: ~ /Home/Login
- 데이터베이스 연결
  - model 추가: models 폴더 우클릭 새 클래스 추가(user.cs)
  - DB 연결
    - 솔루션 우클릭 새폴더 추가(Context) → 보통 DB 연결관련해서 context라는 이름쓰임
    - → 해당 폴더 우클릭 새 클래스 추가(users.cs)
    - > 네이밍시, models는 단일 객체이므로 단수로, context는 복수로 하는 경우가 많음
    - users.cs >클래스 선언부 클래스명 우측에 “: DbContext” 입력
    - → 잠재적 수정사항 > 객체 설치(==> 새 프로젝트마다 매번 설치해야함)> using추가
    - 클래스 생성자 우측에” : base(연결할 DB의 연결문자열){ }”
    - 엔티티collection 추가: public DbSet<user> users {get; set;}  //데이터타입:단일 객체인 user
- 패스워드 암호화 (암호화된 패스워드 저장)
  - GetEncrypt 함수 만들기 
    - 해시 알고리즘의 하나인 MD5 객체 생성, 해시값 받환
    - → 해시 함수 적용 직후 object 타입의 값에 대해 모든 문자를 2자리 16진수로
  - usersController.cs→  메소드 중 user객체와 바인딩된 Create메소드 수정
    - if(modelState.IsValid) 조건에서 user의 pw를 암호화하고 pw를 암호화된 값으로
    - → string sSec = GetEncrypt(user.password);  user.password = sSec;

---------
# 2. 이론
- 인터넷: TCP/IP 프로토콜을 기반으로 전세계 컴퓨터와 네트워크가 연결된 광범위한 통신망
- 웹 페이지: 웹 브라우저를 통해 보여지는 문서 → HTML로 쓰여짐
- 웹사이트: 하나의 유일한 도메인 이름을 가지는 연결된 웹 페이지들의 모임
- 웹 서버: 한개 이상의 웹사이트를 hosting하는 컴퓨터
    - hosting: 모든웹페이지와 관련 파일들이 해당 컴퓨터에서 이용가능함을 의미
  - 역할
    - 호스팅하고 있는 웹 사이트의 웹페이지를 사용자의 요청에 따라 사용자의 브라우저로 보냄
    - 동적 컨텐츠 처리 시 웹 어플리케이션으로 전달, 받은 응답을 다시 사용자에게
    - → 이러한 웹어플리케이션을 만들어 웹 서버에 넣음(서버 사이트 프로그램)
- 웹 프로그래밍
  - 프론트엔드: 브라우저 단에서 동작하는 코드를 작성하는 것, 즉 클라이언트 측 프로그래밍
    - 언어: HTML(페이지 구성), CSS(HTML요소에 디자인 요소 적용), JavaScript(페이지 구성물들에 움직임 부여) 
  - 백엔드: 서버측에서 실행되는 코드를 작성하는 것, 서버사이드프로그래밍
    - 웹 서버 프로그램 - tomcat, apach-> 리눅스에서 돌아감 / 윈도우즈에서는 IIS
    - 언어: JSP, PHP,ASP등 
- 윈도우 어플리케이션: windows에서 동작하는 프로그램 ⇒ *WinForm*
  - 실행 파일이 존재하며, 사용하려면 사용자의 컴퓨터에 설치해야 함
- 웹 어플리케이션: 웹 브라우저 있으면 사용 가능 ⇒ *WebForm* 
--------
# 3. C# 웹 프로그래밍
- 웹 서버 프로그래밍 --> c#에서는 ASP.NET이라는 웹 프레임워크를 활용해 쉽게 작성 가능
  - ASP.NET WebForms
    - 기존 윈폼 프로그래밍 방식을 상당부분 적용한 것, html페이지에 다양한 asp.net서버 컨트롤들을 삽입하면, asp.net엔진이 서버 컨트롤들을 다시 html로 자동 렌더링 해줌
    - 웹폼ui에 대한 이벤트 핸들링을 c#코드로 처리하여 동적 웹페이지 제작 가능, 웹 페이지 전반에 관한 클래스와 동작들 정의
  - ASP.NET MVC
    - MVC디자인 패턴을 asp.net에 도입한 웹 개발 방식, model과 controller를 c#코드로 작성하고 view를 HTML기반으로 작성
    - → 모델뷰 아키텍처는 ui(view)와 데이터 및 비즈니스 로직을 분리(model, control)하여 상호간 독자적인 개발 및 수정,테스트 가능하도록 한 것으로, 대표적인 프레임워크로 Ruby on Rails, Django등에 채용되어 있으며, asp.net mvc또한 이런 관점에서 제공되는 웹 개발
  - ASP.NET Web API
    - REST API개발을 쉽게 해줌, 위 방법론들과 결합 또는 독립적으로 동작 가능
    - 클라이언트가 AJAX프레임워크나 기타 방법을 통해 요청한 정보를 JSON이나 XML등의 형태로 내보내려고 개별 클라이언트에서 응답받은 정보를 적절하게 가공해서 출력하는 형태로 동작
    - --> AJAX 기반으로 웹브라우저에서 대부분의 UI를 구성하는 SPA(single page application) 응용의 경우 필수 요소
- 웹 클라이언트 : c#코드로web리소스를 다운로드하거나 web API를 호출하기 위해 .NET 라이브러리 사용할 수 있다.
  - 주로 사용하는 클래스 : WebClient, HttpWebRequest/ HttpWebResponse

##### ASP.NET MVC
- View 폴더 내부
  - Home 폴더: 뷰이며, 그 안의 cshtml은 action(페이지와 비슷?)
  - Shared 폴더의 layout.cshtml: 기본 레이아웃 → 최상단(header)/ 최하단(footer)
  - View_Start.cshtml: 페이지 전환 요구시, 수행되는 것



