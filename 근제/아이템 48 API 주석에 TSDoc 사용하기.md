# 아이템 48 API 주석에 TSDoc 사용하기

JSDoc처럼 TS에서도 같은 방식으로 주석을 추가할 수 있습니다. 그리고 타입 정의에도 쓸 수 있습니다

```typescript
/**
 * 인사말을 생성합니다.
 * @param name 인사할 사람의 이름
 * @param title 그 사람의 칭호
 * @returns 사람이 보기 좋은 형태의 인사말
 */
function greeFullTSDoc(name: string, title: string) {
  return `Hello, ${title} ${name}`;
}

/** 특정 시간과 장소에서 수행된 측정 */
interface Measurement {
  /** 어디에서 측정되었나 */
  position: Vector3D;
}
```

TSDoc는 마크다운 형식으로 꾸며지므로 편하게 마크다운 문법을 이용해도 좋습니다

- 요약
  - 익스포트된 함수, 클래스, 타입에 주석을 달 때는 JSDoc/TSDoc 형태를 사용합시다.
  - 마크다운 형식을 사용할 수 있습니다
  - 주석에 타입 정보를 포함하면 안됩니다
