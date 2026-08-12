# CI Failure Diagnosis

## Failure 1 - Incorrect calculateDiscount Assertion

### Step
Jest test: `src/payments/calculateDiscount.test.js`

### Exact Error
`Expected: 100`
`Received: 90`

The failure occurred at `src/payments/calculateDiscount.test.js:8`.

### Type
Type 1 - Assertion failure

### Cause
The `calculateDiscount(100, 10)` function correctly returns 90 after applying a 10% discount. The test incorrectly expected 100. Therefore, the test assertion was wrong, not the implementation.

---

## Failure 2 - Incorrect Jest Matcher

### Step
Jest test: `src/utils/formatCurrency.test.js`

### Exact Error
`expect(received).toBe(expected) // Object.is equality`

The test used `toBe()` to compare two objects.

### Type
Type 1 - Assertion failure

### Cause
`toBe()` checks object identity rather than comparing object contents. The `formatCurrency()` function returns an object, so `toEqual()` is the appropriate matcher for comparing its properties and values.

---

## Failure 3 - Test Job Missing Repository Files

### Step
GitHub Actions: `test` job → `Run npm test`

### Exact Error
`npm error enoent Could not read package.json: Error: ENOENT: no such file or directory, open '/home/runner/work/ci-fix-drill/ci-fix-drill/package.json'`

### Type
Type 3 - Configuration failure

### Cause
The `test` job runs on a fresh GitHub Actions runner but does not contain an `actions/checkout` step. Therefore, the repository files, including `package.json`, are unavailable when `npm test` runs. The workflow must check out the repository before running the tests.