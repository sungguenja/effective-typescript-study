# 아이템 16 number 인덱스 시그니처보다는 Array, 튜플, ArraLike를 사용하기

JS의 유래깊은 코메디 중 하나입니다. [관련있는 javascript 유머](https://www.reddit.com/r/ProgrammerHumor/comments/88gniv/old_meme_format_timeless_javascript_quirks/)

```javascript
"0" == 0; // true
```

이 기능으로 아래와 같은 특이 사항도 가능은 하긴 한데... (하지않도록 합시다...)

```typescript
const x = [1, 2, 3];
x[0]; // 1
x["1"]; // 2
```

타입스크립트에서는 어레이를 어떻게 정의할까? 아래와 같이 되어있어서 자바스크립트처럼 스트링 접근은 안됩니다. TS 자체적으로 버그를 잘잡는 것을 돕기 위해 정의를 해놨습니다.

```typescript
interface Array<T> {
  // ...
  [n: number]: T;
}
```

- 요약
  - 배열은 객체이므로 키는 숫자가 아니라 문자열입니다. 인덱스 시그니처로 사용된 number 타입은 버그를 잡기 위한 순수 타입스크립트 코드입니다
  - 인덱스 시그니처에 number를 사용하기보다 Array나 튜플, 또는 ArrayLike타입을 시도하는 것이 좋습니다
