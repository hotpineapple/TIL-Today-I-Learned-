# 렉시컬환경

## 개념
- 환경레코드와 외부렉시컬환경에 대한 참조로 구성
- 함수는 선언 전에도 전역 렉시컬 환경의 환경레코드에 초기화되므로 사용할 수 있다 (호이스팅)
- 함수는 숨김 프로퍼티 `[[Environment]]`에 외부 렉시컬 환경에 대한 **참조**(값이 아님)를 저장한다.
- 함수가 실행될 때마다 독립적인 렉시컬환경이 생성된다
   - (주의) 외부 렉시컬 환경에 대한 참조는 변하지 않는다
- 함수가 실행될때 변수를 내부 환경레코드 -> 외부 렉시컬환경 순서로 찾음

## 예시코드
- 코드
```javascript
function makeCounter() {
  let count = 0;
  return function() {
    return count++;
  }
}
const counter = makeCounter();
console.log(counter()); //0
console.log(counter()); //1
console.log(counter()); //2
```
- 코드의 실행흐름 별 렉시컬환경 변화
  1. 맨 처음
    1. 전역 렉시컬 환경(*) 생성
    2. 이 스코프에서 함수 선언문 스캔
       - makeCounter는 보이지만 makeCounter의 내부 함수는 안보임!
    3. (*)의 환경 레코드에 프로퍼티 makeCounter 생성 및 초기화
  2. makeCounter 선언부
    - makeCounter의 숨김 프로퍼티 `[[Environment]]` 에 (*)에 대한 참조 저장
  3. makeCounter 호출
      1. 맨 처음: makeCounter 함수의 렉시컬 환경(**) 생성
         - (**)의 환경 레코드에 프로퍼티 count <<uninitialized>> 로 생성
         - (**)의 환경 레코드에 프로퍼티 익명함수 생성 및 초기화
      2. count 선언: count = 0 으로 초기화
      3. 익명함수 선언: 이 함수의 숨김 프로퍼티 `[[Environment]]` 에 (**)에 대한 참조 저장
  4. counter 호출 (1)
    1. counter의 렉시컬 환경(***) 환경 생성
    2. count가 내부에 없으므로 외부렉시컬 환경(**) 참조함
    3. (**)의 count 1 증가
  5. counter 호출 (2)
    1. counter의 렉시컬 환경(***) 환경 생성
    2. count가 내부에 없으므로 외부렉시컬 환경(**) 참조함
    3. (**)의 count 1 증가
  6. counter 호출 (3)
    1. counter의 렉시컬 환경(***) 환경 생성
    2. count가 내부에 없으므로 외부렉시컬 환경(**) 참조함
    3. (**)의 count 1 증가
     
## 연습문제 오답노트
- 문제
```javascript
function makeArmy() {
  let shooters = [];

  let i = 0;
  while (i < 10) {
    let shooter = function() { // shooter 함수
      alert( i ); // 몇 번째 shooter인지 출력해줘야 함
    };
    shooters.push(shooter);
    i++;
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0번째 shooter가 10을 출력함
army[5](); // 5번째 shooter 역시 10을 출력함
// 모든 shooter가 자신의 번호 대신 10을 출력하고 있음
```
- 내가 쓴 답(오답)
```javascript
function makeArmy() {
  let shooters = [];

  let i = 0;
  while (i < 10) {
    let shooter = function() { 
      let j = i; // 이부분
      alert( j ); 
    };
    shooters.push(shooter);
    i++;
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 10
army[5](); // 10
```
- 정답
```javascript
function makeArmy() {
  let shooters = [];

  let i = 0;
  while (i < 10) {
    let j = i; // 이 부분
    let shooter = function() {
      alert( j );
    };
    shooters.push(shooter);
    i++;
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0
army[5](); // 5
// 모든 shooter가 자신의 번호 대신 10을 출력하고 있음
```
- 틀린 이유:
  - 조건문, 반복문의 스코프 개념 숙지 못함
  - 렉시컬 환경을 헷갈림
  - 오답이 동작하지 않는이유
    - 내부함수의 외부 렉시컬 환경에 변수 j를 넣으려면 내부함수가 선언될때 j 가 있어야 하는데, 내가 쓴 코드는 내부함수가 실행될 때 선언됨
- 참고: 모던자바스크립트 https://ko.javascript.info/closure
