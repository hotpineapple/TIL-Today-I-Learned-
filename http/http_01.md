# Chaper 1. Basics - Unit 1. Introduction

## 1. HTTP

- 웹 서버로부터 이미지, 비디오, HTML 페이지, 자바 애플릿 등의 정보를 사용자의 PC 에 설치된 웹 브라우저로 옮긴다.
- 신뢰성 있는 데이터 전송 프로토콜을 사용한다.

## 2. 웹 클라이언트와 서버

- 월드 와이드 웹의 기본 요소
- 웹 서버는 인터넷의 데이터(=웹 콘텐츠)를 저장하고 클라이언트가 요청하면 데이터를 제공한다.
- 가장 흔한 HTTP 클라이언트는 웹 브라우저(e.g. 인터넷 익스플로러, 크롬) 이다.
    - 서버에게 HTTP 객체를 요청하고 응답 결과를 처리하여 사용자의 화면에 보여준다.

## 3. 리소스

- 웹 콘텐츠의 원천
- 웹 서버는 웹 리소스를 관리하고 제공한다.
- 정적파일 - 텍스트 파일, HTML파일, MS 워드파일, 이미지, 동영상 등
- 요청에 따라 콘텐츠를 생산하는 동적 콘텐츠 리소스 - 사용자가 누구인지, 요청시각이 언제인지, 어떤 정보를 요청했는지 등에 따라 다름

### 3.1 미디어 타입

- HTTP는 웹에서 전송되는 객체 각각에 MIME 타입(데이터포맷라벨)을 부여한다.
- MIME 타입은 본래 서로 다른 전자메일 시스템 사이에서 메시지가 오갈 때 발생하는 문제점을 해결하기 위해 설계됨
- 웹 브라우저는 서버로부터 객체를 돌려받을 때, 자신이 다룰 수 있는 객체인지 응답메시지의 헤더에 있는 MIME 타입(Content-type)을 통해 확인한다.
- 종류
    - HTML : text/html
    - plain ASCII : text/plain
    - JPEG : image/jpeg
    - GIF : image/gif
    - 애플 퀵타임 비디오 : video/quicktime
    - MS 파워포인트 프레젠테이션 : application/vnd.ms-powerpoint

### 3.2 URI(Uniform Resource Identifier)

- 웹 서버의 리소스는 고유한 값을 갖고 있으므로 클라이언트는 관심있는 리소스를 지목할 수 있다.
- 이 고유한 값은 URI라고 불린다.
- URI 는 프로토콜(스킴), 서버, 리소스를 명시한다.
- URL(~ Locator) 는 URI 의 가장 흔한 형태로, 리소스의 구체적인 위치를 서술한다.

## 4. 트랜잭션

- 요청 명령과 응답 결과로 구성됨

### 4.1 메서드

- 요청 명령에 사용됨
- 서버에게 어떤 동작이 취해져야 하는지를 명시
- 흔히 쓰이는 메서드
    - GET : 지정한 리소스를 클라이언트로 달라.
    - PUT : 클라이언트에서 서버로 보낸 데이터를 지정한 이름의 리소스로 저장하라(전체수정)
    - DELETE : 지정한 리소스를 서버에서 삭제하라
    - POST : 클라이언트 데이터를 서버 게이트웨이 애플리케이션으로 보내라
    - HEAD : 지정한 리소스에 대한 응답에서 헤더부분만 달라.

### 4.2 상태코드

- 응답에 사용됨
- 클라이언트에게 요청이 성공했는지, 추가 조치가 필요한지 등을 알려줌
- 흔히 쓰이는 상태코드
    - 200 : 문서가 바르게 반환됨
    - 302 : 다시 보내라. 다른 곳에 가서 리소스를 가져가라.
    - 404 : 리소스를 찾을 수 없음
- 사유구절은 사람이 이해하기 쉽도록 덧붙여진 것일 뿐 실제 응답 처리에는 상태코드만 사용된다.

### 4.3 웹 페이지는 여러 개의 객체로 이루어져 있다

- 애플리케이션은 보통 하나의 작업을 수행하기 위해 여러 번의 HTTP 트랜잭션을 수행한다.
- 예)
    - 페이지 레이아웃을 서술한 HTML을 가져온다
    - 첨부된 이미지를 가져온다
    - 그래픽 조각을 가져온다
    - 자바 애플릿을 가져온다
    - ...

## 5. 메시지

- 줄 단위의 문자열(이진 형식이 아닌 일반 텍스트)
- 구분
    - 요청 : 웹 클라이언트에서 웹 서버로 보낸 메시지
    - 응답 : 웹 서버에서 웹 클라이언트로 돌려준 메시지
- 구성 요소
    - 시작줄
        - 요청 - 무엇을 해야 하는지
        - 응답 - 무슨 일이 일어났는지
    - 헤더
        - 콜론(:)으로 구분된 이름/값으로 구성
        - 빈줄(≠개행)로 끝남
    - 본문
        - 본문은 어떤 이진 데이터든지 포함할 수 있음

## 6. TCP 커넥션

> 어떻게 메시지가 한 곳에서 다른 곳으로 옮겨갈까?


### 6.1 TCP/IP

- HTTP 는 애플리케이션 계층의 프로토콜, 세부사항은 전송계층/네트워크 계층 프로토콜 TCP/IP 에게 맡김
- TCP/IP는 전 세계적으로 컴퓨터와 네트워크 장치 사이에서 대중적으로 이용되는 프로토콜임
- TCP 특징
    - 오류 없는 데이터 전송
    - 순서에 맞는 전달(보낸 순서대로 도착)
    - 조각나지 않는 데이터 스트림(어떤 크기로든 보낼 수 있음

### 6.2 IP 주소, 포트번호를 이용한 접속

- 클라이언트와 서버 사이의 TCP/IP 커넥션 맺기 위해 IP주소와 포트번호 이용함
- IP 주소는 서버 컴퓨터의 위치, 포트번호는 이 서버에서 실행중인 프로그램을 가리킴
- IP 주소와 포트번호는 URL 에서 추출 가능
- 서버 위치가 호스트명으로 되어있는 경우 DNS를 통해 변환 가능
- 순서
    - 웹 브라우저는 URL에서 서버의 호스트명을 추출
    - 웹 브라우저는 서버의 호스트명을 IP 주소로 변환
    - 웹 브라우저는 URL에서 포트번호를 추출
    - 웹 브라우저는 웹 서버와 TCP 커넥션을 맺음
    - 웹 브라우저는 서버에 HTTP 요청을 보냄
    - 서버는 웹 브라우저에 HTTP 응답을 돌려줌
    - **커넥션이 닫히면** 웹 브라우저는 문서를 보여줌

## 7. 프로토콜 버전

- 현재의 HTTP 버전 : HTTP 1.1

## 8. 웹의 구성요소

- 프락시 (서버)
    - 클라이언트와 서버 사이에 위치한 HTTP 중개자
    - 웹 보안, 애플리케이션 통합, 성능 최적화를 위한 중요 구성요소
    - 클라이언트의 모든 HTTP요청을 받아 대개 이를 수정한 뒤에 서버에 전달
    - 사용자를 위한 프락시, 사용자를 대신해서 서버에 접근함 (+ 리버스프락시 - 서버를 위한 프락시)
    - 주로 보안을 위해 사용, 요청과 응답을 필터링
- 캐시 (프락시)
    - 많이 찾는 웹페이지의 사본을 클라이언트 가까이에 보관하는 HTTP창고
    - 프락시의 일종
    - 클라이언트는 더 빨리 문서를 다운 받을 수 있음
    - HTTP 는 캐시를 효율적으로 동작하게 하고, 캐시된 콘텐츠를 최신버전으로 유지하는 동시에 프라이버시를 보호하기 위한 많은 기능을 정의하고 있음
- 게이트웨이
    - 다른 애플리케이션과 연결된 특별한 웹 서버
    - 다른 서버들의 중개자
    - 주로 HTTP트래픽을 다른 프로토콜로 변환하기 위해 사용
    - 스스로가 리소스를 갖고 있는 진짜 서버인 것 처럼 요청을 다루기 때문에 클라이언트는 자신이 게이트웨이와 통신하고 있음을 알지 못함
- 터널
    - 단순히 HTTP 통신을 전달하기만하는 특별한 프락시
    - 두 커넥션 사이에서 Raw 데이터를 열어보지 않고 그대로 전달해주는 HTTP 애플리케이션
    - 주로 비 HTTP 데이터를 하나 이상의 HTTP 연결을 통해 그대로 전송해주기 위해 사용
    - 암호화된 SSL 트래픽을 터널을 통해 HTTP 커넥션으로 전송함으로써 웹 트래픽만 허용하는 사내 방화벽을 통과시킬 수 있음
- 에이전트
    - 자동화된 HTTP요청을 만드는 준 지능적 웹 클라이언트
    - 웹 요청을 만드는 애플리케이션은 뭐든지 HTTP 에이전트이다.
    - 웹 브라우저 이외에도 스파이더, 웹 로봇 등이 있다.