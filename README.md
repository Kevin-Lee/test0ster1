Test0ster1 (Testosterone)
=========================

[![Build Status](https://github.com/Kevin-Lee/test0ster1/workflows/Build/badge.svg)](https://github.com/Kevin-Lee/test0ster1/actions?workflow=Build)
[![Release Status](https://github.com/Kevin-Lee/test0ster1/workflows/Release/badge.svg)](https://github.com/Kevin-Lee/test0ster1/actions?workflow=Release)

[![Download](https://api.bintray.com/packages/kevinlee/maven/test0ster1/images/download.svg)](https://bintray.com/kevinlee/maven/test0ster1/_latestVersion)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/io.kevinlee/test0ster1/badge.svg)](https://search.maven.org/artifact/io.kevinlee/test0ster1)

Test0ster1 (pronounced as 'Testosterone', Testosterone -> Test0sterone -> Test0ster(one == 1) -> Test0ster1 ) is a simple test helper framework which uses Java 8's new functional features (i.e. Lambda Expressions, Default Methods, etc.) in order to provide simple and easy to use test tools.

## Why is it called 'Testosterone'?

1. It has 'test' in it **Test**-osterone.
2. The binary numbers, 0 and 1, which have something to do with software, are found in 'testosterone' (Test-**o**-ster-**one** => Test-**0**-ster-**1**).
3. The goal of this project is to provide a simple but powerful test framework just like testosterone makes men simple (or often called stupid :) ) and physically strong.


## What does it look like?

* test an expected exception

```java
import static testosterone.Testosterone.*;
import java.util.Objects;

import org.junit.Test;
```
```java

  @Test
  public void testTestosteroneExpectingException() throws Exception {
    /* Given */
    final String value = null;
    
    test("throwingNullTest",
         "Objects.requireNonNull(null, \"message\") should throw NullPointerException.")
    .when(() -> {
      Objects.requireNonNull(value, "value cannot be null.");
    })
    .expect(throwing(NullPointerException.class)
           .containsMessage("cannot be null"));
  }
```

* test an expected exception and its cause

```java
import static testosterone.Testosterone.*;

import org.junit.Test;
```
```java
  private void throwRuntimeException(final String value) {
    if (value == null) {
      throw new RuntimeException("test exception", new NullPointerException("value cannot be null."));
    }
  }

  @Test
  public void testTestosteroneExpectingExceptionWithCause() throws Exception {
    /* Given */
    final String value = null;

    test("throwingNullTest2",
        "throwRuntimeException(null) should throw RuntimeException caused by NullPointerException.")
    .when(() ->
      throwRuntimeException(value)
    )
    .expect(throwing(RuntimeException.class)
           .hasMessage("test exception")
           .containsMessage("test ")
           .causedBy(NullPointerException.class)
           .containsMessage("cannot be null"));
  }
```

* test a void return type method (with [Mockito](https://github.com/mockito/mockito))

```java
import static testosterone.Testosterone.*;
import static org.mockito.Mockito.*;

import org.junit.Test;
```
```java
  @Test
  public void testVerifyingVoidMethods() throws Exception {
    /* given */
    final Runnable innerRunnable1 = mock(Runnable.class);
    final Runnable innerRunnable2 = mock(Runnable.class);
    final Runnable runnable = () -> {
      innerRunnable1.run();
      innerRunnable2.run();
    };
    
    test("verifyVoidMethod",
         "innerRunnable1.run() and innerRunnable2.run() should be invoked when runnable.run().")
    .when(() -> {
      runnable.run();
    })
    .then(() ->
      verify(innerRunnable1, times(1)).run()
    )
    .then(() -> {
      verify(innerRunnable2, times(1)).run();
    });
  }
```

* test a method which returns some result (with [AssertJ](http://joel-costigliola.github.io/assertj/))

```java
import static testosterone.Testosterone.*;
import static org.assertj.core.api.Assertions.*;

import org.junit.Test;
```
```java
  private String nullSafeTrim(final String value) {
    return value == null ? "" : value.trim();
  }

  @Test
  public void testNullSafeTrim() {
    /* Given */
    final String expected = "result";
    final String input = "  " + expected + "  ";
    
    test("assertThat",
         "nullSafeTrim(\"  result  \") should return \"result\".")
    .when(() ->
      nullSafeTrim(input)
    )
    .then(actual ->
      assertThat(actual.length()).isEqualTo(expected.length())
    )
    .then(actual -> {
      assertThat(actual).isEqualTo(expected);
    });
  }
```

## Get Test0ster1
### Maven

```xml
<dependencies>
  ...

  <dependency>
    <groupId>io.kevinlee</groupId>
    <artifactId>test0ster1</artifactId>
    <version>0.2.0</version>
    <scope>test</scope>
  </dependency>

  ...
</dependencies>
```

### Gradle

```gradle
testCompile group: 'io.kevinlee', name: 'test0ster1', version: '0.2.0'
```
  OR

```gradle
testCompile "io.kevinlee:test0ster1:0.2.0"
```
