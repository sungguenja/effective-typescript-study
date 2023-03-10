# 아이템 49 콜백에서 this에 대한 타입 제공하기

[this mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)

this는 dynamic scope라는 것을 이해해야 한다. 코드로 흐름을 잡아봅시다

```typescript
class C {
  vals = [1, 2, 3];
  logSquares() {
    for (const val of this.vals) {
      console.log(val ** 2);
    }
  }
}

const c1 = new C();
c1.logSquares(); // 1,4,9가 순차적으로 정상 출력

const c2 = new C();
const method = c2.logSquares;
method(); // vals 속성 없다는 에러 발생

const c3 = new C();
const method = c3.logSquares;
method.call(c); // 정상 동작
```

- 요약
  - this 바인딩이 동작하는 원리를 이해해야 합니다.
  - 콜백 함수에서 this를 사용해야 한다면, 타입 정보를 명시해야 합니다.
