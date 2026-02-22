# Codebase Analysis Guide

## Table of Contents
1. [Analysis Strategy by Project Type](#analysis-strategy-by-project-type)
2. [Key Files to Identify](#key-files-to-identify)
3. [Component Extraction Patterns](#component-extraction-patterns)
4. [Relationship Detection](#relationship-detection)
5. [Quality Metrics Extraction](#quality-metrics-extraction)
6. [Security Pattern Detection](#security-pattern-detection)
7. [Database Schema Extraction](#database-schema-extraction)
8. [Key Feature Identification](#key-feature-identification)

## Analysis Strategy by Project Type

### Frontend (React/Vue/Angular/Svelte)
- Entry point: `index.html`, `main.tsx`, `App.tsx`, `app.vue`
- Component tree: Trace imports from root component down
- State management: Look for stores (Redux/Vuex/Zustand/Pinia), context providers
- Routing: `router.ts`, route configs, page directories (`pages/`, `app/`)
- API layer: `api/`, `services/`, fetch/axios wrappers

### Backend (Node/Python/Go/Java/Rust)
- Entry point: `main.go`, `app.py`, `server.ts`, `Main.java`, `main.rs`
- Route definitions: Express routes, FastAPI decorators, Spring controllers, Gin handlers
- Middleware: Auth, logging, CORS, rate limiting
- Data layer: Models/schemas, ORM configs, migrations, repositories
- External integrations: API clients, message queues, cache layers

### Full-stack (Next.js/Nuxt/SvelteKit/Rails/Django)
- Combine frontend + backend patterns
- Focus on the boundary: API routes, server actions, RPC layer
- Identify SSR vs CSR vs SSG patterns

### Libraries/Packages
- Public API: Exported functions/classes in `index.ts` or `lib/`
- Internal modules: How they compose together
- Configuration: Options, defaults, plugin systems

### Microservices
- Service boundaries: Each service is a component
- Communication: REST, gRPC, message queues, events
- Shared resources: Databases, caches, config services
- Gateway/proxy layer

### CLI Tools
- Command parser: Argument definitions, subcommands
- Core logic: The main operations
- I/O: File handling, network calls, output formatting

## Key Files to Identify

Priority order for initial analysis:
1. `package.json` / `Cargo.toml` / `go.mod` / `pyproject.toml` / `pom.xml` - Dependencies reveal stack
2. `README.md` - Author's description of the project
3. Entry point file - The starting point of execution
4. Config files - `.env.example`, `config/`, `settings.py`
5. Directory structure - `ls -la` and key subdirectories
6. Test files - Tests reveal intended behavior and API usage

## Component Extraction Patterns

For each identified component, extract:

```json
{
  "id": "unique-kebab-case-id",
  "name": "Human Readable Name",
  "name_zh": "中文名称",
  "type": "frontend|backend|database|external|middleware|config",
  "icon": "emoji representing this component",
  "tooltip": "One-line English description",
  "tooltip_zh": "一句话中文描述",
  "desc": "2-3 sentence English explanation of what it does and why",
  "desc_zh": "2-3句中文说明",
  "tags": ["react", "component", "ui"],
  "key_files": ["src/App.tsx", "src/components/"],
  "connections": [
    { "target": "other-component-id", "direction": "out", "label": "sends requests", "label_zh": "发送请求" }
  ]
}
```

### Type Color Mapping
- `frontend` → `#00d4ff` (cyan)
- `backend` → `#ff6b9d` (pink)
- `database` → `#51cf66` (green)
- `external` → `#ffd43b` (yellow)
- `middleware` → `#c39eff` (purple)
- `config` → `#8888aa` (gray)

## Relationship Detection

### Import/Dependency Tracking
- `import ... from` / `require()` → direct dependency
- API calls (`fetch`, `axios`, HTTP clients) → service-to-service
- Database queries → service-to-database
- Event emitters/listeners → event-driven coupling
- Shared types/interfaces → structural coupling

### Data Flow Direction
- `→` (out): This component sends data to another
- `←` (in): This component receives data from another
- `↔` (bidirectional): Two-way communication

### Common Patterns to Visualize
- Request-Response: User → Frontend → API → Database → API → Frontend → User
- Event Pipeline: Producer → Queue → Consumer → Store
- Pub/Sub: Publisher → Topic → Subscriber1, Subscriber2
- Middleware Chain: Request → Auth → Validate → Handler → Response

## Quality Metrics Extraction

Look for these indicators to populate the Quality & Security tab:

### Test Metrics
- **Test count**: Search for test results in CI config, README badges, or run `grep -r "test\|it(" --include="*.test.*" | wc -l`
- **Coverage**: Look for coverage config (`jest.config`, `.nycrc`, `pytest.ini`), README badges, or CI artifacts
- **Framework**: `jest`, `vitest`, `pytest`, `go test`, `cargo test`, `@testing-library`, `mocha`, etc.
- **Methodology**: TDD indicators (tests next to source), BDD (feature files), snapshot tests

### Build & Lint
- Build tool: `next build`, `vite build`, `webpack`, `tsc`, `cargo build`
- Linters: `eslint`, `prettier`, `ruff`, `clippy`, `golangci-lint`
- Type safety: `strict: true` in tsconfig, `mypy`, Rust's compiler

### Stat Card Guidance
Create 3-4 stat cards from what you find:
```
Test count | Coverage % | Framework | Methodology
```
If a metric is unknown, use the framework name or a qualitative label instead.

## Security Pattern Detection

Scan for these patterns to create security feature cards:

### Authentication & Sessions
- Auth libraries: `@supabase/ssr`, `next-auth`, `passport`, `jsonwebtoken`
- Session management: cookie-based, JWT, token refresh
- Magic links, OAuth providers, RBAC

### Database Security
- **Row-Level Security (RLS)**: PostgreSQL policies, Supabase RLS
- **Parameterized queries**: Prepared statements, ORM query builders
- **Migration safety**: Up/down migrations, destructive operation guards

### Access Control
- Role-based routing: Middleware guards, route protection
- Permission checks: `requireAuth()`, `requireAdmin()`, decorator patterns
- Child/parent visibility rules, tenant isolation

### Input Validation
- Schema validation: Zod, Joi, Pydantic, JSON Schema
- Sanitization: XSS protection, SQL injection prevention
- Rate limiting, CORS configuration

### Sensitive Data
- Environment variables for secrets (not hardcoded)
- Service role keys server-only (not in client bundles)
- Privacy controls (PII handling, anonymous tracking)

### Security Card Guidance
Create 4-6 security cards from what you find. Common patterns:
- Row-Level Security / tenant isolation
- Auth middleware / session management
- Role-based access control
- Privacy-first analytics
- SECURITY DEFINER functions
- Cookie security (httpOnly, SameSite)

## Database Schema Extraction

### Where to Find Schema Information

1. **SQL Migrations** (most reliable):
   - `supabase/migrations/`, `prisma/migrations/`, `db/migrate/`
   - Look for `CREATE TABLE`, `ALTER TABLE`, `CREATE VIEW` statements
   - Check for `COMPLETE_SCHEMA.sql` or consolidated migration files

2. **ORM Models**:
   - Prisma: `schema.prisma`
   - TypeORM: `*.entity.ts`
   - SQLAlchemy: model classes with `__tablename__`
   - Drizzle: `schema.ts`

3. **Type Definitions**:
   - TypeScript types that mirror DB tables (e.g. `types/database.ts`)
   - Supabase generated types

### What to Extract Per Table

```json
{
  "name": "table_name",
  "isView": false,
  "cols": [
    { "label": "id", "type": "pk" },
    { "label": "user_id", "type": "fk" },
    { "label": "name", "type": "" },
    { "label": "created_at", "type": "" }
  ]
}
```

- **Primary keys** (`type: "pk"`): Usually `id`, `uuid`
- **Foreign keys** (`type: "fk"`): Columns ending in `_id` that reference other tables
- **Regular columns** (`type: ""`): Everything else
- **Views** (`isView: true`): Computed/derived tables — get special styling

### Table Selection
- Include **7-12 core tables** (not every join table)
- Always include the "anchor" tables (users, families, organizations)
- Include views that represent computed state (balances, summaries)
- Skip migration tracking tables, session tables, and internal framework tables

### RLS Banner
If the project uses Row-Level Security or tenant isolation, include an RLS banner:
```html
<div class="db-rls-banner">
  🔒 <strong>Row-Level Security:</strong>
  <span data-en="Description..." data-zh="描述...">Description...</span>
</div>
```

### Interactive Demo
For projects with clear insert→approve or create→update workflows, build an interactive demo:
- Button 1: Insert a record (shows pending row with animation)
- Button 2: Approve/complete it (gold flash, status badge change, counter animation)

## Key Feature Identification

### Where to Find Features

1. **README.md**: Feature lists, "What it does" sections
2. **CLAUDE.md / PRODUCT_DOCUMENTATION.md**: Detailed feature descriptions
3. **Route structure**: Each major route often represents a feature
4. **Component directories**: `components/admin/`, `components/shared/` reveal feature areas

### What Makes a Good Feature Card

A feature is worth showcasing if it:
- Has a **unique classification system** (type×scope, tier levels, category matrices)
- Involves a **multi-step workflow** (request→approve→settle)
- Has **interactive elements** worth demonstrating
- Represents a **key differentiator** of the project

### Feature Presentation Styles

**Card grid** (default, good for 3-6 standalone features):
```html
<div class="feature-grid">
  <div class="feature-card">...</div>
</div>
```

**Classification matrix** (for type×scope or dimension systems):
Use CSS grid with colored cells, clickable for details. Put rendering JS in `{{FEATURES_JS}}`.

**Flow diagram** (for multi-step workflows):
Use the same `.econ-flow-node` pattern with directional arrows.

**Custom layout** (for complex systems):
Combine grids, flows, and mini-sections with inline styles. Keep CSS within `{{FEATURES_CONTENT}}` if template classes are insufficient.
