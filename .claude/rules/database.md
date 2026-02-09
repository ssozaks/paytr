---
paths:
  - "**/migrations/**"
  - "**/entities/**"
  - "**/models.py"
  - "**/models/**"
  - "**/schemas/**"
---

# Database Rules

## PostgreSQL Standards
- Encoding: UTF-8 with tr_TR.UTF-8 collation
- All names in snake_case
- Primary key: `id` (serial or UUID)
- Timestamps: `created_at`, `updated_at` (timestamptz)
- Soft delete: `deleted_at` (nullable timestamptz)
- Foreign keys: `<referenced_table>_id`

## Migration Best Practices
- One migration per logical change
- NEVER modify published/deployed migrations
- Test rollback (down) for every migration
- Separate data migrations from schema migrations
- Always backup before production migrations
- Name migrations descriptively: `add_email_verification_to_users`

## Indexing Strategy
- Index ALL foreign keys
- Index columns used in WHERE clauses frequently
- Index columns used in ORDER BY
- Use partial indexes for filtered subsets
- Use GIN indexes for JSONB columns
- Monitor with `EXPLAIN ANALYZE`

## Query Optimization
- Avoid N+1: use select_related/prefetch_related (Django) or eager loading (TypeORM)
- Use database-level constraints (NOT NULL, UNIQUE, CHECK)
- Paginate all list queries
- Use connection pooling in production (PgBouncer)
- Set statement_timeout to prevent runaway queries

## Turkish Collation
- Use `tr_TR.UTF-8` collation for proper Turkish sorting
- Turkish sort order: a, b, c, ç, d, e, f, g, ğ, h, ı, i, j, k, l, m, n, o, ö, p, r, s, ş, t, u, ü, v, y, z
- Test sorting with Turkish data before deployment
