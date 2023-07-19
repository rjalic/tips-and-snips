# Deep Readonly

The baseline `Readonly<T>` utility type only applies to the top level of an object but the nested fields remain mutable.

To solve that issue, the `DeepReadonly<T>` type can be used:

```typescript
export type Primitive = string | number | boolean | undefined | null;

export type StringArray = Array<string>;

export type DeepReadonlyArray<T> = Readonly<Array<DeepReadonly<T>>>;

export type DeepReadonlyObject<T> = {
  readonly [P in keyof T]: DeepReadonly<T[P]>;
};

export type DeepReadonly<T> = T extends Primitive
  ? T
  : T extends Array<infer U>
  ? DeepReadonlyArray<U>
  : DeepReadonlyObject<T>;
```
