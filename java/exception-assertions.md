# Exception assertions

Exceptions were thrown example with `catchThrowable`

```java
import static org.assertj.core.api.BDDAssertions.catchThrowable;

// when
var actual = catchThrowable(() -> service.method(...));

// then
then(actual)
    .isInstanceOf(ExpectedException.class);
```

Exceptions were thrown example with `thenExceptionOfType`

```java
import static org.assertj.core.api.BDDAssertions.thenExceptionOfType;

// when + then
thenExceptionOfType(ExpectedException.class)
    .isThrownBy(() -> service.method(...));
```

No exceptions were thrown with `thenNoException`

```java
import static org.assertj.core.api.BDDAssertions.thenNoException;

// when + then
thenNoException()
    .isThrownBy(() -> service.method(...));
```
