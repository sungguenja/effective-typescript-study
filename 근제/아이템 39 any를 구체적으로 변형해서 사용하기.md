# 아이템 39 any를 구체적으로 변형해서 사용하기

any는 자바스크립트에서 표현할 수 있는 모든 값을 아우르는 매우 큰 범위의 타입입니다. 진짜 피하는 것이 좋은데 써야한다면 그래도 조금 구체적으로 더 써서 안전성을 가지는 것이 좋습니다. 아래와 같은 상황이라면 체크를 해봅시다

```typescript
function getLengthBad(arr: any) {
  // 이건 좀...
  return arr.length;
}

function getLengthBad(arr: any[]) {
  return arr.length;
}
```

- 아래가 더 좋은 이유
  - 함수 내의 `arr.length` 타입이 체크됩니다
  - 함수의 반환 타입이 any대신 number로 추론됩니다
  - 함수 호출될 때 매개변수가 배열인지 체크됩니다

객체를 다가갈 때도 비슷합니다. 함수의 props의 타입을 any보단 `{[key: string]: any}`를 이용해주자. (object로 하면 접근할 수 없다)

- 요약 any를 사용하 ㄹ떄는 정말로 모든 값이 허용되어야하는가를 면밀히 검토합시다
- any보다 더 정확하게 모델링 할 수 있도록 다양한 방법을 고민해봅시다
