# Item 37. 공식 명칭에는 상표를 붙이기

값을 구분하기 위해 타입에 `_brand` 속성을 부여한다.

```ts
interface Vector2D {
    _brand: '2d';
    x: number;
    y: number;
}

function vec2D(x: numner, y: number): Vector2D {
    return { x, y, _brand: '2d' };
}

function calculate(p: Vector2D) {
    return Math.sqrt(p.x * p.x + p.y * p.y);
}

calculate(vec2D({ x: 3, y: 4 }))
```

number 타입에도 brand를 붙여서 사용할 수 있다.
* 산술 연산 후에는 brand가 사라지지만 숫자 단위가 혼합된 경우 구분하기 위한 방법이 될 수 있다.
```ts
type Meters = number & { _brand: 'meter' };

const meters = (m: number) => m as Meters;

const oneKm = meters(1000); // type Meters
```



