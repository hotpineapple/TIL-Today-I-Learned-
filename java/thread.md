# 스레드
## 기초
- 프로세스
  - 프로세스란 실행중인 프로그램
  - 프로세스는 데이터와 메모리를 필요로 함
  - 프로세스는 한 개 이상의 스레드를 가짐
- 멀티스레딩
  - 장점
    - cpu 사용률
    - 자원을 효율적으로 사용
    - 사용자에 응답성 향상
    - 코드 간결  
  - 서버프로그램은 멀티스레드로 작성하는 것이 일반적임
    - 스레드와 사용자의 요청이 일대일로 처리되도록
    - 그러나 자원을 공유하면서 처리되므로 문제점이 있음
      - 동기화, 교착상태(deadlock) 등
## 스레드의 구현과 실행
- 스레드를 구현하는 방법
  - Thread 클래스를 상속받거나 Runnable 인터페이스를 구현
  - Runnable 인터페이스를 구현하는 방법이 일반적
    - 재사용성이 높고 코드의 일관성을 유지할 수 있으므로 더욱 객체지향적임
- 스레드를 구현한다는 것은 `run()` 메서드의 body ({...}) 를 만드는 것
- 스레드의 구현방법에 따라 인스턴스 생성 방법이 다름
- 스레드를 생성한 후 `start()` 메서드를 이용하여 실행해야 실제로 실행됨 
## start()와 run()
- start() 는 스레드가 작업을 실행하는데 필요한 호출스택을 생성한 다음 그 호출스택에서 `run()`을 호출함
  - 한 스레드에서 예외가 발생하여 종료되어도 다른 스레드에는 영향을 미치지 않음 
- 호출스택에서는 가장 위에 있는 메서드가 현재 실행중인 메서드이고 나머지는 대기상태임
- 그러나 스레드가 두 개 이상일 때에는 호출스택의 최상위에 있더라도 os 의 스케줄링에 따라 대기상태일 수 있음에 유의
  - Java 가 os 에 독립적이지 않은 특별한 부분 중 하나임 
  - os 스케줄링에 따라 큐에 저장되어 순서대로 실행됨
- main 메서드의 작업을 수행하는 것도 스레드임(deamon스레드, <-> user스레드)
## 싱글스레드와 멀티스레드
- 싱글코어 환경에서는 싱글스레드로 프로그래밍하는 것이 더 효율적임
  - 스레드간 컨텍스트 스위칭에 걸리는 시간도 있기 때문
  - 또한 자원경쟁 중 대기시간도 추가될 수 있음
    - 두 스레드가 서로 다른 자원을 사용한다면 멀티스레드 프로세스가 더 효율적임
## 스레드의 우선순위
- 스레드 클래스는 우선순위라는 멤버변수를 가짐
- 시각적인 부분이나 사용자에게 빠르게 반응해야 하는 작업을 하는 스레드의 우선순위를 높일 필요가 있을 수 있음
- setPriority, getPriority
- 그러나 os 스케줄링이나 jvm 구현에 따라 기대하는 대로 동작하지 않을 수 있음
  - 차라리 Priority Queue 를 이용하여 작업 자체의 우선순위를 조절하는 것이 나음
## 스레드 그룹
- 보안상의 목적
- 자신이 속한 스레드 그룹이나 하위 스레드 그룹만 변경가능함
- 모든 스레드는 반드시 스레드 그룹에 포함되어야 함
  - Java application 이 실행되면 JVM 은 main 과 system 이라는 스레드 그룹을 만들고
  - main 메서드 수행/JVM 동작과 같이 역할에 따라 스레드들을 분리함
## 데몬스레드
- 다른 (데몬스레드가 아닌)스레드의 작업을 돕는 보조적인 역할
- 가비지컬렉터, 워드프로세서 자동저장, 화면 자동갱신 등
- 무한루프와 조건문을 이용하여 실행후 대기하고 있다가 특정 조건이 만족되면 작업을 수행하고 다시 대기
- 데몬스레드를 직접 만들고 실행하고 싶다면 `start()` 호출 전 `setDaemon(true)`만 호출하면 됨
## 스레드의 실행제어 
- 효율적인 멀티스레드 프로그램을 만들기 위해 정교한 스케줄링이 필요
- 프로세스에 주어진 자원과 시간을 여러 스레드가 낭비없이 사용하도록
- 스레드의 스케줄링과 관련 메서드
  - sleep: 현재 실행중인 스레드를 일시정지시킴
  - join: 다른 스레드를 기다림
  - interrupt: 일시정지상태인 스레드를 깨워 실행대기상태로만듬
  - stop(deprecated)
  - suspend(deprecated)
  - resume(deprecated)
  - yield: 주어진 실행시간을 다른 스레드에게 양보하고 자신은 실행대기상태로 됨
- 스레드의 상태와 관련 메서드
  - NEW: 생성된 후 start 호출되기 전
  - RUNNABLE: 실행 중 또는 실행대기
  - BLOCKED: 동기화 블럭에 의해 일시정지된 상태
  - WAITING, TIMED_WAITING: 스레드의 작업이 종료되지 않았지만 실행가능하지 않은 상태
  - TERMINATED: 스레드의 작업이 종료됨
## 스레드의 동기화
- 한 스래드가 진행중인 작업을 다른 스레드가 간섭하지 못하도록 막는 것을 의미
- 용어
  - 임계영역: 공유데이터를 사용하는 코드의 영역
  - 락: 임계영역을 수행할 수 있는 권한 
- 동기화 하는 방법
  - synchronized 블럭
  - java.util.concurrent.atomic
  - java.util.concurrent.locks 
### synchronized
- 임계영역을 설정하는 데 사용
- 임계영역은 멀티스레드 프로그램의 성능을 좌우하므로 최소화는 것이 필요
### wait, notify
- 특정 스레드가 객체의 락을 가진 상태로 너무 많은 시간을 쓰지 않도록 하는 목적
- 임계 영역 내에서 작업을 진행할 수 없는 상황이면 `wait()` 으로 락 반납
- 다시 진행할 수 있는 상황이 되면 `notify()`로 다시 락을 얻어서 작업 재개
### lock 과 condition
- java.util.concurrent.locks 
### volatile
- 
### fork & join 프레임워크
- 
