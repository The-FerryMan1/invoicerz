# Testing Guide

This project includes comprehensive testing setup for both the frontend (Vue.js) and backend (Elysia.js) applications.

## Frontend Testing (Vue.js + Vite)

### Setup
The frontend uses Vitest with Vue Test Utils and Testing Library for component testing.

### Running Tests

```bash
# Run tests in watch mode (recommended for development)
cd web
bun run test

# Run tests once
bun run test:run

# Run tests with coverage
bun run test:coverage
```

### Test Structure
```
web/src/
├── components/__tests__/          # Component tests
├── stores/__tests__/             # Pinia store tests
├── util/__tests__/               # Utility function tests
└── test/
    └── setup.ts                  # Global test setup
```

### Example Test Files
- `components/__tests__/userName.test.ts` - Component testing example
- `stores/__tests__/counter.test.ts` - Store testing example
- `util/__tests__/csvLink.test.ts` - Utility testing example

## Backend Testing (Elysia.js + Bun)

### Setup
The backend uses Bun's built-in test runner for fast and native testing.

### Running Tests

```bash
# Run tests in watch mode
cd api
bun run test:watch

# Run tests once
cd api
bun run test
```

### Test Structure
```
api/src/
├── modules/__tests__/             # Route integration tests
├── utils/__tests__/              # Utility function tests
└── middleware/__tests__/         # Middleware tests (if needed)
```

### Example Test Files
- `utils/__tests__/csvExported.test.ts` - CSV export utility test
- `utils/__tests__/email.test.ts` - Email utility test
- `modules/__tests__/clients.test.ts` - API route integration test

## Test Categories

### Unit Tests
- Test individual functions and utilities
- Mock external dependencies
- Focus on business logic

### Component Tests
- Test Vue components in isolation
- Mock stores and external dependencies
- Test user interactions and rendering

### Integration Tests
- Test API routes with mocked services
- Test component interactions with stores
- Verify data flow between layers

### Store Tests
- Test Pinia store actions and getters
- Test state mutations
- Verify computed properties

## Best Practices

### Frontend
1. Use `describe` and `it` for clear test organization
2. Mock external dependencies (auth, API calls, etc.)
3. Test user interactions with `@testing-library/vue`
4. Use `beforeEach` to reset state between tests
5. Test both success and error scenarios

### Backend
1. Mock database operations and external services
2. Test API responses and status codes
3. Use Bun's native test assertions
4. Mock authentication middleware
5. Test error handling and edge cases

## Coverage

Run tests with coverage to ensure adequate test coverage:

```bash
# Frontend coverage
cd web && bun run test:coverage

# Backend coverage (if supported)
cd api && bun run test --coverage
```

## CI/CD Integration

Tests can be integrated into CI/CD pipelines:

```yaml
# Example GitHub Actions
- name: Run Frontend Tests
  run: cd web && bun run test:run

- name: Run Backend Tests
  run: cd api && bun run test
```

## Debugging Tests

### Frontend
- Use `console.log` in test files
- Use Vitest's `--reporter=verbose` for detailed output
- Debug components with Vue DevTools in test environment

### Backend
- Use `console.log` for debugging
- Check Bun test output for detailed error messages
- Mock complex dependencies to isolate issues

## Mocking Strategies

### External APIs
```typescript
vi.mock('@/lib/auth-client', () => ({
  authClient: { /* mock implementation */ }
}))
```

### DOM APIs
```typescript
Object.defineProperty(window, 'matchMedia', {
  value: () => ({ /* mock implementation */ })
})
```

### File System (Backend)
```typescript
vi.mock('fs', () => ({
  readFileSync: vi.fn()
}))
```

## Common Testing Patterns

### Testing Async Operations
```typescript
it('handles async operations', async () => {
  const result = await someAsyncFunction()
  expect(result).toBe(expectedValue)
})
```

### Testing Error Scenarios
```typescript
it('handles errors gracefully', async () => {
  mockFunction.mockRejectedValue(new Error('Test error'))
  await expect(asyncFunction()).rejects.toThrow('Test error')
})
```

### Testing Vue Components
```typescript
import { mount } from '@vue/test-utils'

it('renders correctly', () => {
  const wrapper = mount(Component)
  expect(wrapper.text()).toContain('Expected text')
})
```