# Pipeline Design

1. **Triggers:** Push to `main` (excluding Markdown and `docs/` files), pull requests targeting `main`, manual dispatch (`workflow_dispatch`), and a weekly schedule (Mondays at 07:00 UTC).
2. **Jobs & Graph:** 
   - `test` runs first.
   - `package` runs after `test` succeeds.
   - `verify` runs after `package` succeeds.
   - `report` runs after `test` and `package` complete, using `always()` so it executes regardless of success or failure.
   - `pr-comment` runs after `package`, specifically for pull requests.
3. **Artifacts:** A test report (`test-report`) and a packaged distribution archive (`temperature-<version>.tar.gz`).
4. **Pipeline Type:** This is **Continuous Integration (CI)** because it automates building, testing, and verifying code quality on every integration event without automated deployment to production.
5. **Behavior on Test Failure:** The `package` job depends on `test` and will be skipped if tests fail. This prevents broken code from being packaged or released.