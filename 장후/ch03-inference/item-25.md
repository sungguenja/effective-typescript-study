# Item 25. 비동기 코드에는 콜백 대신 async 함수 사용하기

* 자바스크립트 비동기 처리
    * callback
    * Promise
    * async/await

callback 보다 Promise, async/await를 사용해야하는 이유가 무엇일까
* callback 보다 Promise가 코드를 작성하기 쉽다.
    * 병렬 처리가 필요하다면 `Promise.all([])` 활용
* callback 보다 Promise가 타입 추론이 쉽다.

* `Promise.race` 를 사용하여 Promise에 타임아웃을 추가하는 패턴
    * timeout과 fetch 함수 중 먼저 완료되는 것만 처리
    * timeout 시간 안에 fetch 함수가 끝나지 못한다면, timeout reject 발생

    ```ts
    function timeout(millis: number): Promise<never> {
        return new Promise((resolve, reject) => {
            setTimeout(() => reject('timeout'), millis)
        })
    }

    async function fetchWithTimeout(url: string, ms: number) {
        return Promise.race([fetch(url), timeout(ms)])
    }
    ```

    * fetchWithTimeout의 return type은 `Promise<Response>`로 추론된다.
    * fetchWithTimeout의 경우 `Promise<Response | never>` 가 추론 타입이지만, never(공집합)와의 유니온은 의미 없으므로 Response로 추론된다.

* callback API를 래핑하는 경우 Promise를 생성하기보다 async/await를 사용한다.
    * 간결하고 직관적인 코드를 제공
    * async 함수는 항상 Promise 반환 강제

```ts
function getNumber(): Promise<number> {}
async function getNumber() { return 42 } // return type Promise<number>
const getNumber = async () => 42 // return type Promise<number>
const getNumber = () => Promise.resolve(42) // return type Promise<number>
```

* 함수는 동기 혹은 비동기로 실행되어야하며, 혼용해서 사용해서는 안된다.
    * bad
        * 만약 url이 캐시된 경우, 함수가 동기로 호출되기 때문에 문제가 발생할 여지가 있다.
    ```ts
    const _cache: { [url: string]: string } = {};

    function fetchWithCache(url: string, callback: () => void) {
        if (url in _cache) {
            callback();
        } else {
            _cache[url] = text;
            fetchURL(url, text => { callback(); })
        }
    }
    ```

    * good
        * async 를 사용하면 무조건 비동기 코드이다.
    ```ts
    const _cache: { [url: string]: string } = {};

    const fetchWithCache = async (url: string) => {
        if (url in _cache) {
            return _cache[url];
        }

        const res = await fetch(url);
        const text = await res.text();

        _cache[url] = text;

        return text;
    };

    let reqStatus: 'loading' | 'success' | 'error';

    const getUser(userId: string) {
        reqStatus = 'loading';

        const profile = await fetchWithCache('/user/1');

        reqStatus = 'success';
    }
    ```







