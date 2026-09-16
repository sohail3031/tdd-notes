# Test-Driven Development (TDD) with Java, JUnit & Mockito

A hands-on learning repository covering **Test-Driven Development (TDD), unit testing, test doubles, Mockito, and code refactoring** using Java.

This repository was built while completing a practical TDD course. The exercises focus on writing tests, improving existing code through refactoring, managing dependencies, and applying common software design principles.

The goal was not only to learn how to write tests, but also to understand **how testing influences code design and how TDD can be used safely during refactoring**.

---

## 🎯 What I Learned

Throughout the course, I worked with:

- Test-Driven Development (TDD)
- JUnit 5
- Mockito
- Unit testing
- Test doubles
- Dependency Injection
- Arrange-Act-Assert (AAA)
- Parameterized tests
- Mocks, stubs, spies, and dummies
- ArgumentCaptor
- Fakes and in-memory repositories
- Code refactoring
- Extract Class / Extract Method
- Feature Envy
- Code duplication
- Magic numbers
- Long methods
- Large classes
- Parameter Object pattern
- Single Responsibility Principle (SRP)
- Dependency management
- Behavior-preserving refactoring

---

# 📚 Course Topics

## 1. Introduction to TDD

The course started with the fundamentals of Test-Driven Development and the mindset required to approach software development through tests.

### Topics

- Introduction to Test-Driven Development
- TDD mindset
- Generalization in TDD
- JUnit testing environment setup
- Arrange-Act-Assert (AAA) pattern
- Introduction to dependencies in testing
- Managing dependencies in TDD

### TDD Cycle

```text
        ┌─────────────┐
        │     RED     │
        │ Write a     │
        │ failing test│
        └──────┬──────┘
               ↓
        ┌─────────────┐
        │    GREEN    │
        │ Write enough│
        │ code to pass│
        └──────┬──────┘
               ↓
        ┌─────────────┐
        │  REFACTOR   │
        │ Improve the │
        │ design/code │
        └──────┬──────┘
               │
               └──────────→ Repeat
```

The central principle was:

> **Write a failing test → make it pass → improve the code while keeping the tests green.**

---

# 🧪 2. JUnit Testing

The repository contains practical JUnit tests covering different types of application behavior.

### Topics

- JUnit 5
- `@Test`
- `@BeforeEach`
- Assertions
- `assertEquals`
- `assertThrows`
- `assertDoesNotThrow`
- Parameterized tests
- `@ValueSource`
- `@MethodSource`
- `Arguments`
- Test setup and isolation
- Testing positive and negative scenarios

### AAA Pattern

Tests follow the **Arrange → Act → Assert** structure where appropriate.

```java
@Test
void shouldCalculateDiscount() {
    // Arrange
    DiscountCalculator calculator = new DiscountCalculator();
    
    // Act
    double discount = calculator.calculateDiscount(100);
    
    // Assert
    assertEquals(10, discount, 0.001);
}
```

The AAA structure makes tests easier to read and understand.

---

# 🔢 3. Parameterized Tests

Parameterized tests were used when the same behavior needed to be tested with multiple inputs.

Examples include:

- Multiple discount codes    
- Multiple tax values
- Multiple invalid inputs
- Different calculation scenarios

### `@ValueSource`

```java
@ParameterizedTest
@ValueSource(strings = {"SAVE10", "SUMMER20", "VIP15"})
void shouldAcceptValidDiscountCodes(String code) {
    // test
}
```

### `@MethodSource`

More complex test data was supplied using `Stream<Arguments>`:

```java
@ParameterizedTest
@MethodSource("discountTestCases")
void shouldCalculateDiscount(
        double subtotal,
        double expectedDiscount
) {
    // test
}
```

Parameterized testing reduces duplicated test code while increasing test coverage.

---

# 🧩 4. Test Doubles

A major part of the course focused on replacing real dependencies with controlled test doubles.

The repository covers:

| Test Double | Purpose                                                              |
| ----------- | -------------------------------------------------------------------- |
| Dummy       | Passed to satisfy a dependency but not actually used                 |
| Stub        | Provides predefined responses                                        |
| Mock        | Verifies interactions with a dependency                              |
| Spy         | Wraps or observes behavior while allowing interaction verification   |
| Fake        | Working lightweight implementation used instead of a real dependency |

---

# 🎭 5. Mockito

Mockito was used to create and control test doubles.

### Topics

- Introduction to mocks    
- Mockito mocks
- Stubs
- Dynamic mock behavior
- Spies
- ArgumentCaptor
- Dependency Injection with Mockito
- Avoiding unnecessary dependency calls

Example:

```java
PricingService pricingService = mock(PricingService.class);

when(pricingService.getPrice("P001"))
        .thenReturn(100.0);
```

The course also covered verifying interactions:

```java
verify(pricingService).getPrice("P001");
```

---

# 🕵️ 6. Stubs

Stubs were used when the test needed a controlled response from a dependency.

Examples included:

- Weather services    
- Pricing services
- Email services
- External dependencies

The key idea:

> A stub controls what a dependency returns so that the unit under test can be tested deterministically.

---

# 🎭 7. Spies

Spies were used when we wanted to observe or verify behavior while retaining real behavior where appropriate.

Topics included:

- Introduction to spies    
- Notification priority
- Multiple notifications
- Verifying method calls

---

# 📸 8. ArgumentCaptor

`ArgumentCaptor` was used to capture arguments passed to mocked dependencies.

Example concept:

```java
ArgumentCaptor<Notification> captor =
        ArgumentCaptor.forClass(Notification.class);

verify(notificationService).send(captor.capture());

Notification notification = captor.getValue();
```

This is useful when the important behavior is not simply **whether a method was called**, but **what data was passed to it**.

---

# 🗃️ 9. Fakes and In-Memory Repositories

The course introduced fakes as lightweight working implementations for tests.

An in-memory repository can replace a real database during unit testing.

Example architecture:

```text
UserService
     │
     ▼
UserRepository
     │
     ├── Production → Database
     │
     └── Tests → In-Memory Fake
```

Exercises included:

- Duplicate email validation    
- Updating user names
- Handling non-existent users
- Deactivating users
- Handling non-existent users during deactivation

This demonstrated how dependency abstraction makes business logic easier to test.

---

# 🛒 10. Shopping Cart TDD Project

A major part of the course was a progressively developed **Shopping Cart system**.

The project started with basic requirements and gradually introduced additional behavior.

### Initial Requirements

- Empty cart    
- Add a single item
- Add multiple items
- Multiple quantities
- Remove an item
- Remove a non-existent item
- Update item quantity
- Clear all items
- Retrieve item details
- Add an existing item

### Advanced Requirements

- Quantity limits
- Maximum quantity handling
- Percentage discounts
- Bulk discounts
- Discount codes
- Invalid discount codes
- Negative discount percentages
- Very small prices

### Additional Features

- Price levels
- Tax calculation
- Shipping calculation
- Loyalty points
- Electronics bonus points
- VIP customer benefits
- Order summaries

The Shopping Cart project provided a practical environment for applying TDD concepts to a larger system.

---

# 🧱 11. Refactoring

A significant part of the course focused on refactoring code while keeping existing behavior unchanged.

The major refactoring techniques covered include:

### Extract Method

Used to break long methods into smaller, focused methods.

```text
Long Method
     ↓
Extract Method
     ↓
Smaller cohesive methods
```

---

### Extract Class

Used to move a group of related responsibilities into a dedicated class.

For example:

```text
ShoppingCart
     │
     ├── CartItemCollection
     ├── PriceCalculator
     ├── DiscountCalculator
     ├── ShippingCalculator
     └── LoyaltyPointsCalculator
```

This reduced the responsibilities of `ShoppingCart`.

---

### Extracted Calculator Classes

The Shopping Cart system eventually included:

- `PriceCalculator`    
- `DiscountCalculator`
- `ShippingCalculator`
- `LoyaltyPointsCalculator`

Each calculator focuses on a specific responsibility.

---

# 📦 12. CartItemCollection

Cart item management was extracted from `ShoppingCart` into:

```text
CartItemCollection
```

Responsibilities include:

- Adding items    
- Updating quantities
- Removing items
- Retrieving cart items

This reduced the amount of collection-management logic inside `ShoppingCart`.

---

# 💰 13. PriceCalculator

Pricing-related behavior was extracted into:

```text
PriceCalculator
```

Responsibilities include:

- Calculating item prices    
- Applying customer price levels
- Calculating subtotals
- Calculating tax

This allowed the pricing logic to be tested independently.

---

# 🏷️ 14. DiscountCalculator

Discount-related behavior was extracted into:

```text
DiscountCalculator
```

Responsibilities include:

- Storing discount rates    
- Validating discount codes
- Applying discount codes
- Calculating discount amounts

Example:

```text
SAVE10   → 10%
SUMMER20 → 20%
VIP15    → 15%
```

The extracted class also made it possible to create dedicated unit tests for discount behavior.

---

# 🚚 15. ShippingCalculator

Shipping calculations were extracted from `ShoppingCart`.

Responsibilities include:

- Calculating total shipment weight    
- Calculating base shipping cost
- Handling free-shipping conditions
- Considering customer price level

---

# ⭐ 16. LoyaltyPointsCalculator

Loyalty point calculation was extracted into its own class.

Responsibilities include:

- Base points based on subtotal    
- Category-specific bonus points
- VIP point multipliers

This further reduced the responsibilities of `ShoppingCart`.

---

# 👨‍🎓 17. Feature Envy and Move Method

The course covered the **Feature Envy** code smell.

Feature Envy occurs when a method relies heavily on another class's data instead of the data of the class where the method currently resides.

Example:

```text
GradeAnalyzer
      │
      └── Constantly accesses Student grades
```

The behavior was moved closer to the data:

```text
Student
  │
  └── Grades
       │
       └── Grade-related behavior
```

Refactorings included:

- Move behavior into `StudentGrade`    
- Move final grade calculation into `Student`
- Move `getFailedAssignments()` into `Student`
- Extract `StudentGradeCollection`

---

# 🎓 18. StudentGradeCollection

Grade-management responsibilities were extracted from `Student` into:

```text
StudentGradeCollection
```

Responsibilities include:

- Managing grades    
- Adding grades
- Calculating final grades
- Finding failed assignments

The resulting relationship is:

```text
Student
   │
   ▼
StudentGradeCollection
   │
   ├── StudentGrade
   ├── StudentGrade
   └── StudentGrade
```

This improves cohesion by keeping grade-collection operations together.

---

# 🔧 19. Parameter Object Pattern

The course covered the **Long Parameter List** code smell.

Instead of passing many related parameters:

```java
processExamScore(
    courseCode,
    examScore,
    homeworkComplete,
    attendanceScore,
    bonusActivities,
    isRetake,
    examWeight,
    homeworkWeight,
    attendanceWeight,
    passingThreshold,
    ...
);
```

related values were grouped into objects.

### `ScoreWeights`

```java
ScoreWeights
├── examWeight
├── homeworkWeight
└── attendanceWeight
```

### `CoursePolicy`

```text
CoursePolicy
├── bonusPointsPerActivity
├── passingThreshold
├── retakePenaltyPercentage
└── missedHomeworkPenaltyPercentage
```

This makes method signatures easier to understand and maintain.

---

# 🔀 20. Reordering Parameters

Parameters were also reordered to improve clarity and cohesion.

The resulting organization separated:

```text
Course information
        ↓
Student performance
        ↓
Configuration
```

This was a behavior-preserving refactoring.

---

# 🧮 21. Default Parameters with Method Overloading

Java does not provide native default parameter values like some other languages.

The course demonstrated how **method overloading** can be used to provide convenient defaults.

Example:

```java
public ExamResult processExamScore(
        String courseCode,
        double examScore
) {
    return processExamScore(
        courseCode,
        examScore,
        DEFAULT_IS_HOMEWORK_COMPLETE,
        DEFAULT_ATTENDANCE_SCORE,
        ...
    );
}
```

This allows callers to provide only the information they need.

---

# 🔢 22. Refactoring Magic Numbers

Hard-coded values such as:

```java
5
0.9
0.85
0.1
```

can make code difficult to understand.

They were extracted into named constants:

```java
private static final double BULK_DISCOUNT_THRESHOLD = 5;
private static final double BULK_DISCOUNT_RATE = 0.9;
private static final double STUDENT_DISCOUNT_RATE = 0.85;
```

Named constants communicate intent and provide a single place to maintain important values.

---

# ♻️ 23. Removing Code Duplication

Duplicated logic was identified and extracted into reusable methods.

For example:

```text
Duplicated calculation
        ↓
Extract Method
        ↓
Single implementation
        ↓
Multiple callers
```

This follows the **DRY (Don't Repeat Yourself)** principle.

---

# 📏 24. Long Method Code Smell

Long methods were identified as a maintainability problem.

The refactoring process involved:

1. Identifying logically separate responsibilities.    
2. Extracting methods.
3. Giving methods meaningful names.
4. Updating callers.
5. Running tests.
6. Confirming behavior remains unchanged.

---

# 🏢 25. Large Class Code Smell

The course also covered the **Large Class** code smell.

A large class may contain:

- Too many fields    
- Too many methods
- Multiple responsibilities
- Complex conditionals
- Difficult-to-test behavior

The solution explored was **Extract Class**.

The Shopping Cart project provided a practical example of gradually breaking a large class into smaller components.

---

# 🧹 26. Code Smells Covered

| Code Smell / Design Problem | Refactoring          |
| --------------------------- | -------------------- |
| Code duplication            | Extract Method       |
| Magic numbers               | Extract Constants    |
| Long methods                | Extract Method       |
| Large class                 | Extract Class        |
| Feature Envy                | Move Method          |
| Long parameter list         | Parameter Object     |
| Excessive responsibilities  | Extract Class        |
| Tight dependencies          | Dependency Injection |

---

# 🧪 27. Testing After Refactoring

An important lesson throughout the course was that tests act as a **safety net during refactoring**.

The general workflow was:

```text
Existing Tests
      ↓
Understand Behavior
      ↓
Make Small Refactoring
      ↓
Run Tests
      ↓
Tests Pass?
   ↙       ↘
 YES        NO
  ↓          ↓
Continue    Investigate
```

The objective of these refactorings was generally to improve the internal structure **without changing externally observable behavior**.

---

# 🏗️ 28. Dependency Injection

Dependency Injection was used to make classes easier to test.

Instead of a class creating everything it needs internally:

```java
public Service() {
    repository = new DatabaseRepository();
}
```

dependencies can be supplied externally:

```java
public Service(Repository repository) {
    this.repository = repository;
}
```

This makes it possible to substitute:

- Mocks    
- Stubs
- Spies
- Fakes

during testing.

---

# 🔌 29. Abstracting Dependencies

The course also covered abstraction of dependencies such as ID generation and data storage.

Example:

```java
public interface IIDGenerator {
    String generateId();
}
```

and:

```java
public interface IDataStore {
    void store(String userId, UserData userData);
}
```

This separates the application logic from concrete implementations.

---

# 🗂️ 30. User Registration TDD Project

Another practical exercise focused on user registration and repository behavior.

Topics included:

- User registration    
- Duplicate email validation
- Updating user names
- Handling non-existent users
- Deactivating users
- Handling non-existent user deactivation
- Fake repositories
- Dependency injection

This demonstrated how business logic can be tested without requiring a real database.

---

# 🛠️ Technologies Used

- **Java**    
- **JUnit 5**
- **Mockito**
- **Maven/Gradle** where applicable
- **Git**
- **GitHub**

---

# 📂 Repository Structure

The repository is organized around the course exercises and concepts.

A simplified conceptual structure is:

```text
TDD/
│
├── Shopping Cart
│   ├── CartItemCollection
│   ├── PriceCalculator
│   ├── DiscountCalculator
│   ├── ShippingCalculator
│   └── LoyaltyPointsCalculator
│
├── User Registration
│   ├── User validation
│   ├── Fake repository
│   └── Dependency injection
│
├── Weather Alert Service
│   ├── Conditions
│   ├── Stubs
│   └── Mockito mocks
│
├── Notification Service
│   ├── Notification priority
│   ├── Spies
│   └── ArgumentCaptor
│
├── Pricing Service
│   ├── Mocking
│   ├── Validation
│   └── Dynamic mock behavior
│
├── Student / Grade
│   ├── Feature Envy
│   ├── Move Method
│   └── StudentGradeCollection
│
└── Refactoring
    ├── Extract Method
    ├── Extract Class
    ├── Parameter Object
    ├── Magic Numbers
    └── Code Duplication
```

> The exact directory structure may differ from this conceptual overview depending on how the individual exercises are organized in the repository.

---

# 🎓 Key Takeaways

The most important lessons from this course were:

### TDD

- Start with behavior and requirements.    
- Write tests before or alongside implementation.
- Use the Red → Green → Refactor cycle.
- Keep changes small and incremental.

### Unit Testing

- Test one responsibility at a time.
- Keep tests isolated.
- Use meaningful test names.
- Cover both positive and negative scenarios.

### Test Doubles

Understand when to use:

```text
Dummy
  ↓
Stub
  ↓
Mock
  ↓
Spy
  ↓
Fake
```

Each serves a different testing purpose.

### Refactoring

- Improve code structure without unnecessarily changing behavior.
- Remove duplication.
- Extract long methods.
- Break large classes into cohesive classes.
- Move behavior closer to the data it primarily uses.
- Replace long parameter lists with meaningful objects.

### Design

- Apply Single Responsibility Principle.
- Reduce unnecessary coupling.
- Prefer cohesive classes.
- Use dependency injection where appropriate.
- Program against abstractions when it improves testability.

---

# 🔄 Overall Learning Progression

The course progression can be summarized as:

```text
TDD Fundamentals
       ↓
JUnit
       ↓
AAA Pattern
       ↓
Dependencies
       ↓
Test Doubles
       ↓
Mockito
       ↓
Parameterized Tests
       ↓
Fakes & Dependency Injection
       ↓
Shopping Cart TDD
       ↓
Code Smells
       ↓
Extract Method
       ↓
Extract Class
       ↓
Move Method / Feature Envy
       ↓
Parameter Objects
       ↓
Refactoring Large Classes
       ↓
Testing Extracted Components
```

---

# 🚀 Purpose of This Repository

This repository serves as a practical reference for my learning and continued practice with:

**Java + JUnit + Mockito + TDD + Refactoring + Software Design**

Rather than focusing only on writing tests, the exercises demonstrate how automated tests can support the entire development process—from implementing requirements to safely restructuring an existing codebase.

---

## 📌 Core Principle

> **Good tests give us confidence to improve the design of our code.**

TDD is not only about testing code. It is also about designing software that is easier to understand, maintain, change, and test.

