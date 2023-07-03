# Template literal types

## Permutation of multiple types

If the required type is a permutation of one or multiple types it can be done easly using template literal types.

```typescript
type ChessLetter = 'A' | 'B' | 'C';
type ChessNumber = 1 | 2 | 3;

type Board = `${ChessLetter}${ChessNumber}`;
// 'A1' | 'A2' | 'A3' | 'B1' | 'B2' | 'B3' | 'C1' | 'C2' | 'C3'
```

## Interpolated union

If the required type is a union of the types prefixed or suffixed by another string.

```typescript
type Page = 'welcome' | 'home';
type Anchor = 'header' | 'main' | 'footer';

type AllElementIds = `${Page | Anchor}_id`;
// 'welcome_id' | 'home_id' | 'header_id' | 'main_id' | 'footer_id'
```
