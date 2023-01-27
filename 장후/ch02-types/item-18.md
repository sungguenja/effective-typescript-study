# Item 18. 매핑된 타입을 사용하여 값을 동기화하기

```ts
interface ScatterProps {
    xs: number[];
    xy: number[];

    xRange: [number, number];
    yRange: [number, number];
    color: string;

    onClick: (x: number, y: number, index: number) => void;
}
```

## 렌더링 최적화
* 오류 발생에 대한 적극적 방어
* onClick을 제외하고, 값 변경 시 마다 차트가 변경되므로, 너무 자주 컴포넌트가 Re-render된다.
```ts
function sholudUpdate(
    oldProps: ScatterProps,
    newProps: ScatterProps
) {
    let k: keyof ScatterProps;

    for (k in oldProps) {
        if (oldProps[k] !== newProps[k]) {
            if (k !== 'onClick') return true;
        }
    }

    return false;
}
```


* 불필요하게 Re-render 되는 것은 막을 수 있지만, 차트를 다시 그려야하는 상황에 누락될 수도 있다.
```ts
function sholudUpdate(
    oldProps: ScatterProps,
    newProps: ScatterProps
) {
    return (
        oldProps.xs !== newProps.xs ||
        ...
        oldProps.color !== newProps.color
    )
}
```

1. 타입 체커 활용
* 매핑된 타입과 객체 사용
* `[k in keyof ScatterProps]` 매핑된 타입은 타입 체커에게 REQUIRES_UPDATE가 ScatterProps와 동일한 속성을 가져야한다는 것을 제공한다.
```ts
// 업데이트가 되어야하는 항목만 true
const REQUIRES_UPDATE: { [k in keyof ScatterProps]: boolean } = {
    xs: true,
    ys: true,
    ...
    onClick: false
};

function sholudUpdate(
    oldProps: ScatterProps,
    newProps: ScatterProps
) {
    let k: keyof ScatterProps;
    for (k in oldProps) {
        if (oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) {
            return true;
        }
    }

    return false;
}
```

## Summary
* 매핑된 타입을 사용해서 관련된 값과 타입을 동기화한다.
* 인터페이스에 새로운 속성을 추가할 때, 선택을 강제하도록 매핑된 타입을 고려한다.
