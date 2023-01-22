#### 아이템12. 함수 표현식에 타입 적용하기

타입스크립트에서는 함수 문장과 함수 표현식은 다르게 인식한다. 
```typescript
// 함수 문장
function a() {...}

// 함수 표현식
const a = function() {...}
const a = () => {...}
```

함수 문장식보다는 함수 표현식을 사용하자. 함수 문장식에서는 함수의 매개변수에 타입을 선언해야하지만, 함수 표현식에서는 함수 전체의 타입(매개변수부터-리턴값까지) 선언해줄 수 있다.
```typescript
function printStudent(name: string, age: number) {
  return `${name} ${age}`
}
              
type PrintName = (name: string, age: number) => string
const printStudent: PrintName = (name, age) => `${name} ${age}`
```

함수 표현식에 함수 타입의 선언의 장점
* 함수 표현식에 함수의 타입을 선언하게 되면, 매개변수부터 반환값까지 추론이 가능하다.
* 함수 타입 재사용을 통해 불필요한 코드의 반복을 줄여준다. 


