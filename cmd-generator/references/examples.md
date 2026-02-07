# CLAUDE.md Examples & Anti-patterns

## Good Example: Web App Project

```markdown
# Project Overview
Next.js 14 (App Router) + TypeScript + Tailwind CSS + Prisma + PostgreSQL.
E-commerce platform for handmade crafts.

# Commands
- `npm run dev` — Start dev server (port 3000)
- `npm run build` — Production build
- `npm test` — Run vitest
- `npm run db:migrate` — Run Prisma migrations
- `npm run db:seed` — Seed test data

# Architecture
app/           → Next.js App Router pages and layouts
components/    → Reusable React components (PascalCase, e.g. ProductCard.tsx)
lib/           → Shared utilities and API clients
prisma/        → Schema and migrations
actions/       → Server Actions (one file per domain, e.g. cart-actions.ts)

# Coding Conventions
- Components: functional components + hooks only, no class components
- Naming: PascalCase for components, camelCase for utils, kebab-case for files
- Imports: use @/ path alias (mapped to project root)
- CSS: Tailwind utility classes only, no custom CSS files
- Error handling: wrap server actions in try/catch, return { success, error } objects

# Key Decisions
- Auth: NextAuth.js v5 with Google + Email providers
- State: Server-first, use React Server Components by default
- Client state only for interactive UI (cart drawer, modals)
- Images: Cloudinary via next-cloudinary, never store in repo

# Do NOT
- Add new dependencies without discussing first
- Modify prisma/schema.prisma without creating a migration
- Use `any` type — use `unknown` + type guards instead
- Skip writing tests for server actions
- Use relative imports when @/ alias is available
```

## Good Example: Python CLI Tool

```markdown
# Project Overview
CLI tool for batch-processing CSV files. Python 3.11+ with Click + Pandas.

# Commands
- `python -m pytest` — Run tests
- `python -m mypy src/` — Type check
- `pip install -e ".[dev]"` — Install with dev dependencies

# Structure
src/csvtool/     → Main package
  cli.py         → Click command definitions
  processors/    → One module per transform type
  validators.py  → Input validation logic
tests/           → Mirrors src/ structure

# Rules
- All public functions must have type hints and docstrings
- Processors must implement BaseProcessor protocol (see processors/base.py)
- Tests: use pytest fixtures, no unittest.TestCase
- CLI output: use rich for formatting, never bare print()
- Error messages must be user-friendly (no raw tracebacks to stdout)
```

## Anti-patterns to Avoid

### Too Vague
```markdown
# My Project
This is a web app. Use best practices. Write clean code.
```
Problem: Claude already knows "best practices" — this adds no project-specific value.

### Too Verbose / Redundant with Documentation
```markdown
# How React Works
React is a JavaScript library for building user interfaces.
Components are the building blocks of React applications.
useState is a hook that lets you add state to functional components...
(500 more lines of React tutorial)
```
Problem: Claude already knows React. Include only project-specific patterns.

### Contradictory Rules
```markdown
- Always use functional components
- Use class components for complex state management
```
Problem: Conflicting instructions cause inconsistent output.

### Including Secrets
```markdown
DATABASE_URL=postgres://user:password@host:5432/db
API_KEY=sk-abc123...
```
Problem: CLAUDE.md is committed to the repo. Use environment variables.
