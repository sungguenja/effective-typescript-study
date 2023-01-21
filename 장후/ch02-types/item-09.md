# 아이템 9. 타입 단언보다는 타입 선언을 사용하기
* 타입 선언 방식 두 가지
* 첫 번째
    * 변수에 '타입 선언'
```typescript
interface Person { name: string };

const alice: Person = { name: 'Alice' };
```
* 두 번째
    * 변수에 '타입 단언'
```typescript
interface Person { name: string };

const bob = { name: 'Bob' } as Person;
```

* 타입 단언은 추론한 타입이 있다고 하더라도 Person 타입으로 간주한다.
* 즉, 타입에 대한 오류를 무시하라고 하는 것

* 화살표 함수에서의 타입 선언
```typescript
const people = ['a', 'b', 'c'].map((name) => ({ name } as Person))
```
* 단언하게 되면 오류가 뜨지 않는다.

```typescript
const people = ['a', 'b', 'c'].map((name): Person => ({ name })) // 타입은 Person[]
```

* `(name): Person` 은 name의 타입은 없고, return 타입이 Person
* `(name: Person)` 은 name의 타입은 Person이고, return 타입이 없음

* 체이닝의 경우, 체이닝 시작 지점부터 명시된 타입을 가져야한다.

## 타입 단언이 필요한 경우
* DOM 엘리먼트
    * 타입스크립트는 DOM에 접근할 수 없음
    * `!` 를 사용하여 null이 아님을 명시
    * 접두사로 쓰인 `!`는 boolean의 부정문
    * 접미사로 쓰인 `!`는 null이 아니라는 단언문
```ts
const elFoo = document.getElementById('foo'); // HTMLElement | null
const elFoo2 = document.getElementById('foo'); // HTMLElement
```
* 서브 타입인 경우의 타입단언은 가능하지만, 아닌 경우에는 타입 단언도 불가능하다.
```ts
interface Person { name: string };

const elFoo = document.getElementById('foo');
const elFoo2 = elFoo as Person; // 서브타입이 아니라 불가능
```
