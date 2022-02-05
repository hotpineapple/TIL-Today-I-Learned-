# 객체

- 특수한 기능을 가진 연관 배열(associative array)

## 1. 프로퍼티

- `키: 값` 쌍
- 프로퍼티 종류
    - 데이터 프로퍼티
    - 접근자 프로퍼티(함수) - 1.7절

### 1.1 키(key): 문자열, 심볼형만 가능

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
    
    [프로토타입 상속](https://www.notion.so/a9b265aa7a6a40c79e826b228619bbfd)
    
- 심볼형
    
    [심볼](https://www.notion.so/9dd300b013e14e65b675597ae280d4a2)
    

### 1.2 값(value): 모든 자료형 가능

- 함수도 가능함 = 메서드
    
    ```jsx
    // 1. 프로퍼티 생성과 동시에 함수 등록
    let user = {
      name: "John",
      age: 30
    };
    
    user.sayHi = function() {
      alert("안녕하세요!");
    };
    
    // 2. 함수 선언 후 등록
    function sayHi() {
      alert("안녕하세요!");
    };
    
    // 선언된 함수를 메서드로 등록
    user.sayHi = sayHi;
    
    // 3. 객체 리터럴 안에서 함수 등록
    
    user = {
      sayHi: function() {
        alert("Hello");
      }
    };
    
    // 4. 객체리터럴 - 단축 구문
    user = {
      sayHi() { // "sayHi: function()"과 동일합니다.
        alert("Hello");
      }
    };
    ```
    
    - 객체의 프로퍼티인 함수(메서드)의 this
        
        ```jsx
        let user = {
          name: "John",
          age: 30,
        
          sayHi() {
            // 'this'는 '현재 객체'를 나타냅니다.
            alert(this.name);
          }
        
        };
        
        user.sayHi(); // John
        ```
        
        ```jsx
        let user = { name: "John" };
        let admin = { name: "Admin" };
        
        function sayHi() {
          alert( this.name );
        }
        
        // 별개의 객체에서 동일한 함수를 사용함
        user.f = sayHi;
        admin.f = sayHi;
        
        // 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
        // this 값이 달라짐
        user.f(); // John  (this == user)
        admin.f(); // Admin  (this == admin)
        
        admin['f'](); // Admin (점과 대괄호는 동일하게 동작함)
        ```
        
        - 객체 없이 함수 호출 시 this가 가리키는 것
            - 엄격모드 : undefined
            - 비 엄격모드 : window
        - 화살표 함수
            - 일반 함수와는 달리 ‘고유한’ `this`를 가지지 않습니다.
            - 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 ‘평범한’ 외부 함수에서 `this` 값을 가져옵니다.
            
            ```jsx
            let user = {
              firstName: "보라",
              sayHi() {
                let arrow = () => alert(this.firstName);
                arrow();
              }
            };
            
            user.sayHi(); // 보라
            ```
            

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
    

### 1.6 (데이터)프로퍼티의 플래그, 설명자

- 프로퍼티 플래그 : 프로퍼티 값 옵션
    - value : 값
    - writable : true 이면 값 수정 가능
    - enumerable : true 이면 반복문을 사용해 나열 가능
    - configurable : true 이면 프로퍼티 삭제나 플래그 수정 가능
    - **일반적인 방식으로 프로퍼티 생성 시 위 플래그 모두 true**
    
    ```jsx
    let user = {
      name: "John"
    };
    
    let descriptor = Object.getOwnPropertyDescriptor(user, 'name');
    
    alert( JSON.stringify(descriptor, null, 2 ) );
    /* property descriptor:
    {
      "value": "John",
      "writable": true,
      "enumerable": true,
      "configurable": true
    }
    */
    ```
    
    ```jsx
    let user = {};
    
    Object.defineProperty(user, "name", {
      value: "John"
    });
    
    let descriptor = Object.getOwnPropertyDescriptor(user, 'name');
    
    alert( JSON.stringify(descriptor, null, 2 ) );
    /*
    {
      "value": "John",
      "writable": false,
      "enumerable": false,
      "configurable": false
    }
     */
    ```
    
    - `defineProperty`를 사용해 프로퍼티를 만든 경우, `descriptor`
    에 플래그 값을 명시하지 않으면 플래그 값이 자동으로 `false`

### 1.7 프로퍼티 getter, setter

- 프로퍼티 종류
    - 데이터 프로퍼티 (~1.6절)
    - 접근자(accessor) 프로퍼티
        - 값을 획득(get)하고 설정(set)하는 역할
        - 함수이지만 외부 코드에서는 일반 프로퍼티로 보임(가상 프로퍼티 생성)
        - 'getter(획득자)'와 ‘setter(설정자)’ 메서드로 표현
- getter, setter
    - get / set 키워드 + 새 프로퍼티 이름
    
    ```jsx
    let user = {
      name: "John",
      surname: "Smith",
    
      get fullName() {
        return `${this.name} ${this.surname}`;
      }
    
    	set fullName(value) {
        [this.name, this.surname] = value.split(" ");
      }
    };
    
    alert(user.fullName); // John Smith
    
    user.fullName = "Alice Cooper";
    
    alert(user.name); // Alice
    alert(user.surname); // Cooper
    ```
    
- 접근자 프로퍼티의 설명자
    - value, writable 없는 대신 get, set 함수
    - enumerable
    - configurable
- 실제 프로퍼티 값을 감싸는 wrapper 처럼 사용
    - 입력 값 제한
    - `user`의 이름은 `_name`에 저장되고, 프로퍼티에 접근하는 것은 getter와 setter를 통해 이뤄집니다.
    - 기술적으론 외부 코드에서 `user._name`을 사용해 이름에 바로 접근할 수 있습니다. 그러나 밑줄 `"_"` 로 시작하는 프로퍼티는 객체 내부에서만 활용하고, 외부에서는 건드리지 않는 것이 관습입니다.
    
    ```jsx
    let user = {
      get name() {
        return this._name;
      },
    
      set name(value) {
        if (value.length < 4) {
          alert("입력하신 값이 너무 짧습니다. 네 글자 이상으로 구성된 이름을 입력하세요.");
          return;
        }
        this._name = value;
      }
    };
    
    user.name = "Pete";
    alert(user.name); // Pete
    
    user.name = ""; // 너무 짧은 이름을 할당하려 함
    ```
    
- 유용하게 사용하는법 - 호환성
    - 데이터 프로퍼티 `name`과 `age`를 사용해서 사용자를 나타내는 객체를 구현
    - `age`대신에 `birthday`를 저장해야 한다고 해보겠습니다.
    - 기존 코드 유지하며 birthday 프로퍼티 추가하기
    
    ```jsx
    function User(name, birthday) {
      this.name = name;
      this.birthday = birthday;
    
      // age는 현재 날짜와 생일을 기준으로 계산됩니다.
      Object.defineProperty(this, "age", {
        get() {
          let todayYear = new Date().getFullYear();
          return todayYear - this.birthday.getFullYear();
        }
      });
    }
    
    let john = new User("John", new Date(1992, 6, 1));
    
    alert( john.birthday ); // birthday를 사용할 수 있습니다.
    alert( john.age );      // age 역시 사용할 수 있습니다.
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

- cf. 프로퍼티 플래그와 설명자에 따라 수정 가능 여부는 달라질 수 있음(1.6절)
