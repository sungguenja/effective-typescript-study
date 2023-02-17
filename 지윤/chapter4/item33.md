#### 아이템33. string 타입보다 더 구체적인 타입 사용하기

string 타입은 범위가 너무 넓다. string 타입을 줄 때에는 더 좁은 범위는 없는지 생각해보자. stringly typed 하지 말자.
```typescript
// bad - stringly typed
interface Person {
  name: string;
  type: string;
  birth: string;
  address: string;
}

// good
/** 성인인지 아닌지 */
type PersonType = 'child' | 'adult';
interface Person {
  name: string;
  type: PersonType;
  birth: Date;
  address: string;
}
```

string이 아닌 PersonType과 같은 좁은 타입을 사용했을 경우의 장점
* 값의 사용처에서 명시된 타입의 정보를 받아볼 수 있다.
* 주석을 달아 타입을 설명할 수 있다.
* 객체 속성 체크를 할 땐 keyof 연산자로 할 수 있다.



스트링은 범위가 넓어 any스럽다. 구체적으로 좁힐 수 있다면 더 좁은 타입을 사용하자.
```typescript
// bad
const getObjectValue = ({objects, key}: {objects: any, key: string}) => {
  return object[key] // object의 key가 아닌 엉뚱한 string이 들어올 수 있다. - undefined
}

// good
const getObjectValue = <T,>({object, key}: {object: T, key: keyof T}) => {
  return object[key]
}

// better
const getObjectValue = <T, K extends keyof T,>({object, key}: {object: T, key: K}): T[K] => {
  return object[key]
}
```

