#  async 와 await

### async 키워드로 프라미스 객체로 감싸서 반환하기
```javascript
async function foo() {
  return 1;
}

foo()
  .then((value) => { console.log('fulfilled: ' + value )}); 
  .catch((error) => { console.log('rejected: ' + error )}); 
```
결과
`fulfilled: 1`

###  await 키워드로 동기 코드 만들고 try~catch 로 에러 핸들링

```javascript
let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000)
  });

try {
  let result = await promise; // promise 가 처리(이행 OR 거부)될 때까지 기다림(await)
  alert('fulfilled, result: ' + result);
} catch(error) {
  alert('rejected, error: ' + error);
}
```
#### await 주의
await 은 최상위 레벨 코드에서는 동작하지 않는다. 이 경우에는
* then/catch 핸들러를 쓰거나,
* await 이 필요한 코드를 async 익명함수로 감싸는 방법을 사용할 수 있다.


### return awit, return, awit의 차이



### 참고 링크

* 모던 자바스크립트 튜토리얼(https://ko.javascript.info/async-await)
* async와 await를 사용하여 비동기 프로그래밍을 쉽게 만들기(https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Async_await)
* [Node.js] await vs return vs return await: 비동기 이해하기](https://ooeunz.tistory.com/47)
