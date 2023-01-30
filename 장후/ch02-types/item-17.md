# Item 17. 변경 관련된 오류 방지를 위해 readonly 사용하기

```ts
function arraySum(arr: number[]) {
    let sum = 0, num;

    while ((num = arr.pop()) !== undefined) {
        sum += num;
    }

    return sum;
}

function printTriangles(n: number) {
    const nums = [];

    for (let i = 0; i < n; i++) {
        nums.push(i);
    }

    console.log(arraySum(nums)); // 10
    console.log(nums); // []
}

printTriangles(5);
```

* 자바스크립트 pop은 기존 Array의 내용을 변경한다. 따라서 해당 코드를 실행 시키면 기존 배열은 빈 배열이 된다.
* readonly 속성을 사용하면 기존 배열을 변경할 수 없는 `읽기 전용` 속성이 된다.

```ts
function arraySum(arr: readonly number[]) {
    let sum = 0, num;

    while ((num = arr.pop()) !== undefined) {
        sum += num;
    } // Property 'pop' does not exist on type 'readonly number[]'.

    return sum;
}
```

## readonly
* 특징
    * 읽기 전용 속성
    * length 메서드는 실행가능 하지만, 배열의 변경은 불가능하다.
    * 즉, pop을 비롯한 배열을 변경하는 메서드는 사용할 수 없다.
* `number[]` 는 `readonly number[]` 의 서브 타입이다.
    * 가능 > `readonly number[]` 에 `number[]` 를 할당
    * 불가능 >  `number[]`에 `readonly number[]` 를 할당
* 위 예시로 설명
    * 타입스크립트는 매개변수(`nums`)가 함수(`arraySum`) 내에서 변경이 일어나는 지 체크
    * 호출하는 쪽(`printTriangles`)에서 매개변수(`nums`)를 변경 할 수 없도록 보장한다.
    * 호출하는 쪽(`printTriangles`)에서 함수에 readonly 배열(`readonly nums`)을 매개변수로 넣을 수 있다.
* readonly를 사용함으로써 명시적으로 매개변수를 변경하지 않는다고 가정(암묵적)
    * 하지만, 암묵적인 방법은 컴파일러와 개발자에게 좋지 못하다.
    * 결국 solution은 기존 배열을 건들지 않는 방향으로 로직 작성

```ts
function arraySum(arr: readonly number[]) {
    let sum = 0;

    for (const num of arr) {
        sum += num;
    }

    return sum;
}
```

## readonly 단점
* 매개변수가 readonly로 선언되지 않은 함수를 호출해야하는 경우도 있다.
* 함수를 readonly로 선언하여 매개변수 변경 없이 제어
    * 이 경우, readonly 함수를 호출하는 함수도 readonly로 만들어야한다.
* 라이브러리 함수를 호출하는 경우라면 타입 선언을 변경할 수 없기 때문에, 타입 단언을 사용해야 한다.

* 지역 변수와 관련된 모든 종류의 변경 오류를 방지할 수 있다.
    * readonly 속성을 사용하면, 배열에 변경을 가할 수 없기 때문에 오류가 방지된다.
    * 원본 배열에 변경을 가하지 않는 Array method를 사용하여, 오류를 줄인다.

* readonly는 shallow하게 동작한다.
    * 객체로 이루어진 readonly 배열이 있다면, 그 객체들은 readonly 속성이 아니다.
    * `Readonly<>` 도 마찬가지다.
```ts
const dates: readonly Date[] = [new Date()];
dates.push(new Date());

dates[0].setFullYear(2037);
```

```ts
interface Outer {
    inner: {
        x: number;
    }
}

const o: Readonly<Outer> = { inner: { x: 0 }};
o.inner = { x: 1 }; // Cannot assign to 'inner' because it is a read-only property.
o.inner.x = 1; // 정상
```

* readonly는 shallow하게 inner에 적용될 뿐, x까지 적용되지는 않는다.
```ts
type T = {
    readonly inner: {
        x: number;
    };
}
```

* 인덱스 시그니처에도 readonly를 사용할 수 있다.

* 제너릭을 활용하면 deep readonly를 사용할 수 있다.
    * Object literal 사용하는 경우 `as const` 도 고려사항
    * [type-challenge Deep Readonly](https://github.com/type-challenges/type-challenges/blob/main/questions/00009-medium-deep-readonly/README.md)
* `ts-essentials` 에 있는 DeepReadonly를 사용한다.
