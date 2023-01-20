# Item 11. 잉여 속성 체크의 한계 인지하기
* 타입이 명시된 변수에 Object Literal을 할당하면 해당 타입 속성이 있는지와 Excess Property Check(그 외 속성 존재 유무 확인)를 진행한다.
```ts
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}

const r: Room = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: 'present',
// ~~~~~~~~~~~~~~~~~~ Object literal may only specify known properties,
//                    and 'elephant' does not exist in type 'Room'
};
```

* 구조적 타이핑 관점에서는 오류가 발생하지 않아야한다.
```ts
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}
const obj = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: 'present',
};
const r: Room = obj;  // OK
```
* obj의 타입은 `{ numDoors: number; ceilingHeightFt: number; elephant: string; }` 으로 추론되기 때문에, Room의 부분 집합을 포함하므로 할당이 가능하며, 타입 체커도 통과한다.

* Excess Property Check는 할당 가능과는 별도의 과정이다.

* 타입스크립트는 단순히 런터임 오류 코드를 잡아내는 것이 아니라, 의도와 다르게 작성된 코드도 찾아낸다.
```ts
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}
function setDarkMode() {}
interface Options {
  title: string;
  darkMode?: boolean;
}
function createWindow(options: Options) {
  if (options.darkMode) {
    setDarkMode();
  }
  // ...
}
createWindow({
  title: 'Spider Solitaire',
  darkmode: true
// ~~~~~~~~~~~~~ Object literal may only specify known properties, but
//               'darkmode' does not exist in type 'Options'.
//               Did you mean to write 'darkMode'?
});
```

* Obtions에는 구조적 타입 체커의 특성 상 다양한 타입의 변수를 할당할 수 있다.

* Excess Property Check를 활용하면, 타입 시스템을 유지하면서 엄격한 체크를 진행할 수 있다.

* 단, 임시 변수를 도입한다면 Excess Property Check를 건너뛸 수 있다.

* 선택된 속성만 가지는 weak type(Patial과 유사)에서는 값과 선언된 타입에 공통된 속성이 있는지 추가 확인하는 과정이 있다.

* 다만, weak type은 속성이 전혀 다른 경우에만 동작한다.

```ts
interface PrettierConfig {
  printWidth?: number;
  tabWidth?: number;
  semi?: boolean;
}

function createFormatter(config: PrettierConfig) {
  // ...
}

const prettierConfig = {
  printWidth: 100,
  semicolons: true,
};

const formatter = createFormatter(prettierConfig); // 에러가 발생하지 않는다.
```


## Summary
* 객체 리터럴 할당 및 argument로 활용 시 잉여 속성 체크 수행
* 엄격한 타입 검사지만, 일반적인 구조적 할당 가능성 체크와는 다르며 두 가지 개념을 확실히 알아야한다.
* 임시 변수 도입 시 잉여 속성 체크를 건너뛸 수 있다.

## Reference
* [WeakType](https://mariusschulz.com/blog/weak-type-detection-in-typescript)