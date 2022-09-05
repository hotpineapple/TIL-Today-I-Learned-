# Bean

## Beans
- Spring IoC 컨테이너는 하나 이상의 빈을 관리합니다.
- 빈들은 컨테이너에 제공되는 configuration 메타데이터를 이용해서 생성됩니다(예: XML <bean/>정의 형식).
- 컨테이너 자체 내에서 이러한 빈 정의는 BeanDefinition 객체로 표시됨
  - 포함 내용
    - 패키지와 클래스 이름: 일반적으로 정의되는 빈의 실제 구현 클래스
    - Bean이 컨테이너에서 어떻게 동작해야 하는지를 나타내는 Bean 동작 구성 요소(범위, 수명 주기 콜백 등).
    - Bean이 작업을 수행하는 데 필요한 다른 Bean에 대한 참조 (종속성)
    - 새로 생성된 객체에 설정할 기타 구성 설정 — 예를 들어 연결 풀을 관리하는 Bean에서 사용할 연결 수 또는 풀의 크기 제한.
- 이 메타데이터는 각 빈의 정의를 구성하는 속성 집합으로 변환됩니다
  - Class
  - Name
  - Scope
  - Constructor arguments
  - Properties
  - Autowiring mode
  - Lazy initialization mode
  - Initialization method
  - Destruction method
- ApplicationContext은 컨테이너 외부에서 (사용자에 의해) 생성된 기존 객체의 등록도 허용
  - 그러나 일반적인 응용 프로그램은 일반 빈 정의 메타데이터를 통해 정의된 빈으로만 작동
- Bean 메타데이터 및 수동으로 제공되는 싱글톤 인스턴스는 가능한 한 빨리 등록해야 함
  - 컨테이너가 autowiring 및 기타 내부 검사 단계에서 이에 대해 적절하게 추론하기 위해
  - 기존 메타데이터 및 기존 싱글톤 인스턴스를 재정의하는 것은 어느 정도 지원되지만
  - 런타임 시 새 Bean 등록은 동시 액세스 exception, Bean 컨테이너의 일관성 없는 상태를 만들 수 있으며 공식적으로 지원되지 않음
### 빈 naming
- 클래스 경로를 컴포넌트 스캐닝하면서, Spring은 규칙에 따라 이름 없는 컴포넌트에 대한 빈 이름을 생성
  - 표준 Java 규칙을 사용
    - 즉, 빈 이름은 소문자로 시작하고 거기에서 카멜 케이스
    - 예로는 accountManager, accountService, userDao, loginController등
  - 빈의 이름을 일관되게 지정하면 구성을 더 쉽게 읽고 이해할 수 있습니다.
    - Spring AOP를 사용한다면 이름으로 관련된 빈들의 집합에 advice를 적용할 때 많은 도움이 된다.
- 다른 곳에서 정의된 빈의 경우 별칭을 도입하는 것이 때때로 바람직함
  - alias 사용
  ```xml
  <alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
  <alias name="myApp-dataSource" alias="subsystemB-dataSource"/>
  ```
- java code를 사용하는 경우 @Bean 어노테이션으로 alias 사용 가능

### 빈 instance 화
- 빈 정의는 본질적으로 하나 이상의 객체를 생성하기 위한 레시피입니다
- 컨테이너는 요청 시 명명된 빈의 레시피를 살펴보고 해당 빈 정의에 의해 캡슐화된 구성 메타데이터를 사용하여 실제 객체를 생성(또는 획득)합니다.
- XML 기반 구성 메타데이터를 사용하는 경우
  - <bean/> 의 `class`속성 에 인스턴스화할 개체의 유형(또는 클래스)을 지정합니다.
  - 이 class속성(내부적 으로 인스턴스 의 Class속성 임)은 일반적으로 필수입니다.
  - Class 속성을 사용 방법
    - 일반적
      - 컨테이너 자체가 생성자를 반사적으로 호출하여 Bean을 직접 생성하는 경우
      - 생성할 Bean 클래스를 지정
      - 이는 new 연산자를 사용하는 Java 코드와 다소 동일합니다.
    - 덜 일반적
      - 컨테이너가 Bean을 생성하기 위해 클래스에서 정적 팩토리 메소드를 호출하는 경우
      - 객체를 생성하기 위해 호출되는 정적 팩토리 메소드를 포함하는 실제 클래스를 지정
      - static 팩토리 메서드 호출에서 반환된 유형은 완전히 동일한 클래스이거나 다른 클래스일 수 있습니다.
    - 중첩 클래스
      - `.`이나 `$` 사용
- 빈의 인스턴스화 방법
  1. 생성자
  2. static 팩토리 메서드
  3. instance 팩토리 메서드

#### 1. 생성자로 인스턴스화
- 생성자 접근 방식으로 Bean을 생성하면 모든 일반 클래스가 Spring에서 사용 가능하고 호환됩니다.
- 즉, 클래스는 특정 인터페이스를 구현하거나 특정 방식으로 코딩할 필요가 없습니다.
  - Bean 클래스를 지정하기만 하면 충분합니다.
  - 사용하는 IoC 유형에 따라 기본(빈) 생성자가 필요할 수도 있습니다.
- Spring IoC 컨테이너는 관리하고자 하는 거의 모든 클래스를 관리할 수 있습니다.
  - 진정한 JavaBeans 관리에만 국한되지 않습니다.
    - 대부분의 Spring 사용자는 default 생성자와 컨테이너의 속성을 따라 모델링된 적절한 setter, getter 만 있는 실제 JavaBeans를 선호합니다.
    - 컨테이너에 더 이국적인 비-빈 스타일 클래스를 가질 수 있습니다.
      - 예를 들어 JavaBean 사양을 절대적으로 준수하지 않는 레거시 연결 풀을 사용해야 하는 경우 Spring에서도 이를 관리할 수 있습니다.
- XML 기반 구성 메타데이터를 사용하여 다음과 같이 빈 클래스를 지정할 수 있습니다.
```xml
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```
#### 2. static 팩토리 메서드로 인스턴스화
- 사용하는 속성
  - class 속성
    - static 팩토리 메소드를 포함하는 클래스를 지정하고
  - factory-method 속성
    - 팩토리 메소드 자체의 이름을 지정
- factory-method이 메서드를 호출하고(나중에 설명하는 선택적 인수를 사용하여) 라이브 개체를 반환할 수 있어야 합니다.
  - 이 개체는 이후에 생성자를 통해 생성된 것처럼 처리됩니다.
- 이러한 빈 정의의 한 가지 용도는 static레거시 코드에서 팩토리를 호출하는 것입니다.

- 다음 빈 정의는 팩토리 메서드를 호출하여 빈이 생성되도록 지정합니다.
- class 속성에는 반환된 객체의 유형(클래스)을 지정하는 대신 팩토리 메서드를 포함하는 클래스를 지정합니다.
- 이 예에서 createInstance()메서드는 static 메서드여야 합니다
```xml
<bean id="clientService"
class="examples.ClientService"
factory-method="createInstance"/>

```

#### 3. instance 팩토리 메서드로 인스턴스화
- 정적 팩토리 메소드 를 통한 인스턴스화와 유사
- 컨테이너에서 기존 Bean의 non-static 메소드를 호출하여 새 Bean을 생성합니다.
- 사용방법
  - class 속성은 비워두고
  - factory-bean 속성에 객체를 생성하기 위해 호출될 인스턴스 메소드를 포함하는 현재(또는 부모 또는 상위) 컨테이너의 빈 이름을 지정
  - factory-method속성 을 사용하여 팩토리 메소드 자체의 이름을 설정
```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
<!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
factory-bean="serviceLocator"
factory-method="createClientServiceInstance"/>
```
- One factory class can also hold more than one factory method
- 유의점
  - `factory bean` : Spring 컨테이너에 설정되어 인스턴스 또는 정적 팩토리 메소드를 통해 객체를 생성하는 bean
  - `FactoryBean` (대문자로 시작) : Spring-specific FactoryBean implementation class.

#### 빈의 런타임 타입을 결정하기
- 특정 Bean의 런타임 유형은 결정하기 쉽지 않음
  - 빈 메타데이터 정의에서 지정된 클래스는 초기 클래스 참조일 뿐이며
  - 선언된 팩토리 메서드와 잠재적으로 결합되거나
  - FactoryBean빈의 다른 런타임 유형으로 이어질 수 있는 클래스이거나
  - 인스턴스 레벨 팩토리 메소드의 경우 전혀 설정되지 않기 때문
  - 또한 AOP 프록시는 빈을 wrap(래핑) 할 수도 있다
    - 대상 빈의 실제 유형(구현된 인터페이스만)의 노출을 제한하는 인터페이스 기반 프록시로
- 따라서 특정 Bean의 실제 런타임 유형을 찾는 권장 방법은 BeanFactory.getType지정된 Bean 이름을 호출하는 것
  - 이것은 위의 모든 경우를 고려하고
  - BeanFactory.getBean 호출이 동일한 빈 이름에 대해 호출이 반환할 객체 유형을 반환합니다.
