# 배열

- 순서가 있는 컬렉션을 저장하기 위한 자료구조
- 객체의 일종
- 배열선언

```jsx
let arr1 = new Array();
let arr2 = [];
```
- 배열 사용 예

```jsx
let fruits = ["사과", "오렌지", "자두"];

// 요소에 접근
fruits[0]; // 사과
fruits[1]; // 오렌지
fruits[2]; // 자두

// 요소 수정, 요소 추가
fruits[2] = '배'; // 배열이 ["사과", "오렌지", "배"]로 바뀜
fruits[3] = '레몬'; // 배열이 ["사과", "오렌지", "배", "레몬"]으로 바뀜

fruits.length; // 3
```

- 이형 배열(자료형에 제약 없음)

```jsx
let arr = [ '사과', { name: '이보라' }, true, function() { console.log('안녕하세요.'); } ];

console.log(arr[1].name); // 이보라
arr[3](); // 안녕하세요.
```

## 반복문

```jsx
let fruits = ["사과", "오렌지", "자두"];

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}

// 배열 요소를 대상으로 반복 작업을 수행합니다.
for (let fruit of fruits) {
  console.log(fruit);
}

// 배열엔 사용하지 않는 것을 권장, 배열이 아닌 객체 사용할 것
for (let key in arr) {
  console.log(arr[key]); // 사과, 오렌지, 배
}
```

## length 프로퍼티로 배열 조작하기

- `length`의 값을 수동으로 증가시키면 아무 일도 일어나지 않음
- 그런데 값을 감소시키면 배열이 잘림
- 짧아진 배열은 다시 되돌릴 수 없음

```jsx
let arr = [1, 2, 3, 4, 5];

arr.length = 2; // 요소 2개만 남기고 잘라봅시다.
console.log(arr); // [1, 2]

arr.length = 5; // 본래 길이로 되돌려 봅시다.
console.log(arr[3]); // undefined: 삭제된 기존 요소들이 복구되지 않습니다.

arr.length = 0; // 배열 비우기
console.log(arr)
```

## 배열 메서드

### 1. 요소를 추가하거나 제거하는 메서드

- push / pop
    - 배열 끝에 요소 추가 / 배열 끝 요소 제거
    
    ```jsx
    let fruits = ["사과", "오렌지"];
    
    fruits.push("배");
    
    alert( fruits ); // 사과,오렌지,배
    
    alert( fruits.pop() ); // 배열에서 "배"를 제거하고 제거된 요소를 얼럿창에 띄웁니다.
    alert( fruits ); // 사과,오렌지
    ```
    
- unshift / shift
    - 배열 맨 앞에 요소 추가 / 배열 맨 앞 요소 제거
    
    ```jsx
    let fruits = ["오렌지", "배"];
    
    fruits.unshift('사과');
    
    alert( fruits ); // 사과,오렌지,배
    
    alert( fruits.shift() ); // 배열에서 "사과"를 제거하고 제거된 요소를 얼럿창에 띄웁니다.
    alert( fruits ); // 오렌지,배
    
    let fruits2 = ["사과"];
    
    fruits2.push("오렌지", "배");
    fruits2.unshift("파인애플", "레몬");
    
    // ["파인애플", "레몬", "사과", "오렌지", "배"]
    alert( fruits2 );
    ```
    
- splice
    - 배열에서 요소 하나만 지움
    - 첫 번째 매개변수:  조작을 가할 첫 번째 요소를 가리키는 인덱스(index)
    - 두 번째 매개변수: deleteCount로, 제거하고자 하는 요소의 개수
    - 세 번째부터의 매개변수: 배열에 추가할 요소들
    
    ```jsx
    let arr = ["I", "study", "JavaScript"];
    
    arr.splice(1, 1); // 인덱스 1부터 요소 한 개를 제거
    
    alert( arr ); // ["I", "JavaScript"]
    ```
    
- slice
    - `start`인덱스부터 (`end`를 제외한) `end`인덱스까지의 요소를 복사한 새로운 배열을 반환
    
    ```jsx
    let arr = ["t", "e", "s", "t"];
    
    alert( arr.slice(1, 3) ); // e,s (인덱스가 1인 요소부터 인덱스가 3인 요소까지를 복사(인덱스가 3인 요소는 제외))
    
    alert( arr.slice(-2) ); // s,t (인덱스가 -2인 요소부터 제일 끝 요소까지를 복사)
    ```
    
- concat
    - 기존 배열의 요소를 사용해 새로운 배열을 만들거나 기존 배열에 요소를 추가하고자 할 때 사용
    
    ```jsx
    let arr = [1, 2];
    
    // arr의 요소 모두와 [3,4]의 요소 모두를 한데 모은 새로운 배열이 만들어집니다.
    alert( arr.concat([3, 4]) ); // 1,2,3,4
    
    // arr의 요소 모두와 [3,4]의 요소 모두, [5,6]의 요소 모두를 모은 새로운 배열이 만들어집니다.
    alert( arr.concat([3, 4], [5, 6]) ); // 1,2,3,4,5,6
    
    // arr의 요소 모두와 [3,4]의 요소 모두, 5와 6을 한데 모은 새로운 배열이 만들어집니다.
    alert( arr.concat([3, 4], 5, 6) ); // 1,2,3,4,5,6
    ```
    

### 2. 배열 요소에 대해 반복작업을 하는 메서드

- forEach
    
    ```jsx
    ["Bilbo", "Gandalf", "Nazgul"].forEach(console.log);
    ```
    

### 3. 배열을 탐색하는 메서드

- indexOf / lastIndexOf / includes
    - 같은 이름을 가진 문자열 메서드와 문법과 결과가 동일함
    - `includes`는 `NaN`도 제대로 처리한다는 점에서 `indexOf/lastIndexOf`와 다름
    
    ```jsx
    let arr = [1, 0, false];
    
    alert( arr.indexOf(0) ); // 1
    alert( arr.indexOf(false) ); // 2
    alert( arr.indexOf(null) ); // -1
    
    alert( arr.includes(1) ); // true
    
    const arr = [NaN];
    alert( arr.indexOf(NaN) ); // -1 (완전 항등 비교 === 는 NaN엔 동작하지 않으므로 0이 출력되지 않습니다.)
    alert( arr.includes(NaN) );// true (NaN의 여부를 확인하였습니다.)
    ```
    
- find / findIndex
    - **find**
        - true가 반환되면 반복이 멈추고 해당 요소를 반환
        - 조건에 해당하는 요소가 없으면 undefined를 반환
    
    ```jsx
    let users = [
      {id: 1, name: "John"},
      {id: 2, name: "Pete"},
      {id: 3, name: "Mary"}
    ];
    
    let user = users.find(item => item.id == 1);
    
    alert(user.name); // John
    ```
    
    - findIndex
        - `find`와 동일한 일을 하나, 조건에 맞는 요소를 반환하는 대신 해당 요소의 인덱스를 반환함
        - 조건에 맞는 요소가 없으면 `1`이 반환함
- filter
    - `find` 메서드는 함수의 반환 값을 `true`로 만드는 단 하나의 요소를 찾음
    - 조건을 충족하는 요소가 여러 개라면 `filter`를 사용
    - 조건에 맞는 요소 전체를 담은 배열을 반환함
    
    ```jsx
    let users = [
      {id: 1, name: "John"},
      {id: 2, name: "Pete"},
      {id: 3, name: "Mary"}
    ];
    
    // 앞쪽 사용자 두 명을 반환합니다.
    let someUsers = users.filter(item => item.id < 3);
    
    alert(someUsers.length); // 2
    ```
    

### 4. 배열을 변형하는 메서드

- **map**
    - 유용성과 사용빈도가 아주 높음
    - 배열 요소 전체를 대상으로 함수를 호출하고 함수 호출 결과를 배열로 반환함
    
    ```jsx
    let result = arr.map(function(item, index, array) {
      // ...
    });
    
    let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
    alert(lengths); // 5,7,6
    ```
    
- sort
    - 정렬
    - 정렬을 위한 함수를 필요로 함
    
    ```jsx
    function compareNumeric(a, b) {
      if (a > b) return 1;
      if (a == b) return 0;
      if (a < b) return -1;
    }
    
    let arr = [ 1, 2, 15 ];
    
    arr.sort(compareNumeric);
    
    alert(arr);  // 1, 2, 15
    ```
    
- reverse
    - 역순으로 바꿈
    
    ```jsx
    let arr = [1, 3, 2, 4, 5];
    arr.reverse();
    
    alert( arr ); // 5,4,2,3,1
    ```
    
- split / join
    
    ```jsx
    let names = 'Bilbo, Gandalf, Nazgul';
    
    let arr = names.split(', ');
    
    for (let name of arr) {
      console.log( `${name}에게 보내는 메시지` );
    }
    
    let str = arr.join(';'); // 배열 요소 모두를 ;를 사용해 하나의 문자열로 합칩니다.
    
    alert( str ); // Bilbo;Gandalf;Nazgul
    ```
    
- reduce / reduceRight
    - 배열을 기반으로 값 하나를 도출할 때 사용
    - reduce는 배열의 첫 요소부터, reduceRight 는 배열의 마지막 요소부터 연산을 수행함
    
    ```jsx
    let arr = [1, 2, 3, 4, 5];
    
    let result = arr.reduce((sum, current) => sum + current, 0);
    
    alert(result); // 15
    ```
    

### 5. 배열 여부 알아내기

- `Array.isArray( )`
- 자바스크립트에서 배열은 독립된 자료형으로 취급되지 않고 객체형에 속함
- 따라서 `typeof`로는 일반 객체와 배열을 구분할 수가 없음

```jsx
alert(typeof {}); // object
alert(typeof []); // object

alert(Array.isArray({})); // false
alert(Array.isArray([])); // true
```

### +) thisArg

- 함수를 호출하는 대부분의 배열 메서드(`find`, `filter`, `map` 등. `sort`는 제외)는 `thisArg`라는 매개변수를 옵션으로 받을 수 있음
- `thisArg`는 함수의 `this`가 됩니다.

```jsx
let army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    return user.age >= this.minAge && user.age < this.maxAge;
  }
};

let users = [
  {age: 16},
  {age: 20},
  {age: 23},
  {age: 30}
];

// army.canJoin 호출 시 참을 반환해주는 user를 찾음
let soldiers = users.filter(army.canJoin, army);

alert(soldiers.length); // 2
alert(soldiers[0].age); // 20
alert(soldiers[1].age); // 23
```
