# Item 33. string 타입보다 더 구체적인 타입 사용하기

## TL;DR
* string은 모든 문자열을 할당하므로 any랑 다를게 없다.
* 타입 범위를 좁혀 버그 발생을 줄이고 개발자 경험을 높이자

---
string type을 남발하면, 제대로 된 타입 체크가 어렵다. 즉, any와 유사하게 무효한 값을 허용하고 타입 간 관계도 무시한다.

```ts
type RecordingType = 'studio' | 'live';
```

유니온을 활용하여 타입의 범위를 좁히면 타입스크립트는 오류를 세밀하게 체크한다.
1. 값이 전달되어도 타입 정보가 유지된다.
2. 주석을 붙여넣으면 해당 타입을 사용하는 곳에서 정보를 볼 수 있다.
3. `keyof` 를 활용한다면 속성 체크가 쉬워진다.

```ts
interface Album {
    artist: string;
    title: string;
    releaseDate: Date;
    recordingType: 'studio' | 'live';
}

type K = keyof Album;

function pluck<T, K extends keyof T>(records: T[], key: K): T[K][] {
    return records.map(r => r[key])
}

pluck(albums, 'releaseDate'); // type Date[]
```

타입의 범위를 좁히고 싶다면

* `keyof` 를 활용한다.
* `extends keyof` 가 `keyof` 보다 좀 더 구체적으로 속성을 찾아낸다.

따라서, 정확한 타입을 사용하여 오류를 방지하고 코드의 가독성도 향상시키자
