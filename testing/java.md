# Java

## BDDAssertions

### More readable when-then exception asserting

```java
// when
var actual = catchThrowable(() -> service.method(...));

// then
then(actual)
    .isInstanceOf(ExpectedException.class)
    .hasMessage("Exception message");
```

instead of:

```java
// when + then
thenExceptionOfType(ExpectedException.class)
    .isThrownBy(() -> service.method(...))
    .withMessage("Exception message");
```
