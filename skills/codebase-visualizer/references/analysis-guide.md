# Codebase Analysis Guide

## Table of Contents
1. [Analysis Strategy by Project Type](#analysis-strategy-by-project-type)
2. [Key Files to Identify](#key-files-to-identify)
3. [Component Extraction Patterns](#component-extraction-patterns)
4. [Relationship Detection](#relationship-detection)

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
