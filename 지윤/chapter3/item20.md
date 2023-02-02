#### 아이템20. 다른 타입에는 다른 변수 사용하기

타입스크립트에서는 값이 할당된 변수에 있어서, 변수의 값이 바뀔 수는 있어도, 타입이 다른 값을 넣어 타입을 바꿀 순 없다.
```typescript
let id = 123 // 타입 추론. let id: string
id = '123' // Error: Type 'string' is not assignable to type 'number'.
```

만약, 변수에 다양한 타입의 값을 할당하고 싶다면 타입을 확장시켜주면 된다.
```typescript
let id : string | number = 123;
const getUser = (id: number) => {
  ...
}
id = '123'
const getProduct = (id: string) => {
  ...
}
```

위의 코드 처럼 하나의 변수에 타입을 확장시켜 다른 목적의 변수로 사용할 수는 있으나, 이는 지양해야할 코드 스타일이다. 목적이 다른 변수라면, 새로 변수를 선언하고 할당하는 것이 가독성도 좋고, 타입도 깔끔하고 여러모로 보기 좋다.
```typescript
const userId = 123;
getUser(userId)
  
const productId = '123'
getProduct(productId)
```

