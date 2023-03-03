# 아이템 38 any 타입은 가능한 한 좁은 범위에서만 사용하기

최대한 좁게 써야 합니다. 특히, `any`를 리턴해주는 상황은 정말로 피해야 합니다. 아래와 같은 상황을 한번 봅시다

```typescript
function f1() {
  const x: any = expressionReturningFoo();
  processBar(x);
  return x;
}

function g() {
  const foo = f1(); // foo는 any
  foo.fooMethod(); // 호출 체크가 되지 않는다! any 타입으로 인지 되기 때문이다
}

// 그러니 차라리 아래와 같이 하는게 좋다
function f1() {
  const x = expressionReturningFoo();
  processBar(x as any);
  return x;
}

// 아니면 타입스크립트에게 무시하도록 할 수도 있습니다
function f1() {
  const x = expressionReturningFoo();
  // @ts-ignore
  processBar(x);
  return x;
}
```

- 요약
  - 의도치 않은 타입 안전성의 손실을 피하기 위해서 any의 사용 범위를 최소한으로 좁혀야 합니다.
  - 함수의 반환 타입이 any인 경우 타입 안정성이 나빠집니다. 따라서 any 타입을 반환하면 절대 안 됩니다
  - 강제로 타입 오류를 제거하려면 any대신 @ts-ignore 사용하는 것이 좋습니다
