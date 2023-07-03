# Mapped types

Idea is to connect types so that a change on one type is detected and followed through on the other type.

In the example below there is a `MyEvent` type which needs to have `OnMyEvent` functions defined which in the example below do something and return nothing.

```typescript
type MyEvent = {
  add: string;
  remove: string;
  change: string;
};

type OnMyEvent = {
  [Key in keyof MyEvent as `on${Capitalize<Key>}`]: () => void;
};

const userActions: OnMyEvent = {
  onAdd: () => {},
  onRemove: () => {},
  onChange: () => {},
};
```

Alternatively, if the return type of the functions should be the manipulated object of the same type as the key in the `MyEvent` type.

```typescript
type MyEvent = {
  add: string;
  remove: string;
  change: string;
};

type OnMyEvent = {
  [Key in keyof MyEvent as `on${Capitalize<Key>}`]: () => MyEvent[Key];
};

const userActions: OnMyEvent = {
  onAdd: () => {
    return '';
  },
  onRemove: () => {
    return '';
  },
  onChange: () => {
    return '';
  },
};
```
