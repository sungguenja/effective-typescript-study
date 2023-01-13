#### 아이템9. 타입 단언보다는 타입 선언 사용하기

 `타입 선언`과 `타입 단언` 으로 변수에 타입을 명시할 수 있다.

```typescript
type Person = {
	name: string
}

// 타입 선언
const ellie: Person = {name: 'ellie'}
// 타입 단언
const bob = {name: 'bob'} as Person
```

결론을 먼저 말하자면, 타입 단언보다는 타입 선언을 사용하는 것이 좋다. 타입 선언은 타입스크립트의 타입 체커가 동작해 오류를 내뱉지만, 타입 단언은 타입 체커가 오류를 발견해도 무시하고 넘어가게 `타입을 강제`하기 때문이다.



**이유1**

타입이 선언된 변수는, 인터페이스에 명시된 속성이 없으면 타입 체커가 오류를 내뱉는다. 하지만 타입 단언으로 타입이 단언된 변수는,  명시된 인터페이스를 만족하지 못하더라도 오류가 잡히지 않는다.

```typescript
// 타입 선언 - 오류 있음
const ellie: Person = {} // Error: Property 'name' is missing in type '{}' but required in type 'Person'.

// 타입 단언 - 오류 없음
const bob = {name: 'bob'} as Person
```



**이유2**

타입 선언된 변수는, 인터페이스에 명시 된 속성 외의 또 다른 속성이 있다면 타입 체커가 오류를 내뱉는다. 하지만 타입 단언은 인터페이스에 속해있지 않은 속성을 마구 추가해도 단언된 타입이라 간주하기에 오류로 잡지 않는다.

```typescript
// 타입 선언 - 오류 있음(잉여 속성 체크)
const ellie: Person = {name: 'ellie', score: 32} // Type '{ name: string; score: number; }' is not assignable to type 'Person'. Object literal may only specify known properties, and 'score' does not exist in type 'Person'.

// 타입 단언 - 오류 없음
const bob = {name: 'bob', score: 21} as Person
```



화살표 함수를 사용할 때에 추론된 타입이 모호하여 화살표 함수로 할당된 변수의 타입이 원하는대로 추론되지 않을 수 있다. 이럴 때에는 화살표 함수의 반환 값에 타입을 선언하자.

```typescript
type Person = {
    name: string;
}
    
// const people: {name: string}[]
const people = ['alice', 'ellie', 'bob', 'peter'].map(name => ({name})) 

// const people: Person[] -> 오류 안남
const people = ['alice', 'ellie', 'bob', 'peter'].map(name => ({} as Person))

// const people: Person[]
const people = ['alice', 'ellie', 'bob', 'peter'].map((name):Person => ({name}))
```



~~함수 호출 체이닝이 연속되는 곳에서는 체이닝 시작 지점에서부터 명명된 타입을 가져야 한다? 예제 필요~~



그렇다면, 타입 단언이 꼭 필요한 때는 언제일까? 바로 DOM과 같이 타입 체커는 접근할 수 없어 개발자가 변수의 타입을 더 정확하게 알 수 있을 경우이다. 

혹은 접미사로 !를 붙여 해당 값이 반드시 있다라는 것을 단언할 때에 사용한다.

```typescript
const button = document.querySelector('#button') // Element | null 
const button = document.querySelector('#button')! // Element
```

타입 단언문을 사용하여 타입의 변환도 가능하다. 단, 단언하고자 하는 타입이 추론된 타입의 서브 타입인 경우에만 가능하다.

```typescript
const button = document.querySelector('#button') as HTMLElement // HTMLElement
const button = document.querySelector('#button') as HTMLButtonElement // HTMLButtonElement
```

추론된 타입과 다른 타입으로 변환을 원한다면 모든 타입이 속해있는 unknown을 사용하면 된다.

```typescript
const button = document.querySelector('#button') as unknown as string // string
```

