#### 아이템41. any의 진화를 이해하기

```typescript
const arr = []; // const arr: any[]
for (let i = 1; i < 10; i++) {
  arr.push(i);
  return arr; // const arr: number[]
}
```

암시적 any 타입

- any 타입의 진화는 암시적 any 타입에 어떤 값을 할당할 때만 발생한다.
- 어떤 변수가 암시적 any 타입일 때, 값을 읽으려고하면 오류가 발생한다.
- 암시적 any 타입은 함수 호출을 거쳐도 진화하지 않는다.

any의 진화방식은 일반적 변수가 추론되는 원리와 동일하다. 그러니 타입을 안전히 지키기 위해서는 any 진화보다는 명시적 타입 구문을 사용하는 것이 좋다.
