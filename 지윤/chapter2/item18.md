#### 아이템18. 매핑된 타입을 사용하여 값을 동기화하기

한 객체와 다른 객체가 정확하게 같은 속성을 강제하고 싶다면 타입 체커가 동작하도록 매핑된 타입을 사용해보자. 
```typescript
interface Plant {
  id: number;
  name: string;
  age: number;
  someTrouble: any;
}

const REQUIRES_UPDATE: { [key in keyof Plant]: boolean } = {
  id: false,
  name: false,
  age: false,
  someTrouble: true,
};
```

위의 예시에서, Plant에 새로운 타입 속성을 추가하면 에러가 발생한다.
```typescript
interface Plant {
  ...
  anotherTouble: any; // 새로 추가한 타입
}

const REQUIRES_UPDATE: { [key in keyof Plant]: boolean } = { // Error: Property 'anotherTouble' is missing in type '{ id: false; name: false; age: false; someTrouble: true; }' but required in type '{ id: boolean; name: boolean; age: boolean; someTrouble: boolean; anotherTouble: boolean; }'
  id: false,
  name: false,
  age: false,
  someTrouble: true,
};
```

잔잔바리 잡담
* 실패에 닫힌 접근법과 실패에 열린 접근법이란. 오류 발생 시에 대처하는 방향을 이야기한다. 실패에 닫힌 접근법이란, 오류에 적극적으로 대처하는 방식을 이야기하고, 실패에 열린 접근법이란, 오류 발생시에 소극적으로 대처하는 것을 뜻한다. 