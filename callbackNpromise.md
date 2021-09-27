# 콜백과 프로미스
> 자바스크립트의 비동기처리를 하기 위한 콜백
## 비동기처리

## 오류우선패턴 콜백


### 코드로 보기

아래의 lazyFunction 함수는 콜백의 동작을 쉽게 이해하기 위해 `setTimeout(callback,delay)`을 이용하여 억지로 만든 함수로서,

호출된 시점으로 부터 5초가 지나면 result 값을 'zee'로 세팅하는 함수이다.

실무(프론트)에서는 주로 서버 요청과 같이 다소 시간이 소요되는 실행문을 포함하는 함수를 예로 들 수 있으며,

이 함수의 실행과정에서 생성된 결과값을 이용하려는 시도는 이 함수가 완전히 실행 완료된 후에 실행되어야 할 것이다.

lazyFunction 을 호출하는 개발자는 lazyFunction이 어떻게 구현되어있는지 모르기 때문에 언제 완료되는지도 알 수 없다.

그러나 반드시 lazyFunction에서 생성된 result 값을 이용해야만 한다. 

#### 잘못된 예시
```
let result = 'default';
function lazyFunction(arg) {
  setTimeout(() => {
    result = 'zee';
    console.log("now lazyFunction is completed");
  }, 5000);
  console.log(arg + ' ' + result);
}

lazyFunction('hello');

```
#### 바라는 결과
`hello zee`
#### 실제 결과
`hello default`
`now lazyFunction is completed"`

#### 옳은 예시
```
let result = 'default';
function lazyFunction(arg, callback) {
  setTimeout(() => {
    result = 'zee';
    console.log("now lazyFunction is completed");
    callback(arg + ' ' + result);
  }, 5000);
}

lazyFunction('hello', (greeting) => {
  console.log(greeting);
});
```
#### 바라는 결과
`hello zee`
#### 실제 결과
**5초 뒤에** 
`now lazyFunction is completed`
`hello zee`



## 프라미스


