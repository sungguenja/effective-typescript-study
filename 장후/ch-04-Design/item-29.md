# Item 사용할 때는 너그럽게, 생성할 때는 엄격하게

* 함수의 매개변수는 허용되는 타입의 범위가 넓어도되지만, return 타입은 범위가 구체적이어야한다.
* 매개변수 타입이 넓고, 반환 타입의 범위가 좁아야 사용하기 편리하다.

```ts
type LngLat = 
    { lng: number; lat: number; } |
    { lon: number; lat: number; } |
    [number, number]

interface CameraOptions {
    center?: LngLat;
    zoom?: number;
    bearing?: number;
    pitch?: number;
}

type LngLatBounds = 
    { northeast: LngLat; southWest: LngLat; } |
    [LngLat, LngLat] |
    [number, number, number, number];

// 카메라 세팅 함수
declare function setCamera(camera: CameraOptions): void;

// 위치 세팅 함수
declare function viewportForBounds(bounds: LngLatBounds): CameraOptions;

function focusOnFeature(f: Feature) {
    const bounds = caculateBoundingBox(f);
    const camera = viewportForBounds(bounds);

    const { center: { lat, lng }, zoom } = camera; // lat, lng, zoom 이 선택적 속성이기 때문에 타입 에러가 발생한다.
}
```
* 위 예시에서 나올 수 있는 `viewportForBounds` 반환 케이스는 총 48가지로 반환 타입의 범위가 넓다.
* 유니온 타입 케이스 별로 분기해서 사용가능 하지만 코드의 복잡도를 높인다.

```ts
interface LngLat { lng: number; lat: number; };
type LngLatLike = LngLat | { lon: number; lat: number; } | [number, number];

interface Camera {
    center: LngLat;
    zoom: number;
    bearing: number;
    pitch: number;
}

interface CameraOptions extends Omit<Partial<Camera>, 'center'> {
    center: LngLike;
}

type LngLatBounds = 
    { northeast: LngLatLike; southWest: LngLatLike; } |
    [LngLatLike, LngLatLike] |
    [number, number, number, number];

// 카메라 세팅 함수
declare function setCamera(camera: CameraOptions): void;

// 위치 세팅 함수
declare function viewportForBounds(bounds: LngLatBounds): Camera;

function focusOnFeature(f: Feature) {
    const bounds = caculateBoundingBox(f);
    const camera = viewportForBounds(bounds);

    const { center: { lat, lng }, zoom } = camera; // lat, lng, zoom 이 선택적 속성이기 때문에 타입 에러가 발생한다.
}
```
* 위 예시에서 나올 수 있는 `viewportForBounds` 반환 케이스는 1가지다.
* 반환 타입이 분명하기 때문에 lat, lng, zoom이 정상적으로 노출된다.
* 받을 수 있는 매개변수 타입은 다양하지만, 반환 타입 범위를 좁히면서 에러 발생을 최소화함과 동시에 로직의 복잡도도 줄일 수 있다.
* 매개변수에 다양한 타입을 받는 것도 좋은 설계는 아니지만, 반환 타입이 많은 것은 분명 나쁜 설계이다.
