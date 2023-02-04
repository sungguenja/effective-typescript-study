#### 아이템22. 타입 좁히기

타입스크립트는 조건문과 같은 분기처리를 통해서, 넓은 타입을 조건을 만족시키는 특정 타입으로 타입을 좁혀준다.
```typescript
// 타입 좁히기1 - 값의 존재 유무
const isDate = (date?: Date | null) => {
	if (date != null) {
    console.log(date) // date: Date
	} else {
    console.log(date) // date: null | undefined
   	throw new Error('값 없다')
	}
}

// 타입 좁히기2 - instanceof 
const isDate = (date?: Date | null) => {
  if (date instanceof Date) {
    console.log(date) // Date
  } else {
    console.log(date) // date: null | undefined
    throw Error('값 없다')
  }
};

// 타입 좁히기3 - 속성체크
interface Dog {
  eat: () => void;
  bark: () => void;
}

interface Bird {
  eat: () => void;
  fly: () => void;
}

const isBird = (animal: Dog | Bird) => {
  if ('bark' in animal) {
    console.log(animal) // animal: Dog
  } else {
    console.log(animal) // animal: Bird
  }
};

// 타입 좁히기4 - 내장함수 사용
const getStringArray = (value: string | string[]) => {
  const stringArray = Array.isArray(value) ? value : [value] 
  console.log(stringArray) // stringArray : string[]
}

// 타입 좁히기5 - 명시적 태그 사용하기
interface FormValues {}
type UpdateForm = {
  type: 'update';
  formValues: FormValues & { id: number };
};

type CreateForm = {
  type: 'create';
  formValues: FormValues;
};

type Form = UpdateForm | CreateForm;
const handleSubmit = (form: Form) => {
  const { type, formValues } = form;
  switch (type) {
    case 'create':
      console.log(formValues); // FormValues
      const {id} = formValues // Error: Property 'id' does not exist on type 'FormValues'.
      break;
    case 'update':
      console.log(formValues); // FormValues & { id: number }
      break;
    default:
      const invalid = type; // never;
  }
};

// 타입 좁히기 - 사용자 정의 타입 가드
const isDefined = <T,>(val: T | undefined): val is T => {
  return val !== undefined;
};
const getUsersWithId = (users: User[], selectedIds: number[]) => {
  const targetUsers = selectedIds.map((id) => users.find((user) => user.id === id)).filter(isDefined); // targetUsers: User[]
};
```

조건문으로 타입 좁히기 시 주의해야할 점 - 조건문의 조건을 잘 쓰자
```typescript
// 잘못된 typeof 값 사용 - null의 type은 object이다.
const isDate = (date: Date | null) => {
  if (typeof date === 'object') {
    console.log(date); // date: Date | null
  } else {
    console.log(date); // date: never
  }
};

// 잘못된 형변환 값 사용 - 빈문자열('')은 false로 형변환된다.
const hasString = (value?: string) => {
  if (Boolean(value)) {
    console.log(value); // value: string | undefined
  } else {
    console.log(value); // value: string
  }
};
```

궁금증
* 예시 쓰다가 갑자기 궁금해진 `thorw Error`와 `throw new Error`의 차이 
  * throw new Error와 throw Error는 예외를 발생시키는 동작을 하는데에 차이는 없다. 
  * throw new Error는 새로운 Error 객체를 생성해 던진다 - Error 객체에서 제공하는 정보를 사용할 수 있다.
  * throw Error는 기존 Error 객체를 던진다 - Error 객체에서 제공하는 정보를 사용할 수 없다.
  * 그래서 throw new Error를 사용하길 권장한다.