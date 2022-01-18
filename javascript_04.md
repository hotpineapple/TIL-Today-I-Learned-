# 나머지 매개변수와 스프레드 연산자

### 나머지 매개변수

- `...args` 는 함수를 호출할 때 전달된 매개변수 중 이 앞에서 받고 남아있는 것들을 한데 모아 `args`라는 배열에 집어넣음
- 나머지 매개변수는 항상 마지막에 있어야 함

```jsx
function sumAll(sum, ...args) {
  
	for (let arg of args) sum += arg;

  return sum;
}

sumAll(0, 1)); // 1
SumAll(0, 1, 2)); // 3
sumAll(0, 1, 2, 3)); // 6
```

### 스프레드 연산자

- 배열을 통째로 매개변수로 넘기면 요소 각각이 분리되길 원할 때 활용
- 일반 값과 혼합해 사용하는 것도 가능
- 배열을 합칠 때도 활용함

```jsx
// 배열을 매개변수로 넘기기
let arr = [3, 5, 1];

Math.max(arr); // NaN
Math.max(...arr); // 5

// 배열 합치기
let arr1 = [3, 5, 1];
let arr2 = [8, 9, 15];

let merged = [0, ...arr1, 2, ...arr2];

console.log(merged); // 0,3,5,1,2,8,9,15 (0, arr1, 2, arr2 순서로 합쳐집니다.)
```

- 배열이 아니어도 이터러블 객체이면 사용 가능

```jsx
let str = "Hello";

alert(...str); // H (매개변수로 5개의 값을 넘겨주지만 alert는 매개변수를 하나만 받기 때문)
alert([...str]); // [H,e,l,l,o]

// Array.from은 이터러블을 배열로 바꿔줍니다.
alert(Array.from(str)); // [H,e,l,l,
```

- 배열과 객체의 복사본을 만들 수 있음
    - 배열의 복사본
    
    ```jsx
    let arr = [1, 2, 3];
    let arrCopy = [...arr]; // 배열을 펼쳐서 각 요소를 분리 후, 목록으로 만든 다음에
                            // 목록을 새로운 배열에 할당함
    
    alert(JSON.stringify(arr) === JSON.stringify(arrCopy)); // true
    alert(arr === arrCopy); // false (참조가 다름)
    
    arr.push(4);
    alert(arr); // 1, 2, 3, 4
    alert(arrCopy); // 1, 2, 3
    ```
    
    - 객체의 복사본
    
    ```jsx
    let obj = { a: 1, b: 2, c: 3 };
    let objCopy = { ...obj }; // 객체를 펼쳐서 각 요소를 분리 후, 목록으로 만든 다음에
                              // 목록을 새로운 객체에 할당함
    
    alert(JSON.stringify(obj) === JSON.stringify(objCopy)); // true
    alert(obj === objCopy); // false (참조가 다름)
    
    obj.d = 4;
    alert(JSON.stringify(obj)); // {"a":1,"b":2,"c":3,"d":4}
    alert(JSON.stringify(objCopy)); // {"a":1,"b":2,"c":3}
    ```
