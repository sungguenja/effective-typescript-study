# Item 52. 테스팅 타입의 함정에 주의하기

타입에 대한 테스트 진행 시 함수 실행에 대한 테스트 보다는 반환 타입을 체크하는 것이 더 좋은 테스트 코드이다.

테스트를 위해 타입을 불필요하게 선언하면 두 가지 문제점이 있다.

```ts
const lengths: number[] = map(['john', 'paul'], name => name.length);
```

첫 번째, 타입 선언을 위한 불필요한 변수를 만들어야한다.

이는 헬퍼 함수를 정의하여 해결 가능하다.

```ts
function assertType<T>(x: T) {}

assertType<number[]>(map(['terry', 'jerry'], name => name.length));
```

두 번째, 타입에 대한 동일성이 아닌 할당 가능성을 체크한다. 

매개변수가 더 적은 함수 타입에 할당 가능 등의 동일하지 않은 타입에 테스트가 통과하는 경우가 생긴다.

또한, this, 콜백 함수의 추론도 함께 테스트해야 하므로 타입 테스트는 어려운 작업이다.

따라서, `dtslint` 등의 라이브러리를 활용하여 타입 선언을 테스트한다.

```ts
const soul = ['jerry', 'terry'];

map(soul, function(
    name, // $ExpectType string
    index, // $ExpectType number
    array // $ExpectType string[]
) {
    this // $ExpectType string[]

    return name.length; // $ExpectType number[]
})
```

`dtslint` 는 각 변수의 타입을 추출하여 글자가 동일한지 체크한다. `number|string` 과 `string|number` 는 다른 타입으로 인식
