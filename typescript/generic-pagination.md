# Generic pagination

Combining generics and template literal types to create a generic pagination type.

Firstly, to define the directions which can be used for sorting.

```typescript
type OrderByDirection = 'ASC' | 'DESC';
```

Followed by the `OrderBy<T>` type which is a mapped type that takes a generic type `T` and creates a new type with the
same keys but with the values of `ASC` or `DESC`. The goal of this type is to constrain the fields by which the data can
be ordered by.

```typescript
type OrderBy<Type extends Record<string | number, unknown>> = `${keyof Type & (string | number)}${OrderByDirection}`;
```

The implementation above constrains the keys to be either a `string` or a `number` and creates all the possible
combinations of the keys and the directions. Something to note is that the implementation of the type in the current
iteration assumes that the discriminator is `;`. This can be further improved upon by providing a default value for the
discriminator and allowing the discriminator to be overridden.

```typescript
type OrderBy<Type extends Record<string | number, unknown>, Discriminator extends string = ';'> = `${keyof Type & (string | number)}${Discriminator}${OrderByDirection}`;
```

Overriding the default discriminator can be useful when the API isn't standardized or when calling an external API which
has a different discriminator.

Next up is to define the `Pagination` type.

```typescript
type Pagination<Type extends Record<string | number, unknown>, Discriminator extends string = ';'> = {
    page: number;
    limit: number;
    totalItems: number;
    orderBy: OrderBy<Type, Discriminator>;
};
```

The `Pagination` type takes two generic parameters, `Type` and `Discriminator`. The `Type` parameter is used to define
the type of the data that is being paginated. The `Discriminator` parameter is optional and is used to define the
separator between the fields and the direction in the `orderBy` field.

Taking this to the next step to model a `PaginatedResponse` type.

```typescript
type PaginatedResponse<Type extends Record<string | number, unknown>, Discriminator extends string = ';'> = {
    data: Iterable<Type>;
    pagination: Pagination<Type, Discriminator>;
};
```

Finally, since the `data` field might not be best suited depending on which API is being called, it can be further
enhanced by using a generic parameter to define the name of the field.

```typescript
type VariableKeyData<Type extends Record<string | number, unknown>, Key extends string = 'data'> = {
    [key in Key extends '' ? never : Key]: Iterable<Type>;
}
```

Which is then to be merged with the `Pagination` type the following way if the pagination has a flat structure.

```typescript
type PaginatedVariableresponse<Type extends Record<string | number, unknown>, DataKey extends string = 'data', Discriminator extends string = ';'> =
    VariableKeyData<Type, DataKey>
    & Pagination<Type, Discriminator>;
```

Or if the pagination has a nested structure.

```typescript
type PaginatedVariableresponse<Type extends Record<string | number, unknown>, DataKey extends string = 'data', Discriminator extends string = ';'> =
    VariableKeyData<Type, DataKey>
    & { pagination: Pagination<Type, Discriminator> };
```