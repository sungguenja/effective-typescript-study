#### 아이템 50. 오버로딩 타입보다는 조건부 타입을 사용하기

```typescript
// 오버로딩 타입 - 일치하는 타입을 찾을 때까지 오버로딩된 타입을 순차적으로 검색한다.
function double(x:number): number;
function double(x:string): string;
function double(x:any): {return x+x};

const num = double(2) // const num: number
const str = double('abc') // const str: string
```

오버로딩 대신, 조건부 타입을 사용하자

```typescript
function f(x: string | number) {
  return double(x); // error
}

// 해결방법1 - 오버로딩 타입 추가
function double(x: number | string): number | string;

// 해결방법2 - 조건부 타입으로 풀기
function double<T extends string | number>(
  x: T
): T extends string ? string : number;
```
