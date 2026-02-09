Run the test suite for this project.

Detect project type and run appropriate tests:

- **nextjs-saas**: `pnpm test` (Jest + React Testing Library)
- **django-api**: `pytest` (pytest-django)
- **nestjs-api**: `pnpm test` (Jest)
- **wordpress-client**: `wp core verify-checksums`
- **fullstack-nextjs-django**: `pnpm --filter frontend test && cd backend && pytest`
- **fullstack-nextjs-nestjs**: `pnpm --filter frontend test && pnpm --filter backend test`

After running, show:
1. Total tests: passed / failed / skipped
2. Coverage summary (if available)
3. List of failing tests (if any)
4. Suggestions for fixing failures
