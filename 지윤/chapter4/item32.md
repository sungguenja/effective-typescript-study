#### 아이템32. 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

인터페이스의 속성에 유니온 타입을 넣어주고 있다면, 유니온 타입의 인터페이스를 만드는 게 좋지 않을까 생각해보자. 태그된 유니온을 사용해주면 더 명확하다,
```typescript
// bad
interface Field {
	fieldType: 'text' | 'checkbox' | 'select';
  fieldValue: TextFieldValue | CheckboxFieldValue | SelectFieldValue;
}

// good - fieldType과 fieldValue가 이상하게 엮이는 경우를 방지할 수 있다.
interface TextField {
  fieldType: 'text';
  fieldValue: TextFieldValue
}
interface CheckboxField {
  fieldType: 'checkbox';
  fieldValue: CheckboxFieldValue
}
interface SelectField {
  fieldType: 'select';
  fieldValue: SelectFieldValue
}
type Field = TextField | CheckboxField | SelectField
```

