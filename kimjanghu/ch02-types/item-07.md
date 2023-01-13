# 아이템 7. 타입이 값들의 집합이라고 생각하기

* 컴파일 과정에서 각 변수들은 타입을 가지고 있다.
    * 타입: `할당 가능한 값의 집합`
    * 가장 작은 집합은 'never' type. 즉, 공집합

* never
```typescript
// never type은 아무 값도 할당할 수 없다.
const x: never = 12;
```

* literal(unit) type
```typescript
// 한 가지 값만 포함
type A = 'A';
type B = 'B';
```

* union type > 합집합
    * or 의 의미라고 생각 (교집합, 합집합의 개념은 사실 와닿지는 않음)
```typescript
// 두 세가지 값
type AB = 'A' | 'B'
```

```ts
const a: AB = 'A' // 'A'는 {'A', 'B'}의 원소이므로 할당 가능
```

## Intersection
* & 연산자 `intersection`
* typescript의 intersection은 다른 타입을 확장해서 새로운 타입을 만든다.
* interface extends와 유사
* interface에서 extends 키워드는 상속의 의미
    * 제너릭 타입에서 한정자로도 사용되며, ~의 부분집합의 의미를 가진다.

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
interface PersonSpan extends Person{
    birth: Date;
    death?: Date;
}
```
```typescript
<K extends string> // K는 string의 부분집합
```

* Person, Lifespan에 해당하는 union 값은 어떠한 key도 가지고 있지 않다.
```ts
type K = keyof (Person | Lifespan) // type은 never
type PL = keyof (Person & Lifespan) // type은 name | keyof Lifespan
```
* keyof (A | B) > keyof A & keyof B
* keyof (A & B) > keyof A | keyof B

```ts
interface Person {
    name: string;
}

interface Lifespan {
    name: string;
    birth: Date;
}

type K = keyof (Person | Lifespan); // type은 name
```

* 할당할 수 없다 > 상속할 수 없다
    * ~의 부분 집합에 해당하지 않는다의 의미
    * 할당과 상속의 관점에서 타입을 파악해보자
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


## 추가
* Object의 typeof
    * https://stackoverflow.com/questions/51808160/keyof-inferring-string-number-when-key-is-only-a-string
    * 자바스크립트는 object를 인덱싱할 때 number를 string으로 바꾼다.
```ts
type Mapish = { [k: string]: boolean };
type M = keyof Mapish; // type M = string | number
```
