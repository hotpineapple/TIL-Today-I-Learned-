# POJO
- Plain Old Java Object
- <=> SOJO(Specialized ~)

## 정의
- 자바 언어 사양 외에 어떠한 제한에도 묶이지 않은 자바 오브젝트
- 미리 정의된 클래스를 상속받아서는 안된다
- 미리 정의된 인터페이스를 구현해서는 안된다
- 미리 정의된 어노테이션을 포함해서는 안된다

### 왜 POJO인가
- 비침투적 개발을 하기 위함
  - 침투적 개발: 사용자 코드를 프레임워크에 종속
    - 이러한 코드는 프레임워크 외부에서 사용할 수 없으므로 코드 재사용에 적합하지 않
    - 침입은 사용자가 프레임워크와 더 잘 통합되도록 하고 프레임워크에서 제공하는 기능을 더 쉽고 완벽하게 활용할 수 있도록 
  - 비침해 코드는 종속성이 너무 많지 않으며 다른 위치로 쉽게 마이그레이션할 수 
    - 그러나 사용자 코드와 상호 작용하는 방법은 더 복잡
- 테스트에 용이

### 실제
- 기술적 어려움과 다른 이유로 
- 많은 POJO 호환인 소프트웨어 제품과 프레임워크는 
- 예를 들면 영속성이 제대로 작동하기 위해
- 여전히 미리 정의된 어노테이션을 사용할 것을 요구한다

### 핵심 아이디어
- 어떤 객체(실제로는 클래스)가 어노테이션이 추가되기 전에 POJO 이고
- 어노테이션이 제거되면 POJO 상태를 반환한다면 
- 이 객체(클래스)는 POJO로 여겨진다

## 문맥적 변형

### Java beans(사양, specification)
- 직렬화가 가능하고 
- 인수가없는 생성자가 있으며 
- 간단한 명명규칙을 따르는 getter, setter 메서드를 사용하여 속성에 액세스 할 수 있는 POJO

#### 특징
- Java beans 의 사양이 완전히 구현되면, POJO모델을 깨뜨림 
- Serializable 인터페이스를 implements 해야 하기 때문 
- 그러나 많은 JavaBeans라고 불리는 POJO 클래스는 이 요구사항을 만족하지 않음(느슨한 정의)
```java
public class MyBean {

    private String someProperty;

    public String getSomeProperty() {
         return someProperty;
    }

    public void setSomeProperty(String someProperty) {
        this.someProperty = someProperty;
    }
}
```
