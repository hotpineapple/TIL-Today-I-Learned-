# Chapter 6. 고급타입

## 2. 종합성

- 종합성(철저검사, exhaustive checking)
    - 타입 검사기는 필요한 모든 상황을 제대로 처리했는지 검사함
    - 패턴매칭을 사용하는 언어(하스켈, 오캐멀 등)에서 차용
    - 종합성 오류 해결방법
        - 모든 경우 추가하기
        - 반환타입에 undefined 추가
    
    ```tsx
    type Weekday = 'MON' | 'TUE' | 'WED' | 'THU' | 'FRI';
    type Day = Weekday |  'SAT' | 'SUN';
    
    function getNextDay(w: Weekday): Day {
       switch(w) {
    		case 'MON': return 'TUE'; // TS2366: 함수에 마무리 반환문이 없으며 반환타입은 undefined를 포함하지 않음
       }
    }
    
    // 해결 1
    function getNextDay(w: Weekday): Day {
       switch(w) {
    		case 'MON': return 'TUE'; 
    		case 'TUE': return 'WED';	
    		// ... 모든 경우 추가
       }
    }
    
    // 해결 2
    function getNextDay(w: Weekday): Day | undefined {
       switch(w) {
    		case 'MON': return 'TUE'; 
       }
    }
    ```
    

## 3. 고급 객체 타입

- 타입스크립트가 종합성 검사로 찾지 못하는 개발자의 실수를 찾아내도록 하기 위해 더 안전하게 타입을 선언할 수 있는 방법들
    - Record 타입
    - 매핑된 타입
    - 예시: **객체가 특정 키 집합을 정의하도록 강제하기**
    
    ```tsx
    // 옳은 예
    type Weekday = 'MON' | 'TUE' | 'WED' | 'THU' | 'FRI';
    type Day = Weekday | 'SAT' | 'SUN';
    
    // switch 대신 객체 검색을 사용하면  상수시간 소비
    let nextDay = {
       MON: 'TUE',
       TUE: 'WED',
       WED: 'THU',
       THU: 'FRI',
       FRI: 'SAT',
    };
    
    function getNextDay(w: Weekday): Day {
      return nextDay[w] as Day;
    };
    
    let nextDay = {
       MON: 'TUE' // OK, 아래 함수에서 nextDay.TUE에 접근(*) 하려고 해야 컴파일 에러가 발생함. nextDay 을 선언할 때부터 컴파일 에러를 발생시키려면? 
    };
    
    function getNextDay(w: Weekday): Day {
       return nextDay[w]; // (*)
    };
    ```
    
- Record 타입
    - 무언가를 매핑하는 용도로 객체를 활용할 수 있음
    - 객체의 키와 값에 타입을 제공(=제한)
    - 키 타입은 string, number 의 서브타입도 가능
    - cf. 인덱스 시그니처
        - 키 타입은 일반 string, number, symbol 만 가능
    
    ```tsx
    type Weekday = ‘MON’ | ‘TUE’ | ‘WED’ | ‘THU’ | ‘FRI’;
    type Day = Weekday | ‘SAT’ | ‘SUN’;
    
    function getNextDay(w: Weekday): Day {
       return nextDay[w]; // (*)
    }; 
    
    // Record 타입으로 한 주의 각 요일을 다음 요일로 매핑하기
    let nextDay: Record<Weekday, Day> = {
       MON: ‘TUE’ // TS2739: {MON: “TUE”} 타입에는 Record<Weekday,Day> 타입 중 TUE, WED, THU, FRI 가 빠져있음
    };
    ```
    
- 매핑된 타입
    - 객체의 키와 값에 타입을 제공하는 또 다른 방법
    - 타입스크립트만의 고유한 언어 기능
    - 한 객체 당 최대 한 개의 매핑된 타입을 가질 수 있음
    - Record 타입을 구현하는 데 이용됨
        
        ```tsx
        type Record<K extends keyof any, T> = { [key in K] : T };
        ```
        
    - Record 타입보다 강력함
        - 키인 타입과 조합하면 키 이름별로 매핑할 수 있는 값 타입을 제한 가능
    
    ```tsx
    type Weekday = ‘MON’ | ‘TUE’ | ‘WED’ | ‘THU’ | ‘FRI’;
    type Day = Weekday | ‘SAT’ | ‘SUN’;
    
    function getNextDay(w: Weekday): Day {
       return nextDay[w]; // (*)
    }; 
    
    // 매핑된 타입
    let nextDay: { [K in Weekday]: Day } = {
       MON: ‘TUE’ // TS2739: {MON: “TUE”} 타입에는 { MON:Weekday; TUE:Weekday; WED:Weekday; THU:Weekday; FRI:Weekday; } 타입이 정의한 프로퍼티 중 TUE, WED, THU, FRI 가 빠져있음
    };
    ```
    
    ```tsx
    type Account = {
       id: number;
       isEmployee: boolean;
       notes: string[];
    };
    
    type OptionalAccount = {[K in keyof Account]?: Account[K];};
    
    type NullableAccount = {[K in keyof Account]: Account[K] | null;};
    
    type ReadonlyAccount = {readonly [K in keyof Account]: Account[K];};
    
    type Account2 = {-readonly [K in keyof ReadonlyAccount]: Account[K]};
    
    type Account3 = {[K in keyof OptionalAccount]-?: Account[K];};
    ```
    
- 타입스크립트가 제공하는 내장 매핑 타입 (=유틸리티 타입)
    - Record<Keys,Values>
    - Partial<Object> : Object의 모든 필드를 선택형으로
    - Required<Object> : Object의 모든 필드를 필수형으로
    - Pick<Object,Keys> : 주어진 Keys 에 대응하는 Object의 서브타입
    - Readonly<Object> : Object의 모든 필드를 읽기전용으로
    - +) Omit<Object,Keys> 특정 속성만 제거한 타입
- 컴패니언 객체 패턴
    - 스칼라에서 유래
    - 같은 이름을 공유하는 객체와 클래스를 쌍으로 연결
    - 타입과 객체가 의미상 관련되어 있고 이 객체가 타입을 활용하는 유틸리티 메서드를 제공하는 경우 유용
    - 타입과 값 정보를 한 개의 이름으로 그룹화 하고 다른 파일에서 호출자가 한 번에 임포트할 수 있음
    - 예시 코드
    
    ```tsx
    // a.ts
    
    type Currency = {
       unit: 'EUR' | 'GBP' | 'JPY' | 'USD';
       value: number;
    };
    
    let Currency = {
       DEFAULT: 'USD',
       from(): Currency {
    	return { unit, value };
       }
    };
    ```
    
    ```tsx
    //b.ts
    
    import { Currency } from ‘./Currency’;
    let amountDue: Currency = {
       unit: 'JPY',
       value: 83733.10
    };
    
    let otherAmountDue = Currency.from(330, 'EUR');
    ```
    

## 4. 고급 함수 타입

- 튜플 타입 추론 개선
    - 타입스크립트는 튜플 선언에 관대함
        - 튜플 길이, 어떤 위치에 어떤 타입인지는 무시
    - 타입어서션이나 as const 를 이용해서 타입좁히기, 읽기전용 한정자를 적용하지 않고 (배열이 아닌) 튜플 타입으로 추론되기 위한 방법?
    - 나머지 매개변수의 타입을 추론하는 기법 이용
    
    ```tsx
    let a = [1, true]; // (number | boolean)[] 으로 추론
    let b = [1, true] as [number, boolean];
    let c = [1, true] as const; 
    
    function tuple<T extends unknown[]>(...ts: T): T {
       return ts;
    }
    
    let d = tuple(1, true); // [number, boolean] 으로 추론
    ```
    
- 사용자 정의 타입 안전장치
    - 타입 정제의 한계
    ```tsx
        function isString(a: unknown): boolean {
           return typeof a === 'string';
        }

        console.log(isString('a')); // true
        console.log(isString([7])); // false

        function parseInput(input: string | number) {
           let formattedInput: string;
           if(isString(input)) formattedInput = input.toUpperCase(); // Property 'toUpperCase' does not exist on type 'number'
        }

        // 해결1
        function parseInput(input: string | number) {
           let formattedInput: string;
           if(typeof input === ‘string’) formattedInput = input.toUpperCase(); 
        }

        // 해결2
        function isString(a: unknown): a is string {
           return typeof a === 'string';
        }

        console.log(isString('a')); // true
        console.log(isString([7])); // false

        function parseInput(input: string | number) {
           let formattedInput: string;
           if(isString(input)) formattedInput = input.toUpperCase();
        }
    ```

## 5. 조건부 타입

- 타입스크립트가 제공하는 독특한 기능
    - U 와 V타입에 의존하는 T 타입을 선언
        - U가 V와 같거나 서브타입이면 T 를 A에 할당
        - V가 U와 같거나 서브타입이면 T 를 B에 할당
    
    ```tsx
    type IsString<T> = T extends string ? true : false;
    
    type A = IsString<string>; // true 타입
    type B = IsString<number>; // false 타입
    ```
    
- 분배적 조건부
    - 조건부 타입은 분배법칙을 따름
- infer 키워드
    - 조건의 일부를 제네릭 타입으로 선언할 수 있음
    - 제네릭 타입을 인라인으로 선언하는 전용문법 `infer` 키워드
    
    ```tsx
    // U를 T와 함께 미리 선언하지 않음
    type ElementType<T> = T extends unknown[] ? T[number] : T;
    type A = ElementType<number[]> // number
    
    type ElementType2<T> = T extends (infer U)[] ? U : T;
    type B = ElementType2<number[]> // number
    
    // U를 T와 함께 미리 선언하는 경우
    type ElementUgly<T,U> = T extends U[] ? U : T
    type C = ElementUgly<number[]> // TS2314: 두 개의 타입인수를 필요로 함
    ```
    
    - 더 복잡한 예
    
    ```tsx
    type SecondArg<F> = F extends (a:any, b infer B) => any ? B : never;
    
    type F = typeof Array['prototype']['slice'];
    
    type A = SecondArg<F>; // number | undefined
    ```
    
- 내장 조건부 타입
    - 강력한 연산자 몇 가지를 타입 수준에서 표현 가능
    
    ```tsx
    type A = number | string;
    type B = string;
    type C = Exclude<A,B>; // number 
    
    type A = number | string;
    type B = string;
    type C = Extract<A,B>; // string 
    
    type A = { a?: number | null };
    type B = NonNullable<A['a']>; // number
    
    type F = (a: number) => string;
    type C = ReturnType<F>; // string
    
    type A = {new(): B};
    type B = {b: number}
    type C = InstanceType<A>; // {b: number}
    ```
