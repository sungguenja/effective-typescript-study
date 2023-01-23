# Item 12. 함수 표현식에 타입 적용하기

## 12-1. 함수 표현식 
* 자바스크립트는 함수 표현식(expression)과 함수 선언식(statement)을 다르게 인식한다.
    ```ts
    // 함수 표현식 JS
    const fnExpression = function {};
    const fnExpression = () => {};

    // 함수 표현식 TS
    const fnTSExpression = (sides: number): number => { return 1; };

    // 함수 선언식
    function fnExpressiont() {};

    // 함수 선언식 TS
    function fnTSExpressiont(sides: number): number { return 1; };
    ```
* 타입스크립트에서는 함수 표현식을 사용하는 것이 좋다.
    * 함수의 arguments, return 값을 타입으로 선언하여 재사용 가능
    ```ts
    type TFnExpression = (sides: numebr) => number;
    const fnExpressiont: TFnExpression = (sides) => { return 1; }
    ```
    * sides의 타입을 체크해보면 number로 인식한다.
* 함수 타입을 선언하면 불필요한 코드 반복을 줄일 수 있다.
    ```ts
    type TBinartFn = (a: number, b: number) => number;
    const add = (a, b) => { return a + b; };
    const mul = (a, b) => { return a * b; };
    ```
    * 타입, 함수 구현부가 분리되어 있어 로직이 분명해진다.
* 라이브러리를 직접 만든다면, 공통 콜백 함수를 위한 타입도 제공하는 것이 좋다.
    * React에서는 `MouseEvent` 콜백 함수 전체에 적용할 수 있는 `MouseEventHandler` 타입을 제공한다.
* 함수 시그니처
    * 함수가 가지는 argument, return type 등의 특징 정도의 의미라고 생각.
## 12-2. 함수 시그니처의 활용
* 시그니처가 일치하는 다른 함수가 있을 때 함수 타입을 적용
* fetch 함수
    ```ts
    declare function fetch(
        input: RequestInfo, init?: RequestInit
    ): Promise<Response>
    ```
    * checkedFetch와 fetch 함수가 동일한 시그니처를 가지고 있다면 fetch 함수 시그니처를 타입에 적용
        * 타입스크립트가 input과 init의 타입을 추론할 수 있다.
        * return type을 보장해준다.
    ```ts
    const checkedFetch: typeof fetch = async (input, init) => {
        const res = await fetch(input, init);

        if (!res.ok) {
            // throw new Error는 async 함수에서 자동으로 reject promise로 return 된다.
            throw new Error
        }

        return response;
    }
    ```
    * `throw` 대신 `return` 을 사용하는 경우 타입 에러를 잡아낸다.
    ```ts
    // checkedFetch Promise<Response | Error> 형식은 Promise<Response> 에 할당 불가능
    const checkedFetch: typeof fetch = async (input, init) => 
    {
        const res = await fetch(input, init);

        if (!res.ok) {
            // return new Error는 Error를 반환한다는 의미
            return new Error
        }

        return response;
    }
    ```
* 함수 표현식 전체 타입을 정의하는 것이 코드도 안전해진다.
* 동일한 타입 시그니처를 가졌다면 반복해서 argument, return type을 명시하지 말고 함수 전체 타입 선언을 적용해야 한다.
    


## Summary
* argument, return type을 명시하기 보다는 함수 표현식 전체에 타입을 적용하는 것이 좋다.
* 동일한 타입 시그니처는 공통 콜백을 활용해보자
* 다른 함수의 시그니처를 참조하려면 `typeof fn` 을 사용
    * ex) `typeof fetch`