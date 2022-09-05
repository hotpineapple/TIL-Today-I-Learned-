# IoC
- dependency injection (DI)라고도 함.
- 객체가 그들의 의존성(그들이 함께 동작하는 객체)을 정의하는 과정
  - 생성자, 팩토리 메섣의 인자, 또는 객체의 속성을 통해서만
- 그러면 컨테이너는 빈을 만들 때 그 종속성을 주입한다
- 이 과정은 빈이 스스로 그 종속성의 인스턴스화 또는 종속성의 위치를 제어하는 것의 역전이다
  - 클래스의 direct construction 또는 Service Locator 패턴과 같은 메커니즘을 사용하여
- 스프링 IoC 컨테이너의 기초 패키지
  - The org.springframework.beans
  - org.springframework.context packages
- BeanFactory interface
  - 모든 유형의 객체를 관리할 수 있는 고급 configuration 메카니즘을 제공함
- ApplicationContext
  - BeanFactory의 하위 인터페이스
  - 추가사항
    - Spring AOP의 features 과의 더 쉬운 통합
    - 메시지 리소스 처리
      - 국제화에 사용
    - 이벤트 publication
    - Application-layer specific contexts (ex WebApplicationContext)
      - web applications에서 사용
- 요약
  - BeanFactory
    - 설정 프레임워크와 기본 기능을 제공
  - ApplicationContext
    - BeanFactory에 enterprise-specific functionality를 더함
    - BeanFactory 의 complete superset 이다
    - Spring의 IoC 컨테이너에 대한 설명에서 독점적으로 사용됩니다.
- Spring에서 bean은
  - 애플리케이션의 백본을 형성하고 Spring IoC 컨테이너에 의해 관리되는 객체.
  - Spring IoC 컨테이너에 의해 인스턴스화, 조립 및 관리되는 객체
    - 그렇지 않으면 빈은 애플리케이션에 있는 많은 객체 중 하나일 뿐임
  - 빈과 이 빈들간의 종속성은 컨테이너에서 사용하는 구성 메타데이터에 반영됩니다.

## 1.2. Container 개요
- org.springframework.context.ApplicationContext 인터페이스
  - Spring IoC container를 나타냄
  - instantiating, configuring, and assembling the beans에 책임이 있음
- 컨테이너는 configuration metadata를 읽어서 instantiate, configure, and assemble 할 객체에 대한 instruction을 얻음
- configuration metadata는 
  - XML, Java annotations, or Java code로 나타낼 수 있음
  - 표현하는 것
    - 애플리케이션을 구성하는 객체
  - 객체간의 풍부한 상호 종속성
- ApplicationContext 인터페이스의 Several implementations가 spring에서 제공됨
- stand-alone applications에서는 ClassPathXmlApplicationContext or FileSystemXmlApplicationContext의 인스턴스를 만드는것이 보통
- XML이 구성 메타데이터를 정의하는 전통적인 형식이지만 컨테이너가 메타데이터 형식으로 자바 어노테이션이나 코드를 사용하도록 지시할수도 있음
- 대부분의 경우 Spring IoC 컨테이너의 하나 이상의 인스턴스를 인스턴스화하는데 explicit user code 는 요구되지 않음(메타데이터 이용하기 때문)
- how Spring works
  - application classes 가 configuration metadata 와 결합됨

### 1.2.2. Instantiating a container
- 컨테이너가 외부에서 설정 메타데이터를 로드하도록 하는 문자열
  - ApplicationContext 생성자에 제공된 location path or paths
  ```java
  ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
  ```
- example
  - services.xml
  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd">
  
    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
      <property name="accountDao" ref="accountDao"/>
      <property name="itemDao" ref="itemDao"/>
      <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

  </beans>
  ```
  - daos.xml
  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
      class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
    <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
    <!-- additional collaborators and configuration for this bean go here -->
    </bean>

  <!-- more bean definitions for data access objects go here -->

  </beans>
  ```
- the service layer 구성
  - PetStoreServiceImpl 클래스
  - JpaAccountDao and JpaItemDao
- property name 요소는 JavaBean property의 이름을 가리킴
- ref 요소는 또 다른 bean definition의 이름을 가리킴
  - id and ref 의 연결은 expresses collaborating objects의 종속성을 나타냄


### 1.2.3. 컨테이너 사용하기
- The ApplicationContext
  - 빈과 그 종속성 등록을 유지할 수 있는 advanced factory를 위한 인터페이스
- `T getBean(String name, Class<T> requiredType` 을 이용해 빈들의 인스턴스를 얻을 수 있다
  - 빈 정의를 읽고 빈에 접근할 수 있다
  ```java
  // create and configure beans
  ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

  // retrieve configured instance
  PetStoreService service = context.getBean("petStore", PetStoreService.class);

  // use configured instance
  List<String> userList = service.getUsernameList();
  ```
- 이상적으로는 애플리케이션 코드에서 이러한 ApplicationContext 메서드를 사용해서는 안 됩니다
  - 실제로 애플리케이션 코드에는 getBean()메서드에 대한 호출이 전혀 없어야 하므로 Spring API에 대한 종속성이 전혀 없어야 합니다.
- Spring의 웹 프레임워크와의 통합은 컨트롤러 및 JSF 관리 Bean과 같은 다양한 웹 프레임워크 구성 요소에 대한 종속성 주입을 제공하여 메타데이터(예: autowiring 주석)를 통해 특정 Bean에 대한 종속성을 선언할 수 있습니다.
