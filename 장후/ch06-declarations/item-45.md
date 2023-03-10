# Item 45. devDependencies에 typescript와 @types 추가하기

npm은 세 가지 종류의 의존성을 구분한다.

1. dependencies
    * 프로젝트를 실행하는데 필요한 라이브러리 모음, 즉 런타임에 사용되는 라이브러리
2. devDependencies
    * 개발 및 테스트 용으로 사용하는 라이브러리, 런타임에는 필요없는 라이브러리
3. peerDependencies
    * 런타임에 필요하지만, 따로 의존성 관리가 필요없는 라이브러리

타입스크립트와 관련된 라이브러리는 런타임에서 사용하지 않기 때문에, 주로 `devDependencies` 에 포함된다.

프로젝트에서 타입스크립트를 사용한다면
1. 시스템 레벨로 설치하기보다는 `devDependencies` 에 추가하여, 버전 정보를 맞추는 것이 좋다.
2. 타입 의존성 `@type` 을 고려해야한다. 라이브러리에 타입 정보가 없다고 하더라도, `DefinitelyTyped` 에서 타입 정보를 얻는 것이 가능하다.
    * `@type/lodash` 에는 lodash type이 정의되어있다.
    * `@type` 의존성은 개발 환경에서만 사용하기 때문에, `devDependencies` 에 추가해야한다.
s