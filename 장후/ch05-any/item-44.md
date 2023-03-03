# Item 44. 타입 커버리지를 추적하여 타입 안전성 유지하기

`noImplicitAny` 설정 및 명시적 타입 구문을 추가하여도 `any` 타입은 존재할 수 있다.

1. 명시적 any 타입
2. 서드파티 타입 선언

프로젝트 내 any 타입 존재 가능성을 줄이기 위해 `type-coverage` 패키지를 활용하여 any를 추적한다.

```bash
$ npx typx-coverage
```

any 타입이 있는 위치의 정보를 출력할 수 있다.

```bash
$ npx type-coverage detail
```

타입 커버리지를 추적하여 코드를 꾸준히 관리하는 것이 중요하다.