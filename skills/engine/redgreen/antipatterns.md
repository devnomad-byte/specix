# Testing Antipatterns

Common testing mistakes and the gate functions that prevent them.

---

## 1. The Happy Path Only

**Pattern:** Tests only verify the success case. Every function has a happy path test, but no test for invalid inputs, missing data, error conditions, or boundary values.

**Why it is dangerous:** Production does not send happy inputs. Users submit empty forms, APIs return errors, files are missing, networks timeout. Happy-path-only tests give false confidence.

**Gate function:**
```
For every test that asserts success, write a companion test that
asserts failure for the same function. If the function accepts input,
test: null, empty, negative, overflow, wrong type, missing field.
```

---

## 2. The Implementation Tester

**Pattern:** Tests assert on internal implementation details — private method names, variable values inside closures, the order of operations, specific loop counters.

**Why it is dangerous:** When the implementation changes (which it should be free to do during refactoring), the test breaks even though behavior is unchanged. This makes refactoring expensive and discourages cleanup.

**Gate function:**
```
Before writing a test assertion, ask: "If I rewrote this function
with a completely different algorithm, would this test still pass?"
If the answer is no, the test is coupled to implementation.
Assert on public interface behavior only.
```

---

## 3. The Sleep-and-Pray

**Pattern:** Tests include `sleep(500)`, `await delay(1000)`, or other fixed waits to "let things settle" before asserting.

**Why it is dangerous:** Fixed waits are either too short (test flaps) or too long (test suite becomes unusably slow). They do not guarantee the condition is met — they guarantee only that time has passed.

**Gate function:**
```
Replace every fixed delay with a polling assertion or event listener.
Wait for the actual condition to be true, not for an arbitrary time
to elapse. If the condition cannot be polled, the system under test
needs a notification mechanism (callback, event, observable).
```

---

## 4. The Copy-Paste Assert

**Pattern:** A test suite contains multiple tests that assert the same property with slightly different setup. When the assertion needs to change, one test gets updated and the others do not.

**Why it is dangerous:** Creates a maintenance nightmare where some tests validate the old behavior and some validate the new behavior. Contradictory tests can mask real bugs.

**Gate function:**
```
If two tests assert the same property, extract the assertion into a
shared helper function. The assertion lives in one place. Tests
provide different inputs to that shared assertion. Changes to the
assertion logic happen once.
```

---

## 5. The Mystery Guest

**Pattern:** Tests depend on external state that is not visible in the test file — a database row that "should exist," a file in a specific location, an environment variable set somewhere else.

**Why it is dangerous:** The test passes on one machine and fails on another. The test passes today and fails tomorrow. Nobody reading the test file can understand what it needs to run.

**Gate function:**
```
Every external dependency must be set up within the test itself
(or its before/beforeEach hook). If a test needs a database row,
the test inserts that row. If it needs a file, the test creates
that file. No external "test data" directories. No shared mutable
fixtures. Each test is self-contained.
```

---

## 6. The Assertion-Free Test

**Pattern:** A test runs code and does not crash, and the lack of a crash is treated as a passing result. There are no explicit assertions.

**Why it is dangerous:** "It didn't throw" is not the same as "it produced the correct result." The function could return wrong data, create incorrect side effects, or silently fail — and the test would still "pass."

**Gate function:**
```
Every test must contain at least one explicit assertion that verifies
a specific expected outcome. "No exception" is a valid assertion
ONLY if the test is specifically testing that invalid input does not
crash — and that must be written as an explicit assertion:
expect(() => fn(invalid)).not.toThrow()
```

---

## 7. The Over-Fixture

**Pattern:** Test setup (beforeEach, fixture builders, mock factories) is so complex and abstracted that reading a test requires navigating through 5 helper files to understand what "the default setup" actually creates.

**Why it is dangerous:** Tests become unreadable. When a test fails, the developer cannot quickly understand what scenario is being tested because the data is hidden behind layers of indirection.

**Gate function:**
```
A test reader should understand the test scenario from reading the
test function alone, without opening other files. Use inline data
and explicit setup. Extract helpers only when the same literal setup
is repeated in 3+ tests — and name the helper to describe the
scenario, not the mechanism (e.g., createUserWithExpiredSubscription,
not createTestUser with magic parameters).
```
