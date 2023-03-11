#### 아이템51. 의존성 분리를 위해 미러 타입 사용하기

CSV를 parsing하는 함수를 라이브러리로 공개한다고 하자.

```typescript
function parseCSV(content: string | Buffer) {
  ...
}
```

위 함수에서 Buffer는 @types/node를 의존하기 때문에 devDependencies로 @types/node를 넣게될 것이다.

이럴 경우, @types/node가 필요하지 않은 사용자들이 생긴다.

- @types를 사용하지 않는 자바스크립트 사용자
- nodejs를 사용하지 않는 타입스크립트 사용자

각자가 필요한 모듈만 사용할 수 있는 방법은 없을까?

```typescript
// good - 구조적 타이핑
interface CsvBuffer {
  toString: (encoding: string) => string;
}

function parseCSV(content: string | CsvBuffer) {
  ...
}

// good - 미러링
```
