# Item 42. 모르는 타입의 값에는 any 대신 unknown을 사용하기

## TL;DR
모든 타입에 할당 가능한 any와는 다르게 unknown은 unknown과 any에만 할당 가능하므로, any 대신 unknown을 사용하는 것이 안전하다.

---
반환값과 관련된 unknown
* 함수를 호출한 곳에서 반환값을 원하는 타입으로 할당하는 것이 이상적이지만, 호출한 곳에서 타입 선언이 생략되면 any 타입이된다.

* unknown을 반환하게 만드는 경우
    * 어떠한 타입이든 unknown에 할당 가능
    * unknown은 unknown과 any에만 할당 가능

* unknown type을 그대로 사용할 경우, error가 발생하므로 단언을 통해 강제하여 사용해야한다.


변수 선언과 관련된 unknown
* 값이 있는데 타입을 모르는 경우 사용
    * instance of를 사용하여 타입 체크
    ```ts
    function processValue(val: unknown) {
        if (val instanceof Date) {
            // val type is Date
        }
    }
    ```
    * 타입 가드를 사용하여 타입 체크
    ```ts
    function isBook(val: unknown): val is Book {
        return (
            typeof(val) === 'object' && val !== null && 'name' in val && 'author' in val
        );
    }

    function processValue(val: unknown) {
        if (isBook(val)) {
            // val type is Book
        }
    }
    ```

    * 제너릭을 사용하는 것과 기능적으로 동일하지만, 사용자가 직접 단언하거나 원하는 타입을 좁히도록 강제하는 것이 좋다.

단언문과 관련된 unknown
```ts
declare const foo: Foo;
let barAny = foo as any as Bar;
let barUnk = foo as unknown as Bar;
```

* 이중 단언문을 분리할 때 `unknown` 이 분리되는 형태가 더 안전하다.
    * 분리되는 순간 error 발생

그 외
* unknown 대신 object 또는 `{}` 를 사용하는 것도 방법이다.
    * `{}` 은 null, undefined를 제외한 모든 값을 포함한다.

