#### 아이템25. 비동기 코드에는 콜백 대신 async 함수 사용하기

`콜백`보다는 `프로미스`를 사용하자
* 콜백보다 프로미스가 코드 작성이 쉽다
  * Promise.all, Promise.allSettled, Promise.race, Promise.finally
* 콜백보다 프로미스가 타입 추론이 쉽다

 `프로미스`보다는 `async/await`를 사용하자.
* 프로미스보다 async/await가 더 간결하고 직관적 코드 작성이 가능하다.
* 프로미스는 반동기 코드를 작성할 수 있지만 async함수는 항상 비동기 코드를 작성하게 강제한다.

