#### 아이템43. 몽키 패치보다는 안전한 타입을 사용하기

자바스크립트의 특징 중 하나는, 객체와 클래스에 임의의 속성을 추가할 수 있을만큼 유연하다는 것이다. 이 특징을 사용해 종종 window나 document에 값을 할당해 전역 변수를 만든다.

```javascript
document.monkey = 'tada';
```

```typescript
document.monkey = 'tada'(
  // Error: Property 'monkey' does not exist on type 'Document'.

  // bad - 에러는 안남
  document as any
).monkey = 'tada';

// good - interface 선언병합(보강) 사용하기 - 타입 안전, 주석 사용 가능, 자동완성 사용 가능, 몽키 패치 기록 가능
interface Document {
  monkey: string;
}
document.monkey = 'tada';

// good - 보다 구체적 타입 단언문 사용하기
interface MonkeyDocument extends Document {
  monkey: string;
}
(document as MonkeyDocument).monkey = 'tada';
```
