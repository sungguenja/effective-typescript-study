# Item 28. 유효한 상태만 표현하는 타입을 지향하기

```ts
interface State {
    pageText: string;
    isLoading: boolean;
    error?: string;
}

const renderPage = (state: State) => {
    if (state.error) {
        return 'Error'
    } else if (state.isLoading) {
        return 'Loading'
    }

    return 'CurrentPage'
}
```

* 위 코드의 문제점은 분기 조건이 명확하지 않다. 
    * Loading과 Error State가 동시에 노출되는 경우, 명확한 상태 분리가 되어있지 않으므로 구분이 어렵다.
    * 이렇게되면 상태에 대한 정보 부족으로 무효한 상태가 노출되어, 정확한 페이지 렌더링이 불가하다.
    * 즉, 유효한 상태만 표현하여 타입을 설계해야 사용자가 원하는 페이지를 정확하게 노출할 수 있다.

```ts
interface RequestPending {
    state: 'pending';
}

interface RequestError {
    state: 'error';
    error: string;
}

interface RequestSuccess {
    state: 'ok';
    pageText: string;
}

type RequestState = RequestPending | RequestError | RequestSuccess;

interface State {
    currentPage: string;
    requests: { [page: string]: RequestState; };
}
```

* 태그된 유니온을 사용하여 유효한 상태만을 명시적으로 모델링
    * 위 타입 설계는 길지만, 유효한 상태만을 명시했기 때문에, 무효한 상태가 노출될 수 없다.
    * 모든 요청을 정리된 상태로 나타낼 수 있다.

```ts
const renderPage = (state: State) => {
    const { currentPage } = state;
    const requestState = state.requests[currentPage];

    switch(requestState.state) {
        case 'pending':
            return 'Loading';
        case 'Error':
            return 'Error';
        case 'ok':
            return 'CurrentPage';
    }
}

const changePage = (state: State, newPage: string) => {
    state.requests[newPage] = { state: 'pending' };
    state.currentPage = newPage;

    try {
        const res = await fetch(url);

        if (!res.ok) {
            throw new Error('Error');
        }

        const pageText = await res.text();
        state.requests[newPage] = { state: 'ok', pageText };
    } catch (e) {
        state.requests[newPage] = { state: 'error', error: `${e}`};
    }
}
```

* 타입 설계 시 표현할 값과 제외할 값에 대한 고려가 필요하다.
* 유효한 타입만 허용한다면 코드 작성이 쉬워지고 타입 체크가 용이해진다.
