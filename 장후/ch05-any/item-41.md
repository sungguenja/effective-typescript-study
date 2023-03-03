# Item 41. any의 진화를 이해하기

## TL;DR
암시적 any는 진화를 통해 타입 추론이 이루어지지만, 명시적으로 타입을 선언하여 사용하는 것이 더 좋은 설계이다.

---

```ts
function range(start: number, limit: number) {
    const out = []; // type any

    for (let i = start; i < limit; i++) {
        out.push(i);
    }

    return out; // type number[]
}
```

out의 타입이 `any`로 추론되었지만, 배열에 숫자를 추가하면서 `number[]` 타입으로 추론된다.

단, 명시적으로 any를 선언하면 타입이 그대로 유지된다.

```ts
let value: any;

if (Math.random() < 0.5) {
    val = /hello/;
} else {
    val = 12;
}

return val; // type any
```

암시적 any 상태에서 할당 없이 값을 사용하려고 하면 오류가 발생한다. 또한, 함수 호출을 거쳐도 진화하지 않는다.

즉, any의 진화는 어떤 값을 할당할 때만 발생한다. 주의해야될 점은 any의 진화 과정은 변수 타입 추론과 유사하기 때문에, 잘못된 타입이 들어갈 수 있다.
