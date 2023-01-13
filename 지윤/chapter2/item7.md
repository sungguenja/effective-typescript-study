#### 아이템7. 타입이 값들의 집합이라고 생각하기

타입은 `할당 가능한 값들의 집합`이다.

* 42: number
* 'Canada': string
* undefined: number | null
* null: number | object
* never: never



유닛 타입 

한 가지 값만 포함하는 타입 (=유닛 타입 = 리터럴 타입)

```typescript
// 유닛 타입 혹은 리터럴 타입
type Dog = 'dog'
type Cat = 'cat'
```



유니언 타입 

```typescript
// 유니언 타입
type Animal = 'dog' | 'cat'
type Color = 'blue' | 'green'
```

타입 체커는 유니언 타입을 검사 시에, 하나의 집합이 다른 집합의 부분 집합인지 검사한다.



인터섹션 타입

```typescript
// 인터섹션 타입
interface Mom {
	gender: 'female'
}

interface Dad {
	gender: 'male'
}

interface Prents = Mom & Dad
```

