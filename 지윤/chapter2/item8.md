#### 아이템8. 타입 공간과 값 공간의 심벌 구분하기

타입스크립트에는 타입과 값의 공간이 구분되어 심벌이 저장된다. 그렇기에 동일한 심벌 네임을 가지더라도 중복되었다는 오류 없이 선언할 수 있다.
```typescript
type Apple = {
	color: 'red',
	price: 2000
}

const Apple = 'apple'
```

같은 공간에 동일한 이름의 심벌을 선언할 순 없다. 
```typescript
// Error: Duplicate identifier 'Apple'.
type Apple = {
  color: 'red';
  price: 2000;
};

type Apple = {
  color: 'green';
  price: 2000;
};
```

```typescript
// Error: Cannot redeclare block-scoped variable 'Apple'.
const Apple = 'redApple'
const Apple = 'greenApple'
```

동일한 네임을 가졌을 때, 이것이 값인지/타입인지 구분할 수 있어야 한다.
보통 타입은 inerface, type, 타입 선언(:), 타입 단언(as) 뒤에 위치한다. 값은 const나 let 뒤에 위치한다.

class나 enum 뒤에 위치한 심벌은 상황에 따라 값이 되기도, 타입이 되기도 한다. 클래스가 타입으로 쓰이면 속성과 메서드가 사용되고, 값으로 쓰이면 new 생성자 연산자와 함께 사용된다.
```typescript
class Person {
    constructor (public firstName: string, public LastName: string, public age: number) {}

    getFullName() {
        return `${this.firstName} ${this.LastName}`;
    }
}

// 값으로 쓰인 class
const person = new Person('지윤', '박', 20)
// 타입으로 쓰인 class
const printPerson = (person: Person) => {
   console.log(`안녕 ${person.age}살의 ${person.getFullName()}`)
}
```

```typescript
enum Color {
  RED,
  YELLOW,
  GREEN,
  BLUE,
}

// 타입 및 값으로 쓰인 Enum
const color: Color = Color.BLUE
```

class와 enum처럼 값으로도 쓰일 수 있고, 타입으로도 쓰일 수 있는 연산자들이 있다. 
```typescript
interface Person {
    name: string;
    age: number;
}
const p: Person = {name: '지윤', age: 20}

type personType = typeof p // Person
const p1 = typeof p; // "string" | "number" | "bigint" | "boolean" | "symbol" | "undefined" | "object" | "function" -> 런타임이 돌면 object로 반환할 것.
```

속성 접근자인 [] 는 타입으로 쓰일 때에도 동일하게 동작하나, obj['field']와 obj.field는 값이 동일하더라도 타입은 다를 수 있으니, 타입의 속성을 얻을 때에는 반드시 obj['field'] 방식을 사용해야 한다.
```typescript
const name: Person['name'] = p['name']
const name: Person['name'] = p.name
```



타입스크립트 구조 분해 할당 시에도, 값과 타입을 구분해서 명시해주어야 한다.
```typescript
const printFullName = ({firstName, lastName}: {firstName: string, lastName: string}) => {
  console.log(`${lastName} ${firstName}`)
}
```

