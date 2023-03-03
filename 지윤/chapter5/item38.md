#### 아이템38. any 타입은 가능한 한 좁은 범위에서만 사용하기

```typescript
const Foo타입만_받는_함수 = (x: Foo) => {...};

// bad
const a: any = Bar타입을_뱉는_함수(); 
Foo타입만_받는_함수(a);

// better
const a = Bar타입을_뱉는_함수();
Foo타입만_받는_함수(a as any);

/*
	- Foo타입만_받는_함수의 매개변수로 사용되는 a를 단언하는 것이 더 좋다.
	- a가 쓰이는 다른 사용처에서는 any 타입 단언의 영향을 안받고 원래의 Bar 타입으로 사용될 수 있기 때문이다.
*/
                                  
// better
const a = Bar타입을_뱉는_함수();
// @ts-ignore
Foo타입만_받는_함수(a);
```

```typescript
// bad
const apple = {
  a: string;
  b: string;
  c : {
  	something: value	
	}
} as any

// better
const apple = {
  a: string;
  b: string;
  c : {
  	something: value as any
	}
}
```

any 사용법 정리
* 타입 안정성을 지키기 위해 any는 불가피한 경우에만 사용하자. 
* 특히나 함수의 반환 타입만큼은 any를 사용하지 말자. 
* any는 가능한 좁은 범위로 사용하자.
* 강제로 타입 오류를 제거하고 싶다면 any 말고 차라리 @ts-ignore를 쓰자.

