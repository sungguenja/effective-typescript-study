#### 아이템26. 타입 추론에 문맥이 어떻게 사용되는지 이해하기

타입스크립트는 타입 추론 시에 값만 보는 것이 아니라 문맥을 살펴 타입을 추론한다.
```typescript
export enum ReviewStatus {
  임시저장 = 'DRAFT',
  검토중 = 'IN_REVIEW',
  반려됨 = 'DECLINED',
  승인완료 = 'ACCEPTED',
}

const getReviewStatusLabel = (status: ReviewStatus) => {)
  return ...
};

// status에 ReviewStatus 타입이 아닌 다른 타입이 오면 에러가 난다.
let a = 'DRAFT'
getReviewStatusLabel(a) // Error: Argument of type 'string' is not assignable to parameter of type 'ReviewStatus'

// 해결방법1 - 타입 선언
let a: ReviewStatus =  'DRAFT'
getReviewStatusLabel(a)

// 해결방법2 - const 사용
const a = 'DRAFT'
getReviewStatusLabel(a)
```

문맥과 값을 분리하여 사용할 때에, 문맥의 소실로 인해 오류가 발생할 수 있다.
```typescript
// 문맥 소실 오류 발생1 - 튜플 사용시
function duet(pair: [User,User]) {...}
duet([peter, bob]) 
                                  
const pair = [peter, bob] // const pair: User[]
duet(pair) 

// 해결 방법1 - 타입 선언
const pair: [User, User] = [peter, bob]
duet(pair) 
                                  
// 애매한 해결방법 - as const 달아주기 - 단, as const를 사용하면 readonly로 가장 좁은 타입으로 추론되기에, 매개변수에도 readonly를 달아주어야 한다. 이 방법은 변수가 상수일 경우에 사용할 것.
function duet(pair: readonly [User,User]) {...}
const pair = [peter, bob]  as  const // const pair: readonly [User, User]
duet(pair) 

// 문맥 소실 오류 발생2 - 객체 사용시 
// 프로퍼티로 리터럴이나 튜플을 사용할 경우 위와 동일 오류 발생, 해결법도 위와 동일하다. 타입 선언을 해주거나, as const로 상수 단언을 하거나.

// 문맥 소실 오류 발생3 - 콜백 사용시
const addNumber = (a, b) => a+b // a와 b는 any로 나타난다. 

// 해결 방법 - 타입 구문 추가
const addNumber = (a: number, b: number) => a+b 
```

