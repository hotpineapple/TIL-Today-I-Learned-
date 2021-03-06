# 11. 클라이언트 식별과 쿠키

- 웹 서버는 서로 다른 수천 개의 클라이언트들과 동시에 통신
- 이 서버들은 익명의 클라이언트로부터 받는 모든 요청을 처리하는 것 뿐 아니라 서버와 통신하고 있는 클라이언트를 추적해야 할 수도 있음
- 개별 접촉이 필요한 이유
    - 개별 인사
    - 사용자 맞춤 추천
    - 저장된 사용자 정보
    - 세션 추적
        - 장바구니 등
- 서버가 통신하는 대상을 식별하는 데 사용하는 기술들
    - HTTP 헤더
    - IP 주소
    - 로그인
    - URL
    - 쿠키

## HTTP 헤더

- From
    - 사용자의 이메일 주소를 포함
    - 악의적으로 잉메일 주소를 모아 스팸 메일 발송할 수도 있으므로 브라우저는 잘 사용하지 않음
    - 웹 로봇이나 스파이더는 데이터를 수집하는 과정에서 웹 사이트에 문제를 일으켰을 때 항의 메일을 보낼 수 있도록 From 헤더에 이메일 주소를 기입하기도 함
- User-Agent
    - 사용자가 쓰고 있는 브라우저의 이름과 버전 정보, 어떤 경우에는 운영체제에 대한 정보를 포함
    - 브라우저의 속성에 맞추어 콘텐츠를 최적화하는 데 유용
    - 사용자를 식별하는 데는 도움이 되지 않음
- Referer
    - 사용자가 현재 페이지로 유입하게 한 웹페이지의 URL을 가리킴
    - 사용자를 식별할 수는 없음
    - 사용자의 웹 사용 행태나 취향을 파악하는 데 도움이 됨
- Authorization (다음 장)
- Client-ip (다음 장)
- X-Forwarded-For (다음 장)
- Cookie (다음 장)

## 클라이언트의 IP 주소

- 웹 서버는 HTTP 요청을 보내는 반대쪽 TCP 커넥션의 IP 주소를 알아낼 수 있음
- 한계
    - 여러 사용자가 같은 컴퓨터를 사용한다면 식별 불가능함
    - 많은 경우 ISP는 로그인 할 때마다 동적으로 IP주소를 할당하는 방법을 사용함
    - 보안 강화 및 부족한 IP주소 문제 해결을 위해 NAT 방화벽을 통해 인터넷을 사용함
    - HTTP 프락시와 게이트웨이는 원 서버에 새로운 TCP 연결을 하므로 클라이언트의 IP 주소는 알 수 없음

## 사용자 로그인

- 웹 서버는 사용자 이름과 비밀번호로 인증을 요구하는 명시적 식별 요청을 보낼 수 있음
- 한 번 로그인 하면 브라우저는 WWW-Authenticate 와 Authorization 헤더를 이용하여 이 사이트로 보내는 모든 요청에 로그인 정보를 함께 보냄
- 웹 서버는 401 Login Required 응답코드를 보내어 로그인을 요구함
- 사용자가 원하는 사용자 이름이 고유하지 않을 수 있는 한계

## 뚱뚱한 URL

- URL 경로의 처음이나 끝에 어떤 상태 정보를 추가하여 확장함
- 독립적인 HTTP 트랜잭션을 하나의 세션으로 묶는 용도
- 한계
    - 사용자에게 혼란을 줌
    - 사용자의 상태정보를 포함하므로 공유하기 곤란한 문제
    - URL 이 매 번 달라지기 때문에 캐시를 사용할 수 없음
    - 서버의 부하가 가중됨
    - 다른 사이트로 이동하거나 특정 URL로 요청하면 세션에서 이탈함

## 쿠키

- 사용자를 식별하고 세션을 유지하는 방식 중에서 현재까지 가장 널리 사용되는 방식
- 새로운 HTTP 헤더를 정의함
- 캐시와 충돌할 수 있으므로 대부분의 캐시나 브라우저는 쿠키에 있는 내용물을 캐싱하지는 않음

### 타입

- 세션쿠키
    - 사용자가 사이트를 탐색할 때 관련 설정과 선호 사항들을 저장하는 임시 쿠키
    - 브라우저를 닫으면 삭제됨
- 지속쿠키
    - 디스크에 저장되어 브라우저를 닫거나 컴퓨터를 재시작하더라도 삭제되지 않고 더 길게 유지될 수 있음
    - 사용자가 주기적으로 방문하는 사이트에 대한 설정 정보나 로그인 이름을 유지하는 데 사용됨

### 동작

- 처음 사용자가 웹 사이트에 방문하면 웹 서버는 사용자에 대해 아무것도 모름
- 웹 서버는 사용자가 다시 돌아온다면 그 때는 식별하기 위해 유일한 값을 쿠키에 할당하여 HTTP 응답 헤더(Set-Cookie)에 포함하여 사용자에게 전달함
- 브라우저는 Set-Cookie 헤더에 있는 쿠키 콘텐츠를 브라우저의 쿠키 데이터베이스에 저장함
- 사용자가 **같은 사이트**를 다시 방문하면 이 사용자에게 할당했던 쿠키를 요청 헤더에 기술하여 전송함

### 클라이언트 측 상태

- 공식적으로 HTTP 상태관리 체계라고도 함
- 브라우저가 서버 관련 정보를 저장하여 **특정** **사용자**가 해당 서버에 접근할 때마다 그 정보를 함께 전송하는 것
- 브라우저는 쿠키 정보를 저장할 책임이 있음
- 구글 크롬
    - creation_utc: 쿠키의 생성 시점
    - host_key: 쿠키의 도메인
    - value: 쿠키의 값
    - path: 쿠키 관련 도메인의 경로
    - expire_utc: 쿠키의 파기 시점
    - secure: 쿠키를 SSL 커넥션일 경우에만 보냄

### 사이트들의 쿠키

- 브라우저가 모든 사이트에 쿠키 전부를 보내지는 않음
    - 성능 저하
    - 서버에 특화된 이름/값 쌍을 포함하기 때문
    - 잠재적 개인정보 유출
- 보통 브라우저는 쿠키를 생헝한 서버에게만 쿠키에 담긴 정보를 전송함
- Set-Cookie 응답 헤더의 속성
    - Domain: 어떤 사이트가 그 쿠키를 읽을 수 있는지 제어
    - Path: 웹 사이트의 일부만 쿠키를 적용

### 버전

- Version 0(넷스케이프) 쿠키
    - Set-Cookie 응답 헤더
    - Cookie 요청 헤더
    - 쿠키를 조작하는 데 필요한 필드를 정의
- Version1(RFC 2965) 쿠키
    - Version 0 과 호환
    - Set-Cookie2 응답 헤더
    - Cookie2 요청 헤더
    - 다른 점
        - 파기 주기와 상관 없이 강제 삭제 가능
        - 초 단위 상대 값으로 쿠키 생명 주기 결정
        - URL 의 포트번호로도 쿠키 제어 가능
        - 다른 키워드와 구별하기 위한 `$` 키워드

### 쿠키와 캐싱

- 쿠키 트랜잭션과 관련된 문서를 캐싱하는 것은 주의
- 이전 사용자의 쿠키가 다른 사용자에게 할당돼버리거나 누군가의 개인 정보가 다른 이에게 노출되는 최악의 상황
- 주의사항
    - 캐시되지 말아야 할 문서가 있다면 표시하라
    - Set-Cookie 헤더를 캐시하는 것에 주의
    - Cookie 헤더를 가지고 있는 요청을 주의
