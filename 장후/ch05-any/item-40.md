# Item 40. 함수 안으로 타입 단언문 감추기

함수 작성 시, 불필요한 예외 상황까지 타입을 설계할 필요없다. 함수 내부에서는 타입 단언을 사용하고, 외부로 드러나는 타입을 정확하게 명시하는 것이 좋다.

즉, 타입 단언문을 드러내기 보다는, 확실한 타입이 정의된 함수 안으로 녹이는 것이 더 좋은 설계이다.

```ts
function cacheLast<T extends Function>(fn: T): T {
    let lastArgs: any[] | null = null;
    let lastResult: any;

    return function (...args: any[]) {
        if (!lastArgs || !shallowEqual(lastArgs, args)) {
            lastResult = fn(...args);
            lastArgs = args;
        }

        return lastResult;
    } as unknown as T;
}
```

이처럼 내부에는 any가 많이 보이지만 타입의 정의에는 any가 없기 때문에, any 사용 여부를 알 수 없다.

## Summary
* any를 사용하는 것은 버그 발생 등 위험성을 가지고 있지만, 상황에 따라 현실적인 해결책이 된다. 따라서, 불가피한 상황이라면 정확한 정의를 가지는 함수 안으로 숨겨 외부에서는 any 사용 여부를 모르게끔 처리하는 것이 바람직한 설계이다.