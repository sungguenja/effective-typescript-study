#### 아이템40. 함수 안으로 타입 단언문 감추기

함수의 모든 부분을 안전한 타입으로 구현하는 것은 이상적이다. 불필요한 예외 상황까지 고려하며 타입 정보를 힘들게 구성하기 보다는, 함수 내부는 타입 단언을 쓰고 외부 노출 타입을 정확하게 명시하자.

프로젝트 전반에 타입 단언을 흩뿌리지 말고 정확한 타입이 정의된 함수 안으로 타입 단언을 감추자.
```typescript
declare function cacheLast<T extends Function>(fn: T):T;
const cacheLast = <T extends Function,>(fn: T): T => {
  let lastArgs: any[]|null = null;
  let lastResult: any; 
  return function(args: any[]) {
     if (!lastArgs || !shallowEqual(lastArgs, args)) {
      lastResult = fn(...args);
      lastArgs = args
  	}
    return lastResult // any 타입이지만
  } as unknown as T; // T로 타입단언되어 에러 안남
}
```

```typescript
declare function shallowObjectEqual<T extends object>(a: T, b: T): boolean;
const shallowObjectEqual = <T extends object,>(a: T, b: T): boolean => {
  for (const [aKey, aVal] of Object.entries(a)) {
    if (!(aKey in b) || aVal !== (b as any)[aKey]) { // aKey가 b안에 있는 것을 앞에서 확인했으니, 안전한 타입 단언이 된다.
      return false
    }
  }
  return Object.keys(a).length === Object.keys(b).length;
} 
```
