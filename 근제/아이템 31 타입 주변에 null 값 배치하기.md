# 아이템 31 타입 주변에 null 값 배치하기

`strictNullChecks` 설정은 아래처럼 null 이나 undefined 값 관련된 오류들이 갑자기 나타나기 때문에, 오류를 걸러 내는 if 구문을 코드 전체에 추가해야 한다고 생각할 수 있습니다.

```typescript
let myVar: string = null; // Error: Type 'null' is not assignable to type 'string'.
```

어떤 변수가 `null`이 될 수 있는지 없는지를 타입만으로는 명확하게 표현하기 어렵기 때문입니다. 하지만 이 상황은 코드를 작성하는 사람과 타입 체커 모두에게 혼란스러울 것입니다. 아래 코드를 한번 봅시다.

```typescript
function extent(nums: number[]) {
  let min, max;
  for (const num of nums) {
    if (!min) {
      min = num;
      max = num;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num);
    }
  }
  return [min, max];
}
```

위 코드에 설계적 결함이 있고 `strictNullChecks`를 키면 오류가 바로 발생합니다.

- 최솟값이나 최대값이 0인 경우, 값이 덧씌워져 버립니다. `!0 === true`의 문제를 생각하면 됩니다. 따라서 extent([0,1,2])의 값은 [1,2]가 됩니다
- 빈 배열일 경우에는 [undefined, undefined]가 되어버립니다

undefined를 포함하는 객체는 다루기 어렵고 권장하지 않습니다. 조금 수정을 진행해봐야할 듯 합니다

```typescript
function extent(nums: number[]) {
  let result: [number, number] | null = null;
  for (const num of nums) {
    if (!result) {
      result = [num, num];
    } else {
      result = [Math.min(num, result[0]), Math.max(num, result[1])];
    }
  }
  return result;
}
```

extent는 결과값으로 단일 객체를 사용하면서 개선하였고 리턴값이 null인 경우를 저희는 구분할 수 있게 되어 함수를 이용하는데에서도 적당한 필터링이 가능해졌습니다.

null을 섞어 쓰는 곳의 문제는 클래스에서도 발생합니다. 아래와 같이 사용자와 사용자의 게시글을 나타내는 클래스를 만들어 봅시다.

```typescript
class UserPosts {
  user: UserInfo | null;
  posts: Post[] | null;

  constructor() {
    this.user = null;
    this.posts = null;
  }

  async init(userId: string) {
    return Promise.all([
      async () => (this.user = await fetchUser(userId)),
      async () => (this.posts = await fetchPostsForUser(userId)),
    ]);
  }
}
```

안전성을 위해 요청간 null인 상황을 대비하여 null속성까지 해줬지만 이렇게 할 경우 총 네가지의 경우가 존재하게 되고 속성값의 불확실성이 클래스의 모든 메서드에 안좋은 영향을 주게 됩니다. 모든 코드에 null체크를 계속하게 될 것입니다. 아래와 같이 수정하는 것이 좀 더 좋습니다

```typescript
class UserPosts {
  user: UserInfo;
  posts: Post[];

  constructor(user: UserInfo, posts: Post[]) {
    this.user = user;
    this.posts = posts;
  }

  static async init(userId: string): Promise<UserPosts> {
    const [user, posts] = Promise.all([
      fetchUser(userId),
      fetchPostsForUserId(userId),
    ]);
    return new UserPosts(user, posts);
  }
}
```

이제 UserPosts 클래스는 완전히 null이 아니며 메서드를 작성하기 쉬워졌습니다. 물론 부분적으로 준비된 경우가 있다면 경우를 체크해주긴 해야합니다.

- 요약
  - 한 값의 null 여부가 다른 값의 null 여부에 암시적으로 관련되도록 설계하면 안됩니다.
  - API 작성 시에는 반환 타입을 큰 객체로 만들고 반환 타입 전체가 null 이거나 null 이 아니게 만들어야 합니다. 사람과 타입 체커 모두에게 명료한 코드가 될 것입니다
  - 클래스를 만들 때는 필요한 모든 값이 준비되었을 때 생성하여 null이 존재하지 않도록 하는 것이 좋습니다
  - strictNullChecks를 설정하면 코드에 많은 오류가 표시되겠지만, null 값과 관련된 문제점을 찾아낼 수 있기 때문에 반드시 필요합니다
