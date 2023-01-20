# 아이템 8. 타입 공간과 값 공간의 심벌 구분하기
* 타입스크립트의 심벌은 타입, 값 공간 중 하나에 존재한다.
* 즉, 속하는 공간에 따라 타입 혹은 값으로 나뉜다.

* type, interface, `:`, as > 타입
* const, `=` > 값

* `class`
    * class가 타입으로 쓰일 때는 속성과 메서드가 사용
    * class가 값으로 쓰일 때는 생성자가 사용

* `typeof`
    * 타입의 관점에서는 타입스크립트 타입을 반환
    * 값의 관점해서는 자바스크립트 런타임의 typeof 연산자
```typescript
interface Person {
    first: string;
    last: string;
}

const p: Person = { first: 'happy', last: 'new year' }

type T1 = typeof p; // 타입 p의 타입 > Person

const v1 = typeof p; // 변수 p의 타입 > Object
```

* this
    * 값: 자바스크립트 this
    * 타입: 다형성 this > 서브클래스 메서드 체인 구현
* `&, |`
    * 값: AND, OR 연산자
    * 타입: intersection, union
* const
    * 값: 변수 선언
    * `as const` 리터럴 또는 리터럴 표현식의 추론된 타입 변경
        * readonly type으로 바꿈
        * Object.freeze의 느낌 (개념은 다르다.)
    ```ts
    const tmp = {
        hi: 'hi',
        bye: 'bye'
    } as const

    tmp.bye = 'thithi'; // 불가능 > Cannot assign to 'bye' because it is a read-only property
    ```
* extends
    * 서브클래스 `class A extends B`
    * 서브타입 `interface A extends B`
    * 제너릭 타입의 한정자 `<T extends number>`
* in
    * `for (key in object)`
    * mapping된 타입에 등장
        * The in operator returns true if the specified property is in the specified object.
    ```ts
    propNameOrNumber in objectName
    ```
