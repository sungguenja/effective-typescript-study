# Item 35. 데이터가 아닌, API와 명세를 보고 타입 만들기

눈 앞의 데이터가 아닌 명세 기반으로 타입을 설계하자
* 데이터에 집중하다보면 예외 케이스를 놓칠 확률이 높다.

GraphQL은 자체적으로 타입이 정의 되어있기 때문에, 특정 쿼리에 대해 타입스크립트 타입을 생성할 수 있다.
