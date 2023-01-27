#### 아이템17. 변경 관련된 오류방지를 위해 readonly를 사용하기

`number[]` 와 `readonly number[]`의 차이점
* 배열을 읽을 수는 있지만, 쓸 수는 없다.
* length를 읽을 수는 있지만, 바꿀 수는 없다.
* 배열을 수정하는 pop 등의 메소드를 사용할 수 없다.

readonly는 함수의 매개변수가 수정되지 않을 때 사용하면 매개 변수의 타입도 명확하게 보이고, 매개 변수의 변경을 방지할 수도 있다.

```typescript
// 주의점: readonly는 얕게 동작한다.
const dateRange: readonly Date[] = [new Date(), new Date()];
dateRange.push(new Date()) // Error: Property 'push' does not exist on type 'readonly Date[]'.
dateRange[0].setHours(10) // 에러 안남

// deep readonly
type DeepReadonly<T> = {
  readonly [t in keyof T]: T[t] extends Record<any, unknown>
    ? DeepReadonly<T[t]> // 타입은 재귀적으로 사용이 가능하다.
    : T[t];
};
```

