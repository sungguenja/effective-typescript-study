#### 아이템39. any를 구체적으로 변형해서 사용하기

any는 숫자, 문자, 배열, 객체, 정규식, 함수, 클래스, dom element, null, undefined 등 모든 타입의 값을 받을 수 있다. 

보통의 경우, any 보다 더 구체적으로 표현할 수 있는 타입이 있을 가능성이 매우 크기에, 더 구체적인 타입을 지정해 타입 안정성을 높여아한다.

```typescript
// bad 악
const getArrayLength = (array: any) => {
  return array.length // any를 반환
}

// good...차악
const getArrayLength = (array: any[]) => {
  return array.length // number을 반환
} 

// 배열의 배열 - any[][]
// 객체 - { [key: string]: any } = 키 접근 가능
// 객체 - object = 키 접근 불가
// 객체 - unknown = 속성 접근 불가

// bad 악
type fn = any

// good...차악
type fn = () => void; // 매개변수가 없군
type fn = (arg: any) => any // 매개변수가 있군
type fn = (...args: any[]) => any 
```



