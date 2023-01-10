# 아이템 7. 타입이 값들의 집합이라고 생각하기

* 컴파일 과정에서 각 변수들은 타입을 가지고 있다.
    * 타입: `할당 가능한 값의 집합`
    * 가장 작은 집합은 'never' type. 즉, 공집합


* never
```typescript
// never type은 아무 값도 할당할 수 없다.
const x: never = 12;
```

* literal type
```typescript
// 한 가지 값만 포함
type A = 'A';
type B = 'B';
```

* union type > 합집합
```typescript
// 두 세가지 값
type AB = 'A' | 'B'
```

## Intersection
* & 연산자 `intersection`
* typescript의 intersection은 다른 타입을 확장해서 새로운 타입을 만든다.
* interface extends와 유사

```typescript
interface Person {
    name: string;
}

interface Lifespan {
    birth: Date;
    death?: Date;
}

// 아래 두 가지는 동일하다.
type PersonSpan = Person & Lifespan;
interface PersonSpan {
    name: string;
    birth: Date;
    death?: Date;
}
```

```typescript
type K = keyof (Person | Lifespan) // type은 never
```
* Person, Lifespan에 해당하는 union 값은 어떠한 key도 가지고 있지 않다.

* interface에서 extends 키워드는 상속의 의미
* 제너릭 타입에서 한정자로도 사용되며, ~의 부분집합의 의미를 가진다.
```typescript
<K extends string> // K는 string의 부분집합
```

```typescript
interface Point {
    x: number;
    y: number;
}

type PointKeys = keyof Point; // 'x' | 'y'

function sortBy<K extends keyof T, T>(vals: T[], key: K): T[] {
    return 
}

// Point가 T에 할당 > T의 keyof는 Point의 key 집합
const pts: Point[] = [{ x: 1, y: 2}, { x: 2, y: 0 }];
sortBy(pts, 'x'); // 'x'는 'x' | 'y' 상속
sortBy(pts, 'y'); // 'y'는 'x' | 'y' 상속
sortBy(pts, 'z'); // 'z'는 'x' | 'y'를 상속하지 않는다.
```

* 배열과 튜플의 관계
```typescript
// X
const list = [1, 2] // number[]
const tuple: [number, number] = list;

// O
const list: [number, number] = [1, 2];
const array: number[] = list;
```
* `[], [1], [2], [1, 2]` 은 number[]의 부분 집합
* 하지만 `[], [1], [2]`는 [number, number]의 부분 집합이 아니기 때문에 tuple에 number[] type을 할당할 수 없다.
* 반대의 경우는 가능

```typescript
const triple: [number, number, number] = [1, 2, 3];

// 할당 불가
const double: [number, number] = triple;
```
* typescript는 해당 타입을 `{ 0: number, 1: number, length: 2 }` 로 모델링
* length 값이 맞지 않기 때문에 할당 불가하다.