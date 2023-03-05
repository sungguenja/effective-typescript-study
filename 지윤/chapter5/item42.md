#### 아이템42. 모르는 타입의 값에는 any 대신 unknown 사용하기

```typescript
// bad
const parseYAML = (yaml: string): any => {...}
const book = parseYAML(`name: abc`)
alert(book.title) // 오류 안남
book('read') // 오류 안남

// good - unknown인 채로 값을 사용/함수호출/연산 시 오류 발생.
const safeParseYAML = (yaml: string): unknown => {...}
const book = safeParseYAML(`name: abc`)
alert(book.title) // Error: 'book' is of type 'unknown'.
book('read') // Error: 'book' is of type 'unknown'.

// unknown으로 반환되는 함수값을 사용하려면 반드시 타입 단언이 필요하다.
const book: Book = safeParseYAML(`name: abc`)
```

할당가능성 관점에서의 any

- 어떤 타입이든 any 타입에 할당 가능하다
- any 타입은 어떤 타입으로도 할당 가능하다

할당가능성 관점에서의 unknown

- 어떤 타입이든 unknwon 타입에 할당 가능하다
- unknown타입은 오직 unknown과 any에만 할당가능하다.

any 타입으로 함수를 반환시에, 사용되는 값에 타입 단언을 하지 않아도 사용이 되지만, unknown 타입으로 함수를 반환 시에, 사용되는 값에 타입 단언을 하지 않으면 에러가 나게 된다. 이러한 부분은 사용자가 직접 단언문을 사용하여 원하는대로 타입을 좁히도록 강제한다.
