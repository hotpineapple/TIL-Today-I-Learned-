# 웹 애플리케이션 

## 역사
- 웹 서버: 웹 요청을 받아 고정된 HTML 파일을 응답함
- 웹을 통해 다양하고 복잡한 리소스에 접근하고자 하는 필요가 생김
  - 애플리케이션 실행결과
  - 실시간으로 변하는 데이터
  - 사용자에 따라 다른 HTML파일
- 이를 위해서는 웹 요청을 받아 애플리케이션을 실행해야 함
  - 애플리케이션의 API 를 통해 요청을 전달
- 리소스 게이트웨이
  - 리소스와 클라이언트를 연결하는 서버 측 게이트웨이
  - 애플리케이션 게이트웨이라고도 함 
- **CGI(공용 게이트웨이 인터페이스)**
  - 웹서버가
  - 웹 요청에 따라 프로그램을 실행하여 그 결과를 TTP응답으로 회신하기위해
  - 사용하는 표준화된 인터페이스
  - (모든 요청이 하나의 게이트웨이를 거쳐 리소스에 접근한다는 점에서 공용이라는 단어를 쓴 것으로 추측함)
- CGI 서버
  -  초기의 애플리케이션 서버
  -  CGI(인터페이스)를 구현함
  -  리소스 부담이 큰 단점
    -  요청마다 프로세스 생성
    -  동일 요청에 다른 구현체 생성
- Fast CGI
  - 각각의 요청에 대해서 새로운 프로세스를 만드는 대신에 하나의 프로세스에서 여러 요청을 처리 
- **Servlet**
  - 웹서버가
  - 웹 요청에 따라 프로그램을 실행하여 그 결과를 TTP응답으로 회신하기위해
  - 사용하는 표준화된 인터페이스 (2)
  - 기존 CGI를 C,C++로 구현한 것과 달리 Java로 구현가능하여 이식성 높음
  - 요청마다 프로세스 아닌 스레드 생성하여 CGI 단점 보완
    - 프로세스의 주소공간에서 스레드들은 data 와 code 영역을 공유, stack만 따로 관리하여 스위치 속도가 빠르고 리소스 사용이 적음  
  - 서블릿 컨테이너가 싱글톤 패턴으로 서블릿 구현체를 관리
  - HTTP 프로토콜을 사용할 경우
    - Servlet 인터페이스를 구현한 GenericServlet을 상속한 HTTPServelt 을 상속하여 사용하면   
    - HTTP 메시지 파싱, 메서드 처리가 간편
  - 서블릿 인터페이스를 알아야 함(*)
  - 요쳥마다 서블릿 생성하여 사용 -> 공통로직 중복(**)
    - Front Controller 패턴으로 해결 가능 
- Spring Web MVC
  - 자바 코드만 가지고 구현 가능(*)
  - Front Controller 패턴 + 역할 분담
    - 서블릿 하나만 사용(=Dispatcher Servlet, 프론트 컨트롤러)
    - 공통로직 처리 가능(**)
  - Handelr mapping
  - Handler adapter
  - View resolver
  - => 비즈니스 로직에 집중할 수 있음!

## Spring Web MVC
- 웹 트랜잭션 동작
  - Front Controller: 웹 요청 전달
  - Handelr mapping: controller 반환
  - Front Controller: controller 호출
  - Handler adapter: Model and View 반환
  - Front Controller: view 요청
  - View resolver: view 반환
  - Front Controller: 웹 응답
 
 ### Dispatcher Servlet
 - 구성
  - Servlet Web application context: 웹 요청 관련 설정
  - Root Web application context: 서비스로직, repository(데이터, 모델관련) 설정
 - 컨테이너는 이러한 설정을 토대로 빈을 생성하고 관리함(주입 등)
 
