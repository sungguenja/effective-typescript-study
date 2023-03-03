#### 아이템43. 몽키 패치보다는 안전한 타입을 사용하기

자바스크립트의 특징 중 하나는, 객체와 클래스에 임의의 속성을 추가할 수 있을만큼 유연하다는 것이다. 이 특징을 사용해 종종 window나 document에 값을 할당해 전역 변수를 만든다.

```javascript
document.monkey = 'tada';
```

```typescript
document.monkey = 'tada'(
  // Error: Property 'monkey' does not exist on type 'Document'.

  document as any
).monkey = 'tada';
```
