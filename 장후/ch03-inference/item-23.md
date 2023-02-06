# Item 23. 한꺼번에 객체 생성하기

* 변수 값은 변경될 수 있지만, 타입은 변하지 않기 때문에 자바스크립트 패턴을 타입스크립트로 모델링하기 용이하다.
* 한번에 객체 리터럴로 객체를 생성하는 것이 추론에 유리하다.

```ts
interface Point { x: number; y: number; };

const point: Point = {
    x: 3,
    y: 4
};
```

* 반드시 객체를 property를 나눠서 할당해야한다면 타입 단언문 고려

```ts
const point = {} as Point

point.x = 3;
point.y = 3;
```

* Spread Operator를 활용하여 객체 생성

```ts
const pt0 = {};
const pt1 = { ...pt0, x: 3 };
const point: Point = { ...pt1, y: 4 };
```

* 조건부 property 할당
    * 하나의 property를 선택적으로 추가하면 선택적 속성으로 타입 추론
    ```ts
    declare let hasMiddle: boolean;

    const firstLast = { first: 'Terry', last: 'Jerry' };
    const name = { ...firstLast, ...(hasMiddle ? { middle: 'Umm' } : {} ) }

    // type { first: string; last: string; middle?: string; }
    ```
    * 두 가지 이상의 property를 선택적으로 추가하면 유니온으로 타입 추론
        * 추가된 property들이 항상 함께 정의된다.
        * 선택적 속성으로 추론하게 하고 싶다면 다른 방법을 사용해야한다.
    ```ts
    declare let hasDates: boolean;

    const name = { name: 'ab', title: 'cd' };
    const pharaoh = { ...name, ...(hasDates ? { start: 1, end: 10 } : {} )}

    // type { name: string; title: string; } | { name: string; title: string; start: number; end: number; }
    ```
    
    * 헬퍼 함수를 활용하여 두 가지 이상 property를 선택적으로 표현
    ```ts
    function addOptional<T extends object, U extends object>(a: T, b: U | null): T & Partial<U> {
        return { ...a, ...b }
    }

    const name = addOptional(name, hasDates ? { start: 1, end: 10 } : null );
    ```

* 기존 배열을 기반으로 새로운 객체나 배열을 생성하고 싶을 때는, loop 보다는 Array methods나 lodash 같은 라이브러리를 사용하는 것이 좋다.