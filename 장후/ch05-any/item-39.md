# Item 39. any를 구체적으로 변형해서 사용하기

## TL;DR
일반적인 any보다 최대한 좁은 범위에 적용되도록 any를 사용해야한다.

---

any를 사용하기보다는 구체적인 타입을 적용하여 타입 안전성을 높이는 것이 좋다.

```ts
function foo (array: any) {
    return array.length;
}

function bar (array: any[]) {
    return array.length;
}
```

foo 함수처럼 매개변수에 `any`를 넣기보다는 `any[]` 를 넣는 것이 더 낫다.

* array의 length 타입이 체크된다.
* 반환 타입이 any가 아닌 number로 추론된다.
* 호출 시 매개변수가 배열인지 체크한다.

객체의 경우 아래와 같은 타입도 사용 가능하다.
* `object`
* `{ [key: string]: any }`
* `unknown`

함수의 경우 아래와 같이 타입 범위를 좁힐 수 있다.
* `() => any` 매개변수 없이 호출 가능
* `(arg: any) => any` 매개변수 한 개
* `(...args: any[]) => any` 매개변수 여러 개, Function type과 동일하다.

```ts
// Bad
const numArgs = (...args: any) => args.length; // return type any

// Good
const numArgs = (...args: any[]) => args.length // return type number
```

