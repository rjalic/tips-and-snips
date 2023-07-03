# Recursive types

## Tuple type

Typed tuple of `n` elements of `T` type done using a recursive type.

```typescript
type Tuple<
  Length extends number,
  TupleType,
  Accumulator extends TupleType[] = []
> = Accumulator['length'] extends Length
  ? Accumulator
  : Tuple<Length, TupleType, [...Accumulator, TupleType]>;

type RgbTuple = Tuple<3, number>;

const rgb: RgbTuple = [1, 1, 1];
```

The recursion stops once the length of the provided `n` is reached, otherwise the type calls the `Tuple` type again recursively but with the current accumulator value provided.

## Object path type

Array based access to a property on an object.

```typescript
type Form = {
  firstName: string;
  lastName: string;
  address: {
    street: {
      streetName: string;
      streetNumber: string;
    };
  };
};

type Path<T> = T extends object
  ? { [Key in keyof T]: [Key] | [Key, ...Path<T[Key]>] }[keyof T]
  : never;

const streetName: Path<Form> = ['address', 'street', 'streetName'];
```

This array based access type can then be further expanded into a JSON path type.

```typescript
type JsonPathHelper<T extends unknown[]> = T extends [infer U, ...infer Rest]
  ? U extends string
    ? Rest extends unknown[]
      ? `${U}${JsonPathHelper<Rest> extends ''
          ? ''
          : '.'}${JsonPathHelper<Rest>}`
      : never
    : never
  : '';

type JsonPath<T> = JsonPathHelper<PathArray<T>>;

const streetName: JsonPath<Form> = 'address.street.streetNumber';
```
