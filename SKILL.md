---
name: codebase-visualizer
description: "Transform any codebase into an interactive visual HTML guide that a 10-year-old can understand. Generates a single self-contained HTML file with animated architecture diagrams, clickable component explorer, story-mode narrative, data flow animations, and bilingual CN/EN support. Use when the user asks to: (1) visualize a codebase or project architecture, (2) create an interactive guide or explainer for a project, (3) make a codebase understandable for non-technical people or kids, (4) generate a visual overview of how a project works, (5) explain a project's design model or technical architecture visually. Triggers on phrases like: 'visualize this codebase', 'explain this project visually', 'make an interactive guide', 'show me how this project works', '把这个项目可视化', '生成项目架构图', '做一个交互式指南'."
---

# Codebase Visualizer

Generate a single self-contained HTML file that turns any codebase into a kid-friendly, interactive visual guide with four views: Story Mode, Architecture Diagram, How-to-Use Guide, and Data Flow Animation. Supports bilingual EN/CN toggle.

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

### 2. Create the Story Narrative

Read [references/storytelling-guide.md](references/storytelling-guide.md) for metaphor library and story structure.

**Follow the 5-act structure:**
1. **WHAT** — One-sentence pitch with mascot emoji (choose an animal/character that fits the project)
2. **WHO** — Introduce each component as a character with personality
3. **HOW** — Walk through the primary user journey step by step
4. **COLLABORATION** — Show components talking to each other like characters
5. **SUPERPOWERS** — Highlight 2-3 cool technical features

**Language rules:**
- Sentences under 15 words
- Active voice, present tense
- Real-world metaphors (restaurant, school, playground, toy factory)
- All text bilingual: `data-en="..."` and `data-zh="..."`
- Chinese should be natural/colloquial, not literal translations

### 3. Build the HTML

Read the template at [assets/template.html](assets/template.html). Use it as the structural foundation, then populate with real content.

**Replace these placeholders in the template:**

| Placeholder | Content |
|---|---|
| `{{PROJECT_NAME}}` | Project name |
| `{{DEFAULT_THEME}}` | `dark` or `light` — choose based on user preference. Default to `dark` (starry night purple theme). |
| `{{MASCOT_EMOJI}}` | A fun emoji mascot |
| `{{STORY_TITLE_EN/ZH}}` | Story mode title |
| `{{STORY_INTRO_EN/ZH}}` | 1-2 sentence project pitch |
| `{{STORY_STEPS}}` | Story timeline HTML (see format below) |
| `{{ARCHITECTURE_SVG}}` | Inline SVG with component nodes and flow arrows |
| `{{ARCHITECTURE_LEGEND}}` | Legend items for component types |
| `{{HOWTO_CARDS}}` | How-to-use card grid HTML |
| `{{DATAFLOW_SVG}}` | Inline SVG showing data flow with animated paths |
| `{{COMPONENTS_JSON}}` | JavaScript object with all component metadata |

#### Story Step HTML Format
```html
<div class="story-step" data-icon="emoji">
  <span class="step-num">STEP 1</span>
  <h3><span data-en="EN Title" data-zh="中文标题">EN Title</span></h3>
  <p><span data-en="EN summary" data-zh="中文概述">EN summary</span></p>
  <div class="detail">
    <span data-en="EN detailed explanation" data-zh="中文详细说明">EN detailed explanation</span>
  </div>
</div>
```

#### Architecture SVG Format

Build an inline `<svg>` with these elements:

**Component nodes** (clickable):
```svg
<g class="node-group" data-id="component-id" data-name="Component Name">
  <rect class="node-rect" x="X" y="Y" width="140" height="70"
        fill="var(--node-fill)" stroke="COLOR_BY_TYPE" />
  <text class="node-label" x="X+70" y="Y+30">Icon Name</text>
  <text class="node-sublabel" x="X+70" y="Y+50">role description</text>
</g>
```

**Flow arrows** (animated dashed lines):
```svg
<path id="flow-1" class="flow-path" stroke="#00d4ff"
      d="M startX,startY C cp1x,cp1y cp2x,cp2y endX,endY" />
<text class="flow-label"><textPath href="#flow-1" startOffset="50%">label</textPath></text>
```

**Color by component type:**
- frontend: `#00d4ff`
- backend: `#ff6b9d`
- database: `#51cf66`
- external: `#ffd43b`
- middleware: `#c39eff`
- config: `#8888aa`

**SVG layout tips:**
- Use `viewBox="0 0 900 500"` for landscape layout
- Place frontend components on the left, backend in the middle, database/external on the right
- Space nodes at least 180px apart horizontally, 100px vertically
- Use cubic bezier curves (`C`) for smooth flow arrows between nodes
- Add `marker-end` arrowheads to flow paths

#### Data Flow SVG Format
The `{{DATAFLOW_SVG}}` is an inline SVG showing animated data flow pipelines. **All text labels must include `data-en` and `data-zh` attributes** for bilingual switching (the `toggleLang()` function uses `querySelectorAll('[data-en]')` which matches both HTML and SVG elements).

**Section headers:**
```svg
<text x="480" y="25" fill="#00d4ff" font-size="14" font-weight="bold"
      text-anchor="middle" data-en="Main Flow" data-zh="主流程">Main Flow</text>
```

**Node boxes with bilingual sublabels:**
```svg
<rect x="130" y="52" width="120" height="56" rx="10"
      fill="var(--node-fill)" stroke="#00d4ff" stroke-width="2"/>
<text x="190" y="75" fill="var(--node-label)" font-size="10"
      font-weight="600" text-anchor="middle">ComponentName</text>
<text x="190" y="90" fill="var(--node-sublabel)" font-size="8"
      text-anchor="middle" data-en="EN description" data-zh="中文描述">EN description</text>
```

**Animated flow paths** (same as architecture):
```svg
<path id="d1" class="flow-path" stroke="#00d4ff"
      d="M 250,80 L 290,80" marker-end="url(#da)"/>
```

**Key rules:**
- Every `<text>` with human-readable content MUST have `data-en` and `data-zh`
- Technical names (function names, class names) stay as-is — don't translate code identifiers
- Use `var(--node-fill)`, `var(--node-label)`, `var(--node-sublabel)` for theme support
- Group related nodes into horizontal rows (one row per pipeline/subsystem)
- Use `viewBox="0 0 960 580"` or adjust for number of rows

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

Typical how-to cards: "Getting Started", "Daily Usage", "Common Tasks", "Troubleshooting".

#### COMPONENTS_JSON Format
```javascript
const COMPONENTS = {
  "component-id": {
    name: "Component Name",
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

Write the final HTML file to the user's project directory as `{project-name}-guide.html`. Confirm the file path and suggest opening in a browser.

## Quality Checklist

- [ ] All four tabs work: Story, Architecture, How-to, Data Flow
- [ ] Theme toggle (🌙/☀️) switches between dark and light modes smoothly
- [ ] Language toggle switches all visible text between EN and CN (including SVG text in Architecture and Data Flow tabs)
- [ ] All architecture nodes are clickable and show detail panel
- [ ] Flow arrows animate with dashed-line animation
- [ ] Story steps expand on click to show details
- [ ] Component search filters the architecture view
- [ ] No external dependencies (everything inline in one HTML file)
- [ ] Metaphors are age-appropriate (10-year-old level)
- [ ] 3-7 components (not too many, not too few)
