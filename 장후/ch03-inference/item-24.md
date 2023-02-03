# Item 24. 일관성 있는 별칭 사용하기

* 변수명을 남발해서 사용하면 제어 flow를 분석하기 힘들다.

```ts
interface Coordinate {
    x: number;
    y: number;
}

interface BoundingBox {
    x: [number, number];
    y: [number, number];
}

interface Polygon {
    exterior: Coordinate[];
    hole: Coordinate[][];
    bbox?: BoundingBox;
}
```

해당 코드를 개선 해보자
```ts
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
    if (polygon.bbox) {
        if (pt.x < polygon.bbox.x[0] || pt.x > polygon.bbox.x[1] || pt.y < polygon.bbox.y[0] || pt.y > polygon.bbox.y[1]) {
            return false;
        }
    }
} 
```

* 따로 변수를 재할당하기 보다는, 구조 분해 할당을 통해 변수명의 일관성을 유지하자
    * 주의사항
        * bbox에서 나온 x, y가 선택적 속성일 때, 분해 할당을 하게되면 값을 못찾고 error가 발생할 수 있다. 따라서 타입 경계에 null을 추가하여 null check를 하는 것도 좋다.
        * 빈 배열을 통해 해당 property가 없다는 것을 명시하는 것도 좋다.
```ts
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
    const { bbox } = polygon;

    if (bbox) {
        const { x, y } = bbox;

        if (pt.x < x[0] || pt.x > x[1] || pt.y < y[0] || pt.y > y[1]) {
            return false;
        }
    }
}
```

* 별칭을 잘못 사용하면 런타임에도 혼동을 준다.
```ts
const { bbox } = polygon;

if (!bbox) {
    calculateBox(polygon); // 이렇게 할당하게 되면 bbox와 polygon.bbox는 reference가 달라진다.
}
```

## Summary
* 별칭(alias)는 타입을 좁히는걸 방해하기 때문에, 변수명을 일관되게 사용하자
