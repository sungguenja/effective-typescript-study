#### 아이템28. 유효한 상태만 표현하는 타입을 지향하기

```typescript
// bad - 분기 조건이 모든 케이스를 커버하고 있지 않음 - ex) loading && errorMessage
type Response<T> = {
  success: T;
  pending: boolean;
  error?: string;
}

const renderState = (state: State) => {
  if (state.pending) return '로딩...';
  if (state.error) return `에러! ${state.error.message}`;
 	return `${state.success.something}`
}

// good - 태그된 유니온을 사용해 상태를 명시적으로 관리하여 모든 요청이 하나의 상태로 떨어진다.
interface SuccessResponse<T> {
  resultType: 'SUCCESS';
  success: T;
  error: null;
}

interface FailResponse {
  resultType: 'FAIL';
  success: null;
  error: FailError;
}

interface PendingResponse {
  resultType: 'PENDING';
  success: null;
  error: null;
}

type Response<T> = SuccessResponse<T> | FailResponse | PendingResponse;
const renderState = (state: Response<Something>) => {
  const { resultType } = state;
  switch(resultType) {
    case 'PENDING':
      return '로딩...';
    case 'FAIL':
      return `에러! ${state.error.message}`;
    case 'SUCCESS':
      return `${state.success.something}`
    default:
      throw new Error('요상한 result type!')   
  }
}
```

타입을 설계할 때부터 잘 설계해야 함수에서 문제가 없다. 어떤 값들을 포함하고 어떤 값들을 제외할 지 신중하게 생각해야 하며, 유효한 상태를 표현하는 값을 허용하는 코드를 작성하면, 코드 작성이 쉬워지고 타입 체크가 용이해진다.

생각
* 유효한 상태 - 요것을 다른 단어로 풀어서 이야기하면 뭐가 될까.
* 유효한 상태라는 말이 와닿지 않는다.
