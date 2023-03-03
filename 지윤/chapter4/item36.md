#### 아이템36. 해당 분야의 용어로 타입 이름 짓기

```typescript
// bad
interface Book {
  language: 'korean'
}

// good
interface Book {
  languageCode : LanguageCode;
}
type LanguageCode = 'en' | 'ko' | 'fr' | 'nl' | 'zh'
```

DO
* 표준 분류체계가 있다면 말을 만들어내지말고 해당 표준 분류체계를 타입으로 적극 활용하자.
* data, info, object 같은 모호하고 의미없는 이름을 쓰지 말자
