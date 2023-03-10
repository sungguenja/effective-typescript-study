# Item 38. any 타입은 가능한 한 좁은 범위에서만 사용하기

## TL;DR
* any를 사용하는 경우 최대한 좁은 범위에 영향을 끼치도록 설계하자

---
any를 사용한다면 f2의 방식이 권장된다.

```ts
function f1() {
    const x: any = foo();
    
    bar(x);
}

function f2() {
    const x = foo();
    bar(x as any);
}
```

f1 방식은 return type, 매개변수 등으로 x값이 사용되는 경우, 타입 정보가 유지되어 코드에 영향을 끼친다.

반면 f2 방식은 다른 코드에 영향을 끼치지 않는다.
따라서, any를 사용해야만 한다면 사용 범위를 최대한 좁혀서 사용하는 것이 권장된다.

`@ts-ignore` 를 사용하여 타입 체크에 대한 오류를 해결할 수 있다.
하지만, 타입 체커의 오류는 에러를 야기시킬 가능성이 크기 때문에, 근본적으로 해결하고 대처하는 것이 바람직하다.

```ts
const config: Config = {
    a: 1,
    b: 2,
    c: {
        key: value as any
    }
}
```

Object를 사용하는 경우에도 최대한 좁게 any를 할당해서 사용하는 것이 좋다.

## Summary
* any 사용 범위를 최대한으로 좁히자.
* 반환 타입이 any인 경우는 무조건 피하자.
* 강제로 타입 오류를 제거하려면 `@ts-ignore` 를 사용하는 것도 방법이다.