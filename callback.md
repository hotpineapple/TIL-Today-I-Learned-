# 콜백과 프로미스
> 자바스크립트의 비동기처리를 하기 위한 콜백
## 비동기 처리가 필요한 이유


## 콜백기본

아래의 lazyFunction 함수는 콜백의 동작을 쉽게 이해하기 위해 `setTimeout(callback,delay)`을 이용하여 억지로 만든 함수로서,

아이디를 매개변수로 받아서 호출된 시점으로 부터 5초가 지나면 name 값을 'zee'로 세팅하는 함수이다.

실무(프론트)에서는 주로 서버 요청과 같이 다소 시간이 소요되는 실행문을 포함하는 함수를 예로 들 수 있으며,

이 함수의 실행과정에서 생성된 결과값을 이용하려는 시도는 이 함수가 완전히 실행 완료된 후에 실행되어야 할 것이다.

lazyFunction 을 호출하는 개발자는 lazyFunction이 어떻게 구현되어있는지 모르기 때문에 언제 완료되는지도 알 수 없다.

그러나 반드시 lazyFunction에서 생성된 result 값을 이용해야만 한다. 

### 잘못된 예시
```javascript
let username = 'default';
function lazyFunction(userid) {
  setTimeout(() => {
    if(userid === 'jh1994') username = 'zee';
  }, 5000);
}

lazyFunction('jh1994');
console.log('hello ' + username);
```
#### 바라는 결과
`hello zee`
#### 실제 결과
`hello default`

### 옳은 예시
```javascript
let username = 'default';
function lazyFunction(userid, callback) {
  setTimeout(() => {
    if(userid === 'jh1994') username = 'zee';
    callback(username);
  }, 5000);
}

lazyFunction((name) => { console.log('hello ' + name) });

```

#### 바라는 결과
`hello zee`

#### 실제 결과
**5초 뒤에** 

`hello zee`

여기서는 콜백함수의 구현부(console.log('hello ' + name))를 화살표 함수로 작성했지만 코드가 더 길고 복잡하며 다른 곳에서도 사용된다면 함수로 만들고 매개변수로 함수 이름만 전달할 수도 있다.

```javascript
let username = 'default';
function lazyFunction(userid, callback) {
  setTimeout(() => {
    if(userid === 'jh1994') username = 'zee';
    callback(username);
  }, 5000);
}

function greeting(name){
  console.log('hello ' + name);
}

lazyFunction(greeting); // 이 때 소괄호()는 쓰지 않고 함수 이름만 전달한다. ()를 쓰면 함수를 실행하여 그 반환값이 나오기 때문임.

```

## 콜백 에러 핸들링

```javascript
let username = 'default';
function lazyFunction(userid, callback) {
  setTimeout(() => {
    if(userid === 'jh1994') username = 'zee';
    
    if(username !== 'default') callback(null, username);
    else callback(new Error(`${userid}에 해당하는 유저 이름이 없습니다..`));
  }, 5000);
}
```
#### 호출 예시 1
```javascript
lazyFunction('jh1994', function(error, name) {
  if (error) {
    alert(`오류 발생: ${error}`);
  } else {
    console.log('hello ' + name);
  }
});
```
#### 결과
`hello zee`
#### 호출 예시 2
```javascript
lazyFunction('jihye19940314', function(error, name) {
  if (error) {
    alert(`오류 발생: ${error}`);
  } else {
    console.log('hello ' + name);
  }
});

```
#### 결과
`오류 발생: Error: jh19940314에 해당하는 유저 이름이 없습니다..`

### 콜백 마무리

여러 개의 비동기 처리가 중첩될 경우 코드가 오른쪽으로 계속 밀리는 멸망의 피라미드 혹은 콜백 지옥이 발생하는데, 이는 프라미스를 사용해서 해결가능하다.


---





