# Item 26. 타입 추론에 문맥이 어떻게 사용되는지 이해하기

* 타입은 값이 존재하는 문맥을 고려하여 추론된다.
* 원치 않는 추론을 방지하기 위해서 아래 두 가지 방법이 있다.
    1. 타입 선언 시 가능한 값을 제한한다.
    ```ts
    type Language = 'JS' | 'TS';
    let language: Language = 'JS'
    ```

    2. const로 변수를 선언한다.
    ```ts
    const language = 'JS';
    ```

* 하지만 위 방법은 문맥을 전혀 고려하지 않았음으로 문제가 발생할 여지가 존재한다.

문맥 소실 시 발생할 수 있는 문제점에 대해 몇 가지 케이스를 통해 알아보자.

### 튜플 사용 시 주의점
* 문맥을 고려하지 않고, 값을 할당했고 이로인해 함수에 매개변수로 값을 넘길 때, 에러가 발생한다.
    ```ts
    const panTo = (where: [number, number]) => {}

    panTo([10, 10]) // Success

    const loc = [10, 10]; // type number[]
    panTo(loc) // Error
    ```

* 변수 선언 시 타입을 명확하게 선언하여 에러를 해결

    ```ts
    const loc: [number, number] = [10, 10];
    panTo(loc) // Success
    ```

* `as const` 를 통해 최대한 좁게 추론

    ```ts
    const loc = [10, 10] as const; // type readonly[10, 10]
    panTo(loc); // Error 
    ```

    * 위의 경우 loc이 `readonly[10, 10]` 으로 추론되었지만, panTo 매개변수는 불변이 아니므로 에러가 발생한다.
    * 매개변수에 readonly를 선언한다면, 해결 가능

    ```ts
    const panTo = (where: readonly[number, number]) => {}
    ```

    * as const는 문맥에 대한 문제를 해결할 수 있다.
    * 하지만 튜플에 요소를 추가 가능성 등 타입 정의에 대한 실수가 존재한다면 에러는 호출하는 부분에서 발생하기 때문에, 에러 흐름을 파악하기 어렵다.

    ```ts
    const loc = [10, 20, 30 ] as const; // 에러가 발생 해야하는 부분
    panTo(loc) // 실제 에러 발생 부분
    ```

### 객체 사용 시 주의점
* 객체 내 값들은 타입이 넓게 추론되기 때문에, 값을 넣을 시 문맥을 파악하지 못하고 에러가 발생할 수 있다.
* 따라서, 변수 선언 시 타입 선언을 통해 명시하거나 as const를 사용한다.

### 콜백 사용 시 주의점
* 콜백을 매개변수로 전달하는 경우, 따로 값을 선언한다면 매개변수의 타입은 any로 추론된다.
* 따라서, 매개변수에 타입 구문을 추가하여 해결히거나 함수 표현식에 타입 선언을 한다.

