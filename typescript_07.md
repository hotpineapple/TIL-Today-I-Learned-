# 7. 에러 처리

- 타입스크립트는 런타임에 발생할 수 있는 예외를 컴파일 타임에 잡을 수 있도록 최선을 다함
- 이런 목표 하에 강력한 정적, 기호적 분석을 수행하는 풍부한 타입 시스템을 도입함
- 그러나 어떤 언어를 사용하든 런타임 예외는 발생함
    - 네트워크 장애, 파일 시스템 장애, 사용자 입력 파싱 에러, 스택 오버플로, 메묄 부족 에러 등
- 타입 스크립트에서 에러를 표현하고 처리하는 4가지 패턴
    - null 반환
    - 예외 던지기
    - 예외 반환
    - Option 타입

## 1. null 반환

- 타입 안전성을 유지하면서 에러를 처리하는 가장 간단한 방법
- 문제가 생긴 원인을 알 수 없다는 단점

```tsx
function ask() {
	return propmt('When is your birthday?');
}

function parse(birthday: string): Date | null {
	let date = new Date(birthday);
	
	if(!isValid(date) return null;
	return date;
}

function isValid(date: Date): boolean {
	return Object.prototype.toString.call(date) === '[object Date]' && !Number.isNaN(date.getTime());
}

let date = parse(ask());

if(date) console.info('Date is', date.toISOString());
else console.error('Error parsing date for some reason');
```
