# Item 43. 몽키 패치보다는 안전한 타입을 사용하기

## TL;DR
* 몽키 패치를 남용하는 것은 side effect 등 다양한 문제를 야기 시킨다.
* 만약 꼭 필요한 상황이라면, 보강 혹은 확장을 통한 타입 단언 등으로 타입 안정성을 확보하고 사용하자

---

JavaScript는 객체와 클래스에 임의의 property를 추가할 수 있는 특징이 있다.

이러한 특징 때문에, window나 document 객체에 property를 임의로 할당하여 전역 변수를 만들거나 DOM Element에 property를 추가하여 사용한다.

하지만, 기본적으로 이러한 몽키패치로 이루어진 property는 전역변수가 되기 때문에 side effect 발생 확률이 올라간다. 또한 ts-server에는 임의로 추가된 property에 대한 정보가 없기 때문에 에러가 발생한다.

이 오류를 해결하는 간단한 방법은 any를 사용하는 것이다.
* 타입 체커는 통과하지만, 제대로된 체크를 받지 못하기 때문에 불안하다.
```ts
(document as any).monkey = 'monkey';
```

최선의 해결책은 데이터를 분리하는 것이지만, 그럴 수 없는 경우 두 가지 차선책이 있다.

첫 번째, 보강 기능 활용
* 타입 체커가 동작하기 때문에, 타입이 더 안전하다.
* property에 주석을 붙일 수 있다.
* property에 자동완성을 사용할 수 있다.
* 어느 부분에 적용된 건지 확인이 용이하다.

```ts
interface Document {
    // 주석
    monkey: string;
}

document.monkey = 'monkey';
```

module 관점에서 보강 기능을 제대로 사용하기 위해서는 global 선언을 추가해야한다.
* 보강은 전역으로 적용되기 때문에, 코드나 라이브러리로 부터 분리할 수 없다.
* 애플리케이션 실행 이후 property가 할당되면 해당 시점에서 보강을 적용할 수 없다.

```ts
export {};
declare global {
    interface Document {
        // 주석
        monkey: string;
    }
}

document.monkey = 'monkey'
```

두 번째, 구체적인 타입 단언문 사용

```ts
interface MonkeyDocument extends Document {
    monkey: string;
}

(document as MonkeyDocument).monkey = 'monkey';
```

Document type을 확장했기 때문에, 위 타입 단언은 안전하다. 또한 Document type을 직접 건드리지 않고 새로운 타입을 도입했기 때문에 모듈 문제도 해결 가능하다.
