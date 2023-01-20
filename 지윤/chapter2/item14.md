#### 아이템14. 타입 연산과 제너릭 사용으로 반복 줄이기

`DRY: do not repeat yourself`  타입 또한 DRY 원칙을 지켜 중복 코드를 피하자.

* 기존 타입 확장해서 새로운 타입 명명하기
```typescript
interface Person {
  name: string;
  age: number;
}

// bad
interface Student {
  name: string;
  age: number;
  major: string;
}

// good - 확장을 사용하자
interface Student extends Person {
  major: string;

}
// good - 인터섹션 연산자를 사용하자
type Student = Person & { major: string }
```

* 기존 타입에서 부분 타입을 사용한다면, 기존 타입의 부분집합 타입 만들기
```typescript
interface User {
  id: number;
  name: string;
  age: number;
  birthday: Date;
  hobby: string;
  address: string;
}

// good - 인덱싱
interface UserBaseInfo {
  id: User['id'];
  name: User['name'];
  age: User['age'];
}

// better - 매핑된 타입
interface UserBaseInfo {
  [k in 'id' | 'name' | 'age'] : User[k]
}

// better - 유틸리티 타입
type UserBaseInfo = Pick<User, 'id' | 'name' | 'age'>
```

* 전체 속성을 optional한 값으로 만들고 싶다면,
```typescript
interface AllRequired {
  name: string;
  age: string;
  address: string;
}

type AllOptional = { [k in keyof AllRequired]?: AllRequired[k] };
type AllOptiinal = Partial<AllRequired>
```

* 값으로부터 타입을 추론하고 싶다면,
```typescript
const DEFAULT_FORM_VALUES = {
  title: '설 연휴, 무엇을 먹을 예정인가요?',
  description: '설 연휴에 어떤 음식을 주로 먹는지 조사해보자.',
};

type FormValues = typeof DEFAULT_FORM_VALUES;
```

* 함수나 메서드 반환값으로부터 타입을 추론하고 싶다면,
```typescript
const getUser = (userId: string) => {
  ...
  return {
    id: 1,
    name: 'peter',
    age: 20,
    isAdmin: false,
  };
};

type User = ReturnType<typeof getUser>;
```

함수는 DRY 원칙을 지킬 때 유용하게 사용되는데(=재사용성이 좋으므로), 제너릭 타입은 타입의 함수와 같으니, 타입의 재사용성을 위해서는 제너릭 타입을 잘 사용하자. 다만, 제너릭 타입이 너무 광범위한 범위를 가지는 경우가 많으니, 범위를 제한하기 위해서는 extends를 사용하자.
```typescript
// bad - K가 T와 무관하고 범위가 너무 넓다 - Error: Type 'key' cannot be used to index type 'T'
type Pick<T,K> = {[key in K]: T[key]}

// good
type Pick<T, K extends keyof T> = {[key in K]: T[key]}
```

