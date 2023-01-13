#### 아이템8. 타입 공간과 값 공간의 심벌 구분하기

타입스크립트에는 타입 공간과 값 공간이 구분되어 심벌이 저장된다. 그렇기에 동일한 심벌 네임을 가지더라도 중복되었다는 오류 없이 선언할 수 있다.

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

동일한 심벌 네임을 가졌을 때, 이것이 값을 나타내는 심벌인지/타입을 나타내는 심벌인지 구분할 수 있어야 한다.

보통 타입 심벌은 inerface, type, 타입 선언(:), 타입 단언(as) 뒤에 위치한다. 값 심벌은 const나 let 뒤에 위치한다.



class나 enum 뒤에 위치한 심벌은 상황에 따라 값 심벌이 되기도, 타입 심벌이 되기도 한다. 클래스가 타입으로 쓰이면 속성과 메서드가 사용되고, 값으로 쓰이면 new 생성자 연산자와 함께 사용된다.

 ~~-> 이 부분 예제 보충이 필요하다. + enum 에 대한 예제도 같이 넣어보자~~





