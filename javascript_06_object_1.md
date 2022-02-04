# 객체 기본

- 특수한 기능을 가진 연관 배열(associative array)

## 1. 프로퍼티

- `키: 값` 쌍

### 1.1 값: 모든 자료형 가능

### 1.2 키: 문자열만 가능

- 여러 단어 조합 문자열도 가능(단, `""` 로 묶어야 함)
    - 점 표기법으로 접근 불가
    - 대괄호 표기법로 접근 가능

```jsx
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // 복수의 단어는 따옴표로 묶어야 합니다.
};

alert(user["likes birds"]); // true
```

- 표현식(문자열)도 가능

```jsx
let user = {
  name: "John",
  age: 30
};

let key = prompt("사용자의 어떤 정보를 얻고 싶으신가요?", "name");

// 변수로 접근
alert( user[key] ); // John (프롬프트 창에 "name"을 입력한 경우)

let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");

let bag = {
  [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 옵니다.
};

alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력됩니다.
```

- 숫자형은 문자열로 자동변환됨

```jsx
let obj = {
  0: "test" // "0": "test"와 동일합니다.
};

// 숫자 0은 문자열 "0"으로 변환되기 때문에 두 얼럿 창은 같은 프로퍼티에 접근합니다,
alert( obj["0"] ); // test
alert( obj[0] ); // test (동일한 프로퍼티)
```

- `__proto__` 는 특별함
    - 
    
    [프로토타입 상속](https://www.notion.so/a9b265aa7a6a40c79e826b228619bbfd)
    

### 1.3 `delete` 연산자

- 프로퍼티 삭제 가능

### 1.4 `in` 연산자

- 프로퍼티 존재 여부 확인
    - 일부 경우, 프로퍼티 값이 `undefined` 인지 비교하는 방법도 가능
    - 프로퍼티 존재하나 값이 undefined(개발자 임의로 할당)인 경우 사용 불가
        - 값을 알 수 없거나 비어있다는 것을 나타낼 때는 보통 null을 사용하기는 함
- 키는 `""` 로 묶어야함
    - 묶지 않으면 변수의 값이 조사 대상이 됨
    
    ```jsx
    let user = { name: "John", age: 30 };
    
    alert( user.noSuchProperty === undefined ); // true는 '프로퍼티가 존재하지 않음'을 의미
    
    alert( "age" in user ); // user.age가 존재하므로 true가 출력.
    alert( "noSuchProperty" in user ); // user.blabla는 존재하지 않기 때문에 false가 출력
    ```
    

### 1.5 `for ... in` 반복문

- 객체의 모든 키 순회
    
    ```jsx
    let user = {
      name: "John",
      age: 30,
      isAdmin: true
    };
    
    for (let key in user) {
      // 키
      alert( key );  // name, age, isAdmin
      // 키에 해당하는 값
      alert( user[key] ); // John, 30, true
    }
    
    let obj = {};
    for (let key in obj ) alert("hi"); // 출력되지 않음
    ```
    

## 2. 객체 유의사항

### 2.1 상수 객체는 수정될 수 있음

```jsx
const user = {
  name: "John"
};

user.name = "Pete"; // (*)

alert(user.name); // Pete
```

- cf. 프로퍼티 플래그와 설명자

```jsx

```

### 2.2 객체의 복사

- 객체는 참조에 의해 저장되고 복사됨
    
    ```jsx
    let user = { name: 'John' };
    
    let admin = user;
    
    admin.name = 'Pete'; // 'admin' 참조 값에 의해 변경됨
    
    alert(user.name); // 'Pete'가 출력됨. 'user' 참조 값을 이용해 변경사항을 확인함
    ```
    
- 참조 비교
    - 
- 기존 객체와 똑같으면서 독립적인 객체 만들기
    
    1. 원시형 프로퍼티
    
    - 키 순회하며 primitive 프로퍼티 복사하기
    - `Object.assign`
    
    ```jsx
    let user = {
      name: "John",
      age: 30
    };
    
    let clone = {}; // 새로운 빈 객체
    
    // 빈 객체에 user 프로퍼티 전부를 복사해 넣습니다.
    for (let key in user) {
      clone[key] = user[key];
    }
    
    // 이제 clone은 완전히 독립적인 복제본이 되었습니다.
    clone.name = "Pete"; // clone의 데이터를 변경합니다.
    
    alert( user.name ); // 기존 객체에는 여전히 John이 있습니다.
    
    let user2 = { name: "John" };
    
    let permissions1 = { canView: true };
    let permissions2 = { canEdit: true };
    
    // permissions1과 permissions2의 프로퍼티를 user2로 복사합니다.
    Object.assign(user2, permissions1, permissions2);
    
    // now user2 = { name: "John", canView: true, canEdit: true }
    ```
    
    2. 객체 프로퍼티 복사하기 (= 중첩 객체 복사)
    
    - 독립적이지 않은 size (객체) 프로퍼티 : 얕은 복사
        
        ```jsx
        let user = {
          name: "John",
          sizes: {
            height: 182,
            width: 50
          }
        };
        
        alert( user.sizes.height ); // 182
        
        let clone = Object.assign({}, user);
        
        alert( user.sizes === clone.sizes ); // true, 같은 객체입니다.
        
        // user와 clone는 sizes를 공유합니다.
        user.sizes.width++;       // 한 객체에서 프로퍼티를 변경합니다.
        alert(clone.sizes.width); // 51, 다른 객체에서 변경 사항을 확인할 수 있습니다.
        ```
        
    - 독립적인 size (객체) 프로퍼티 : 깊은 복사
        - 자바스크립트 라이브러리 [lodash](https://lodash.com/)의 메서드인 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)
