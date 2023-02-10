#### 아이템23. 한꺼번에 객체 생성하기

객체의 속성들의 정확한 타입 추론을 원한다면, 속성을 하나씩 추가하기 보단, 생성 시점에 한꺼번에 넣어 만드는 것이 유리하다.
```typescript
interface Box {
  width: number;
  height: number;
}

// 타입체커를 통과하는 객체 생성 방법1
const box: Box = { width: 200, height: 100 };

// 타입체커를 통과하는 객체 생성 방법2
const box = {} as Box;
box.width = 200;
box.height = 100;

// 객체들을 하나의 큰 객체로 만들고 싶다면 - 전개 연산자(Spread operator)
const room = {
  roomNumber: 1301,
};

const options = {
  earlyCheckin: true,
  lateCheckout: false,
};

const roomWithOptions1 = { ...room, ..options };

// 조건부로 속성을 추가할 경우
const isRoomService = true;
const roomWithOptions2 = { ...roomWithOptions1, ...(isRoomService ? { roomService: true } : {}) };
```

조건부로 추가한 속성을 옵셔널로 바꿔주는 헬퍼 함수 -> ??? 근데 이게 필요한가? 애초에 타입스크립트가 옵셔널한 필드로 추론을 해주는데???
```typescript
const addOptional = <T extends object, U extends object>(a: T, b: U | null): T & Partial<U> => {
  return { ...a, ...b };
};
```

