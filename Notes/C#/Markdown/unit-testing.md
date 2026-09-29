# Writing Good Unit Tests {#writing-good-unit-tests dir="auto" line="0"}

## Bad vs Good Examples {#bad-vs-good-examples dir="auto" line="2"}

This handout shows **common testing mistakes** and how to fix them
using **xUnit best practices** expected in SDEV2301.

## 1️⃣ Bad vs Good Test Structure {#1️⃣-bad-vs-good-test-structure dir="auto" line="7"}

### ❌ Bad Test (Low Value) {#-bad-test-low-value dir="auto" line="9"}

``` {dir="auto" line="11"}
[Fact]
public void Test1()
{
    var calc = new DiscountCalculator();

    var r1 = calc.Calculate(100, "Regular");
    var r2 = calc.Calculate(100, "VIP");

    if (r1 != 90)
        throw new Exception("Wrong result");

    if (r2 != 80)
        throw new Exception("Wrong result");
}
```

**Problems**

- Vague test name
- Multiple behaviors in one test
- Manual checks instead of assertions
- Hard to tell what failed

### ✅ Good Test (Clear & Scalable) {#-good-test-clear--scalable dir="auto" line="36"}

``` {dir="auto" line="38"}
[Theory]
[InlineData(100, "Regular", 90)]
[InlineData(100, "VIP", 80)]
public void Calculate_ValidCustomerTypes_ReturnsDiscount(
    double price,
    string type,
    double expected)
{
    // Arrange
    var calc = new DiscountCalculator();

    // Act
    var result = calc.Calculate(price, type);

    // Assert
    Assert.Equal(expected, result);
}
```

**Why this is better**

- Descriptive name
- One behavior, many cases
- Uses `[Theory]`
- Clear Arrange--Act--Assert structure

## 2️⃣ Bad vs Good `[Fact]` Usage {#2️⃣-bad-vs-good-fact-usage dir="auto" line="65"}

### ❌ Bad `[Fact]` Usage {#-bad-fact-usage dir="auto" line="67"}

``` {dir="auto" line="69"}
[Fact]
public void Calculate_Discount_Test()
{
    var calc = new DiscountCalculator();

    Assert.Equal(90, calc.Calculate(100, "Regular"));
    Assert.Equal(80, calc.Calculate(100, "VIP"));
    Assert.Equal(100, calc.Calculate(100, "Other"));
}
```

**Problems**

- Multiple scenarios in one test
- Hard to see which case failed
- Should be data-driven

### ✅ Good `[Theory]` Usage {#-good-theory-usage dir="auto" line="88"}

``` {dir="auto" line="90"}
[Theory]
[InlineData(100, "Regular", 90)]
[InlineData(100, "VIP", 80)]
[InlineData(100, "Other", 100)]
public void Calculate_ReturnsCorrectAmount(
    double price,
    string type,
    double expected)
{
    var calc = new DiscountCalculator();
    Assert.Equal(expected, calc.Calculate(price, type));
}
```

**Rule**

> If you are repeating assertions, use `[Theory]`.

## 3️⃣ Bad vs Good Exception Tests {#3️⃣-bad-vs-good-exception-tests dir="auto" line="109"}

### ❌ Bad Exception Test {#-bad-exception-test dir="auto" line="111"}

``` {dir="auto" line="113"}
[Fact]
public void Divide_ByZero_Test()
{
    var calc = new Calculator();

    try
    {
        calc.Divide(4, 0);
    }
    catch (Exception)
    {
        return;
    }

    Assert.True(false);
}
```

**Problems**

- Catches any exception
- Manual control flow
- Test can pass for the wrong reason

### ✅ Good Exception Test {#-good-exception-test dir="auto" line="139"}

``` {dir="auto" line="141"}
[Fact]
public void Divide_ByZero_ThrowsDivideByZeroException()
{
    var calc = new Calculator();

    Assert.Throws<DivideByZeroException>(() =>
        calc.Divide(4, 0));
}
```

**Rule**

> Always assert the exact exception type.

## 4️⃣ Bad vs Good Test Naming {#4️⃣-bad-vs-good-test-naming dir="auto" line="157"}

### ❌ Bad Names {#-bad-names dir="auto" line="159"}

``` {dir="auto" line="161"}
Test1
DiscountTest
CalculatorWorks
```

**Problems**

- No behavior described
- Useless when tests fail

### ✅ Good Names {#-good-names dir="auto" line="173"}

``` {dir="auto" line="175"}
Divide_ByZero_ThrowsDivideByZeroException
Calculate_RegularCustomer_ReturnsDiscount
Save_NullInput_ThrowsArgumentNullException
```

**Naming Pattern**

``` {dir="auto" line="183"}
MethodName_State_ExpectedResult
```

**Rule**

> If you can't tell what failed from the name, the name is wrong.

## 5️⃣ Bad vs Good Arrange--Act--Assert (AAA) {#5️⃣-bad-vs-good-arrangeactassert-aaa dir="auto" line="192"}

### ❌ Bad AAA {#-bad-aaa dir="auto" line="194"}

``` {dir="auto" line="196"}
[Fact]
public void Add_TwoNumbers_ReturnsSum()
{
    var calc = new Calculator();
    var r1 = calc.Add(3, 4);
    Assert.Equal(7, r1);

    var r2 = calc.Add(1, 2);
    Assert.Equal(3, r2);
}
```

**Problems**

- Multiple behaviors
- No clear structure
- Hard to read and extend

### ✅ Good AAA {#-good-aaa dir="auto" line="215"}

``` {dir="auto" line="217"}
[Fact]
public void Add_TwoNumbers_ReturnsSum()
{
    // Arrange
    var calc = new Calculator();

    // Act
    var result = calc.Add(3, 4);

    // Assert
    Assert.Equal(7, result);
}
```

**Rule**

> One behavior per test. Arrange → Act → Assert should be obvious.

## 6️⃣ Final Rules to Remember {#6️⃣-final-rules-to-remember dir="auto" line="237"}

- Prefer `[Theory]` when testing multiple cases
- Use clear, descriptive test names
- Follow Arrange--Act--Assert
- One main assertion per test
- Tests should describe **behavior**, not implementation
- A failing test is useful information
