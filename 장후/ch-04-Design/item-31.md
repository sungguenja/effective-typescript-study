# Item 31. 타입 주변에 null 값 배치하기

* 값이 전부 null이거나 null이 아닌 경우 타입 모델링을 하기 더 쉽다.

```ts
// before
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

* 위 코드의 2가지 문제점
    1. 최솟값이나 최댓값이 0인 경우 원하는 결과가 나오지 않는다.
    2. return 값이 `[undefined, undefined]` 로 나올 수 있다.

```ts
// after
function extent(nums: number[]) {
    let result: [number, number] | null = null;

    for (const num of nums) {
        if (!results) {
            result = [num, num];
        } else {
            result = [Math.min(num, result[0]), Math.max(num, result[1])];
        }
    }

    return result;
}
```

* 위의 코드는 return 타입이 2가지로 명확해졌다.
    * 타입 단언 혹은 null check를 통해 min, max에 대한 원하는 로직을 수행할 수 있다.


* 만약 null과 null이 아닌 값을 섞어 쓴다면 도출되는 경우의 수가 많아지기 때문에 로직 상 혼란이 올 가능성이 크다.

```ts
// before
class UserPosts {
    user: UserInfo | null;
    posts: Post[] | null;

    constructor() {
        this.user = null;
        this.posts = null;
    }

    async init(userId: string) {
        return Promise.all([
            async () => this.user = await fetchUser(userId),
            async () => this.posts = await fetchPosts(userId)
        ])
    }
}
```

* 위의 코드는 총 4가지의 경우의 수가 나오기 때문에, null 체크를 동반해야하며 버그 발생 확률이 높다.

```ts
// after
class UserPosts {
    user: UserInfo;
    posts: Post[];

    constructor(user: UserInfo, posts: Post[]) {
        this.user = user;
        this.posts = posts;
    }

    // static은 instance 생성 없이 사용 가능
    static async init(userId: string) {
        const [user, posts] = Promise.all([
            await fetchUser(userId),
            await fetchPosts(userId)
        ]);

        return new UserPosts(user, posts);
    }
}
```

* 위의 코드는 user와 posts의 타입이 명확하기 때문에, null 체크 등의 추가 로직이 필요없다.
