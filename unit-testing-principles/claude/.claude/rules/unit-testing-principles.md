# Unit Testing Principles

## Purpose

This repository follows the unit testing discipline described in **Unit Testing: Principles, Practices, and Patterns** by Vladimir Khorikov:
write tests that are **fast, isolated, readable, and maintainable**, verifying behavior through precise assertions.

All test generation, test reviews, and production code decisions must optimize for:
- tests that run in milliseconds, not seconds
- full isolation from external systems, databases, networks, and other units
- clear Arrange-Act-Assert structure in every test
- meaningful assertions that verify behavior, not implementation
- test code that is as well-structured as production code
- tests that survive refactoring without constant rewrites

This file is a binding engineering policy for Claude.

---

## Primary Directive

When writing tests, do **not** treat them as secondary code.
Write each test as a clear, isolated verification of one specific behavior.
Write production code with testability as a first-class concern.

Prefer:
1. one test per behavior
2. AAA structure (Arrange, Act, Assert)
3. fast, deterministic execution
4. isolation through fakes and stubs
5. assertions that verify state or verified interactions

Reject tests that:
- depend on databases, networks, files, or other external systems
- mix multiple behaviors in one test
- assert implementation details instead of observable behavior
- require manual setup or shared fixtures with hidden state
- take more than a few milliseconds to run

---

## What Counts as a Unit Test

A unit test here means:
- testing a single unit of work (a method, a function, a small class)
- full isolation from other units via mocks, stubs, or fakes
- deterministic results on every run
- execution in milliseconds
- assertions on state changes or verified interactions

A unit test here does **not** mean:
- integration tests (even if they are fast)
- tests that touch databases, file systems, networks, or message queues
- end-to-end flows spanning multiple layers
- smoke tests or acceptance tests
- tests that rely on shared mutable fixtures

---

## Non-Negotiable Rules

1. **Isolate Everything**
   - Every dependency must be replaceable with a fake, stub, or mock.
   - No database calls, no HTTP calls, no file I/O, no system clock.
   - If a test hits an external system, it is not a unit test.

2. **One Behavior Per Test**
   - Each test verifies exactly one behavior or scenario.
   - Do not combine multiple assertions about different behaviors into one test.
   - If a test reads like a paragraph, split it.

3. **Follow AAA Structure**
   - Every test must have a clear Arrange phase, Act phase, and Assert phase.
   - Separate each phase with blank lines or comments.
   - Never mix arrangement with action or assertions with setup.

4. **Name Tests to Explain Intent**
   - Test names must describe the scenario and expected outcome.
   - Use a consistent naming convention (e.g., `MethodName_Scenario_ExpectedBehavior`).
   - A reader should understand the test from its name alone.

5. **Assert Behavior, Not Implementation**
   - Verify state changes through public interfaces.
   - Verify interactions only when the behavior IS the interaction.
   - Do not assert private fields, internal method calls, or implementation details.

---

## AAA Structure Rules

### Arrange Phase
1. Set up the system under test (SUT).
2. Configure all dependencies (stubs, mocks, fakes).
3. Prepare input data and preconditions.
4. Keep arrangement minimal — only what the test needs.
5. Do not perform the action or make assertions in this phase.

### Act Phase
1. Call exactly one method or function on the SUT.
2. Capture the return value or result.
3. Keep this phase to one or two lines.
4. Do not arrange or assert in this phase.

### Assert Phase
1. Verify the expected outcome with precise assertions.
2. Assert state changes (return values, property values, collection contents).
3. Assert interactions only when the behavior under test is the interaction itself.
4. Use assertion libraries that provide clear failure messages.
5. Do not arrange or act in this phase.

---

## Test Naming Rules

1. Use a consistent convention across the project.
2. Recommended format: `MethodName_Scenario_ExpectedBehavior`
   - Example: `Withdraw_InsufficientFunds_ThrowsException`
   - Example: `CalculateDiscount_PremiumMember_Returns20Percent`
3. The name should make the assertions self-evident.
4. Avoid vague names like `Test1`, `ShouldWork`, or `BasicTest`.
5. Do not include framework boilerplate in the name (e.g., avoid `Test_` prefix if the framework adds it).

---

## Mocking Rules

### When to Use Mocks
1. Use mocks when the behavior being tested is an interaction with a dependency.
2. Verify that a specific method was called with expected arguments.
3. Verify call counts, order, or absence of calls when they define the behavior.
4. Mock only the direct collaborators of the SUT.

### When to Use Stubs
1. Use stubs to provide predetermined responses from dependencies.
2. Use stubs for data retrieval, configuration, or lookup behavior.
3. Configure stubs to return values that exercise the SUT logic.
4. Do not verify stub interactions — they exist only to support the SUT.

### When to Use Fakes
1. Use fakes for in-memory replacements of complex dependencies (e.g., in-memory repositories, fake mail senders).
2. Fakes implement the real interface with simplified, test-friendly behavior.
3. Prefer fakes over mocks when the dependency is used across many tests.
4. Fakes should be simple enough to understand in a single read.

### Mocking Anti-Patterns
1. Do not mock value objects or simple data structures.
2. Do not mock types you do not own (mock only your own abstractions).
3. Do not verify every interaction — verify only what defines the behavior.
4. Do not use mocks when a simple stub or fake suffices.
5. Avoid over-mocking: if you need more than 2-3 mocks, the SUT may have too many dependencies.

---

## State-Based vs Interaction-Based Testing

### State-Based Testing (Default)
1. Prefer state-based testing for most scenarios.
2. Call the SUT method, then assert the resulting state.
3. State includes return values, property changes, collection contents, and object graph changes.
4. State-based tests survive refactoring better than interaction-based tests.

### Interaction-Based Testing
1. Use interaction-based testing when the behavior IS the interaction.
2. Examples: sending a notification, publishing an event, calling an external service through an interface.
3. Verify only the interactions that define the behavior under test.
4. Combine with state assertions when both matter.

### Choosing Between Them
1. Default to state-based.
2. Switch to interaction-based only when there is no meaningful state to assert.
3. Use both when the behavior has observable state AND required interactions.

---

## Testing Difficult Scenarios

### Testing Time-Dependent Code
1. Abstract the system clock behind an interface (e.g., `IClock`, `TimeProvider`).
2. Inject the clock interface instead of calling `DateTime.Now` or `Date.now()`.
3. Provide a fixed or controllable time in tests.
4. Never use real time in unit tests.

### Testing Random Values
1. Abstract random number generation behind an interface (e.g., `IRandom`).
2. Inject a predictable sequence in tests.
3. Test boundary conditions with specific sequences.
4. Never rely on actual randomness in unit tests.

### Testing External Services
1. Abstraction over the service via an interface.
2. Stub or mock the interface in tests.
3. Reserve real service calls for integration tests only.
4. Design the interface to be testable (avoid passing raw HTTP clients).

### Testing File I/O
1. Abstract file operations behind an interface (e.g., `IFileReader`, `IFileSystem`).
2. Use in-memory implementations in tests.
3. Never read or write real files in unit tests.

---

## Test Code Quality Rules

1. **Apply the same standards to test code as production code.**
   - Clean names, small methods, no duplication.
   - Refactor tests when they become messy.

2. **Remove duplication across tests.**
   - Extract shared arrangement into helper methods or factories.
   - Do not create complex base test classes with hidden setup.
   - Prefer inline clarity over clever extraction.

3. **Keep test methods short.**
   - A test should fit on a screen.
   - If arrangement is complex, extract it into a named helper.
   - The AAA structure should be visible at a glance.

4. **Do not test private methods.**
   - Test behavior through the public interface.
   - If a private method seems to need its own test, the class may be doing too much.
   - Private methods are tested indirectly through public behavior.

5. **Do not assert on implementation details.**
   - Do not verify the exact type of an internal collection.
   - Do not assert on private fields through reflection.
   - Do not verify that a specific internal method was called.
   - Assert only what an external observer would see.

---

## Test Organization Rules

1. **Group tests by the unit under test.**
   - One test class per production class (or per cohesive group of methods).
   - Name the test class after the SUT (e.g., `OrderServiceTests`).

2. **Order tests logically.**
   - Group related scenarios together within a test class.
   - Use region markers or section comments if the test class grows large.

3. **Keep test classes focused.**
   - If a test class exceeds ~50 tests, consider splitting by responsibility.
   - Do not combine unrelated scenarios into one test class.

4. **Arrange test files to mirror production structure.**
   - Place test files near their production counterparts when the build system supports it.
   - Maintain a clear mapping between production and test code.

---

## Test Data Rules

1. **Use meaningful test data.**
   - Values should make the scenario clear (e.g., `$100` for a balance test, not `$0`).
   - Avoid `null` unless the test is specifically about null handling.
   - Choose values that exercise the behavior being tested.

2. **Keep test data minimal.**
   - Provide only the data needed for the scenario.
   - Do not over-provision objects with irrelevant properties.

3. **Use factories or builders for complex objects.**
   - Extract object construction into factories when it repeats across tests.
   - Keep factories simple and focused on test setup.
   - Do not hide test behavior inside factory methods.

4. **Avoid shared mutable test data.**
   - Each test should create its own data.
   - Do not rely on data set up by other tests.
   - Do not use static or global test fixtures with mutable state.

---

## Code Generation Rules

When generating tests, follow this order:
1. Identify the single behavior to verify.
2. Name the test to describe the scenario and expected outcome.
3. Arrange: create the SUT, configure dependencies, prepare inputs.
4. Act: call one method on the SUT.
5. Assert: verify the expected state or interaction.
6. Review: is the test fast, isolated, readable, and maintainable?

When generating production code, consider testability:
1. Design dependencies as interfaces, not concrete types.
2. Avoid static calls to non-testable services (clocks, randomness, I/O).
3. Keep methods focused on a single behavior.
4. Favor dependency injection over service locators or globals.
5. If code is hard to test, refactor it before writing the test.

---

## Review Rules for Claude

When reviewing or generating tests, actively look for:
- tests that depend on databases, networks, or file systems
- tests that verify multiple behaviors
- missing AAA structure or mixed phases
- vague test names
- assertions on implementation details
- over-mocking or under-mocking
- tests that rely on real time or randomness
- duplicated test setup code
- tests that are slow or non-deterministic
- private method tests
- shared mutable fixtures
- production code that cannot be tested in isolation

---

## Forbidden Patterns

Do not generate or keep these patterns unless explicitly required and justified.

### Integration Test Disguised as Unit Test
- calling real databases or APIs from a "unit test"
- relying on test containers or embedded servers in unit tests
- testing multiple layers together and calling it a unit test

### God Test
- one test method that verifies five different behaviors
- massive Arrange sections with dozens of setup lines
- mixing multiple scenarios into a single test

### Implementation Assertions
- asserting on private fields
- verifying internal method calls
- checking exact collection types rather than behavior
- testing that a specific algorithm was used rather than the result

### Mock Overload
- mocking value objects
- mocking types you do not own
- verifying every single interaction in a test
- needing more mocks than a reader can hold in working memory

### Fragile Tests
- tests that break on every refactor of the SUT
- tests coupled to parameter order, internal class structure, or framework details
- tests that assert on exception messages word-for-word
- tests that depend on execution order

### Slow Tests
- tests that take more than a few milliseconds
- tests with warm-up delays or sleep calls
- tests that load large data sets from files
- tests that initialize heavy frameworks

---

## Stopping Rules

Stop adding tests when:
- every public behavior has at least one test
- every meaningful scenario (happy path, edge cases, error conditions) is covered
- the test suite runs in under a few seconds total
- adding more tests would only verify trivial or obvious behavior
- the cost of maintaining additional tests outweighs their value

Stop refactoring tests when:
- AAA structure is clear and consistent
- duplication is removed without losing clarity
- names explain intent
- the test suite is fast and deterministic

---

## Review Checklist

Before finalizing any test, verify:
- Does the test name describe the scenario and expected outcome?
- Is the AAA structure clear and separated?
- Does the test verify exactly one behavior?
- Are all dependencies isolated (no external systems)?
- Are assertions on behavior, not implementation details?
- Is the test fast (milliseconds, not seconds)?
- Is the test deterministic (same result every run)?
- Is the test readable by someone unfamiliar with the code?
- Does the test survive reasonable refactoring of the SUT?
- Is the test code clean and free of duplication?

If any answer is no, revise before shipping.

---

## Final Instruction

When uncertain, write the simplest test that verifies the behavior through the public interface,
using AAA structure, full isolation, and a clear name.
Reject tests that are slow, fragile, coupled to implementation, or harder to read than the code they test.
