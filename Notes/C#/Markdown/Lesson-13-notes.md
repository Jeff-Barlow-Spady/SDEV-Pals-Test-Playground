# SDEV2301 Lesson 13 Student Guide --- Introduction to LINQ and Collection Queries

**Course version:** Fall 2026 / Winter 2027\
**Environment:** .NET 10 and Visual Studio 2026\
**Lesson focus:** `Where`, `Select`, and simple method chaining

## Lesson Goal

Learn how to describe simple collection queries in C# using LINQ.

By the end of this lesson, you should be able to:

- explain what a short LINQ query does;
- use `Where` to filter elements;
- use `Select` to transform elements;
- convert a simple filtering loop into LINQ;
- predict the values and element type produced by a query; and
- explain why the original collection is unchanged.

## Files Used in This Lesson

Open the supplied solution:

    L13_LinqIntro.slnx

It contains:

    L13_LinqIntro
    ├── Program.cs
    ├── Student.cs
    └── Product.cs

::: {.callout .file-note}
**File workflow**

- Do **not** create another solution or project.
- Do **not** create a new source file for each activity.
- Continue editing `Program.cs` unless your instructor asks you to
  inspect `Student.cs` or `Product.cs`.
- Comment out or replace an earlier query when you are ready for the
  next activity.
- Keep the supplied `students` and `products` collections unchanged.
:::

## What Is LINQ?

LINQ stands for **Language Integrated Query**. It provides C# methods
for working with data from collections and other sources.

In this lesson, LINQ helps us answer questions such as:

- Which students have a mark of at least 70?
- What are the names of those students?
- Which products belong to the `"Tech"` category?

LINQ lets us describe the result we want without manually writing every
loop and temporary collection.

## The LINQ Mental Model

> **Source sequence → query operation → result sequence**

For a longer chain, ask what sequence exists after every operation:

    List<Student>
        → Where(...): IEnumerable<Student>
        → Select(...): IEnumerable<string>

Always ask:

1.  What is the source?
2.  What does this operation do?
3.  Which values remain or what does each value become?
4.  What is the element type now?
5.  What is the final result type?

## The Two Core Methods

  Method     Question it answers                What happens to the element type?
  ---------- ---------------------------------- -----------------------------------
  `Where`    Which elements should remain?      It stays the same.
  `Select`   What should each element become?   It may change.

### `Where` Filters Elements

`Where` keeps only the elements that satisfy a condition.

``` csharp
var passingStudents = students
    .Where(student => student.Mark >= 70);
```

- Source elements: `Student`
- Result elements: `Student`
- Result type: `IEnumerable<Student>`

The expression inside `Where` must produce `true` or `false` for each
element.

### `Select` Transforms Elements

`Select` produces one result value from each source element.

``` csharp
var studentNames = students
    .Select(student => student.Name);
```

- Source elements: `Student`
- Result elements: `string`
- Result type: `IEnumerable<string>`

`Select` does not mean "choose some elements." It means "choose what
each element becomes."

## Reading a Lambda Expression

``` csharp
student => student.Mark >= 70
```

Read this as:

> For this student, determine whether the student's mark is at least 70.

``` csharp
student => student.Name
```

Read this as:

> For this student, produce the student's name.

The variable name on the left of `=>` represents one element from the
sequence.

## From a Loop to LINQ

### Familiar loop

``` csharp
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
var evens = new List<int>();

foreach (var number in numbers)
{
    if (number % 2 == 0)
    {
        evens.Add(number);
    }
}
```

### Equivalent LINQ query

``` csharp
var evens = numbers
    .Where(number => number % 2 == 0);
```

Both approaches can produce the values `2`, `4`, and `6`. The loop
describes the individual steps. The LINQ query describes which values
should remain.

LINQ does not replace every loop. It is especially useful when the task
is a collection query such as filtering or transforming elements.

## Combining `Where` and `Select`

To obtain the names of passing students:

``` csharp
var passingNames = students
    .Where(student => student.Mark >= 70)
    .Select(student => student.Name);
```

  Stage            Meaning                                      Element type
  ---------------- -------------------------------------------- --------------
  `students`       Start with every student                     `Student`
  `.Where(...)`    Keep students whose mark is at least 70      `Student`
  `.Select(...)`   Produce the name of each remaining student   `string`

The final result type is `IEnumerable<string>`.

## LINQ Does Not Modify the Source

``` csharp
var passingStudents = students
    .Where(student => student.Mark >= 70);
```

This query creates a result sequence. It does not remove failing
students from `students`. The original collection still contains all of
its original elements.

This is different from calling a method whose purpose is to modify a
collection, such as `Remove` or `RemoveAll`.

## Do You Need `ToList()`?

Not every query needs `ToList()`. For the basic activities in this
lesson, you can store and enumerate the `IEnumerable<T>` returned by
`Where` and `Select`:

``` csharp
IEnumerable<string> names = students
    .Select(student => student.Name);
```

Use `ToList()` when the requirement specifically needs a `List<T>` or a
separate list at that point in the program:

``` csharp
List<string> names = students
    .Select(student => student.Name)
    .ToList();
```

Do not add `ToList()` automatically after every query.

## Predict Before You Run

For each query, identify its purpose, output values, and element type
before running it.

``` csharp
var query1 = students
    .Where(student => student.Mark < 70);

var query2 = students
    .Select(student => student.Mark);

var query3 = products
    .Where(product => product.Price <= 15m)
    .Select(product => product.Name);
```

1.  Which elements remain?
2.  Does the element type change?
3.  What is the final result type?
4.  Is the original collection modified?

## Practice Activities

::: practice
### Student Queries

**File:** Continue editing `L13_LinqIntro/Program.cs`. Do not create a
new source file.

1.  Names of students with marks of at least 80.
2.  Marks of students who are failing with a mark below 70.
3.  Names containing at least five characters.
:::

::: practice
### Product Queries

**File:** Continue editing the same `Program.cs` file and use the
supplied `products` collection.

1.  Names of products priced above `$20`.
2.  Categories of products priced at or below `$10`.
3.  Uppercase names of products in the `"School"` category.
:::

For each practice query, use LINQ method syntax, predict the result's
element type before running it, and keep the original collection
unchanged. Avoid `foreach` for these particular activities.

## Common Mistakes

### Using `Select` when you need `Where`

Ask whether you want fewer elements or a new value from every element.

- Fewer elements → `Where`
- A transformed value from every element → `Select`

### Forgetting that the result type can change

After `Select(student => student.Name)`, the sequence contains `string`
values, not `Student` objects.

### Writing the methods in the wrong order

Read the chain from left to right and confirm that each operation
receives the type it needs.

### Expecting the original collection to change

The query creates a result sequence. It does not automatically add,
remove, or replace elements in the source.

### Creating a new project or source file for every activity

Continue using the supplied solution and `Program.cs` throughout this
lesson.

## Topics for Later Lessons

You are not expected to use these topics yet:

- sorting with `OrderBy`, `OrderByDescending`, or `ThenBy`;
- grouping with `GroupBy`;
- element operators such as `First` or `Single`;
- aggregate methods;
- query-expression syntax;
- database or EF Core queries;
- repository or query-service classes; or
- unit testing LINQ queries.

Focus on reading and writing clear `Where` and `Select` chains.

## Quick Reference

``` csharp
// Keep matching objects.
var passingStudents = students
    .Where(student => student.Mark >= 70);

// Produce one property value from every object.
var names = students
    .Select(student => student.Name);

// Filter first, then produce a different result shape.
var passingNames = students
    .Where(student => student.Mark >= 70)
    .Select(student => student.Name);
```

> **`Where` decides which elements stay. `Select` decides what each
> element becomes.**

## Self-Check

- I can identify the source sequence.
- I can explain a predicate in plain language.
- I use `Where` when I need to filter elements.
- I use `Select` when I need to transform elements.
- I can trace the element type after each method.
- I can predict the result before running the code.
- I understand that a LINQ query does not modify the source collection.
- I know that "no `foreach`" is a constraint for today's LINQ practice,
  not a rule for every C# program.

## Optional Take-Home Practice

Complete the **LINQ Basics -- Pokémon** practice after class. It
reinforces the same `Where` and `Select` skills using a different
domain.

Leave the longer `PokemonQueryService` exercise until sorting and
additional LINQ operators have been introduced.
