# 자바스크립트 기본

## 1. 원시형 자료의 형변환(Type conversion)

- 문자형으로의 변환
    - 이항연산자 + 를 사용하거나 출력할 때는 자동변환
    - 명시적 형변환
    
    ```jsx
    let value = true;
    alert(typeof value); // boolean
    
    value = String(value); // "true"가 저장.
    alert(typeof value); // string
    ```
    
- 숫자형으로의 변환
    - 수학과 관련된 함수와 표현식에서는 자동변환
    - 명시적 형변환
        - undefined : `NaN`
        - null : `0`
        - true and false : `1` 과 `0`
        - string
            - 문자열의 처음과 끝 공백 제거. 공백 제거 후 남아있는 문자열이 없다면 `0`, 그렇지 않다면 문자열에서 숫자를 읽음. 변환에 실패하면 `NaN`.
    
    ```jsx
    let str = "123";
    alert(typeof str); // string
    
    let num = Number(str); // 문자열 "123"이 숫자 123으로 변환됩니다.
    
    alert(typeof num); // number
    
    let age = Number("임의의 문자열 123");
    
    alert(age); // NaN, 형 변환이 실패합니다.
    
    alert( Number("   123   ") ); // 123
    alert( Number("123z") );      // NaN ("z"를 숫자로 변환하는 데 실패함)
    alert( Number(true) );        // 1
    alert( Number(false) );       // 0
    ```
    
- 불린형으로의 변환
    - 조건문에 사용된 표현식에서 (논리연산 시) 자동변환
    - 명시적 형변환
        - 숫자 `0`, 빈 문자열, `null`, `undefined`, `NaN`과 같이 직관적으로도 “비어있다고” 느껴지는 값들은 `false`.
        - 그 외의 값은 `true`로 변환.
    
    ```jsx
    alert( Boolean(1) ); // 숫자 1(true)
    alert( Boolean(0) ); // 숫자 0(false)
    
    alert( Boolean("hello") ); // 문자열(true)
    alert( Boolean("") ); // 빈 문자열(false)
    ```
    

## 2. 연산자

### 2.1 수학연산자

- 단항 `-`
    - 숫자형에 붙이면 부호 뒤집음(-1 을 곱한 값)
    
    ```jsx
    let x = 1;
    
    x = -x;
    alert( x ); // -1
    ```
    
- 단항 `+`
    - 숫자형에는 아무런 동작도 하지 않음
    - 숫자형이 아닌 변수는 (가능한 경우) 숫자형으로 자동변환
    
    ```jsx
    alert( +true ); // 1
    alert( +"" );   // 0
    
    let apples = "2";
    let oranges = "3";
    
    // 이항 덧셈 연산자가 적용되기 전에, 두 피연산자는 숫자형으로 변환.
    alert( +apples + +oranges ); // 5
    ```
    
- 이항 `+`
    - 문자열을 병합
    - 문자형이 아닌 변수는 문자형으로 자동변환
    
    ```jsx
    let s = "my" + "string";
    alert(s); // mystring
    
    alert( '1' + 2 ); // "12"
    alert( 2 + '1' ); // "21"
    ```
    
- 이항 `-` , `*`  `/` , `%` (나머지), `**`(거듭제곱)
    - (가능한 경우) 숫자형으로 자동변환
    
    ```jsx
    alert( 6 - '2' ); // 4, '2'를 숫자로 바꾼 후 연산이 진행.
    alert( '6' / '2' ); // 3, 두 피연산자가 숫자로 바뀐 후 연산이 진행.
    ```
    

### 2.2 논리연산자

- truthy 와 falsy
    - truthy(참 같은) : `1`, `“0”`
    - falsy(거짓 같은) : `0`, `“”`, `null`, `undefined`, `NaN`
- `||` (OR)
    - 주어진 조건 중 하나라도 참이면 true
    - 첫 번째 truthy 를 찾음
    - 단락평가 (truthy를 만나면 평가를 중단)
    
    ```jsx
    let firstName = "";
    let lastName = "";
    let nickName = "졔졔";
    
    alert( firstName || lastName || nickName || "익명"); // 졔졔
    
    true || alert("not printed");
    false || alert("printed");
    ```
    
- `&&` (AND)
    - 주어진 조건 중 하나라도 거짓이면 false
    - 첫 번째 falsy 를 찾음
    - 단락평가 (falsy를 만나면 평가를 중단)
    - if 를 대체할 수 있음
        - `&&`의 오른쪽 피연산자는 평가가 `&&` 우측까지 진행되어야 실행됨
        - 왼쪽 피연산자가 참인 경우에만 평가가 `&&` 우측까지 진행됨
    
    ```jsx
    // 첫 번째 피연산자가 truthy이면,
    // AND는 두 번째 피연산자를 반환.
    alert( 1 && 0 ); // 0
    alert( 1 && 5 ); // 5
    
    // 첫 번째 피연산자가 falsy이면,
    // AND는 첫 번째 피연산자를 반환하고, 두 번째 피연산자는 무시.
    alert( null && 5 ); // null
    alert( 0 && "아무거나 와도 상관없습니다." ); // 0
    
    let x = 1;
    
    (x > 0) && alert( '0보다 큽니다!' );
    ```
    
- cf. `??` (Nullish coalesce 연산자)
    - null 이나 undefined 가 아닌지 판단
    - 첫 번째 정의된(defined) 값을 반환
    
    ```jsx
    let height = 0;
    
    alert(height || 100); // 100
    alert(height ?? 100); // 0
    ```
    
    - null 과 undefined
        - 비교연산자를 사용할 수 있으나 따로 처리하는 코드 추가하는 것을 권장
        - null : 값이 정해지지 않은 경우(다른 언어들과 다른 의미를 가지므로 유의)
        - undefined : 선언만 하고 할당되지 않은 상태
    
    ```jsx
    Number(null) // 0
    Number(undefined) // NaN
    
    null === undefined // false
    null == undefined // true
    ```
    

## 3. 함수

### 3.1 함수 선언식과 함수 표현식 비교

- 자바스크립트 엔진의 함수 생성시점
    - 함수 선언식 : 스크립트 실행 전
    - 함수 표현식 : 스크립트 실행 후, 실행 흐름이 표현식에 도달했을 때
- 스코프 (함수선언식 또는 표현식이 블록 내에 위치한 경우)
    - 함수 선언식 : 블록 내부
    - 함수 표현식 : 블록 내, 외부 ⇒ 동적 생성 가능
    
    ```jsx
    let age = 16;
    
    if (age < 18) {
      welcome();               // \   (실행)
                               //  |
      function welcome() {     //  |
        alert("안녕!");         //  |  함수 선언문은 함수가 선언된 블록 내
      }                        //  |  어디에서든 유효합니다
                               //  |
      welcome();               // /   (실행)
    
    } else {
    
      function welcome() {
        alert("안녕하세요!");
      }
    }
    
    // 여기는 중괄호 밖이기 때문에 중괄호 안에서 선언한 함수 선언문은 호출할 수 없음.
    
    welcome(); // Error: welcome is not defined
    ```
    
    ```jsx
    let age = prompt("나이를 알려주세요.", 18);
    
    let welcome;
    
    if (age < 18) {
    
      welcome = function() {
        alert("안녕!");
      };
    
    } else {
    
      welcome = function() {
        alert("안녕하세요!");
      };
    
    }
    
    welcome(); // 제대로 동작합니다.
    ```
    

### 3.2 화살표 함수

- 함수 표현식의 간단한 표현
- 화살표 함수는 일반 함수와는 달리 ‘고유한’ `this`를 가지지 않음
    - 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 ‘평범한’ 외부 함수에서 `this` 값을 가져옴

```jsx
let user = {
  name: "지혜",
  sayHi() {
    let arrow = () => alert(this.name);
    arrow();
  }
};

user.sayHi(); // 지혜
```
