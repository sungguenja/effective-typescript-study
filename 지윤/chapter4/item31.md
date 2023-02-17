#### 31. 타입 주변에 null 값 배치하기

```typescript
// bad - null 여부가 파생되는 변수만들기
let a: number | null
const b = a * 2 

// bad - 일부 값만 nullable 체크하기
const getMinMax = (nums: number[]) => {
  let min, max;
  for (const num of nums) {
    if (!min) { // min null 체크
      min = date
      max = date;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num); // Error: Type 'undefined' is not assignable to type 'number'. - max도 nullable 하나 체크를 안함
    }
  }
  return [min, max] 
}
  
const [min, max] = getMinMax([1,24,2])
const a = max - min // Error: Object is possibly 'undefined'.

// better - 값의 객체를 만들어 객체에 nullable 체크하기
const getMinMax = (nums: number[]) => {
  let res: [number, number] | null = null;
  for (const num of nums) {
    if (res == null) {
      res = [num, num];
    } else {
      res = [Math.min(num, res[0]), Math.max(num, res[1])];
    }
  }
  return res;
};

// 대신 단언 사용 해줘야한다. 혹은 조건문으로도 null 체크해줘서 사용 가능
const [min, max] = getMinMax([1, 24, 2])!; 
const a = max - min;

// bad - null과 null이 아닌 값 섞어쓰기
class UserPosts {
  user: User | null;
  posts: Post[] | null;
  
  constructor() {
    this.user = null;
    this.posts = null
  }
  
  async init(id: string) {
    return Promise.all([
      async () => this.user = awiat fetchUser(id),
      async () => this.posts = awiat fetchPosts(id)
    ])
  }
  
  getUserPosts() {
    // null 체크가 난무하게 되는 상황...
    // - user가 null이고, posts도 null
    // - user가 null이고, posts는 not null
    // - user가 not null이고, posts는 null
    // - user가 not null이고, posts도 not null
  }
} 
// 위와 같은 상황을 마주하지 말자. - 데이터가 준비되면 사용하자. 
```

생각)
react-query 사용시에 useQuery 반환 값이 T | undefined 인 거 어떻게 처리들 하고 있나요? 

