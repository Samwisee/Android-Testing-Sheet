# Android Testing Cheat Sheet

## General Best Practices

1. **Follow the AAA Pattern**: Arrange, Act, Assert.
2. **Isolate Units**: Make sure each test case is independent.
3. **Descriptive Test Names**: Use clear and descriptive test method names.
4. **Test Behavior, Not Implementation**: Your tests should not break if you refactor code.

### Red-Green-Refactor Cycle

- **Red**: Write a failing test.
- **Green**: Write just enough code to make the test pass.
- **Refactor**: Clean up the code while keeping it functional.

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
    MockitoAnnotations.openMocks(this)
}
```

### Basic Test

```kotlin
@Test
fun shouldAddTwoNumbers() {
    // Arrange
    val a = 5
    val b = 10

    // Act
    val result = a + b

    // Assert
    assertEquals(15, result)
}
```

### Mocking with Mockito

```kotlin
@Mock
lateinit var userRepository: UserRepository

// Stubbing
@Before
fun setUp() {
    `when`(userRepository.authenticate("user", "pass")).thenReturn(true)
}
```

### Verifying Method Calls

```kotlin
@Test
fun shouldCallAuthenticate() {
    // Arrange
    val viewModel = LoginViewModel(userRepository)

    // Act
    viewModel.login("user", "pass")

    // Assert
    verify(userRepository).authenticate("user", "pass")
}
```

## Integration Testing

- Use `AndroidJUnitRunner` for running integration tests.
- Test the interaction between different units of your application.

### Example

```kotlin
@RunWith(AndroidJUnit4::class)
class MyIntegrationTest {

    @Mock
    private lateinit var dataRepository: DataRepository

    @Before
    fun setUp() {
        MockitoAnnotations.openMocks(this)
        `when`(dataRepository.fetchData()).thenReturn("Hello, World!")
    }

    @Test
    fun shouldFetchAndDisplayData() {
        // Launch MainActivity
        ActivityScenario.launch(MainActivity::class.java)

        // Verify that the TextView displays the fetched data
        onView(withId(R.id.textView)).check(matches(withText("Hello, World!")))
    }
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
@Test
fun shouldUpdateTextOnClick() {
    onView(withId(R.id.myButton)).perform(click())
    onView(withId(R.id.myTextView)).check(matches(withText("Button Clicked")))
}
```

### Test RecyclerView

```kotlin
@Test
fun shouldSelectRecyclerViewItem() {
    onView(withId(R.id.myRecyclerView))
        .perform(RecyclerViewActions.actionOnItemAtPosition<RecyclerView.ViewHolder>(0, click()))
}
```

## Robolectric

- Use Robolectric for tests that you want to run on the JVM instead of an Android device.

### Example

```kotlin
@RunWith(RobolectricTestRunner::class)
class MyRobolectricTest {
    @Test
    fun shouldChangeActivityTitle() {
        val activity: MainActivity = Robolectric.setupActivity(MainActivity::class.java)
        activity.title = "New Title"

        assertEquals("New Title", activity.title)
    }
}
```

## Continuous Integration

- Use CI tools like Jenkins, CircleCI, or GitHub Actions to automate your tests.

## Full Test Example

```
interface NumberRepository {
    fun getNumbers(): List<Int>
}

class NumberViewModel(private val numberRepository: NumberRepository) : ViewModel() {
    fun getOddNumbers(): List<Int> {
        val numbers = numberRepository.getNumbers()
        return numbers.filter { it % 2 != 0 }
    }
}

class NumberViewModelTest {

    private lateinit var viewModel: NumberViewModel

    @Mock
    private lateinit var repository: NumberRepository

    @Before
    fun setUp() {
        MockitoAnnotations.openMocks(this)
        viewModel = NumberViewModel(repository)
    }

    @Test
    fun testGetOddNumbers() {
        // Arrange
        `when`(repository.getNumbers()).thenReturn(listOf(1, 2, 3, 4, 5))

        // Act
        val result = viewModel.getOddNumbers()

        // Assert
        assertEquals(listOf(1, 3, 5), result)
    }
}
```
