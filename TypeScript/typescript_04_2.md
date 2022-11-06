# 함수
### 4.2 다형성과 제네릭

- 구체타입(Concrete type)
    - 기대하는 타입을 정확하게 알고 있고 실제 이 타입이 전달되었는지 확인할 때 유용
    - 때로는 어떤 타입을 사용할지 미리 알 수 없는 상황이 있음
- 제네릭
    - 여러 장소에 타입 수준의 제한을 적용할 때 사용하는 플레이스홀더 타입
    - 다형성 타입 매개변수(polymorphic type parameter)라고도 부름
    
    ```tsx
    type Filter = {
    	<T>(array: T[], f: (item: T) => boolean): T[]
    };
    
    const filter: Filter = (array, f) => {
    	let result = [];
    	for (let i=0;i<array.length;i++) {
    		const item = array[i];
    		if(f(item)) result.push(item);
    	}
    	return result;
    }
    
    console.log(filter([1,2,3,4], val => val < 3)); // [1,2]
    console.log(filter(['apple','banana','pineapple'], str => str.indexOf('apple') >= 0)); //['apple','pineapple']
    ```
    
    - 동작
        - 타입스크립트는 전달된 array의 타입을 보고 T 의 타입을 추론함
        - filter 함수를 호출한 시점에 타입스크립트가 T의 타입을 추론한 타입으로 대체함
        - 타입 검사기가 문맥을 보고 이 플레이스홀더 타입을 실제 타입으로 채움
    - 언제 제네릭 타입이 한정되는가?
        - 제네릭 타입의 선언 위치에 따라 다름
        - 보통 다음과 같이 사용함
        
        ```tsx
        type Filter<T> = {
        	(array: T[], f: (item: T) => boolean): T[]
        };
        
        type StringFilter = Filter<string>;
        
        const filter: StringFilter = (array, f) => {
        	let result = [];
        	for (let i=0;i<array.length;i++) {
        		const item = array[i];
        		if(f(item)) result.push(item);
        	}
        	return result;
        }
        ```
        
    - 제네릭 타입 추론
        - 제네릭 타입은 추론되지만 명시할때는 모든 제네릭을 명시하거나 모두 명시하지 않아야 함
    - 제네릭 타입 별칭
        - 타입별칭과 같이 반복적으로 사용되는 경우 별칭을 사용할 수 있음
    - 한정된 다형성
        - 이진트리 예
        - 두가지 타입
            1. 말단노드 : 말단 노드여부(isLeaf)
            2. 말단이 아닌 노드: 자식노드(children)
            3. 공통 : 값(value)
            ```tsx
            type TreeNode = { value: string };
            type LeafNode = TreeNode & { isLeaf: true };
            type InnerNode = TreeNode & { children: [TreeNode] | [TreeNode, TreeNode] } // 한 개나 두 개의 자식을 가리킬 수 있음(튜플 문법)
            
            function mapNode<T extends TreeNode>(
              node: T, (**)
              f: (value: string) => string
            ): T {
              return {
                ...node,
                value: f(node.value) //(*)
              };
            }
            
            let a: TreeNode = {value: 'a'};
            let b: LeafNode = {value: 'b', isLeaf: true};
            let c: InnerNode = {value: 'c', children: [b]};

            let a1 = mapNode(a, n => n.toUpperCase());
            let b1 = mapNode(b, n => n.toUpperCase());
            let c1 = mapNode(c, n => n.toUpperCase());

            console.log(a1);
            console.log(b1);
            console.log(c1);
            ```
            * 그냥 T 라고 쓰면 `(*)` 에서 컴파일 오류
            * T 대신 TreeNode 라고 쓰면 `b1.isLeaf` 나 `c1.children`에서 컴파일 오류(`(**)`에서 타입정보 날아가고 그냥 TreeNode로 매핑되어서)
         
    - 제네릭타입 기본값
        - 기본값을 사용할 수 있음

### 4.3 타입 주도 개발

- 타입 시그니처를 먼저 정하고 값을 나중에 채우는 프로그래밍 방식
- 표현식이 수용할 수 있는 값의 타입을 제한하는 것이 핵심
- 표현력이 높은 타입시스템을 함수에 적용하면 함수 타입 시그니처를 통해 함수관련 많은 정보를 얻을 수 있음
