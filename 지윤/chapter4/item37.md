#### 아이템37. 공식 명칭에는 상표를 붙이기

```typescript
interface Person {
  name: string;
  id: number;
}

interface Pet {
  name: string;
  id: number;
}

const iamPerson: Person = {
  name: 'peter',
  id: 1,
};

const iamPet: Pet = iamPerson; // 엥 요상.
```

Type ‘“Person”’ is not assignable to type ‘“Pet”’을 원한다면 상표를 붙이자

```typescript
// Branding
interface BrandedPerson {
  brand: 'person';
  name: string;
  id: number;
}

interface BrandedPet {
  brand: 'pet';
  name: string;
  id: number;
}

const iamPerson = {
  name: 'peter',
  id: 1,
} as BrandedPerson;

const iamPet: BrandedPet = iamPerson; // Error: Type 'BrandedPerson' is not assignable to type 'BrandedPet'. Types of property 'brand' are incompatible. Type '"person"' is not assignable to type '"pet"'

// Enum
enum PersonType {}
type Person = PersonType & {
  name: string;
  id: number;
};

enum PetType {}
type Pet = PetType & {
  name: string;
  id: number;
};

const iamPerson = {
  name: 'peter',
  id: 1,
} as Person;

const iamPet: Pet = iamPerson; // Error: Type 'Person' is not assignable to type 'Pet'. Type 'Person' is not assignable to type 'PetType'.
```
