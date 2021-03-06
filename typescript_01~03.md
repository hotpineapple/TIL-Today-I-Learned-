## 1. TypeScript Introduction

> 타입 스크립트는 *더 **안전한** 프로그램*을 구현하는 것을 보장한다.
> 
- 안전한 = **타입 안정성**
- 타입 안정성 : 타입을 이용해 프로그램이 유효하지 않은 작업을 수행하는 것을 방지
    - 유효하지 않은 동작의 예
        - 숫자와 리스트 곱하기
        - 객체 리스트를 인수로 받는 함수에 문자열 리스트를 인수로 전달해 호출하기
        - 객체에 존재하지 않는 멤버 함수를 호출하기
        - 최근에 다른 곳으로 이동된 모듈 임포트하기
- 자바스크립트 vs 타입 스크립트
    - 자바스크립트
    
    ```jsx
    3 + [] // 문자열 3으로 평가
    
    let obj = {};
    obj.foo // undefined 로 평가
    
    function a(b) {
    	return b/2;
    }
    a('z'); // NaN 으로 평가
    ```
    
    - 타입스크립트
    
    ```tsx
    3 + [] // TS2365: 3타입과 never[] 타입에 연산자 + 를 적용할 수 없음
    
    let obj = {};
    obj.foo // TS2339: {} 타입에 foo 프로퍼티가 존재하지 않음
    
    function a(b) {
    	return b/2;
    }
    a('z'); // TS2345: number 타입의 매개변수에 z 라는 인수 타입을 할당할 수 없음
    ```
    

## 2. Abstract

### 2.1 타입스크립트 컴파일러(TSC)

- 보통의 컴파일 과정
    - 텍스트 파일(소스코드) → 추상문법트리(AST) → 바이트코드 → 실행
- 타입스크립트 컴파일러의 특별한 점
    - 바이트 코드가 아닌 자바스크립트 코드로 변환한다는 점
    - AST 를 만들어 결과 코드를 내놓기 전에 타입 검사기를 거친다
        - 타입 검사기(Type checker) : 코드의 타입 안전성을 검증하는 특별한 프로그램
    - TS 소스 → TS AST → **타입검사기** → JS 소스 → JS AST → JS 바이트코드 → 실행(브라우저, Node.js 등)
        - 타입검사 이후에는 개발자가 사용한 타입을 사용하지 않는다

### 2.2 타입 시스템

- 타입 검사기가 프로그램에 타입을 할당하는 데 사용하는 규칙의 집합
    - 명시적 타입
    - 자동으로 추론되는 타입
- 타입 결정방식
    - gradually typed
    - 그러나 모든 코드 타입을 컴파일 타임에 지정하는 것을 목표로 해야 함
- 타입 변환 여부
    - 가능하나 필요한 경우 한해 명시적으로 사용해야 함
    
    ```tsx
    3 + [1]; // TS2365: 3과 number[] 타입에 + 연산자를 적용할 수 없음
    (3).toString() + [1].toString() // 31로 평가
    ```
    
- 타입 확인 시점
    - 컴파일 타임
    - 코드를 실행하기 전에 실수를 바로잡을 수 있음
- 에러 검출 시점
    - 컴파일 타임
    - 런타임 예외를 제외한 많은 에러를 검출할 수 있음

### 2.3 코드 편집기 설정

- tsconfig.json 파일 생성 후 설정

## 3. 타입의 모든 것

### any vs. unknown

- any 와 달리 unknown은 모든 연산을 지원하지는 않는다.
- typeof, instanceof 연산자로 정제(refine)한 후에야 일부 연산을 지원한다
- unknown은 추론되지 않으므로 개발자가 명시적으로 설정해야만 한다.

### type literal

- 오직 하나의 값을 나타내는 타입
- 안전성을 추가로 확보해주는 강력한 언어 기능
    
    ```tsx
    const c: boolean = true;
    let e: true = true;
    ```
    

### 객체

- 구조기반 타입화(↔ 이름기반타입화)
    - 객체의 이름에 상관없이 객체가 어떤 프로퍼티를 갖고 있는지를 따짐
- 객체를 서술하는 데 타입을 이용하는 방식
    1. object
        - 서술하는 값에 대한 정보를 거의 알려주지 않음
        - any 보다 조금 더 좁은 타입
        - 값이 자바스크립트 객체(null 이 아님) 임을 말해줄 뿐임
        
        ```tsx
        let a: object = { b: 'x' };
        a.b // TS2339: b 프로퍼티는 object 에 존재하지 않음
        ```
        
    2. 객체 리터럴 (=자동 추론하도록)
        
        ```tsx
        let a = { b: 'x' };
        a.b 
        ```
        
        - const 객체의 타입 추론
            - 기본타입과 달리 객체는 const 로 선언해도 더 좁은 타입으로 추론하지 않는다
            - 자바스크립트 객체의 값은 바뀔 수 있으며 타입스크립트도 개발자가 객체를 만든 후 필드 값을 바꾸려 할 수 있다는 사실을 알기 때문이다
            
            ```tsx
            const a: {b: number} = { b: 12 };
            a // {b: number} 타입
            ```
            
        - 객체 리터럴 문법은 “이런 형태의 물건이 있다”라는 뜻
            - 이 ‘물건’은 객체 리터럴 또는 클래스
            
            ```tsx
            let c: {
            	firstName: string;
            	lastName: string;
            } = {
            	firstName: 'john',
            	lastName: 'barrowman'
            };
            
            class Person {
            	constructor (
            		public firstName: string,
            		public lastName: string
            	) { }
            }
            
            c = new Person('matt', 'smith'); // OK
            ```
            
            - `{ firstName: string; lastName: string}`  은 객체의 형태를 묘사함과 동시에 객체 리터럴, 클래스 인스턴스 모두 이 형태를 만족하므로 타입스크립트는 Person 을 c에 할당하는 동작을 허용함
            - 더 많은 프로퍼티를 할당하거나 필요한 프로퍼티를 제공하지 않으면 타입스크립트 오류
                - 어떤 프로퍼티는 선택형이고, 예정에 없던 프로퍼티가 추가될 수도 있다고 타입스크립트에 알려주기
                
                ```tsx
                let a: {
                	b: number;
                	c? string;
                	[key: number]: boolean;
                }
                
                a = { b: 1 };
                a = { b: 1, c: undefined };
                a = { b: 1, c: 'd' };
                a = { b: 1, 10: true };
                a = { b: 1, 10: true, 20: false };  
                a = { 10: true } // TS2741: {10: true} 타입에는 b 프로퍼티가 없음
                a = { b: 1, 33: 'red' } // TS2741: string 타입은 boolean 타입에 할당할 수 없음
                ```
                
                - 인덱스 시그니처
                    
                    `[key: T]: U`
                    
                    - 타입스크립트에 어떤 객체가 여러 키를 가질 수 있음을 알려주는 문법
                    - 모든 T 타입의 키는 U 타입의 값을 갖음
                    - 명시적으로 정의한 키 외에 다양한 키를 객체에 안전하게 추가할 수 있음
                    - T 타입은 number 나 string 에 할당할 수 있는 타입이어야 함
                    - 키 이름은 key 가 아니어도 됨
        - readonly 한정자
            - 특정 필드를 읽기 전용으로 정의
            - 객체 프로퍼티에 const 를 적용한 효과
            - (참고) 자바스크립트에서 객체 할당 시 프로퍼티 대괄호의 의미: https://velog.io/@bigbrothershin/React-%EC%B4%88%EA%B8%B0-%EC%84%A4%EC%A0%95-%EC%9B%B9%ED%8C%A9-%EC%84%A4%EC%A0%95-2020.2.5

### union

- 타입에도 연산을 할 수 있다

```tsx
type Returns = string | null;

function trueOrNull(isTrue: boolean): Returns {
	if(isTrue) {
		return 'true';
	}
	return null;
}
```

### 튜플
* 길이가 고정되었고 각 인덱스의 타입이 알려진 배열의 일종
* 다른 타입과 달리 선언할 때 타입을 항상 명시해야 함
* 순수 배열보다 안전성을 높일 수 있음

```tsx
let trainFares: [number, number?][] = [
	[3.75],
	[8.25, 7.70],
	[10.50]
]
// 위와 아래 코드는 같게 동작함
let moreTrainFares: ([number] | [number, number])[] = [
	[3.75],
	[8.25, 7.70],
	[10.50]
]

// 최소 한 개의 요소를 갖는 배열
let friends: [string, ...stirng[]] = ['Sara', 'Tali', 'Chole'];

// 이형(heterogeneous) 배열
let list: [number, boolean, ...string[]] = [1,false,'a','b','c']; 
```
* readonly 를 이용하여 읽기 전용 배열과 튜플을 만들 수 있음
* 조금만 바뀌어도 원래 배열을 복사하므로 주의하지 않으면 성능이 느려질 수 있음
* 불변 배열을 자주 사용해야 한다면 immutable 같은 라이브러리를 고려하는 것을 권장함

### enum

- 열거형을 안전하게 사용하는 방법은 까다로우므로 사용하지 않는 것을 권장
- 타입 스크립트에서는 값 또는 키로 enum 열거형에 접근하는 것을 허용함
- 그런데 존재하지 않는 값에 대해서도 열거형 접근을 허용함
- const enum 문자열 리터럴로만 열거형 접근을 허용함
    - 그런데 모든 숫자를 열거형에 할당할 수 있음 → 문자열 값을 갖는 열거형으로 해결 가능
    - const enum은 필요한 곳에 열거형 멤버의 값을 채워넣는 기능이 있음
    - 컴파일한 이후 열거형을 갱신하면 런타임에 같은 열거형이 버전에 따라 다른 값을 갖게 되므로 채워넣기 기능을 피해야함
    - NPM 으로 배포하거라 라이브러리로 제공할 프로그램에서는 사용하지 말아야 함
