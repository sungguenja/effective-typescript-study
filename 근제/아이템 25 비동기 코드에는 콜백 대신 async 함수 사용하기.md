# 아이템 25 비동기 코드에는 콜백 대신 async 함수 사용하기

저희 모두 어린 시절 비동기를 제어할 때 제대로된 방법을 몰라 아래와 같이 짠 경우가 많을 겁니다. ~~(나만 그랬나?)~~

```typescript
firstFunction(firstFunctionsProps, secondFunction(response1) {
    thirdFunction(thirdFunctionsProps, ForthFunction(response2) {
        // ....
        // stop... pls
    })
})

firstPromise().then().then().then().then()....................
```

그러니까 우리들은 위와 같은 상황을 멀리하고 `async/await`를 쓰는 게 낫습니다. `try/catch`로 에러핸들링도 잘 할 수 있습니다. 그리고 코드의 작성과 타입의 추론이 더 쉬워서 `async/await`를 진짜 가까이 두는게 좋습니다

```typescript
interface SomeAsyncFunctionReturn {
    returnProp: string
}

const someAsyncFunction = async () => {
    return await axios.get<SomeAsyncFunctionReturn>(url)
}

/////////////
import someAsyncFunction from '..?'

const someAsync = async () => {
    const someReturns = await someAsyncFunction() // someReturns타입은 SomeAsyncFunctionReturn
}

/// 책의 예제
const _cache = {[url:string]: string} = {}
async function fetchWithCache(url: string) {
  if (url in _cache) {
    return _cache[url]
  }
  const response = await fetch(url)
  const text = await response.text()
  _cache[url] = text
  return text
}

let requestStatus: 'loading' | 'success' | 'error'
async function getUser(userId:string) {
  requestStatus = 'loading'
  const profile = await fetchWithCache(`/user/${userId}`)
  requestStatus = 'success'
}
```

- 요약
  - 콜백보다는 프로미스를 사용하는 게 코드 작성과 타입 추론 면에서 유리합니다.
  - 가능하면 프로미스를 생성하기보다는 `async`와 `await`를 사용하는 것이 좋습니다. 간결하고 직관적인 코드를 작성할 수 있고 모든 종류의 오류를 제거할 수 있습니다
  - 어떤 함수가 프로미스를 반환한다면 `async`로 선언하는 것이 좋습니다
