이전 내용: [콜백](https://github.com/hotpineapple/TIL-Today-I-Learned-/edit/main/callback.md)

## 프라미스

프라미스는 제작코드 (이전 예시에서는 lazyFunction 함수) 와 소비코드(이전 예시에서는 console.log('hello ' + result); 또는 greeting 함수)를 이어주는 자바스크립트 객체이다.

사용법은 아래와 같다.

### 프라미스 선언과 
```javascript
let promise = new Promise(function(resolve, reject) {
  // executor(제작코드)
});

// pseudo code
promise.handler(소비코드);
```
프라미스는 내부 프로퍼티로 result 와 state를 가지며, 이 프로퍼티들은 프라미스가 처리(settled) 될 때 값이 변경된다.
* result : undefined(초기값) -> value(이행) OR  error(거부)
* state : pending(초기값) -> fulfilled(이행)  OR rejected(거부)

resolve 와 reject 콜백함수는 자바스크립트에서 정의된 함수로서, 개발자는 인터페이스에 맞게 사용하면 된다.
* resolve는 제작코드가 정상적으로 실행되었을 때 호출하는 함수로, 매개변수로 전달받은 인자를 프라미스의 result에 저장하고, 프라미스의 상태를 fulfilled로 변경한다. 그리고 이 프라미스 핸들러가 then 이라면 첫 번째 인자로 전달받은 함수를 실행한다. 
* reject 제작코드가 비정상적으로 실행(오류발생)되었을 때 호출하는 함수로, 매개변수로 전달받은 인자(보통 Error객체 또는 Error를 상속받은 클래스의 객체)를 프라미스의 result에 저장하고, 프라미스의 상태를 rejected로 변경한다. 그리고 이 프라미스 핸들러가 then 이라면 두 번째 인자로 전달받은 함수를 실행하고, catch라면 첫 번째 인자로 전달받은 함수를 실행한다.

executor는 resolve나 reject 중 하나를 반드시 호출해야 하며, 이후 다른 함수를 호출하더라도 *상태는 더 이상 변하지 않는다*.

지금까지 프라미스를 이용한 제작코드 작성에 대하여 알아보았다. 제작코드를 함수로 분리해서 프라미스를 반환하는 제작함수로 구현할 수도 있다.

```javascript
function lazyFunction(userid) {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      if(userid === 'jh1994') username = 'zee';

      if(username !== 'default') resolve(null, username);
      else reject(new Error(`${userid}에 해당하는 유저 이름이 없습니다..`));
     }, 5000);
  }
};
```

---


소비코드는 프라미스 핸들러를 통해 제작코드의 결과값, 즉 result 값를 사용할 수 있다.

#### 핸들러 종류 
* then
* catch
* finally

### .then 메서드(핸들러)
```javascript
promise.then(
  function(result) { ... }, // 쉼표임에 주의
  function(error) { ... }
);
```

### 프라미스 사용 예시1 
```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done"), 5000);
});

promise.then(
  result => alert('프라미스 이행: ' + result),
  error => alert('프라미스 거부: ' + error) 
);
```
#### 결과
`done`

### 호출 예시 2  
```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

promise.then(
  result => alert('프라미스 이행: ' + result),
  error => alert('프라미스 거부: ' + error) 
);
```
#### 결과
`에러 발생!`
