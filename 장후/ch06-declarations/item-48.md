# Item 48. API 주석에 TSDoc 사용하기

공개 API를 만드는 경우, JSDoc 형태로 함수에 대한 설명을 명시하면 툴팁으로 해당 정보를 보여준다.

```ts
/** 안녕하세요 */
function greetJSDoc(name: string) {
    return `Hello ${name}`
}
```

타입 정의에 JSDoc을 사용할 수 있다.
```ts
/** 측정 정보 */
interface Measurement {
    /** 어디서 측정 되었니 */
    position: Vector3D;
    /** 언제 측정 되었니 */
    time: number;
    /** 측정된 운동량 */
    momentum: Vector3D;
}
```

Markdown 형태로도 작성 가능하지만 너무 장황하게 쓰기 보다는, 간단하게 요점만 명시해준다.

JSDoc에서는 타입 정보를 명시하는 규칙이 있지만, TSDoc에서는 타입 정보가 코드에 있기 때문에 명시할 필요가 없다.
