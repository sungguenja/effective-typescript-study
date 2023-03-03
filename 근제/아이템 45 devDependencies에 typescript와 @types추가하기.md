# 아이템 45 devDependencies에 typescript와 @types추가하기

package.json에서 설정할 수 있는 속성들을 살펴봅시다

- dependencies
  - 패키지가 실행될 때 필요한 라이브러리들입니다
  - 일반적으로 `npm install package`로 실행하면 설치됩니다
  - 실행에 필요한 의존성들이 설치됩니다
  - 예를 들어 `express`, `react` 가 여기에 설치되어야합니다
- devDependencies
  - 개발하고 테스트하는 때에만 실행이 됩니다
  - `npm install -D` 라는 명령어로 요 항목에 설치가 가능합니다
  - dependencies와 다른 점은 개발과 테스트일때만 실행되고 런타입에는 필요 없습니다
  - 예를 들어 `Jest`가 여기에 포함되며 타입스크립트를 이용중이라면 서드파티들의 타입도 여기에 설치되는 것이 맞습니다
- peerDependencies
  - 런타임에 필요하긴 하지만, 의존성을 직접 관리하지 않는 라이브러리들입니다
  - 플러그인이 대표적인 예입니다
