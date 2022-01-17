### 5. 구조기반 타입

- 클래스는 구조기반 타입을 지원함
- 클래스는 자신과 똑같은 프로퍼티와 메서드를 정의하는 기존의 일반 객체를 포함해 클래스의 형태를 공유하는 다른 모든 타입과 호환됨
- 단 `prvate` 필드를 갖는 클래스는 그 클래스의 인스턴스만 가능함
- 단 `protected` 필드를 갖는 클래스는 클래스나 그 서브클래스의 인스턴스만 할당 가능함

### 6. 값, 타입 선언

- 타입스크립트에서 값과 타입은 별도의 네임스페이스에 존재함
- 타입스크립트는 문맥을 파악해 값 또는 타입으로 해석함
    - 이 기능을 이용해 컴패니언 타입을 구현 가능함
    
    ```tsx
    let a = 2022;
    function b() {}
    
    type a = number;
    interface b {
    	(): void;
    }
    
    // ... 
    
    if(a + 1 > 3) { ... } // 값 a로 추론
    let x:a = 3; // 타입 a로 추론
    ```
    
- 그런데 클래스(와 enum타입)는 값과 타입을 각각의 네임스페이스에 동시에 선언함
- 클래스는 클래스의 인스턴스를 가리킨다
    - 클래스 자체를 가리키려면 `typeof` 연산자를 사용
    
    ```tsx
    type State = {
        [key: string]: string;
    }
    
    class StringDatabase {
        state: State = { };
        get(key: string): string | null {
            return key in this.state ? this.state[key] : null;
        }
        set(key: string, value: string): void {
            this.state[key] = value;
        }
        static from(state:State) {
            let db = new StringDatabase;
            for(let key in state) {
                db.set(key, state[key]);
            }
            return db;
        }
    }
    ```
    
    - `StringDatabase` 인스턴스 타입
        
        ```tsx
        interface {
        	state: State;
        	get(key: string): string | null; 
        	set(key: string, value: string): void; 
        }
        ```
        
    - `typeof StringDatabase` 클래스의 생성자 타입
        
        ```tsx
        interface {
        	new(): StringDatabase; // 생성자 시그니처
        	from(state: State): StringDatabase; // 클래스 메서드 (static으로 선언됨)
        }
        ```
        
    - 생성자 시그니처
        - 위 코드의 `new()`
        - 타입스크립트의 new 연산자로 해당 타입을 인스턴스화할 수 있음을 정의하는 방식
        - 타입스크립트는 구조기반으로 타입을 구분하기 때문에 이 방식이 클래스가 무엇인지를 기술하는 최선의 방법임

### 7 다형성

- 함수와 타입처럼 클래스와 인터페이스도 기본값과 상한/하한 설정을 포함한 다양한 제네릭 타입 매개변수 기능을 지원함
- 제네릭 타입의 범위는 클래스나 인터페이스 전체일 수도 있고 특정 메서드로 한정할 수도 있음
- 클래스 예
    
    ```tsx
    class MyMap<K,V> {
    	constructor(initialKey: K, initialValue: V) { ... }
    	get(key: K) { ... } 
    	set(key: K, value: V) { ... } 
    	merge(map: MyMap<K1,V1>): MyMap<K | K1, V | V1> { ... } 
    	static of<K,V>(k: K, v: V): MyMap<K,V> { ... }
    }
    ```
    
- 인터페이스 예
    
    ```tsx
    interface MyMap<K,V> {
    	get(key: K): V;
    	set(key: K, value: V): void; 
    }
    ```
    
- 함수와 마찬가지로 제네릭에 구체 타입을 명시하거나 타입스크립트가 타입으로 추론하도록 할 수 있음
    
    ```tsx
    // 위의 클래스 정의 코드와 같음
    
    let a = new MyMap<string, number>('k', 1); // 명시
    let b = new MyMap('k', true); // 추론
    
    a.get('k'); // 1
    b.set('k', false);
    ```
    

### 8. 믹스 인

- `trait` , `mixin` 키워드
    - 둘 이상의 클래스를 상속받는 다중 상속과 관련된 기능을 제공하는 키워드
    - 역할 지향 프로그래밍을 제공
        - 이것은 Shape 이에요 라고 표현하는 대신
        - 측정할 수 있어요, 네 개의 면을 가졌어요 처럼 속성을 묘사함
    - 자바스크립트와 타입스크립트에서는 제공하지 않지만 손쉽게 직접 구현 가능함
- 믹스인 패턴
    - 동작과 프로퍼티를 클래스로 혼합할 수 있게 해주는 패턴
    - 규칙
        - 상태를 가질 수 있음(예: 인스턴스 프로퍼티)
        - 구체 메서드만 제공할 수 있음
        - 생성자를 가질 수 있음(클래스가 혼합된 순서와 같은 순서로 호출됨)
    - 필요한 수의 믹스인을 클래스에 제공함으로써 더 풍부한 동작을 제공
    - 타입 안전성 보장
    - 동작을 캡슐화할 뿐 아니라 동작을 재사용할 수 있도록 도움
    
    ```tsx
    // 사용 예
    class User() { ... }
    User.debug(); // User({'id': 3, 'name': 'zee'}) 으로 평가
    
    // 믹스인 구현
    type ClassConstructor<T> = new (...args: any[]) => T
    
    function withEZDebug<C extends ClassConstructor<{getDebugValue(): objct}>>
    (Class: C) {
    	return class extends Class {
    		constructor(...args: any[]) { super(...args); } // 생략 가능
    		debug() {
    			let Name = this.constructor.name;
    			let value = this.getDebugValue();
    			return Name + '(' + JSON.stringify(value) + ')';
    		}
    	};
    }
    ```
    

### 9. 데코레이터

- 타입스크립트의 실험적 기능
- 클래스, 클래스 메서드, 프로퍼티, 메서드 매개변수를 활용한 메타 프로그래밍에 깔끔한 문법을 제공함
