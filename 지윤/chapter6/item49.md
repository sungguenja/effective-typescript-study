#### 아이템49. 콜백에서 this에 대한 타입 제공하기

자바스크립트에서 this는 dynamic scope의 변수이다. (=호출된 방식에 따라 값이 달라진다.)

this binding 방법

- .call method
- .bind method
- arrow function

타입스크립트 또한 this binding을 신경써야 한다.

```typescript
function addKeyListener(
  el: HTMLElement,
  fn: (this: HTMLElement, e: KeyboardEvent) => void
) {
  el.addEventListener("keybown", (e) => {
    fn.call(el, e);
  });
}
```
