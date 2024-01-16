# Never type

Used when a value should never have a certain value. The benefit of using it is to catch errors in compile time instead of them being caught in runtime.

## Compile time non-empty strings

```typescript
type NonEmptyString<T extends string> = T extends '' ? never : T;

function failOnEmptyString<T extends string>(input: NonEmptyString<T>) {
  if (!input.length) {
    throw new Error('RuntimeError');
  }
}

failOnEmptyString('');
// error will now occur at compile time
```

## Exhaustive switch case

The function in the example will have a compile time error when:

- the passed in city isn't in the `City` type
- a city is removed from the `City` type
- a city is added to the type `City` type

```typescript
type City = 'Zurich' | 'Oslo' | 'London';

function getCountry(city: City) {
  switch (city) {
    case 'Zurich':
      return 'Switzerland';
    case 'Oslo':
      return 'Norway';
    case 'London':
      return 'GB';
    default:
      const _exhaustive: never = city;
      return _exhaustive;
  }
}

getCountry('Munich');
// error will now occur at compile time
```
