# Item 22. 타입 좁히기

* 타입을 좁혀가는 과정을 의미한다.

1. null check
    * if 분기문을 통해 null check 후 타입을 좁힌다.
```ts
const el = document.getElementById('foo'); // type HTMLElement | null

if (el) {
    el.innerHTML = 'hi'; // type HTMLElement
} else {
    alert('No Element'); // type null
}
```

2. instanceof
    * instanceof를 기준으로 분기하여 타입을 좁힌다.
```ts
function contains(text: string, search: string | RegExp) {
    if (search instanceof RegExp) {
        return !!search.exec(text); // type RegExp
    } else {
        return text.includes(search); // type string
    }
}
```

3. 속성 체크
```ts
interface A { a: number; };
interface B { b: number; };

function pickAB(ab: A | B) {
    if ('a' in ab) {
        console.log(ab); // type A
    } else {
        console.log(ab); // type B
    }

    console.log(ab); // type A | B
}
```

4. 내장 함수
```ts
function contains(text: string, terms: string | string[]) {
    const termList = Array.isArray(terms) ? terms : [term]; // type string[]
}
```

5. 명시적 태그 (tagged union or discriminated union)
```ts
interface UploadEvent { type: 'upload'; filename: string; contents: string; };
interface DownloadEvent { type: 'download'; filename: string; };

type AppEvent = UploadEvent | DownloadEvent;

function handleEvent(e: AppEvent) {
    switch (e.type) {
        case 'download':
            consloe.log(e) // type DownloadEvent
            break;
        case 'upload':
            console.log(e) // type UploadEvent
            break;
    }
}
```

6. 커스텀 함수 도입 (사용자 정의 타입 가드)
```ts
function isInputElement(el: HTMLElemnt): el is HTMLInputElement {
    return 'value' in el;
}

function getElementContent(el: HTMLElement) {
    if (isInputElement(el)) {
        return el.value // type HTMLInputElement
    } else {
        return el.textContent // type HTMLElement
    }
}
```

* 배열 탐색 수행 시 사용
    * filter는 타입 넓히기에 의해 `(type | undefined)[]` 로 추론된다.
    ```ts
    const jackson5 = ['Jackie', 'Tito', 'Jermaine', 'Marlon', 'Michael'];

    const members = ['Janet', 'Michael'].map(who => jackson5.find(n => n === who).filter(who => who !== undefined)); // type (string | undefined)[]
    ```
    * type guard를 통해 type을 좁혀야한다.
    ```ts
    function isDefined<T>(x: T | undefined): x is T { return x !== undefined; }

    const members = ['Janet', 'Michael'].map(who => jackson5.find(n => n === who).filter(isDefined)); // type string[]
    ```

## Reference
https://github.com/microsoft/TypeScript/issues/20812

https://www.benmvp.com/blog/filtering-undefined-elements-from-array-typescript/