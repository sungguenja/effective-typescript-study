# Item 32. 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

* 각 타입의 계층을 분리된 인터페이스로 구현한다면 타입의 범위도 좁힐 수 있고, 유효한 타입만을 정의할 수 있다.

```ts
interface Layer {
    layout: FillLayout | LineLayout | PointLayout;
    paint: FillLayout | LineLayout | PointLayout;
}
```

* 위의 예시는 유효한 타입이 아닌, 오류가 발생할 수 있는 타입의 조합이 생길 여지를 준다.

```ts
interface FillLayer {
    type: 'fill';
    layout: FillLayout;
    paint: FillPaint;
}

interface LineLayer {
    type: 'line';
    layout: LineLayout;
    paint: LinePaint;
}

type Layer = FillLayer | LineLayer;
```

* 위의 예시는 Layer를 인터페이스의 유니온으로 변환한 것이다.
    * 이 경우 유효한 타입(FillLayer, LineLayer)만을 허용한다.
    * 타입에 태그(type)을 넣는다면 분기를 통해 타입을 좁힐 수 있다.

```ts
interface Person {
    name: string;
    placeOfBirth?: string;
    dateOfBirth?: Date;
}
```

* 위 코드의 문제점을 place와 birth의 경우의 수가 많아지므로 문제가 발생할 확률이 높아진다.

```ts
interface Person {
    name: string;
    birth?: {
        place: string;
        date: Date;
    };
}
```
* 따라서, 관련 있는 두 가지 값을 하나의 객체로 관리하는 것이 더 나은 설계다.

```ts
interface Name {
    name: string;
}

interface PersonWithBirth extends Name {
    placeOfBirth: string;
    dateOfBirth: Date;
}

type Person = Name | PersonWithBirth;
```

* 확장을 통해 좀 더 유연하게 타입을 모델링 할 수 있다.

