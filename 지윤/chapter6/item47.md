#### 아이템47. 공개 API에 등장하는 모든 타입을 익스포트하기

라이브러리로 제공할 경우, 모듈 타입은 모두 명시적으로 export 해주자. 어차피 어떻게든 타입은 노출된다.

```typescript
// 라이브러리
interface SecretInfo {
  something: string
}

export function getSomething(a: string): SecretInfo {
  ...
}

// 라이브러리에서 type을 export하지 않게 해놨어도 꺼내쓸 수 있다.
type info = ReturnType<typeof getSomething>
```
