#### 아이템45. devDependencies에 typescript와 @types 추가하기

npm은 자바스크립트 라이브러리 저장소와 프로젝트가 의존하고 있는 라이브러리들의 버전을 지정하는 package.json을 제공한다.

```json
// package.json
{
  "dependencies": {},
  "devDependencies": {},
  "peerDependencies": {}
}
```

`dependencies`는 프로젝트를 실행하는 데(=런타임에) 필수적인 라이브러리들이 들어간다.
`devDependencies`는 프로젝트를 개발하고 테스트하는데는 필요하지만, 런타임에는 필요 없는 라이브러리들이 들어간다.
`peerDependencies`는 런타임에 필요하긴 하지만, 의존성을 직접 관리하지 않는 라이브러리들이 포함된다. 단적인 예로 플러그인들을 들수 있다.

타입스크립트는 개발 도구이고, 타입 정보는 런타임에 들어가지 않아, devDependencies에 추가되어야 한다.
