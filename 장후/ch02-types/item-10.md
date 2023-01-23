# 아이템 10. 객체 래퍼 타입 피하기
* Object Wrapper
    * 기본형을 감싸는 객체
    * String, Number 등
    * 자바스크립트는 string(Primitive type)의 charAt과 같은 메서드를 사용할 때 String Object로 Wrapping하고, 메서드를 호출하고, Wrapping Object를 버린다.
    * 이처럼 자바스크립트는 Primitive Type과 Wrapper Type을 자유롭게 변환한다.

* Primitive type를 Wrapper Object Type로 모델링한다.
    * string > String
    * number > Number
    * boolean > Boolean
    * symbol > Symbol
    * bigint > BigInt

* Primitive Type은 Wrapper Object Type으로 변환 가능
* Wrapper Object Type은 Primitive Type으로 변환 불가능

* 타입스크립트 Wrapper Object Type 사용은 `지양` 하고 Primitive Type을 `지향` 한다.
