# Item 13. 타입과 인터페이스의 차이점 알기

* 타입을 정의하는 방법은 type, interface 두 가지가 있다.
* 대부분의 경우에는 type, interface 두 가지 다 사용해도 된다.
* 하지만, 차이점은 존재하며 이 차이를 분명하게 파악하고 일관성 있게 사용하는 것이 중요하다.

## 13-1. 타입과 인터페이스 공통점

1. 변수에 TState, IState를 할당하는 경우 동일하게 타입이 적용
```ts
type TState = { name: string; };
interface IState { name: string; };

const invalidState: TState = {
    name: 'Jerry',
    age: 20 // > 할당 불가능
};
```

2. 인덱스 시그니처 활용 가능
```ts
type TIndexSignature = { [key: string]: string; };
interface IIndexSignature { [key: string]: string; };
```

3. 함수 타입 정의 가능
```ts
type TFunction = (x: number) => string;
interface IFunction { (x: number): string; }

const fnNumberToString: TFunction = (x) => String(x);
```

* 일반적으로는 type 선언이 나은 선택이지만, 추가 Property가 존재하는 경우 type, interface 둘 다 상관 없다.
    * 자바스크립트에서 함수는 Object이기 때문에 Property 할당이 가능하다.
```ts
type TFuntionWithProperties = {
    (x: number): number;
    prop: string;
}

interface IFuntionWithProperties {
    (x: number): number;
    prop: string;
}

// 이렇게 사용하는 케이스가 많을지...
const testFunction: IFuntionWithProperties = (x) => { return x; };
testFunction.prop = 'hihi';
```

4. 제너릭 가능
```ts
type TGeneric<T> = { first: T; };
interface IGeneric<T> { first: T; };
```

5. 확장 가능
* 인터페이스는 타입을 확장 가능
    * 유니온은 확장 불가
* 타입은 인터페이스를 확장 가능
```ts
interface IStateWithHobby extends TState {
    hobby: string;
};

type TStateWithHobby = IStates & { hobby: string; };
```

* 복잡한 타입을 확장하고 싶으면 타입 alias와 `&` 사용

6. 클래스 implements
* 타입 implements
```ts
class State implements TStateWithHoddy {
    name: 'Jerry';
    age: 20;
    hobby: 'Game';
}
```
* 인터페이스 implements
```ts
class State implements IStateWithHobby {
    name: 'Jerry';
    age: 20;
    hobby: 'Game';
}
```

## 13-2. 타입과 인터페이스 차이점
1. 유니온
* 유니온 타입은 가능
```ts
type AorB = 'a' | 'b';
```
* 인터페이스는 타입 확장은 가능하지만 유니온 확장은 불가능
* 유니온 타입 확장

```ts
type Input = { input: string };
type Output = { output: string };

interface VariableMap {
    [name: string]: Input | Output;
}

const variable: VariableMap = {
    jerry: { input: 'hihi' }
}

type NamedVariable = (Input | Output) & { name: string }; // Input & { name: string } or Output & { name: string }

const typeVariable: NamedVariable = { input: 'hihi', name: 'jerry' };
```

* 일반적으로 type은 interface보다 확장성이 좋다.
    * 유니온, 인터섹션, 매핑된 타입, 조건부 타입 등 활용 가능

2. 튜플과 배열 타입
```ts
type PairTuple = [number, number];
type PairArray = number[];
type NamedNums = [string, ...number[]];

const namedNums: NamedNums = ['Jerry', 1, 2, 3, 4];
```
* 인터페이스로 튜플 구현은 가능하지만 메서드는 사용 불가능하다.

```ts
interface Tuple {
    0: number;
    1: number;
    length: 2;
}

const interfaceTuple: Tuple = [10, 20];

console.log(interfaceTuple.length); // 가능 > (property) Tuple.length: 2
interfaceTuple.concat() // 불가능 > Property 'concat' does not exist on type 'Tuple'
```

3. 인터페이스는 augment 가능
* declaration merging (속성 확장)
```typescript
interface IState {
    name: string;
    age: number;
}

interface IState {
    hobby: string;
}

const info: IState = {
    name: 'Jerry',
    age: 20,
    hobby: 'sleep'
}
```
* declaration merging은 주로 타입 선언 파일에서 사용된다.
* 타입 선언 파일을 작성할 때는 declaration merging을 지원하기 위해 interface로 타입 선언

* Declaration Merging Example
    * Array type은 interface로 `lib.es5.d.ts` 에 정의 되어 있다.
    * `tsconfig.json` lib 목록에 `ES2015` 를 추가하면 타입스크립트는 `lib.es2015.d.ts` 에 선언된 인터페이스를 merge 한다.
    * 이러한 과정을 통해 각 interface declaration이 병합되어 Array는 es5, es2015 등에 적용된 모든 메서드를 가질 수 있다.

* 타입은 병합이 불가능하기 때문에 기존 타입에 추가 보강이 없는 경우에만 사용

## 13-3. 결론
* 복잡한 타입이다. > `type`
* API 변경 등 Declaration Merging 가능성이 있다. > `interface`
    * 하지만 프로젝트 내부에서만 사용하는 타입에 Declaretion Merging이 발생하는 것은 잘못된 설계
* 타입과 인터페이스 둘 다 표현 가능한 간단한 타입 > 일관성과 Merging의 관점

## Summary
* 인터페이스와 타입의 공통점, 차이점을 이해하자
* 한 타입을 type, interface 두 가지 문법을 사용해서 작성해보자
* 일관된 스타일을 유지하며, Declaration Merging 가능성을 고려하자