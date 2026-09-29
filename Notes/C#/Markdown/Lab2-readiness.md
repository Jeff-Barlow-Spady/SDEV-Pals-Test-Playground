# Lab 2 Readiness: OOP and Unit Testing Practice Path

Practice C# object-oriented programming, xUnit, nullable reference types, and the **Red → Green → Refactor** cycle before Lab 2.

> **This is ungraded take-home practice.**
>
> Complete the Core activities before Lab 2. Continue with the Recommended and Challenge activities if you want more practice. Nothing needs to be submitted unless your instructor says otherwise.

---

## Choose Your Practice Path

| Priority | Activities | Suggested Time |
|----------|------------|----------------|
| **Core** | `Book` and `BankAccount` | 60-90 minutes |
| **Recommended** | `Rectangle` with parameterized tests | 30-45 minutes |
| **Challenge** | `Song` and `Playlist` composition | 45-60 minutes |
| **Extra Practice** | `Movie`, `Student`, or `Car` | As needed |

---

## One-Time Setup in Visual Studio 2026

1. Create an **Empty Solution** named `SDEV2301Lab2Practice`.
2. Add a **Class Library** project targeting **.NET 10**. Name it `OopPractice`.
3. Add an **xUnit Test Project** targeting **.NET 10**. Name it `OopPractice.Tests`.
4. In `OopPractice.Tests`, add a project reference to `OopPractice`.
5. Delete the automatically generated `Class1.cs` and `UnitTest1.cs` files.
6. Create a `Models` folder inside `OopPractice`.

> Visual Studio may create either a `.slnx` or `.sln` solution file. Either is acceptable.
>
> No additional assertion package is required. Use the built-in xUnit `Assert` methods.

### Target Layout

```text
SDEV2301Lab2Practice/
│
├── [Visual Studio solution file]
│
├── OopPractice/
│   └── Models/
│       ├── Book.cs
│       ├── BankAccount.cs
│       └── ...
│
└── OopPractice.Tests/
    ├── BookTests.cs
    ├── BankAccountTests.cs
    └── ...
```

---

## Test-Driven Development (TDD) Cycle

For every feature or behavior:

1. **RED** - Write **one** test for one behavior.
2. Run the test and confirm it fails for the expected reason.
3. **GREEN** - Write the minimum code necessary to make the test pass.
4. Run all tests and confirm they pass.
5. **REFACTOR** - Improve code quality without changing behavior.
6. Run all tests again and repeat.

> **Important**
>
> Do not write all tests first and then implement the entire class.
>
> Work in small Red → Green → Refactor cycles. A useful test must fail before the behavior is implemented.

### Testing Conventions

- Use **Arrange - Act - Assert** structure.
- Name tests using:
  ```text
  Member_Scenario_ExpectedResult
  ```
- Use `[Fact]` for a single test case.
- Use `[Theory]` and `[InlineData]` for parameterized tests.
- Use `Assert.Throws<TException>()` for exceptions.
- Use precision-based assertions when comparing `double` values.

### Reusable Test Template

```csharp
using OopPractice.Models;
using Xunit;

namespace OopPractice.Tests;

public class ExampleTests
{
    [Fact]
    public void Member_Scenario_ExpectedResult()
    {
        // Arrange

        // Act

        // Assert
    }
}
```

---

# Core Practice A: Book

> ✅ Complete before Lab 2

### Files

**Production**

```text
OopPractice/Models/Book.cs
```

**Tests**

```text
OopPractice.Tests/BookTests.cs
```

### Required Behavior

- `Title` is required.
- Blank or whitespace titles throw `ArgumentException`.
- `Pages` must be greater than zero.
- Invalid page counts throw `ArgumentOutOfRangeException`.
- `Subtitle` is optional and declared as `string?`.
- Constructor accepts required values and an optional subtitle.
- `DisplayTitle()` returns:
  - `"Title: Subtitle"` when a subtitle exists.
  - `"Title"` when no subtitle exists.

### Required Tests

1. `Constructor_ValidValues_SetsProperties`
2. `Constructor_BlankTitle_ThrowsArgumentException`
3. `Constructor_NonPositivePages_ThrowsArgumentOutOfRangeException`
4. `DisplayTitle_SubtitleProvided_IncludesSubtitle`
5. `DisplayTitle_SubtitleMissing_ReturnsTitleOnly`

> **Nullable Reminder**
>
> Use `string?` for optional values and handle null values intentionally. Avoid suppressing nullable warnings with `!`.

---

# Core Practice B: BankAccount

> ✅ Complete before Lab 2

### Files

**Production**

```text
OopPractice/Models/BankAccount.cs
```

**Tests**

```text
OopPractice.Tests/BankAccountTests.cs
```

### Required Behavior

- `AccountNumber` is required.
- Blank or whitespace values throw `ArgumentException`.
- `Balance` is a `decimal`.
- Balance can only be modified by the class.
- Opening balance defaults to `0m`.
- Opening balance cannot be negative.
- `Deposit(decimal amount)` only accepts values greater than zero.
- `Withdraw(decimal amount)` only accepts values greater than zero.
- Withdrawing more than the available balance:
  - throws `InvalidOperationException`
  - leaves the balance unchanged

### Required Tests

1. `Constructor_NoOpeningBalance_SetsBalanceToZero`
2. `Constructor_NegativeOpeningBalance_ThrowsArgumentOutOfRangeException`
3. `Deposit_PositiveAmount_IncreasesBalance`
4. `Deposit_NonPositiveAmount_ThrowsArgumentOutOfRangeException`
5. `Withdraw_AmountWithinBalance_DecreasesBalance`
6. `Withdraw_AmountExceedsBalance_ThrowsInvalidOperationException`
7. `Withdraw_AmountExceedsBalance_LeavesBalanceUnchanged`

---

# Recommended Practice: Rectangle

> Additional parameterized testing practice

### Files

**Production**

```text
OopPractice/Models/Rectangle.cs
```

**Tests**

```text
OopPractice.Tests/RectangleTests.cs
```

### Required Behavior

- `Length` and `Width` must be greater than zero.
- Invalid dimensions throw `ArgumentOutOfRangeException`.
- `GetArea()` returns:

```text
Length × Width
```

- `GetPerimeter()` returns:

```text
2 × (Length + Width)
```

### Suggested Tests

- `GetArea_ValidDimensions_ReturnsExpectedArea`
- `GetPerimeter_ValidDimensions_ReturnsExpectedPerimeter`
- `Constructor_NonPositiveLength_ThrowsArgumentOutOfRangeException`
- `Constructor_NonPositiveWidth_ThrowsArgumentOutOfRangeException`

### Example

```csharp
Assert.Equal(expected, actual, precision: 2);
```

---

# Challenge Practice: Playlist Composition

> Multi-class design exercise

### Files

**Production**

```text
OopPractice/Models/Song.cs
OopPractice/Models/Playlist.cs
```

**Tests**

```text
OopPractice.Tests/SongTests.cs
OopPractice.Tests/PlaylistTests.cs
```

### Required Behavior

#### Song

- Requires nonblank `Title`
- Requires nonblank `Artist`
- `LengthInSeconds` must be greater than zero

#### Playlist

- Maintains a private song collection
- Exposes `SongCount`
- Prevents replacement of the collection from outside the class
- `AddSong(Song song)` throws `ArgumentNullException` for null
- `TotalLengthInSeconds()`:
  - Returns 0 for an empty playlist
  - Returns the combined length of all songs otherwise

### Suggested Tests

- `AddSong_ValidSong_IncreasesSongCount`
- `AddSong_NullSong_ThrowsArgumentNullException`
- `TotalLengthInSeconds_EmptyPlaylist_ReturnsZero`
- `TotalLengthInSeconds_MultipleSongs_ReturnsCombinedLength`

---

# Extra Practice Menu

Choose any of the following if you need more practice.

## Movie

Create:

```text
Movie.cs
MovieTests.cs
```

Requirements:

- Nonblank title
- Rating from 0 to 10
- Use `[Theory]` tests for boundary values

---

## Student

Create:

```text
Student.cs
StudentTests.cs
```

Requirements:

- Nonblank first name
- Nonblank last name
- Nonblank program
- Nullable `PreferredName`
- `DisplayName()` method uses preferred name when available

---

## Car

Create:

```text
Car.cs
CarTests.cs
```

Requirements:

- Nonblank make
- Nonblank model
- Year must be 1886 or later
- Override `ToString()` to return:

```text
Year Make Model
```

Only test behavior owned by the `Car` class. Do not create tests for built-in `List<T>` functionality.

---

# Responsible Use of AI

AI tools can support learning but should not replace your thinking or your TDD process.

- Design the next test yourself.
- Use AI for explanations, hints, troubleshooting, or feedback.
- Do not generate the entire solution at once.
- Verify that tests fail and pass for the intended reasons.
- Be able to explain every assertion, exception, and code change.

---

# Lab 2 Readiness Checklist

- [ ] I can explain why production code and test code are in separate projects.
- [ ] The test project references the class library.
- [ ] I can create and run `[Fact]` and `[Theory]` tests.
- [ ] I use Arrange - Act - Assert correctly.
- [ ] I can test exceptions with `Assert.Throws<TException>()`.
- [ ] I understand the Red → Green → Refactor workflow.
- [ ] I understand the difference between `string` and `string?`.
- [ ] I can diagnose failing tests using Test Explorer.
- [ ] All Core practice tests pass.

---

## Optional Evidence

Consider:

- Making Git commits after meaningful TDD cycles.
- Saving screenshots of a failing test and the same test passing.

These are optional unless specifically requested by your instructor.

---

## Recommended Workflow

```text
Write one failing test
        ↓
Implement minimum code
        ↓
Run all tests
        ↓
Refactor
        ↓
Repeat
```
