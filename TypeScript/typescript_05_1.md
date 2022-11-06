## 5. 클래스와 인터페이스

### 5.1 기본

```tsx
// 체스게임을 위한 클래스...

type Color = 'Black' | 'White';
type MyFile = 'A' | 'B';
type Rank = 1 | 2;

class Position {
    constructor(
        private file: MyFile, // 멤버 변수 선언 없이 사용 가능한 타입스크립트 문법
        private rank: Rank
    ) {}
    distanceFrom(position: Position) {
        return {
            rank: Math.abs(position.rank - this.rank),
            file: Math.abs(position.file.charCodeAt(0) - this.file.charCodeAt(0))
        }
    }
}

abstract class Piece {
    protected position: Position;
    constructor(
        private readonly color: Color,
        file: MyFile,
        rank: Rank
    ) {
        this.position = new Position(file, rank);
    }
    moveTo(position: Position) {
        this.position = position;
    }
    abstract canMoveTo(position: Position): boolean;
}

// const piece = new Piece('Black','A',1); // ERROR : 추상클래스를 인스턴스화할 수 없음

class King extends Piece {
    canMoveTo(position: Position) {
        let distance = this.position.distanceFrom(position);
        return distance.rank < 2 && distance.file < 2;
    }

}

const blackKing = new King('Black','A',1);
console.log(blackKing);
const destination = new Position('B',2);
if(blackKing.canMoveTo(destination)) blackKing.moveTo(destination);
console.log(blackKing);
```

### 5.2 super

- 자식 클래스가 부모 클래스에 정의된 메서드를 오버라이드 하면 자식 인스턴스는 super를 이용해 부모 버전의 메서드를 호출할 수 있음
- 두 가지 경우
    - `super.xxx()` 와 같은 일반 메서드 호출
    - 생성자 함수에서만 호출할 수 있는 `super()`. 자식 클래스에 생성자 함수가 있다면 `super()`를 반드시 호출해야만 부모 클래스와 정상적으로 연결됨
- super 키워드를 이용하여 부모 클래스에 메서드에 접근할 수 있으나 (특히 private 접근제어자로 선언된) 프로퍼티에는 접근할 수 없음

### 5.3 this 를 반환타입으로 사용하기

- this 를 값 뿐 아니라 타입으로도 사용할 수 있음
- 클래스 메서드의 반환타입을 지정할 때 유용하게 활용가능함
- 예) Set 자료구조의 연산 구현하기
    
    ```tsx
    // 주어진 사용 예
    let set = new Set;
    set.add(1).add(2).add(3);
    console.log(set.has(2));
    
    // 구현
    class Set {
    	has(value: number) {
    		// ...
    	}
    	add(value: number): this { // add를 호출하면 Set 인스턴스를 반환함, 서브클래스에서 오버라이드 필요 없음
    		// ...
    	}
    }
    
    class MutableSet extends Set {
    	delete(): boolean {
    		// ...
    	}
    	// add(): MutableSet { ... } // 오버라이드하지 않아도 Set이 아닌 MutableSet 인스턴스를 반환함
    }
    ```
    
    - 연쇄(chained) API 에서 유용하게 사용 가능함

### 5.4 인터페이스

- 클래스는 인터페이스를 통해 사용할 때가 많음
- 인터페이스는 타입 별칭과 마찬가지로 타입에 이름을 붙여주는 수단
- 타입별칭과 인터페이스
    
    ```tsx
    // 타입 별칭
    type Food = {
    	calories: number;
    	tasty: boolean;
    }
    type Sushi = Food & {
    	salty: boolean;
    }
    type Cake = Food & {
    	sweet: boolean;
    }
    
    // 인터페이스
    interface Food {
    	calories: number;
    	tasty: boolean;
    }
    interface Sushi extends Food {
    	salty: boolean;
    }
    interface Cake extends Food {
    	sweet: boolean;
    }
    ```
    
    - 차이점
        - 타입별칭이 더 일반적임
            - 타입 별칭의 오른 편에는 타입표현식(타입, 타입연산자 등)을 표현한 모든 타입이 올 수 있음
            - 인터페이스의 오른편에는 반드시 형태가 나와야 함
            
            ```tsx
            // 아래 코드는 인터레이스로 작성할 수 없음 
            type A = number;
            type B = A | string;
            ```
            
        - 인터페이스를 상속할 때 타입스크립트는 상속받는 인터페이스의 타입에 상위 인터페이스를 할당할 수 있는지를 확인함
            - 인터페이스가 제공하는 할당성 확인 기능을 이용하면 쉽게 에러를 검출 가능
            
            ```tsx
            interface A {
            	good(x: number): string;
            	bad(x: number): string;
            }
            
            interface B extends A {
            	good(x: string | number): string; // OK
            	bad(x: string): string; //TS2430: 인터페이스 B는 인터페이스 A를 올바르게 상속받지 않음(number 타입은 string 타입에 할당할 수 없음)
            }
            
            // 이렇게 쓰고 싶다면 아래와 같이 고칠 수 있음
            type A = {
            	good(x: number): string;
            	bad(x: number): string;
            }
            
            type B = A & {
            	good(x: string | number): string; // OK
            	bad(x: string): string; // OK
            }
            
            const b: B = {
                good: (name) => 'good ' + name, 
                bad: (name) => 'bad ' + name
            };
            
            console.log(b.bad('zee')); // bad zee
            ```
            
        - 이름과 범위가 같은 인터페이스가 여러 개 있다면 이들이 자동으로 합쳐짐(=선언 합침, Declaration Merge)
            - 타입별칭의 경우  `TS2300: 중복된 식별자` 에러
            - 한 프로퍼티의 타입이 일치하지 않는 경우 `TS2428: XXX의 모든 선언은 같은 타입 매개변수를 가져야 함` 에러
            - 제네릭을 선언한 인터페이스의 경우 제네릭 선언 방법과 이름까지 똑같아야 합칠 수 있음
- implements(구현) 키워드
    - 클래스 선언 시 `implements` 키워드를 이용해 특정 인터페이스를 만족시킴을 표현 가능함
    - 구현에 문제가 있는 경우 쉽게 파악 가능
    - 어댑터, 팩토리, 전략 등 흔한 디자인 패턴을 구현하는 대표적인 방식
    
    ```tsx
    interface Animal {
    	readonly name: string;
    	eat(food: string): void;
    	sleep(hours: number): void;
    }
    
    interface Feline {
    	meow(): void;
    {
    
    class Cat implements Animal, Feline {
    	eat(food: string) { console.log('Ate some', food); }
    	sleep(hours: number) { console.log('Slept for ', hours, 'hours'); }
    	meow() { console.log('Meow~!'); }
    }
    ```
    
- 인터페이스 vs. 추상클래스
    - 인터페이스가 더 범용으로 쓰이고, 추상클래스는 특별한 목적과 풍부한 기능을 가짐
    - 인터페이스는 객체, 배열, 함수, 클래스, 클래스 인스턴스를 정의할 수 있음
    - 인터페이스는 아무런 자바스크립트 코드를 만들지 않으며 컴파일 타임에만 존재함
    - 추상 클래스는 오직 클래스만 정의할 수 있으며 런타임의 자바스크립트 클래스 코드를 만듦
    - ⇒ 여러 클래스에서 공유하는 구현이라면 추상 클래스를 사용하고 가볍게 이 클래스는 T다 라고 말하는 것이 목적이라면 인터페이스 사용을 권장
