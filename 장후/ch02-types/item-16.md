# Item 16. number 인덱스 시그니처보다는 Array, 튜플, ArrayLike를 사용하기

* 자바스크립트 객체에서 숫자, 복잡한 객체를 key로 하는 경우, 문자열로 치환한다.
* 타입스크립트는 숫자 키를 허용한다.
    * 런타임에서는 문자열로 인식하지만, 컴파일 과정에서 오류를 잡을 수 있다.

* 배열 순회
    * Bad
        * `for ... in`
    * Good
        * 값만 필요하다 > `for ... of` 
        * index, 값이 필요하다 > `Array.prototype.forEach`
        * 중간에 break 할 경우 > `for`

* 인덱스 시그니처 타입을 number로 지정해도 자바스크립트 런타임에서는 배열의 index type이 결국 string이다.
    * number 타입으로 인덱스 항목을 지정하고 싶다면, `Array` 또는 튜플을  사용한다.

* 길이를 가지는 배열과 비슷한 형태의 튜플을 사용하고 싶다면 ArrayLike 사용
    * 단, 이 경우 튜플 메서드 사용 불가능
    ```ts
    const tupleLike: ArrayLike<string> = {
        '0': 'A',
        
    }
    ```
