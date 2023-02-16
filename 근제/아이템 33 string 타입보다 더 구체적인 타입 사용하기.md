# 아이템 33 string 타입보다 더 구체적인 타입 사용하기

string은 정말 넓은 타입입니다. 그러니 혹시 더 좁은 타입으로 가능한지 검토를 해보는 것이 좋습니다. (당장 아이템 32의 `Layer` 예제만 봐도 알 수 있습니다) 아래와 같이 음악 컬렉션을 만들기 위한 타입이 있다고 예를 들어봅시다.

```typescript
interface Album {
  artist: string;
  title: string;
  releaseDate: string; // YYYY-MM-DD
  recordingType: string; // 예를 들어, 'live', 'studio'
}
```

위와 같이 된다면 아래와 같이 신기하게도 적용이 될 것입니다.

```typescript
const kindOfBlue: Album = {
  artist: "Miles Davis",
  title: "Kind of Blue",
  releaseDate: "August 17th, 1959", // 원하던 형식 X
  recordingType: "Studio", // 대문자로 시작
};
```

그러니 위의 타입보단 조금 더 좁게 아래와 같이 하는게 더 좋을 것입니다

```typescript
type RecordingType = "studio" | "live";

interface Album {
  artist: string;
  title: string;
  releaseDate: Date;
  recordingType: RecordingType;
}
```

좁히면 많은 장점들이 있다. 알아보자

1. 타입을 명시적으로 정리함으로써 다른 곳으로 값이 전달되어도 타입 정보가 유지됩니다.
2. 타입을 명시적으로 정의하고 해당 타입의 의미를 설명하는 주석을 붙여 넣을 수 있습니다.
3. keyof 연산자로 더욱 세밀하게 객체의 속성 체크가 가능해집니다.

- 요약
  - `문자열을 남발하여 선언된 코드`를 피합시다. 모든 문자열을 할당할 수 있는 string 타입보다는 더 구체적인 타입을 사용하는 것이 좋습니다.
  - 변수의 범위를 보다 정확하게 표현하고 싶다면 string 타입보다는 문자열 리터럴 타입의 유니온을 사용하면 됩니다. 타입 체크를 더 엄격히 할 수 있고 생산성을 향상시킬 수 있습니다.
  - 객체의 속성 이름을 함수 매개변수로 받을 때는 string보다 keyof T를 사용하는 것이 좋습니다
