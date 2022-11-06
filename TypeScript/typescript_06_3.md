## 6. 탈출구

- 타입을 완벽하게 지정하지 않고도 어떤 작업이 안전하다는 사실을 개발자는 알고 있음
- 타입스크립트에게 이를 알리고 싶을 때 활용할 수 있는 탈출구가 있음

### 6.1 타입 assertion

- 어떤 타입은 그 타입의 슈퍼타입 또는 서브타입이라고 assertion 할 수 있음
    - number 와 string은 서로 관련이 없으므로 어서션할 수 없음
- 두 가지의 타입 assertion 문법
    - `as` 키워드 이용(권장)
    - `<Type>` 이용
    
    ```tsx
    function formatInput(input: string) { ... }
    
    function getUserInput(): string | number { ... }
    
    let input = getUserInput();
    
    // 첫 번째 방법
    formatInput(input as string);
    
    // 두 번째 방법
    formatInput(<string>input);
    ```
    

### 6.2 Nonnull assertion

- 널이 될 수 있는 특별한 상황을 대비해 어떤 값이 타입이 null이나 undefined가 아니라 T임을 단언하는 특수 문법을 제공
- `!` 이용
    
    ```tsx
    type Dialog = { id?: string };
    
    function closeDialog(dialog: Dialog) {
        if(!dialog) return;
    
        setTimeout(() => 
            removeFromDom(
                dialog,
                document.getElementById(dialog.id!)! // assertion
            )
        );
    }
    
    function removeFromDom(dialog: Dialog, element: Element) {
        element.parentNode!.removeChild(element); // assertion
        delete dialog.id;
    }
    ```
    
- 너무 많은  assertion은 리팩토링의 징후일 수 있음
- union 이용하여 리팩토링 (*)
    
    ```tsx
    type VisibleDialog = { id: string };
    type DestroyedDialog = { };
    type Dialog = VisibleDialog | DestroyedDialog; // (*)
    
    function closeDialog(dialog: Dialog) {
        if(!('id' in dialog)) return;
    
        setTimeout(() => 
            removeFromDom(
                dialog,
                document.getElementById(dialog.id)! // assertion 줄임
            )
        );
    }
    
    function removeFromDom(dialog: VisibleDialog, element: Element) {
        element.parentNode!.removeChild(element); // assertion
        delete dialog.id;
    }
    ```
    

### 6.3 확실한 할당 assertion

- `!` 이용

```tsx
function fetchUser() {
	userId = globalCache.get('userId');
}

let userId!: string; // assertion
fetchUser();
userId.toUpperCase(); // assertion 하지 않으면 에러
```

## 7. 이름기반 타입 흉내내기

- 타입스크립트는 구조기반 타입이지만 이름기반 타입도 유용할 때가 있음
- 타입 브랜딩이라는 기법으로 이름기반 타입을 흉내낼 수 있음
- 구조기반 타입의 문제 상황
    - `UserID` 은 타입별칭일 뿐 근본은 string이기 때문
    
    ```tsx
    type CompanyID = string;
    type OrderID = string;
    type UserID = string;
    type ID = CompanyID | OrderID | UserID;
    
    function queryForUser(id: UserID) { ... }
    
    let id: CompanyID = 'b0314';
    queryForUser(id); // OK, 그러면 안되는데..
    ```
    
- 타입 브랜딩
    - 브랜드는 컴파일 타임에만 쓰이는 구조물로 런타임 오버헤드가 거의 없음
    - 대부분의 프로그램에 사용하기에는 과할 수 있으며 규모가 큰 응용프로그램, 다양한 종류의 ID를 사용해 타입이 헷갈리는 경우 브랜디드 타입 사용을 권장함
    
    ```tsx
    type CompanyID = string & { readonly brand: unique symbol };
    type OrderID = string & { readonly brand: unique symbol };
    type UserID = string & { readonly brand: unique symbol };
    type ID = CompanyID | OrderID | UserID;
    
    function CompanyID(id: string) { return id as CompanyID; }
    function OrderID(id: string) { return id as OrderID; }
    function UserID(id: string) { return id as UserID; }
    
    function queryForUser(id: UserID) { ... }
    
    let id: CompanyID = CompanyID('b0314');
    queryForUser(id); // TS2345
    ```
    

## 8. 프로토타입 안전하게 확장하기

- 자바스크립트는 동적언어이므로 모든 내장객체 프로토타입에 접근할 수 있음
- 타입스크립트의 정적타입 시스템을 이용하면 안전하게 프로토타입을 확장할 수 있음
