---
paths:
  - "**/*.test.*"
  - "**/*.spec.*"
  - "tests/**"
  - "__tests__/**"
  - "e2e/**"
---

# Testing Rules

## Coverage Requirements
- Minimum 80% coverage for new code
- Critical paths (auth, payments): 95%+
- Utility functions: 100%

## Test Structure (AAA Pattern)
```
// Arrange - Set up test data and conditions
// Act - Execute the function/action being tested
// Assert - Verify the expected outcome
```

## Naming Convention
```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create a user with valid data', () => {});
    it('should throw error with duplicate email', () => {});
    it('should handle Turkish characters in name', () => {});
  });
});
```

## What to Test
- Happy path (normal expected behavior)
- Error cases (invalid input, missing data)
- Edge cases (empty strings, null, undefined)
- Turkish character handling (İ, ı, ğ, ş, ü, ö, ç)
- Authorization (forbidden access, expired tokens)
- Pagination boundaries (first page, last page, empty)

## Test Data
- Use factories or fixtures for consistent test data
- Include Turkish test strings: "İstanbul", "Güneş", "çiçek"
- Never use production data in tests
- Clean up test data after each test

## Mocking
- Mock external APIs and services
- Mock database for unit tests
- Use real database for integration tests
- Never mock the function being tested

## Do Not
- Do NOT write tests that depend on execution order
- Do NOT use random data without seeding
- Do NOT skip failing tests; fix the root cause
- Do NOT test implementation details; test behavior
