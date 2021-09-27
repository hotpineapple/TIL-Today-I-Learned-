이전 내용: [콜백](https://github.com/hotpineapple/TIL-Today-I-Learned-/edit/main/callback.md)

## 프라미스

프라미스는 제작코드 (이전 예시에서는 lazyFunction) 와 소비코드(위 예시에서는 console.log('hello ' + result); )를 이어주는 자바스크립트 객체이다.

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done"), 5000);
});
```

### .then
```javascript
promise.then(
  function(result) { ...}, // 쉼표임에 주의
  function(error) { ... }
);
```
### 호출 예시1 
```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done"), 5000);
});

promise.then(
  result => alert(result),
  error => alert(error) 
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
  result => alert(result), 
  error => alert(error) 
);
```
#### 결과
`에러 발생!`
