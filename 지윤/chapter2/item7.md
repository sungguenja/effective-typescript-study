#### 아이템7. 타입이 값들의 집합이라고 생각하기

타입은 `할당 가능한 값들의 집합`이다.

유닛 타입 (=한 가지 값만 포함하는 타입)
```typescript
// 유닛 타입
type Dog = 'dog'
type Cat = 'cat'
```

유니언 타입 
```typescript
// 유니언 타입
type Animal = 'dog' | 'cat'
type Color = 'blue' | 'green'

interface Mom {
  name: string,
  gender: 'female',
  age: number,
}

interface Dad {
  name: string,
  gender: 'male',
  hobby: string,
}

type Parents = Mom | Dad
const mom : Parents = {name: 'abc', gender: 'female', age: 20 }
```
타입 체커는 유니언 타입을 검사 시에, 하나의 집합이 다른 집합의 부분 집합인지 검사한다.

인터섹션 타입
```typescript
// 인터섹션 타입
interface Dog  {
  eat: () => void;
}

interface Cat {
  sleep: () => void
}

type Animal = Dog & Cat;
const puppy: Animal = {
  eat: () => console.log('eat'),
  sleep: () => console.log('sleep')
}
```

