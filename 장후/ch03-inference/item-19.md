# Item 19. 추론 가능한 타입을 사용해 장황한 코드 방지하기

* 타입 추론이 된다면 타입 명시는 필요하지 않다.
* 타입스크립트는 예상보다 더 정확하게 추론한다.
* 타입스크립트의 타입 추론 타이밍은 일반적으로, 변수가 처음 등장할 때이다.

```ts
const x: string = 'x'; // type string
const y = 'y' // type y
```

* string보다 y가 더 정확한 추론이다.
* 정보가 부족하여 타입 추론이 불가능한 경우에는 명시된 타입이 필요하다.
    * 아래와 같이 매개변수 타입 지정 등
```ts
interface Product {
    id: string;
    name: string;
    price: number;
}

function logProduct(product: Product) {
    const { id, name, price } = product;
}
```

* 이상적인 타입스크립트 코드는 함수/메서드 시그니처에서 타입 구문을 포함하지만, 함수 내에서 생성된 지역 변수에는 타입 구문을 넣지 않는 것이다.
* ***타입 구문을 생략하여, 코드를 읽는 사람이 로직에 집중할 수 있게 하는 것이 좋다.***
* 매개변수에 기본 값이 있는 경우에는 타입 명시를 생략한다.

```ts
function parseNumber(str: string, base = 10) {
    ...
}
```

* 타입이 정해져 있는 라이브러리에서, 콜백함수의 매개변수 타입은 추론된다.

```ts
// bad
app.get('/endpoint', (request: express.Request, response: express.Response) => { response.send('OK') });

// good
app.get('/endpoint', (request, response) => { response.send('OK') });
```

* Object literal의 경우 타입 추론이 되지만, 잉여 속성 체크를 위해 타입을 명시한다.
    * 변수를 할당하는 시점에 타입 체크를 한다.
    * 타입을 명시하지 않는 다면 잉여 속성 체크는 동작하지 않고, 객체가 사용되는 곳에서 타입 체크가 된다.
```ts
const elmo: Product = {
    name: 'Tickle Me Elmo',
    id: '121212',
    price: 100000
}
```

* 함수 return 값에도 타입을 명시하여 오류를 방지한다.
    * 추론이 가능하더라도, 함수를 호출한 곳에 영향을 주지 않기 위해 명시하는 것이 좋다.
    * 함수에 대한 입력, 출력 등 명확한 정보 파악이 가능하다.
    * 반환 타입을 명시함으로써, 직관적인 표현이 된다.
    