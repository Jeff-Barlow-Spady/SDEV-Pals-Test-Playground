# SDEV2301 xUnit Assertions --- Quick Cheat Card

Use this card while writing tests with the course xUnit project.
Examples are compatible with the course package version, xUnit 2.9.3.

## Test attributes

    [Fact]          // One test case
    [Theory]        // The same behaviour checked with supplied data
    [InlineData()]  // One data row for a Theory

## Arrange --- Act --- Assert

    // Arrange
    var circle = new Circle(5);

    // Act
    var area = circle.GetArea();

    // Assert
    Assert.Equal(78.54, area, precision: 2);

AAA is a useful reading and organization pattern. Short tests may
combine Arrange and Act in one clear statement.

## Choose the assertion that matches the evidence

  Evidence to check                             Useful assertion
  --------------------------------------------- ------------------------------------------------------------
  Exact expected value                          `Assert.Equal(expected, actual)`
  Values must differ                            `Assert.NotEqual(notExpected, actual)`
  Condition must be true or false               `Assert.True(condition)` / `Assert.False(condition)`
  Value must be missing or present              `Assert.Null(value)` / `Assert.NotNull(value)`
  Text or collection must contain an item       `Assert.Contains(expected, actual)`
  Text or collection must not contain an item   `Assert.DoesNotContain(notExpected, actual)`
  Collection must be empty or non-empty         `Assert.Empty(collection)` / `Assert.NotEmpty(collection)`
  Value must be within an acceptable range      `Assert.InRange(actual, low, high)`
  An action must throw an exception             `Assert.Throws<TException>(() => action)`

## Exact and floating-point values

    Assert.Equal(expected, actual);
    Assert.NotEqual(notExpected, actual);

For floating-point results, specify suitable tolerance rather than
relying on exact binary equality.

    // precision is the number of decimal places
    Assert.Equal(78.54, area, precision: 2);

    // Use a range when the acceptable limits are part of the requirement
    Assert.InRange(area, 78.53, 78.55);

## Boolean and null checks

    Assert.True(account.Balance >= 0);
    Assert.False(order.IsCancelled);

    Assert.Null(result);
    Assert.NotNull(customer);

## Strings and collections

    Assert.Contains("expected text", actualString);
    Assert.StartsWith("Hello", actualString);
    Assert.EndsWith("!", actualString);

    Assert.Empty(items);
    Assert.NotEmpty(items);
    Assert.Contains(expectedItem, items);
    Assert.DoesNotContain(unwantedItem, items);

## Expected exceptions

    Assert.Throws<InvalidOperationException>(() =>
        account.Withdraw(200));

Capture the exception only when another required detail should be
checked.

    var ex = Assert.Throws<ArgumentException>(() =>
        account.Deposit(-1));

    Assert.Equal("amount", ex.ParamName);

::: callout
**Do not catch the exception manually inside the test.** Do not require
an exact exception message unless the message itself is specified
behaviour; message wording can change.
:::

## Parameterized tests

Use a Theory when the same behaviour should hold for several data rows.

    [Theory]
    [InlineData(1.0, 3.14)]
    [InlineData(5.0, 78.54)]
    public void GetArea_WithDifferentRadii_ReturnsExpectedArea(
        double radius, double expected)
    {
        var circle = new Circle(radius);

        Assert.Equal(expected, circle.GetArea(), precision: 2);
    }

## Multiple related assertions

One logical behaviour may require more than one related assertion. Keep
unrelated behaviours in separate tests.

    Assert.Multiple(
        () => Assert.Equal(32, rectangle.GetArea()),
        () => Assert.Equal(24, rectangle.GetPerimeter())
    );

`Assert.Multiple` is supported by the course xUnit package, but it is
optional. Two ordinary related assertions are also acceptable in an
introductory test.

## Test names

A useful pattern is `Member_Scenario_ExpectedResult`.

    Constructor_ValidName_SetsNames
    FullName_ValidName_ReturnsLastCommaFirst
    Withdraw_AmountExceedsBalance_Throws

## Unit testing and TDD

- Unit testing can be added to behaviour that already exists.
- In test-driven development, write one focused test before adding the
  next small behaviour.
- TDD cycle: **Red** --- observe the intended failure; **Green** --- add
  the smallest clear code that passes; **Refactor** --- improve without
  changing behaviour, then run all tests again.
- "Fails first" is a TDD check, not a requirement that every unit test
  in every project must have been written first.

## Common problems to avoid

- Reversing `expected` and `actual` in `Assert.Equal`.
- Comparing floating-point results without an appropriate precision or
  range.
- Printing a result instead of using an assertion.
- Catching exceptions manually instead of using
  `Assert.Throws<TException>`.
- Combining unrelated behaviours in one test.
- Testing only trivial auto-properties instead of validation or
  observable behaviour.
- Using unexplained values that make the scenario difficult to
  understand.
- Running only the newest test after production code changes; use **Run
  All** to detect regressions.

## Before submitting

- The test name explains the member, scenario, and expected result.
- Arrange, Act, and Assert are easy to identify.
- The assertion matches the required evidence.
- The test checks observable behaviour, not private implementation
  details.
- Normal, boundary, and invalid cases are considered where relevant.
- All tests pass.
- For a TDD task, the intended Red was observed before production code
  was added.
