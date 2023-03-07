# 아이템 41 any의 진화를 이해하기

any는 문맥에 따라서 evolve할 수 있다.

```typescript
function range(start: number, limit: number) {
  const out = []; // any[]
  for (let i = start; i < limit; i++) {
    out.push(i); // number[]
  }
  return out; // number[]
}

let val = null; // any
try {
  somethingFunction();
  val = 12; // number
} catch (e) {
  console.log(e);
}
val; // number | null
```

- 요약
  - 일반적인 타입들은 `정제되기만 하는 반면`, 암시적 any와 any[] 타입은 `evolve할 수 있습니다`. 이러한 동작이 발생하는 코드를 인지하고 이해할 수 있어야 합니다.
  - any를 evolve시키는 방식보다 명시적 타입 구문을 사용하는 것이 안전한 타입을 유지하는 방법입니다.
