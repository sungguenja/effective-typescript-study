#### 아이템24. 일관성 있는 별칭 사용하기

제어 흐름을 분석을 수월하게 하기 위해서는 일관성 있는 변수를 사용하자

```typescript
interface Product {
  category: 'A' | 'B' | 'C';
  name: string;
  price: number;
  discount?: {
    startDate: Date;
    endDate: Date;
    discountPersent: number;
  };
}

// 조건문으로 분기칠 때, 별칭을 조건으로 걸지 않으면 별칭의 타입은 좁혀지지 않는다.
const getDiscountPrice1 = (product: Product) => {
  const discountInfo = product.discount; 
  if (product.discount) {
    const a = product.discount; // const a: Discount
    const b = discountInfo; // const b: Discount | undefined
    if (b.startDate) { // Error: Object is possibly 'undefined'.
     ... 
    }
  }
};
  
// 해결방법1 - 조건문 내부에서 사용할 별칭으로 조건 걸기
const getDiscountPrice1 = (product: Product) => {
  const discountInfo = product.discount;
  if (discountInfo) {
    const b = discountInfo; // const b: Discount
    if (b.startDate) {
    }
  }
};
  
// 해결방법2 - better - 비구조화 할당으로 변수 뽑아 쓰기
const getDiscountPrice1 = (product: Product) => {
  const { discount } = product;
  if (discount) {
    if (discount.startDate) {
    }
  }
};
```

별칭 사용시 주의사항
* 타입스크립트는 함수가 타입 정제한 것을 무효화하지 않는다고 가정한다.  하지만 실질적으로, 함수 호출이 타입 정제를 무효화할 수 있다!!! 그러니 객체를 받아 수정하지 말고 객체를 받으면 새로운 지역 변수의 객체를 만들어 쓰자.