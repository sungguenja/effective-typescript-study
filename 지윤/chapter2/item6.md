#### 아이템6. 편집기를 사용하여 타입 시스템 탐색하기

타입스크립트를 설치하면 tsc와 tsserver를 사용할 수 있다. 편집기로 탐색해보면서 타입 시스템과 친해져보자

**편집기로 타입 시스템 탐색하는 방법**
* 심벌 위에 마우스 커서를 대고, 타입스크립트가 그 타입을 어떻게 판단/추론 하는 지 보자.
  * 원시형 타입은 물론 함수의 반환값의 타입까지 추론해준다.
  * 조건문에 걸리면 값을 좁혀 추론해주기도 한다.
    ```typescript
    const printMessage = (msg: string | null) => {
      if (msg != null) {
        console.log(msg); // string
        return msg;
      } else {
        console.log(msg); // null
      }
    };
    ```
  * 객체에서는 객체에 속한 개별 속성을 살펴 객체의 타입을 추론한다.
    ```typescript
    const fruits = [1, 2, '3', { price: 2000 }]; 
    // const fruits: (string | number | { price: number })[]
    ```
* 편집기상 타입 오류를 살펴보자
* 언어 서비스를 이용해 라이브러리와 라이브러리의 타입 선언을 탐색해 보자 - ex)  `Go to definition` 옵션 