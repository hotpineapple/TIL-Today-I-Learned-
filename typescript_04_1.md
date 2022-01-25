## 4. 함수

### 4.1 함수 선언과 호출

- 자바스크립트에서 함수는 일급객체
    - 함수를 변수에 할당 가능
    - 함수를 다른 함수에 전달 가능
    - 함수에서 함수를 반환 가능
    - 객체와 프로토타입에 할당 가능
    - 함수에 프로퍼티를 기록 가능
    - 함수에 기록된 프로퍼티를 읽기 가능
- 함수 선언 방법 (안전하지 않는 방법 제외)
    - 매개변수는 명시적으로 정의해야 함
    - 반환타입은 타입스크립트가 자동으로 추론하지만 원한다면 명시 가능함
    
    ```tsx
    function greet (name: string) {
    	return 'hello' + name;
    }
    
    let greet2 = function (name: string) {
    	return 'hello' + name;
    }
    
    let greet3 = (name: string) => {
    	return 'hello' + name;
    }
    
    let greet4 = (name: string) => ('hello' + name);
    ```
    
- 함수 호출
    - 호출할 때 전달하는 인수의 타입 정보는 따로 제공할 필요 없음
    - 타입스크립트가 함수의 매개변수와 인수의 (개수 및) 타입이 호환되는지 확인함
- 선택적 매개변수, 기본 매개변수
    - ? 를 이용해 선택적 매개변수를 지정함
    - 함수 선언 시 필수 매개변수를 먼저 지정하고 선택적 매개변수를 뒤에 추가함
    - 매개변수에 기본값을 지정하면 의미상 선택적 매개변수와 같음
        - 그러나 기본 매개변수는 순서에 구애받지 않음
- 나머지 매개변수
    - 인수를 여러 개 받는 함수라면 그 목록을 배열 형태로 건낼 수 있음
    
    ```tsx
    function sum (numbers: number[]) {
    	return numbers.reduce((total, n) => total + n, 0);
    }
    
    sum([1,2,3]); // 6으로 평가
    ```
    
    - 그런데 때로는 인수의 개수가 고정되지 않고 인수의 개수가 달라질 수 있음
    - arguments 객체
        - 자바스크립트 런타임이 함수에 자동으로 arguments를 정의해 개발자가 함수로 전달한 인수 목록을 할당함
        - 타입스크립트에서 안전하지 않은 방법
            - 매개변수 타입을 명시하지 않아서 any로 추론함
            - 매개변수를 명시하지 않아서 `0개의 인수가 필요한데 n개의 인수가 제공됨` 에러 발생
        
        ```tsx
        function sumVariadic() : number {
        	return Array
        			.from(arguments)
        			.reduce((total, n) => total + n, 0);
        }
        
        sumVariadicSafe(); // 0
        sumVariadic(1,2,3); // TS2554: ...
        ```
        
        - 타입스크립트에서 안전한 방법
            - 나머지 매개변수 이용
            - 함수는 최대 한 개의 나머지 매개변수를 가질 수 있음
            - 나머지 매개변수는 함수의 매개변수 목록 중 맨 마지막에 위치해야 함
        
        ```tsx
        function sumVariadicSafe(...numbers: number[]) : number {
        	return Array
        			.from(arguments)
        			.reduce((total, n) => total + n, 0);
        }
        sumVariadicSafe(); // 0
        sumVariadicSafe(1,2,3); // 6
        sumVariadicSafe(1,2,3,4) // 10
        ```
        
- 함수를 호출하는 다른 방법 : call, apply, bind
    - call : **함수 안에서 this값을 한정**하고 다음 인수들을 순서대로 함수에 매개별수로 전달
    - apply : **함수 안에서 this값을 한정**하고 두 번째 인수를 펼쳐 함수에 매개변수로 전달
    - bind : call과 비슷하나 새로운 함수를 반환하므로 따로 호출해야 함
    
    ```tsx
    function add(a: number, b: number) {
    	return a + b;
    }
    
    // 아래 결과는 모두 30
    add(10, 20);
    add.apply(null, [10, 20]);
    add.call(null, 10, 20);
    add.bind(null, 10, 20)();
    ```
    
    - this 를 한정한다는 것을 더 잘 보이는 예: (바로 아래에서 더 자세히 살펴봄)
    
    ```tsx
    const person = {
        name: 'jane'
    };
    
    function greet() {
    	console.log(this); // { name: 'jane' }
    	console.log(`hello, ${this.name}`); // hello, jane
    }
    
    greet.call(person);
    ```
    
- this 의 타입
    - 자바스크립트에서 this 변수가 클래스에 속한 메서드 뿐 아니라 모든 함수에서 정의됨
    - this 의 값은 함수를 어떻게 호출했는지에 따라 달라짐
    - 자바스크립트의 this는 복잡한데, 타입스크립트가 이 문제를 해결가능함
    - 함수에서 this를 사용할 때 기대하는 this의 타입을 함수의 첫 번째 매개변수로 선언한다
    
    ```tsx
    function fancyDate(this: Date) {
    	return ${this.getDate()}/${this.getMonth()}/${this.getFullYear()};
    }
    
    fancyDate.call(new Date); // OK
    fancyDate(); // TS2684: void타입의 this를 메서드에 속한 Date 타입의 this에 할당할 수 없음
    ```
    
- 제너레이터 함수(generator) 와 반복자(iterator)
    - 별도의 파일로 정리 예정
- 호출 시그니처(=타입 시그니처)
    - 함수의 전체 타입을 표현하는 방법
        
        ```tsx
        function sum(a: number, b: number) {
        	return a + b;
        }
        ```
        
        - `Function` : 모든 함수의 타입을 뜻할 뿐이며 특정 함수의 타입과 관련되 정보는 없음
        - `(a: number, b: number) => number` : 호출 시그니처
            - 타입 수준 코드
            - 매개변수 기본값은 표현 불가
            - 바디를 포함하지 않아 반환타입 추론 불가하므로 명시해야 함
            
            ```tsx
            type Log = (message: string, userId?: string) => void
            
            let log: Log = ( // 호출시그니처 사용했으므로 반환타입 명시 안해도 됨
            	message, // 매개변수 타입 명시할 필요 없음: 문맥적 타입화
            	userId = 'Not signed in' // 기본값 지정은 여기서 해야함
            ) => {
            	let time = new Date().toISOString();
            	console.log(time, message, userId);
            }
            ```
            
- 문맥적 타입화의 다른 예
    - 콜백함수
        
        ```tsx
        // f를 n 번 호출하는 times 함수
        function times(
        	f: (index: number) => void,
        	n: number
        ) {
        	for(let i=0;i<n;i++) {
        		f(i);
        	}
        }
        
        // OK
        // 인수로 전달하는 n의 타입을 명시할 필요가 없다
        //(times 함수의 호출 시그니처로부터 문맥상 추론)
        times(n => console.log(n), 4); 
        
        // NOT OK
        function foo(n) { // n은 any타입으로 추론됨
        	console.log(n);
        }
        
        times(foo, 4); // TS에러
        ```
        
- 오버로드된 함수 타입
    - 호출 시그니처가 여러개인 함수
    - 단축형 호출 시그니처의 더욱 명확한 표현이 있음 = 전체 시그니처
    - 전체 시그니처
        - 특히 함수 타입을 오버로딩할 때 유용하게 사용가능함
        - 함수의 프로퍼티를 만들 때도 사용 가능함
    
    ```tsx
    type Reserve = {
    	(from: Date, to: Date, destination: string): Reservation;
    	(from: Date, destination: string): Reservation
    }
    
    let reserve: Reserve = (
    	from: Date, 
    	toOrDestination : Date | string, 
    	destination?: string
    ) => {
    	if (toOrDestination instanceof Date && destination !== undefined) {
    		// 왕복여행 예약
    	} else if (typeof toOrDestination === 'string') {
    		// 편도여행 예약
    	}
    }
    ```
