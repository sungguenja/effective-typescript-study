# Item 14. 타입 연산과 제너릭 사용으로 반복 줄이기
* DRY(Don't repeat yourself)는 코드 뿐만아니라 타입에서도 고려해야한다.
* 타입 간 매핑하는 방법에 익숙해지자


## 14-1. 타입에 이름을 붙인다.
* 상수 활용과 유사한 방법이다.
```ts
interface Position { x: number; y: number; };

function distance (a: Position, b: Position) {
    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
}
```

* 동일한 함수 시그니쳐를 가지는 경우
```ts
type HTTPFuncion = (url: string, opt: Options) => Promise<Response>;

const get: HTTPFuntion = (url, opt) => {};
const post: HTTPFuntion = (url, opt) => {};
```

* 인터페이스 확장
```ts
interface Person {
    firstName: string;
    lastName: string;
}

interface PersonWithBirth extends Person {
    birth: Date;
}

const info: PersonWithBirth = {
    firstName: 'Terry',
    lastName: 'Jerry',
    birth: new Date('2023-01-01');
}
```

* 타입 확장
    * 유니온 확장 시 유용하다.
```ts
type PersonWithBirth = Person & { birth: Date };
```

## 14-2. 매핑된 타입 활용: Pick
```ts
interface State {
    userId: string;
    pageTitle: string;
    recentFiles: string[];
    pageContents: string;
};

interface TopNavState {
    userId: string;
    pageTitle: string;
    recentFiles: string;
}
```

* 전체 상태에 대한 타입 `State` 와 부분 상태에 대한 타입 `TopNavState`
* 이러한 경우 부분 상태인 `TopNavState` 를 확장하는 것보다 `State` 의 부분 집합 개념을 활용하는 것이 전체 앱 상태를 하나의 인터페이스로 유지하는데 좋다.

* 인덱싱 활용
    ```ts
    type TopNavState = { 
        userId: State['userId']; 
        pageTitle: State['pageTitle']; 
        recentFiles: State['recentFiles'];
    }
    ```
    * 하지만 중복되는 코드가 번거롭다.

    ```ts
    type TopNavState = {
        [k in 'userId' | 'pageTitle' | 'recentFiles']: State[k]
    }
    ```

    * 매핑된 타입은 루프 도는 것과 같은 방식이다.
    * 표준 라이브러리에서는 `Pick` 기능 제공

    ```ts
    type Pick<T, K> = { [k in K]: T[k]};

    type TopNavState = Pick<State, 'userId' | 'pageTitle' | 'recentFiles'>;
    ```

* 유니온에서도 중복이 발생할 수 있다.
```ts
interface SaveAction {
    type: 'save',
    ...
}

interface LoadAction {
    type: 'load',
    ...
}

type Action = SaveAction | LoadAction;
```
* Action 유니온을 인덱싱하면 Action의 type을 정의할 수 있다.
```ts
type ActionType = Action['type'] // "save" | "load"
```

* Pick을 사용하면 type을 속성으로 가지는 인터페이스가 생성된다.
```ts
type ActionRec = Pick<Action, 'type'>; // { type: "save" | "load"; }
```

## 14-3. 선택적 속성: Partial
* 생성 이후 데이터가 업데이트되는 클래스 정의하는 경우
    * update 메서드의 arguments들은 대부분 선택적 필드가된다.
```ts
interface Options {
    width: number;
    height: number;
    color: string;
    label: string;
}

interface OptionsUpdate {
    width?: number;
    height?: number;
    color?: string;
    label?: string;
}

class UIWidget {
    constructor(init: Options) {};

    update(options: OptionsUpdate) {};
}
```
* 매핑된 타입`[k in keyof Options]`과 keyof를 사용하면 Options를 활용해 OptionsUpdate를 만들 수 있다.
    * 매핑된 타입은 순회하며 Options에 k에 해당하는 속성이 있는지 찾는다.
    * `?` 는 선택적 속성 부여    
```ts
type OptionsUpdate = { [k in keyof Options]?: Options[k] };

type OptinsKeys = keyof Options; // 'width' | 'height' | 'color' | 'label'
```

* `Partial`
```ts
type OptionsUpdate = Partial<Options>

class UIWidget {
    constructor(init: Options) {};

    update(options: OptionsUpdate) {};
}
```

## 14-4. 값의 형태에 해당하는 타입 정의
```ts
const OPTIONS = {
    width: 600,
    height: 300,
    color: '#000000'
}

interface Options {
    width: number;
    height: number;
    color: string;
}

type Options = typeof OPTIONS;
```

* 값으로 부터 타입을 만들 때는 타입 정의를 먼저하고 값이 그 타입에 할당할 수 있게끔 선언

## 14-5. return 값 타입: ReturnType

```ts
const getUserInfo = (userId: string) => {
    return {
        name: 'Jerry',
        age: 20
    }
}
```
* 추론된 return 타입은 `{ name: string; age: number; }`

```ts
type UserInfo = ReturnType<typeof getUserInfo>;
// { name: string; age: number; }
```
* ReturnType에는 함수의 값(getUserInfo)이 아닌 함수의 타입(typeof getUserInfo)이 적용되었다.
* 제너릭에 들어가는 값이 값인지 타입인지 구분해서 사용하낟.

## 14-6. 제너릭 타입
* 제너릭 타입에서 매개변수를 제한할 방법이 필요하다.
* extends 사용하여 특정 타입을 확장한다고 명시

```ts
interface Name { first: string; last: string; };

type User<T extends Name> = [T, T];

const userInfoData: User<Name> = [
    { first: 'hi', last: 'bye' },
    { first: 'Terry', last: 'Jerry' }
]
```

* Pick의 경우에도 K의 할당 범위를 T의 key로 제한해서 사용해야한다.
```ts
type Pick<T, K extends keyof T> = { [k in K]: T[k]}
```

```ts
type TestPick<T, K extends keyof T> = { [k in K]: T[k] };

interface Name { first: string; last: string; };

type FirstName = TestPick<Name, 'first' | 'last'>;

type FirstMiddle = TestPick<Name, 'first' | 'middle'>; // Error
  /** 
   * Type '"first" | "middle"' does not satisfy the constraint 'keyof Name'.
   * Type '"middle"' is not assignable to type 'keyof Name'.
  */
```

## Summary
* DRY 원칙을 타입에서도 준수하자
* 타입에 이름을 붙여서 반복을 피한다.
* extends 활용
* 타입 매핑을 위해, `typeof`, `keyof`, 인덱싱, 매핑된 타입 등의 도구를 활용한다.
* 제너릭을 활용하여 타입 매핑하자. 단, extends를 활용하여 타입을 제한해서 사용하자
* `Pick`, `Partial`, `ReturnType` 등의 표준 라이브러리 활용하자


## Reference
[Mapped-types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

