# Testing Rules

## Core Principles

- Do not make assumptions
- Read the code you are testing before writing tests
- If you cannot determine what test data should be, ask
- All logic of the code should be tested
- Tests for both the happy path (passing all validation) and the unhappy
  path (failing validation) must be created
- All unhappy paths must be tested. For example, if there is a validation
  that requires a number to be within a certain range, e.g. 1-10 inclusive,
  there should be an unhappy path test for a value of 0 and a value of 11
- If the response returns errors for each validation that did not succeed, then
  validations can be grouped
- If you are fixing tests, DO NOT change the test to accept an unexpected
  error or response. Investigate why the error/bad response is happening

## Coverage Requirements

- **Branch coverage**: Every conditional branch must be tested (if/else, ternary, switch cases)
- **Edge cases**: Test boundary conditions (empty, null, undefined, zero, negative, max values)
- **Error paths**: Every thrown error or rejected promise must have a test
- **Integration points**: Mock or stub external dependencies; test the integration separately
- **Minimum thresholds**:
  - Functions: 100% line coverage required
  - Conditionals: 100% branch coverage required
  - Error handlers: 100% coverage for catch blocks and error conditions
- **Coverage reporting**: Run coverage reports before submitting PRs; coverage must not decrease
