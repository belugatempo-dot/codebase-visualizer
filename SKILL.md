---
name: codebase-visualizer
description: "Transform any codebase into an interactive visual HTML guide that a 10-year-old can understand. Generates a single self-contained HTML file with animated architecture diagrams, clickable component explorer, story-mode narrative, data flow animations, quality metrics, key feature showcase, optional database schema explorer, and bilingual CN/EN support. Use when the user asks to: (1) visualize a codebase or project architecture, (2) create an interactive guide or explainer for a project, (3) make a codebase understandable for non-technical people or kids, (4) generate a visual overview of how a project works, (5) explain a project's design model or technical architecture visually. Triggers on phrases like: 'visualize this codebase', 'explain this project visually', 'make an interactive guide', 'show me how this project works', '把这个项目可视化', '生成项目架构图', '做一个交互式指南'."
---

# Codebase Visualizer

Generate a single self-contained HTML file that turns any codebase into a kid-friendly, interactive visual guide with up to seven views: Story Mode, Architecture Diagram, How-to-Use Guide, Data Flow Animation, Quality & Security, Key Features, and Database Schema. Supports bilingual EN/CN toggle.

## Workflow

### 1. Analyze the Codebase

Determine the project type and identify components. Read [references/analysis-guide.md](references/analysis-guide.md) for patterns by project type (frontend, backend, full-stack, library, microservices, CLI).

**Files to read first** (in priority order):
1. Package manifest (`package.json`, `Cargo.toml`, `go.mod`, `pyproject.toml`, `pom.xml`)
2. `README.md`
3. Entry point file(s)
4. Directory structure (`ls` key directories)
5. Config files (`.env.example`, `config/`)
6. 2-3 representative source files per major directory

**Extract for each component:**
```json
{
  "id": "kebab-case-id",
  "name": "Name", "name_zh": "中文名",
  "type": "frontend|backend|database|external|middleware|config",
  "icon": "emoji",
  "tooltip": "One-liner EN", "tooltip_zh": "一句话中文",
  "desc": "2-3 sentences EN", "desc_zh": "2-3句中文",
  "tags": ["tech", "tags"],
  "key_files": ["src/path.ts"],
  "connections": [
    { "target": "other-id", "direction": "out|in", "label": "EN label", "label_zh": "中文标签" }
  ]
}
```

Limit to **3-7 major components**. Group minor modules under a parent component.

**Quality metrics** (for Tab 5):
- Test count, coverage %, test framework
- Methodology indicators (TDD, CI/CD)
- Lint/format tools in use

**Security patterns** (for Tab 5):
- Auth middleware, session management
- RLS policies, role-based access
- Input validation, encryption
- Environment variable handling

**Database schema** (for Tab 7, if applicable):
- Find migration files, ORM models, or schema definitions
- Extract table names, columns, primary keys, foreign keys
- Identify views vs tables
- Note RLS or other security policies

**Key features** (for Tab 6):
- Notable feature systems (classification matrices, economies, workflows)
- Interactive or unique capabilities worth showcasing
- Credit systems, notification systems, demo modes

### 2. Create the Story Narrative

Read [references/storytelling-guide.md](references/storytelling-guide.md) for metaphor library and story structure.

**Follow the 7-act structure:**
1. **ACT 1 — WHAT**: One-sentence pitch with mascot emoji (choose an animal/character that fits the project)
2. **ACT 2 — WHO**: Introduce each component as a character with personality
3. **ACT 3 — [VERB]**: First key user journey step (e.g. CREATE, SETUP, CONNECT). Label with one verb.
4. **ACT 4 — [VERB]**: Second key user journey step (e.g. DISCOVER, SEARCH, BROWSE). Label with one verb.
5. **ACT 5 — [VERB]**: Third key user journey step (e.g. EARN, DEPLOY, PUBLISH). Label with one verb.
6. **ACT 6 — COLLABORATE**: Show components talking to each other like characters
7. **ACT 7 — SUPERPOWERS**: Highlight 2-3 cool technical features

Pick the 3 most important actions in the primary user journey for Acts 3-5. Each verb should capture a distinct phase.

**Language rules:**
- Sentences under 15 words
- Active voice, present tense
- Real-world metaphors (restaurant, school, playground, toy factory)
- All text bilingual: `data-en="..."` and `data-zh="..."`
- Chinese should be natural/colloquial, not literal translations

### 3. Build the HTML

Read the template at [assets/template.html](assets/template.html). Use it as the structural foundation, then populate with real content.

**Replace these placeholders in the template:**

#### Core placeholders (all projects)

| Placeholder | Content |
|---|---|
| `{{PROJECT_NAME}}` | Project name (shown in nav logo and footer) |
| `{{DEFAULT_THEME}}` | `dark` or `light` — default to `dark` (starry night theme) |
| `{{MASCOT_EMOJI}}` | A fun emoji mascot |
| `{{STORY_TITLE_EN/ZH}}` | Story mode title |
| `{{STORY_INTRO_EN/ZH}}` | 1-2 sentence project pitch |
| `{{STORY_STEPS}}` | Story timeline HTML (see format below) |
| `{{ARCHITECTURE_SVG}}` | Inline SVG with component nodes and flow arrows |
| `{{ARCHITECTURE_LEGEND}}` | Legend items for component types |
| `{{HOWTO_CARDS}}` | How-to-use card grid HTML |
| `{{DATAFLOW_SVG}}` | Inline SVG showing data flow with animated paths |
| `{{COMPONENTS_JSON}}` | JavaScript object with all component metadata |

#### Quality tab placeholders (Tab 5 — always present)

| Placeholder | Content |
|---|---|
| `{{QUALITY_STATS}}` | Stat cards: test count, coverage, framework, methodology |
| `{{SECURITY_CARDS}}` | Security feature cards (auth, RLS, roles, etc.) |
| `{{QUALITY_BADGES}}` | Code quality badge spans (TypeScript strict, coverage, etc.) |

#### Features tab placeholders (Tab 6 — always present)

| Placeholder | Content |
|---|---|
| `{{FEATURES_TAB_EN/ZH}}` | Tab button label (e.g. "Key Features" / "特色功能") |
| `{{FEATURES_TAB_DISPLAY}}` | Empty string (tab visible) or `style="display:none"` |
| `{{FEATURES_SECTION_DISPLAY}}` | Empty string or `style="display:none"` |
| `{{FEATURES_TITLE_EN/ZH}}` | Tab heading |
| `{{FEATURES_INTRO_EN/ZH}}` | Tab intro paragraph |
| `{{FEATURES_CONTENT}}` | Feature showcase HTML (card grid, matrix, or custom layout) |
| `{{FEATURES_JS}}` | JavaScript for interactive features (or empty string) |

#### Database tab placeholders (Tab 7 — optional, hidden if no DB)

| Placeholder | Content |
|---|---|
| `{{DB_TAB_DISPLAY}}` | Empty string (tab visible) or `style="display:none"` |
| `{{DB_SECTION_DISPLAY}}` | Empty string or `style="display:none"` |
| `{{DB_TABLES_JSON}}` | `var DB_TABLES = [...]` JavaScript variable (see format below) |
| `{{DB_RLS_BANNER}}` | RLS explanation banner HTML (or empty string) |
| `{{DB_DEMO_HTML}}` | Interactive demo HTML (buttons + live table) or empty string |
| `{{DB_DEMO_JS}}` | Demo insert/approve JavaScript functions (or empty string) |
| `{{DB_LIVE_GRID_COLS}}` | CSS grid columns for live table (e.g. `80px 160px 80px 80px 100px`) |

#### Footer placeholders

| Placeholder | Content |
|---|---|
| `{{FOOTER_CTA_URL}}` | Project URL (production site, GitHub, etc.) |
| `{{FOOTER_TAGLINE_EN/ZH}}` | Short tagline describing the project |

### 3.5 Optional Tabs

- **Tab 5 (Quality)** and **Tab 6 (Features)**: Always present. Every project has quality characteristics and notable features.
- **Tab 7 (Database)**: Optional. Hidden if the project has no database. Set `{{DB_TAB_DISPLAY}}` and `{{DB_SECTION_DISPLAY}}` to `style="display:none"` and leave `{{DB_TABLES_JSON}}`, `{{DB_RLS_BANNER}}`, `{{DB_DEMO_HTML}}`, `{{DB_DEMO_JS}}` as empty strings.

For projects without a database, also set `{{DB_LIVE_GRID_COLS}}` to `1fr` (fallback).

#### Story Step HTML Format
```html
<div class="story-step" data-icon="emoji">
  <span class="step-num">ACT 1 — WHAT</span>
  <h3><span data-en="EN Title" data-zh="中文标题">EN Title</span></h3>
  <p><span data-en="EN summary" data-zh="中文概述">EN summary</span></p>
  <div class="detail">
    <span data-en="EN detailed explanation" data-zh="中文详细说明">EN detailed explanation</span>
  </div>
</div>
```

For Acts 3-5, use verb labels: `ACT 3 — CREATE`, `ACT 4 — DISCOVER`, `ACT 5 — EARN`, etc.

#### Architecture SVG Format

Build an inline `<svg>` with these elements. The detail panel is **inline** (slides down between the SVG and the legend), not a side panel.

**Component nodes** (clickable):
```svg
<g class="node-group" data-id="component-id" data-name="Component Name">
  <rect class="node-rect" x="X" y="Y" width="180" height="79"
        fill="var(--node-fill)" stroke="COLOR_BY_TYPE" />
  <text class="node-label" x="X+90" y="Y+28" data-en="Icon Name" data-zh="图标 中文名">Icon Name</text>
  <text class="node-sublabel" x="X+90" y="Y+50" data-en="EN role" data-zh="中文角色">EN role</text>
</g>
```

**Flow arrows** (animated dashed lines):
```svg
<path id="flow-1" class="flow-path" stroke="#00d4ff"
      d="M startX,startY C cp1x,cp1y cp2x,cp2y endX,endY" marker-end="url(#ah)" />
<text class="flow-label"><textPath href="#flow-1" startOffset="50%"
      data-en="EN label" data-zh="中文标签">EN label</textPath></text>
```

**Color by component type:**
- frontend: `#00d4ff`
- backend: `#ff6b9d`
- database: `#51cf66`
- external: `#ffd43b`
- middleware: `#c39eff`
- config: `#8888aa`

**SVG layout tips:**
- Use `viewBox="0 0 960 520"` for landscape layout
- Place frontend components on the left/top, backend in the middle, database/external on the right/bottom
- Space nodes at least 180px apart horizontally, 100px vertically
- Use cubic bezier curves (`C`) for smooth flow arrows between nodes
- Add `marker-end="url(#ah)"` arrowheads to flow paths
- Include a `<defs>` block with an arrowhead marker:
  ```svg
  <defs>
    <marker id="ah" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto">
      <polygon points="0 0, 8 3, 0 6" fill="var(--text-dim)" opacity="0.5"/>
    </marker>
  </defs>
  ```

#### Data Flow SVG Format
The `{{DATAFLOW_SVG}}` is an inline SVG showing animated data flow pipelines. **All text labels must include `data-en` and `data-zh` attributes** for bilingual switching.

**Section headers:**
```svg
<text x="480" y="25" fill="#00d4ff" font-size="16" font-weight="bold"
      text-anchor="middle" data-en="Main Flow" data-zh="主流程">Main Flow</text>
```

**Node boxes with bilingual sublabels:**
```svg
<rect x="130" y="52" width="120" height="56" rx="10"
      fill="var(--node-fill)" stroke="#00d4ff" stroke-width="2"/>
<text x="190" y="75" fill="var(--node-label)" font-size="13"
      font-weight="600" text-anchor="middle" data-en="Component" data-zh="组件">Component</text>
<text x="190" y="93" fill="var(--node-sublabel)" font-size="11"
      text-anchor="middle" data-en="EN description" data-zh="中文描述">EN description</text>
```

**Key rules:**
- Every `<text>` with human-readable content MUST have `data-en` and `data-zh`
- Technical names (function names, class names) stay as-is — don't translate code identifiers
- Use `var(--node-fill)`, `var(--node-label)`, `var(--node-sublabel)` for theme support
- Group related nodes into horizontal rows (one row per pipeline/subsystem)

#### Quality Stat Card Format
```html
<div class="stat-card">
  <div class="stat-value">2,849</div>
  <div class="stat-label"><span data-en="Tests Passing" data-zh="测试通过">Tests Passing</span></div>
</div>
```

Typical stats: test count, coverage %, framework name, methodology (TDD, CI/CD).

#### Security Card Format
```html
<div class="security-card">
  <div class="sec-icon">🛡️</div>
  <h4><span data-en="Feature Name" data-zh="功能名称">Feature Name</span></h4>
  <p><span data-en="Description" data-zh="描述">Description</span></p>
</div>
```

#### Quality Badge Format
```html
<span class="quality-badge"><span data-en="TypeScript strict" data-zh="TypeScript 严格模式">TypeScript strict</span></span>
```

#### Features Content

For **simple projects**, use a card grid:
```html
<div class="feature-grid">
  <div class="feature-card">
    <div class="feature-icon">🔥</div>
    <h4><span data-en="Feature Name" data-zh="功能名称">Feature Name</span></h4>
    <p><span data-en="Description" data-zh="描述">Description</span></p>
  </div>
</div>
```

For **projects with classification systems** (e.g. type×scope matrices, pricing tiers), use custom layouts with CSS grid. Provide the necessary CSS inline within `{{FEATURES_CONTENT}}` if the template's generic `.feature-grid` is insufficient. Put associated JavaScript in `{{FEATURES_JS}}`.

#### DB_TABLES_JSON Format
```javascript
var DB_TABLES = [
  { name: 'users', cols: [
    { label: 'id', type: 'pk' },
    { label: 'email', type: '' },
    { label: 'family_id', type: 'fk' }
  ]},
  { name: 'child_balances', isView: true, cols: [
    { label: 'child_id', type: 'fk' },
    { label: 'total_stars', type: '' }
  ]}
];
```

Column types: `'pk'` (primary key, shows 🔑), `'fk'` (foreign key, shows 🔗), `''` (regular, shows ·). Tables with `isView: true` get a special gold border and "VIEW" label.

#### DB Demo Format

The interactive demo lets users INSERT a record and then APPROVE it, with animated transitions. Provide two functions in `{{DB_DEMO_JS}}`:
- `dbDemoInsert()` — adds a pending row to `#db-live-rows`
- `dbDemoApprove()` — animates approval, updates a balance counter via `animateCounter()`

#### How-To Card HTML Format
```html
<div class="howto-card">
  <div class="card-icon">emoji</div>
  <h3><span data-en="EN title" data-zh="中文标题">EN title</span></h3>
  <p><span data-en="EN description" data-zh="中文描述">EN description</span></p>
  <ol class="steps">
    <li><span data-en="Step 1 EN" data-zh="步骤1中文">Step 1 EN</span></li>
    <li><span data-en="Step 2 EN" data-zh="步骤2中文">Step 2 EN</span></li>
  </ol>
</div>
```

Typical how-to cards: "Getting Started", "Daily Usage", "Common Tasks", "Troubleshooting", "Power Features".

#### COMPONENTS_JSON Format
```javascript
const COMPONENTS = {
  "component-id": {
    name: "Component Name", name_zh: "中文名",
    tooltip: "EN tooltip", tooltip_zh: "中文提示",
    desc: "EN description", desc_zh: "中文描述",
    tags: ["tag1", "tag2"],
    connections: [
      { target: "other-id", direction: "out", label: "EN", label_zh: "中文" }
    ]
  }
};
```

### 4. Output

Write the final HTML file to the user's project directory as `{project-name}-visualization.html`. Confirm the file path and suggest opening in a browser.

## Quality Checklist

- [ ] All seven tabs work: Story, Architecture, How-to, Data Flow, Quality, Features, Database
- [ ] Theme toggle (🌙/☀️) switches between dark and light modes smoothly
- [ ] Language toggle switches all visible text between EN and CN (including SVG text in Architecture and Data Flow tabs)
- [ ] All architecture nodes are clickable and show inline detail panel (slides down, not side panel)
- [ ] Clicking the same node again closes the inline panel (toggle behavior)
- [ ] Flow arrows animate with dashed-line animation
- [ ] Story steps expand on click to show details (7 acts with verb labels)
- [ ] Component search filters the architecture view
- [ ] Quality tab shows stats, security cards, and badges
- [ ] Features tab displays project-specific showcase content
- [ ] Database tab renders schema tables (if applicable) with interactive demo
- [ ] Footer shows project name, tagline, and CTA link with auto-locale
- [ ] No external dependencies (everything inline in one HTML file)
- [ ] Metaphors are age-appropriate (10-year-old level)
- [ ] 3-7 components (not too many, not too few)
- [ ] Responsive: works at 1280px, 768px, and 480px breakpoints
