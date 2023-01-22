#### 아이템13. 타입과 인터페이스의 차이점 알기

타입스크립트에서 타입을 명시하는 방법은 두가지가 있다.
```typescript
// type alias
type TPerson = {
  name: string;
  age: number
}

// interface
interface IPerson {
  name: string;
  age: number
}
```

**type alias와 interface의 비슷한 점**
* 잉여 속성 체크가 된다
* 인덱스 시그니처를 사용할 수 있다
* 함수의 타입으로 정의할 수 있다.
* 제너릭이 가능하다
* 인터페이스는 타입을 확장할 수 있고, 타입은 인터페이스를 확장할 수 있다.
  * 주의할 점은, 인터페이스는 유니언 타입같은 복잡한 타입은 확장하지 못한다. 복잡한 타입을 확장하고 싶다면, type alias를 사용하자.
* 클래스 구현(implements)시에 사용 가능하다.

**type alias와 interface의 차이점**
* 타입은 유니언 타입이 있지만, 인터페이스는 없다.
  * 인터페이스는 타입을 확장할 수 있지만, 유니언 타입을 확장할 수 없다.
* 타입은 유니언이 될 수 도, 매핑된 타입 혹은 조건부 타입 같은 고급 기능에 활용된다.
* 타입은 튜플과 배열 타입을 나타낼 수 있다.
  ```typescript
  type Pair = [number, number];
  type Strings = string[];
  type NamesNums = [string, ...number[]];
  ```
  * 인터페이스로도 튜플을 비슷하게 구현할 순 있으나, 인터페이스로 튜플을 구현하면, 튜플에서 사용할 수 있던 메소드들을 사용하지 못한다.
    ```typescript
    // interface 튜플
    interface Pair {
      0: number;
      1: number;
      length: 2;
    }
    const ab: Pair = [1, 2]; // ab. -> 0,1,length를 선택할 수 있다. 메소드는 없다.
    
    // type 튜플
    type Pair = [number, number]
    const ab: Pair = [1, 2]; // ab. -> 0,1,at,concat,filter,findIndex...
    ```
* 인터페이스는 선언 병합이 가능하다.
  * 동일한 interface name으로 다시 선언하면 이전에 선언했던 interface와 저절로 merge가 된다. 

**그러면 type과 interface 중 사용해야 하는 것은 무엇?**
* 복잡한 타입(-유니언타입, 매핑된 타입 등등)이라면 type alias를 사용해라. 
* 전반적인 프로젝트 코드의 일관성을 생각해라. 
* api에 대한 타입 선언은 추후 보강의 가능성을 두고 인터페이스로 선언하는 것이 좋다.
* 프로젝트 내부에서 사용되는 타입에 선언 병합이 생기는 것은 잘못된 설계이므로 타입을 사용하는 것이 좋다. 