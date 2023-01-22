#### 아이템11. 잉여 속성 체크의 한계 인지하기

타입스크립트는 인터페이스에 명시된 타입 외의 추가적인 속성(=잉여 속성)이 있는 지 확인한다. 하지만 이런 잉여 속성 체크가 항상 보장되는 것은 아니고 구조적 할당 가능성 체크로 인해 잉여 속성이 있더라도 에러가 나지 않을 때도 있다. 

잉여 속성 체크는 객체 리터럴을 변수에 할당하거나, 함수에 매개변수로 전달할 때 잉여 속성 체크가 실행된다. 구조적 할당 가능성 체크로 인해 임시 변수를 값으로 할당 시에 잉여 속성 체크를 건너뛴다.
```typescript
interface Fruits {
  name: string;
  price: number;
}

// 잉여 속성 체크 후 에러 - Error: Type '{ name: string; price: number; color: string; }' is not assignable to type 'Fruits'. Object literal may only specify known properties, and 'color' does not exist in type 'Fruits'.
const redApple: Fruits = {
  name: 'apple',
  price: 1000,
  color: 'red'
};

const obj = {
  name: 'apple',
  price: 1000,
  color: 'green'
};

// 임시 변수 obj, 잉여 속성이 있어도 Fruits의 속성을 포함하는 superset - 할당 가능하므로 에러 안남
// 잉여 속성 체크와 할당 가능 검사와는 별도의 과정이다.
const apple: Fruits = obj;
```

잉여 속성 체크가 실행되지 않는 케이스
```typescript
interface Person {
  name: string;
  age: number;
}
// 변수에 할당되는 값이 객체 리터럴이 아닐 때(=임시변수가 값으로 할당될 때)
const student = {name: 'bob', age: 20, score: 98}
const a: Person = student;

// 타입 단언문을 사용할 때
const b = {name: 'lisa', age: 21, score: 99} as Person

// 인덱스 시그니처를 사용할 때
interface Person {
  name: string;
  [another: string]: unknown;
}
```

