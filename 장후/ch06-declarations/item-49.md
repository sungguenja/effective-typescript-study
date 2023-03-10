# Item 49. 콜백에서 this에 대한 타입 제공하기

자바스크립트에서 this는 호출된 방식에 따라 달라지는 다이나믹 스코프이다.

this 바인딩은 타입스크립트에서도 동일하게 동작한다.

만약 this를 사용하는 함수가 있다면, 다음과 같이 매개변수에 this를 추가하여 사용하면 된다.

```ts
function addKeyListener (
    el: HTMLElement,
    fn: (this: HTMLElement, e: KeyboardEvent) => void
) {
    el.addEventListener('keydown', e => {
        fn.call(el, e)l
    })
}
```

콜백함수의 argument에 this를 추가한다면 this 바인딩이 추가되기 때문에, 오류를 잡을 수 있다.

또한 콜백함수에서 this를 참조할 수 있다.

만약 콜백함수를 화살표 함수로 작성했다면 this 참조할 때 타입 에러가 발생한다.

```ts
class Foo {
    registerHandler(el: HTMLElement) {
        addKeyListener(el, e => {
            this.innerHTML // 'Foo'에 innerHTML 속성이 없다.
        })
    }
}
```