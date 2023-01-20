# item-15. 동적 데이터에 인덱스 시그니처 사용하기

## 15-1. 인덱스 시그니처
* 타입에 인덱스 시그니처를 명시하면 유연한 타입 매핑이 가능하다.

```ts
type UserInfo = { [info: string]: string; };

const user: UserInfo = {
    name: 'Jerry',
    hoddy: 'Game'
}

```

* `info` > key name
    * key의 위치 지정
* `info: string` > key type
    * key type은 `string`, `number`, `symbol` 의 조합
* `[info: string]: string` > value type

## 15-2. 인덱스 시그니처의 단점
* 잘못된 key를 포함한 모든 key를 허용
* 특정 key가 필요 없다.
    * `{}`
* key마다 다른 타입을 가질 수 없다.
    * string 이라고 선언하면 string만 가능
* key에 제한이 없기 때문에, 자동 완성 기능 불가능

```ts
interface UserInfo {
    name: string;
    hobby: string;
}

const user: UserInfo = {
    name: 'Jerry',
    hoddy: 'Game'
}
```
* 타입 선언을 통해 user를 정의하는 것이 더 정확한 방법이다.

## 15-3. 인덱스 시그니처 활용
* 동적 데이터에서 활용
    * csv 파일 `column` 등 미리 알 수 없는 값들이 있는 경우
    * 만약 미리 `column` 을 알고 있는 경우 타입을 선언하여 단언문 사용

```ts
function parseCSV(input: string): { [column: string]: string; }[] {
    const lines = input.split('\n');
    const [header, ...rows] = lines;
    const headerColumns = header.split(',');

    return rows.map((rowName) => {
        const row: { [column: string]: string; } = {};
        rowName.split(',').forEach((cell, i) => {
            row[headercolumns[i]] = cell;
        })
    })
}
```

* `column` 을 알고 있는 경우
```ts
interface ProductRow {
    productId: string;
    name: string;
    price: string;
}

declare let csvData: string;

const products = parseCSV(csvData) as unknown as ProductRow[];
```

* 런타임 환경에서 선언한 타입이 모든 열에 맞는다는 보장은 없다.
    * `undefined` 를 활용하여 예외 처리

```ts
function safeParseCSV(input: string): { [column: string]: string | undefined }[] {
    return parseCSV(input);
}

const safeRows = safeParseCSV(csvData);

for (const row of safeRows) {
    if (!!row) {
        prices[row.productId] = Number(row.price);
    }
}
```

* associative array
    * Object literal로 이해
    * 인덱스 시그니처 보다는 Map을 사용하는 것이 좋다.
    * Item 58에서 자세한 설명
        * prototype 과 동일한 key가 들어오는 경우 등 예상치 못한 문제가 발생할 수 있다.
        * Map을 활용하여 key, value 타입을 명확하게 정의 `new Map<string, string>`

```ts
const exampleAssociativeArray = {
    item1: 'value1',
    item2: 'value2'
}
```

## 15-4. 인덱스 시그니처 대안
* 가능한 필드가 제한된 경우 인덱스 시그니처를 활용하여 모델링하면 안된다.
    * key가 a, b, c, d로 제한

    ```ts
    interface Item {
        [key: string]: number
    }
    ```

    * 인덱스 시그니처를 활용하기에는 타입의 범위가 너무 넓다.
    * 그렇다고 가능한 부분 집합을 선택적 필드 또는 유니온으로 표현하는 것도 번거롭다.

1. `Record` 를 활용하면 key 타입에 유연성을 제공한다.
    * 인덱스 시그니처에 문자열을 할당하는 경우 에러가 발생한다.
```ts
type AppleProducts = { [name: 'IPad' | 'IPhone' | 'Mac']: number };
// An index signature parameter type cannot be a literal type or generic type. Consider using a mapped object type instead.
```
* 문자열로 이루어진 key를 사용할 수 있다.
```ts
type AppleProducts = 'IPad' | 'IPhone' | 'Mac';

type AppleProductsPrice = Record<AppleProducts, number>;

const appleProductsProce: AppleProductsPrice = {
    IPad: 1500000,
    IPhone: 1700000,
    Mac: 2500000
}
```
2. 매핑된 타입 사용
```ts
type ABC = {[k in 'a' | 'b' | 'c']: number};

type ABC = {
    'a': number;
    'b': number;
    'c': number;
}
```


* value 타입을 선택적으로 적용 가능
```ts
type ABC = {[k in 'a' | 'b' | 'c']: k extends 'b' ? string : number}

type ABC = {
    'a': number;
    'b': string;
    'c': number;
}
```

## Summary
* 인덱스 시그니처는 런타임 때까지 객체 key, value 등의 속성을 확실하게 알 수 없는 경우에만 활용한다.
* value 타입에 undefined를 추가하여 예외 처리하는 것도 방법이다.
* 가능한 interface, Record, 매핑된 타입 등의 정확한 타입을 선언해서 사용하자


## Reference
* [Declaration Reference](https://www.typescriptlang.org/docs/handbook/declaration-files/by-example.html)