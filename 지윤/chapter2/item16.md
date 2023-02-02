#### 아이템16. number 인덱스 시그니처보다는 Array, 튜플, ArrayLike를 사용하자.

자바스크립트에서의 객체란, 키/값 쌍의 모음이다. 키는 보통 문자열이고, 값는 무엇이든지 될 수 있다. 복잡한 객체나, 숫자를 키로 사용하려하면, 자바스크립트는 해당 객체나 숫자를 문자열로 변환해 키로 사용한다. 배열 또한 객체이기에, 배열의 인덱스 또한 키라, 문자열로 취급된다. 
```javascript
const numbers = [1,2,3];
console.log(numbers['2']) // 3
console.log(Object.keys(numbers)) // ['0', '1', '2']
```

배열의 인덱스에 문자열이 들어가는타입스크립트는 숫자키를 허용한다. 런타임 시에 타입 정보가 제거되기에, 런타임에는 문자열로 변환되기는 하지만, 타입 체크 시점에 오류를 잡을 수 있어 유용하다.
```typescript
// ??? 하지만 에러가 안난다..? 왜지?
const numbers = [1,2,3]
const a = numbers['1']
console.log(numbers['1'])
```

배열을 순회할 때 인덱스 타입이 불확실한 것은 배열 순회의 속도를 늦춘다.
```typescript
// bad - 타입이 불확실해 속도가 느리다.
const numbers = [1,2,3]
const keys = Object.keys(numbers) // string[]
for (const key in numbers) { // numbers의 key들을 순회, key는 string이다.
  const target = numbers[key] // key는 string이나, 인덱스로 사용되고 있다.
}

// good - 인덱스가 필요 없다면
for (const x of numbers) {
  console.log(x) // numbers의 값들을 순회
}

// good - 인덱스가 필요 하다면
numbers.forEach((num, index) => {
  console.log(num) // numbers의 값들을 순회
  console.log(index) // number
})

// good - 순회 도중 멈추어야 한다면
for (let i = 0; i < numbers.length; i++) {
  if (i === 1) {
    break;
  }
  console.log(numbers[i]) // numbers의 값들을 순회
}
```

배열을 순회할 때, 타입이 불명확하다면, 속도가 확연하게 느려진다. 일반적으로  인덱스 시그니처로 string말고 number를 사용하는 경우는 드물다. 숫자로 인덱스 항목을 지정해야 한다면, Array 또는 Tuple 타입, 혹은 ArrayLike 타입을 사용하자.