# 6. 고급 타입

### 이 장에서 알아볼 개념

- 서브타입화
- 할당성
- 가변성(variance)
- 타입 넓히기
- 제어 흐름 기반의 타입확인 기능
    - 정제
    - 종합성(totality)
- 객체 타입을 키로 활용하고 매핑하는 방법
- 조건부 타입 사용
- 자신만의 타입 안전 장치 정의
- 타입 Assertion
- 확실한 할당 Assertion
- 타입 안전성을 더할 수 있는 패턴
    - 컴패니언 객체 패턴
    - 튜플 타입의 추론 개선
    - 이름기반 타입 흉내내기
    - 안전하게 프로토타입 확장하기

---

## 1. 타입 간의 관계

### 1.1 서브타입과 슈퍼타입

- 서브타입 예시
    - 배열은 객체의 서브타입
    - 튜플은 배열의 서브타입
    - 모든 것은 any의 서브타입
    - never 은 모든 것의 서브타입
- 슈퍼타입 예시
    - 배열은 튜플의 슈퍼타입
    - 객체는 배열의 슈퍼타입
    - any는 모든 것의 슈퍼타입
    - never 은 어떤 것의 슈퍼타입도 아님

### 1.2 가변성

- 서브타입 간단한 예시
    - number 는 number | string 의 서브타입
- 그러나 매개변수화된(제네릭) 타입 등 복합 타입에서는 복잡함
    - 다른 타입을 포함하는 타입 (예: Array<A>)
    - 형태 (예: { a: number }, (a: A) => B)
- 형태와 배열 가변성
    - 어떤 객체를 슈퍼타입을 기대하는 곳에 사용하는 것을 허용
    - 어떤 형태를 요구하는 위치에 건낼 수 있는 타입은
        - 요구되는 타입에 포함된 프로퍼티 각각에 대해
        - 기대하는 타입과 같거나 기대하는 타입의 서브타입인 프로퍼티를 가지고 있어야 함(=공변, covariance)
        - 기대하는 프로퍼티 타입의 슈퍼타입인 프로퍼티가 있다면 건낼 수 없음
        - 함수의 매개변수 타입만이 contra-variance(기대하는 타입과 같거나 기대하는 타입의 슈퍼타입인 프로퍼티를 가지고 있어야 함) 함
        - 가변성
            - 불변: 정확히 같은 타입
            - 공변: 같거나 서브타입
            - 반변: 같거나 슈퍼타입
            - 양변: 공변 또는 반변
- 함수 가변성
    - 다음의 조건을 만족하면 함수 A 는 함수 B의 서브타입이다
        - 함수 A가 함수 B 와 같거나 그보다 적은 수의 매개변수를 가짐
        - A의 this 타입을 따로 지정하지 않으면 A의 this 타입은 B의 this 타입과 같거나 슈퍼타입
        - A의 각 매개변수는 B의 각각에 대응하는 매개변수과 같거나 슈퍼타입
        - A의 반환타입은 B의 반환타입과 같거나 서브타입
    
    ```tsx
    class Animal {
    	eat() { console.log('eat..'); }
    }
    class Bird extends Animal {
    	chirp() { console.log('chirp! chirp!'); }
    }
    class Crow extends Bird {
    	caw() {console.log('caw~'); }
    }
    
    function chirp(bird: Bird): Bird {
    	bird.chirp();
    	return bird;
    }
    
    // chirp(new Animal); // TS2345: 인수 Animal타입을 매개변수 Bird 타입에 할당할 수 없음
    chirp(new Bird); // OK
    chirp(new Crow); // OK
    
    function clone(f: (b: Bird) => Bird): void {
    	let parent = new Bird;
    	let baby = f(parent); 
    	baby.chirp(); // (*)
    }
    
    function birdToBird(b: Bird): Bird { return new Bird; }
    function birdToCrow(b: Bird): Crow { return new Crow; }
    function birdToAnimal(b: Bird): Animal { return new Animal; }
    
    clone(birdToBird); // OK
    clone(birdToCrow); // OK
    // clone(birdToAnimal); // TS2345: 인수 (b: Bird) => Animal 타입은 매개변수 (b: Bird) => Bird 타입에 할당할 수 없음. (*) 에서 문제
    
    function animalToBird(a: Animal): Bird { 
    	a.eat();	
    	return new Bird; 
    }
    function crowToBird(c: Crow): Bird { 
    	c.caw(); // (**)
    	return new Bird;
    }
    
    clone(animalToBird); // OK
    clone(birdToBird); // OK
    // clone(crowToBird); // TS2345: 인수 (c: Crow) => Animal 타입은 매개변수 (b: Bird) => Bird 타입에 할당할 수 없음. (**) 에서 문제
    ```
    

### 1.3 할당성

- A 라는 타입을 다른 B 라는 타입이 필요한 곳에 사용할 수 있는지 결정
- 비 열거형
    - A가 any 인가
    - A가 B 와 같거나 서브타입인가
- 열거형(권장하지 않음)
    - A는 열거형 B 의 멤버인가
    - B는 number 타입의 멤버를 최소 한 개 이상 가지고 있으며 A는 number 타입인가

### 1.4 타입 넓히기

- 타입스크립트의 타입 추론 동작 원리 이해하는데 도움
- let이나 var 로 값을 바꿀 수 있는 변수를 선언하면 그 변수의 타입이 리터럴 값에서 리터럴이 속한 기본 타입으로 넓혀짐
    - 객체의 경우 const 로 선언해도 넓혀짐에 유의
    - 타입을 타입리터럴로 명시하면 넓혀지지 않음
- null이나 undefined로 초기화된 변수는 any 타입으로 넓혀짐
- null이나 undefined로 초기화된 변수가 선언 범위를 벗어나면 타입스크립트는 확실한(좁은) 타입을 할당함
- const 타입
    - 타입 assertion으로 활용할 수 있음
    - 타입이 넓혀지지  않고 가능한 좁은 타입으로 추론되길 원할 때 사용
    - 추가적으로 멤버에 `readonly` 적용됨
    
    ```tsx
    let a = {x: 3}; // {x: number} 타입으로 추론
    let b: {x: 3}; // {x: 3} 타입으로 추론
    let c = {x: 3} as const; // {readonly x: 3} 타입으로 추론
    ```
    
- 초과 프로퍼티 확인
    - 타입스크립트는 한 객체 타입을 다른 객체 타입에 할당할 수 있는지 확인할 때에도 타입 넓히기를 이용함
    
    ```tsx
    type Options = {
        baseURL: string;
        cacheSize?: number;
        tier?: 'prod' | 'dev';
    }
    
    class API {
        constructor(private options: Options){ }
    }
    
    const api1 = new API({
        baseURL: 'https://api.mysite.com',
        tier: 'prod'
    });
      
    const api2 = new API({
        baseURL: 'https://api.mysite.com',
        tierr: 'prod'; // TS2345: Argument of type '{ baseURL: string; tierr: string; }' is not assignable to parameter of type 'Options'. Object literal may only specify known properties, but 'tierr' does not exist in type 'Options'. Did you mean to write 'tier'?
    });
    
    const api3 = new API({
    	baseURL: 'https://api.mysite.com',
    	tierr: 'prod'
    } as Options);
    
    const badOptions = {
    	baseURL: 'https://api.mysite.com',
    	tierr: 'prod'
    };
    
    const api4 = new API(badOptions); // (*) 왜 에러가 검출되지 않을까? 아래 설명 참고
    ```
    
    - 객체타입은 그 멤버와 공변인데 어떻게 에러를 검출했을까?
        - 슈퍼타입에 서브타입을 전달하면
        - 기대되는 타입(`{baseURL: string, cacheSize: number | undefiend, tier: 'prod' | 'dev' | undefined, tierr: undefined}`) 각각의 프로퍼티에 대해
        - 전달하는 타입(`{baseURL: string, cacheSize: undefined, tier: undefined tierr: 'prod'}`)의 대응 프로퍼티가 공변하기 때문에
            - `baseURL` : `string` 은 `string` 과 같고
            - `cacheSize` : `undefined` 은 `undefined` 와 같고
            - `tier` : `undefined` 은 `undefined` 와 같음
        - 원래는 에러를 검출하지 못해야 함
    - 그럼에도 불구하고 에러를 검출할 수 있는 이유는 **초과 프로퍼티 확인**을 했기 때문
        - 신선한 객체 리터럴 타입 T를 다른 타입 U에 할당하려는 상황에서
        - ***T가 U가 가지고 있지 않은 프로퍼티를 가지고 있다면 에러로 처리함***
        - 신선한 객체 리터럴 타입 : 타입스크립트가 객체 리터럴로부터 추론한 타입
            - 객체 리터럴이 타입 assertion을 사용하거나 변수로 할당되면 일반 객체 타입으로 넓혀지면서 신선함이 사라짐 `(*)`
            - 신선함이 사라지면 타입스크립트는 초과 타입 확인을 하지 않음

### 1.5 정제 (Refinement)

- 타입스크립트는 `Symbolic execution` 의 일종인 흐름 기반 타입 추론을 수행함
- 타입검사기는
    - `typeof`, `instanceof`, `in` 등의 타입 질의 뿐 아니라
    - `if`, `?`, `||`, `switch` 같은 제어 흐름 문장까지 고려하여 타입을 정제함
- 예시
    
    ```tsx
    type Unit = 'cm' | 'px';
    
    let units: Unit[] = ['cm', 'px'];
    
    function parseUnit(value: string): Unit | null {
        for (let i=0;i<units.length;i++) {
            if(value.endsWith(units[i])) {
                return units[i];
            }
        }
        return null;
    }
    
    type Width = {
        unit: Unit;
        value: number;
    }
    
    function parseWidth(width: number | string | null | undefined): Width | null {
        if(!width) { // number | string 으로 정제
            return null;
        }
        if(typeof width === 'number') { // 참이면 number 로 정제, 아니면 string 으로 정제
            return {unit: 'px', value: width};
        }
    
        let unit = parseUnit(width);
        if(unit) { // Unit | null 에서 정제
            return {unit, value: parseFloat(width)};
        }
    
        return null;
    }
    ```
    
- 차별된 유니온 타입
    
    ```tsx
    type UserTextEvent= {
        value: string;
        target: HTMLInputElement;
    }
    
    type UserMouseEvent = {
        value: [number, number];
        target: HTMLElement;
    }
    
    type UserEvent = UserTextEvent | UserMouseEvent;
    
    function handleBad(event: UserEvent) {
        if(typeof event.value === 'string') {
            event.value // string 타입 <- OK
            event.target // HTMLInputElement | HTMLElement 타입 <- 왜?
        }
        event.value // [number, number] 타입 <- OK
        event.target // HTMLInputElement | HTMLElement 타입 <- 왜?
    }
    ```
    
    - `target` 이 정제되지 않은 이유
        - `handleBad` 함수가 `UserEvent` 타입의 매개변수를 받는다는 것은
        - `UserTextEvent` 나 `UserMouseEvent` 만 전달할 수 있다는 의미가 아님
        - `UserTextEvent | UserMouseEvent` 타입을 전달할 수도 있음
        - 유니온의 멤버가 서로 중복될 
        - **리터럴 타입**(아래 코드에서는 문자열 리터럴)**을 이용**해 유니온 타입이 만들어낼 수 있는 *각각의 경우*를 `태그(tag)` 하는 방식으로 해결 가능
        - 좋은 태그의 조건
            - 유니온 타입의 각 경우와 같은 위치
            - 리터럴 타입
            - 제네릭이 아님
            - 상호 배타적임
    
    ```tsx
    type UserTextEvent= {
        type: 'TextEvent';
        value: string;
        target: HTMLInputElement;
    }
    
    type UserMouseEvent = {
        type: 'MouseEvent';
        value: [number, number];
        target: HTMLElement;
    }
    
    type UserEvent = UserTextEvent | UserMouseEvent;
    
    function handleGood(event: UserEvent) {
        if(event.type === 'TextEvent') {
            event.value // string 타입 <- OK
            event.target // HTMLInputElement 타입 <- OK
        }
        event.value // [number, number] 타입 <- OK
        event.target // HTMLElement 타입 <- OK
    }
    ```
