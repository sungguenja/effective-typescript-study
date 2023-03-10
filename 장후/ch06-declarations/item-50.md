# Item 50. 오버로딩 타입보다는 조건부 타입을 사용하기

타입스크립트에서는 동일한 함수 타입 선언을 여러개 할 수 있다. (구현채는 하나)

```ts
// 타입
function double(x: number): number;
function double(x: string): string;

// 구현체
function double(x: any) { return x + x; };

const num = double(12);
const str = double('x');
```

하지만, 만약 argument에 유니온 타입 `string | number` 가 들어오는 경우 에러가 발생한다.

`string | number` 가 반환되기를 기대하지만, 타입스크립트는 오버로딩된 함수 선언 중 동일한 타입을 찾을 때까지 검색한다.

따라서 `string | number` 을 반환하는 타입 선언을 찾지 못하기 때문에 오류가 발생한다.

타입 오버로딩보다 더 나은 해결책은 조건부 타입을 사용하는 것이다.

```ts
function double<T extends string | number>(x: T): T extends string ? string : number;

function double(x: any) { return x + x; };

function f(x: number | string) {
    return double(x);
}
```

* T가 string 부분 집합이면, 반환 타입이 string이고 그 외의 경우엔 number로 반환된다.

타입 오버로딩은 독립적으로 처리되지만, 조건부 타입은 단일 표현식으로 받아들여 유니온 문제를 해결 할 수 있다.
