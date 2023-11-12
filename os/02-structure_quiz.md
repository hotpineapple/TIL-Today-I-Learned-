# 연습문제

1. 시스템 콜의 목적
  - 운영체제의 작업에 대한 인터페이스 제공
2. 명령 인터프리터의 목적과 통상적 커널에 포함되지 않는 이유
  - 커맨드라인을 통해 사용자와의 인터페이스를 제공, 유틸리티라는 점에서 커널보다는 시스템 프로그램에 보다 더 적절함
3. unix 시스템에서 새 프로세스를 시작하기 위해 명령 인터프리터나 셸에서 실행되어야 하는 시스템콜
  - fork
4. 시스템 프로그램의 목적
  - 프로그램 개발과 실행을 도움
5. 시스템 설계 시 계층화된 접근방식의 주요 장점과 단점
  - 계층이 구분되어 있어 구현과 디버깅이 쉬움
  - 사용자 요청시 모든 계층을 모두 통과해야 하므로 오버헤드가 큼
6. 운영체제에서 제공하는 5가지 서비스와 각 서비스가 사용자에게 편의를 제공하는 방법, 사용자 수준 프로그램이 이러한 서비스를 제공할 수 없는 경우는?
  - 사용자 인터페이스, 프로세스 제어, 입출력 연산, 파일 관리, 통신
  - 특권 모드 전환이 필요하여 사용자 수준 프로그램은 일반적으로 이러한 기능에 직접 수행할 수 없다.
  - 사용자 수준 프로그램은 운영체제에서 제공하는 API(Application Programming Interface)를 통해 이러한 서비스에 접근할 수 있다.
7. 일부 시스템은 운영체제를 펌웨어에 저장하고 다른 시스템은 디스크에 저장하는 이유
  - 모바일 환경과 같이 한정된 하드웨어 자원이 있는 경우 
8. 부팅할 운영체제를 선택할 수 있도록 시스템을 설계하는 방법은? 이 때 부트스트랩 프로그램이 해야할 일은?
  - 모듈 구조, 모든 커널의 주소 공간을 알고 있어야 한다