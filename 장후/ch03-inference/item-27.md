# Item 27.함수형 기법과 라이브러리로 타입 흐름 유지하기

* Lodash, Lamda 등의 라이브러리를 사용하면 타입 흐름이 전달되기 때문에 타입 정보가 잘 유지된다.
* 타입스크립트와 함께 서드파티 라이브러리를 사용하면 타입 정보를 파악해서 작업 속도가 빨라진다.
    * Lodash를 사용하면 타입에 대한 처리가 정확하게 된다.

```ts
const rowsB = rawRows.slice(1).map(rowStr => rowStr.split(',').reduce((row, val, i) => { return row[headers[i]] = val}, {}))
// string 인덱스 시그니처를 찾을 수 없음 error 발생

const rowsA = rawRows.slice(1).map(rowStr => _.zipObject(headers, rowStr.split(',')));
// type _.Ditionary<string>[]
```

* `Array.prototype.map` 대신 `_.map` 을 사용하는 것의 이점
    * 콜백 함수 전달하는 대신 속성의 이름을 전달해도 map을 사용한 것과 동일한 결과를 낼 수 있다.
    ```ts
    const namesA = allPlayers.map(player => player.name);
    const namesB = _.map(allPlayers, 'name')
    ```

* 이처럼 서드파티 라이브러리가 타입 정보를 잘 유지할 수 있는 이유는 매개변수 값을 건드리지 않고 새로운 값을 return 하면서, 새로운 타입을 반환한다.