#### 아이템19. 추론 가능한 타입을 사용해 장황한 코드 방지하기

타입 추론이 되는 곳에, 명시적 타입 구문 작성을 지양하자. 이는 코드 가독성을 낮출 뿐더러, 리팩터링에도 방해가 된다. 

타입 명시가 아니라 추론되는 타입을 사용하면, 리팩터링이 용이하다. 변수에 타입을 명시해버리면, 추후 타입이 변경되었을 때 명시한 타입 또한 변경해줘야 하는 수고로움이 발생한다. 자동으로 추론되는 타입을 사용하며 타입 명시를 해놓지 않았다면, 코드를 수정할 필요가 없다.
```typescript
// 만약, id의 타입이 number에서 string으로 변경된 상황이라면 
interface User {
  id: string;
  name: string;
  age: number
}

// bad - 타입을 명시했을 때
const getUserId = (user: User) => {
  const userId: number = user.id; // Error: Type 'string' is not assignable to type 'number'.
}

// good - 비구조화 할당(destructuring assignment)
const getUserId = (user: User) => {
  const {id} = user; // 타입 추론. const id: string
}
```

DO 타입 명시 케이스
```typescript
interface Person {
  name: string;
  age: number;
}

// 객체 리터럴을 정의할 때 - 잉여 속성 체크를 사용하기 위해서
const p1: Person = {
  name: 'peter',
  Age: 20, // Error: Type '{ name: string; Age: string; }' is not assignable to type 'Person'. Object literal may only specify known properties, but 'Age' does not exist in type 'Person'. Did you mean to write 'age'?
}

// 함수의 매개 변수 및 반환 값에 타입 명시 - 함수의 시작에 타입 구문을 포함하고 함수 내 타입 구문 넣지 않는 것이 좋다.
const getUser = (users: User[], targetId: number): User => {
  return targetId // Error: Type 'number' is not assignable to type 'User'.
}

/**
 * 함수에 반환 타입을 명시하면 좋은 점
 * - 함수 구현부를 살펴보지 않아도 함수 선언부에서 함수의 입력과 출력에 대해 명확하게 알 수 있다.
 * - 반환된 값으로 따로 명시된 타입이 반환되는 경우, 꼭 명시해주자. 타입스크립트는 함수 반환 값을 추론해주지만, 명명된 이름을 알진 못한다.
*/
```

쫌쫌따리 꿀팁
* eslint 규칙 중 `no-inferrable-types` 를 사용하면 필요하지 않은 타입 구문을 걸러준다.