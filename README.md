# Android Testing Cheat Sheet

## General Best Practices

1. **Follow the AAA Pattern**: Arrange, Act, Assert.
2. **Isolate Units**: Make sure each test case is independent.
3. **Descriptive Test Names**: Use clear and descriptive test method names.
4. **Test Behavior, Not Implementation**: Your tests should not break if you refactor code.

## Libraries and Tools

- **JUnit**: For unit tests.
- **Mockito**: For mocking objects.
- **Espresso**: For UI tests.
- **Robolectric**: For running Android unit tests on the JVM.
- **UI Automator**: For system and user acceptance tests.

## Unit Testing

### Setup

```kotlin
@Before
fun setUp() {
    // Initialize objects, mocks, etc.
}
```

### Basic Test

```kotlin
@Test
fun shouldDoSomething() {
    // Arrange
    // Act
    // Assert
}
```

### Mocking with Mockito

```kotlin
@Mock
lateinit var mockObject: SomeClass

// Stubbing
`when`(mockObject.someMethod()).thenReturn(someValue)
```

### Verifying Method Calls

```kotlin
verify(mockObject).someMethod()
```

## Integration Testing

- Use `AndroidJUnitRunner` for running integration tests.
- Test the interaction between different units of your application.

### Example

```kotlin
@RunWith(AndroidJUnit4::class)
class MyIntegrationTest {
    // Your test code
}
```

## UI Testing with Espresso

### Basic Test

```kotlin
@Test
fun shouldDisplayText() {
    onView(withId(R.id.myTextView))
        .check(matches(withText("Hello World")))
}
```

### Click Action

```kotlin
onView(withId(R.id.myButton)).perform(click())
```

### Test RecyclerView

```kotlin
onView(withId(R.id.myRecyclerView))
    .perform(RecyclerViewActions.actionOnItemAtPosition<RecyclerView.ViewHolder>(0, click()))
```

## Robolectric

- Use Robolectric for tests that you want to run on the JVM instead of an Android device.

### Example

```kotlin
@RunWith(RobolectricTestRunner::class)
class MyRobolectricTest {
    // Your test code
}
```

## Continuous Integration

- Use CI tools like Jenkins, CircleCI, or GitHub Actions to automate your tests.

