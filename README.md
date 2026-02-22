# Codebase Visualizer

A Claude Code skill that transforms any codebase into an interactive, single-file HTML guide with a starry night theme.

## What it does

Give Claude Code any codebase, and it generates a **self-contained HTML file** (zero dependencies) with up to 7 interactive tabs:

| Tab | Description |
|---|---|
| **Story Mode** | 7-act narrative with verb-labeled user journey steps |
| **Architecture** | Interactive SVG diagram — click any component to see inline details |
| **How to Use** | Step-by-step developer guide with code snippets |
| **Data Flow** | Animated SVG showing how data moves through the system |
| **Quality & Security** | Test metrics, security features, code quality badges |
| **Key Features** | Project-specific feature showcase (grids, matrices, flows) |
| **Database** | Schema explorer with interactive INSERT/APPROVE demo (optional) |

### Features

- **Bilingual** — Toggle between English and Chinese (EN / 中文)
- **Dark / Light theme** — Default: slate-to-indigo starry night with gold accents
- **Fully self-contained** — Single HTML file, no build step, no dependencies
- **Interactive** — Click nodes, search components, hover for tooltips
- **Inline detail panel** — Slides down below architecture diagram (not a side panel)
- **Optional tabs** — Database tab hidden when project has no DB
- **Footer with CTA** — Auto-detects locale for project link
- **3 responsive breakpoints** — Works on desktop, tablet, and mobile

## Screenshot

![Excalidraw Example](examples/excalidraw-guide.html)

## Installation

### Option 1: Clone to skills directory

```bash
git clone <this-repo-url> ~/.claude/skills/codebase-visualizer
```

### Option 2: Symlink (for development)

```bash
git clone <this-repo-url> ~/my-skills/codebase-visualizer
ln -s ~/my-skills/codebase-visualizer ~/.claude/skills/codebase-visualizer
```

After installation, restart Claude Code. The skill will be available in all your projects.

## Usage

Just ask Claude Code:

```
Help me visualize the codebase at ./my-project
```

or

```
Generate an interactive guide for this repository
```

Claude will analyze the codebase and produce a single HTML file you can open in any browser.

## File Structure

```
codebase-visualizer/
├── SKILL.md              # Skill instructions (Claude reads this)
├── assets/
│   └── template.html     # HTML template with placeholders
├── references/
│   ├── analysis-guide.md     # How to analyze a codebase
│   └── storytelling-guide.md # How to write engaging stories
├── examples/
│   └── excalidraw-guide.html # Example output for Excalidraw
├── README.md
└── .gitignore
```

## How it works

1. **SKILL.md** tells Claude how to analyze a codebase and what to generate
2. **template.html** provides the HTML/CSS/JS skeleton with `{{PLACEHOLDER}}` tokens
3. **references/** provide guidance on analysis methodology and storytelling
4. Claude fills in the placeholders with codebase-specific content and writes the final HTML file

## Customization

The template uses CSS custom properties, so you can easily adjust:

- `--bg`, `--card`, `--accent` etc. in the `:root` block
- Star count and nebula colors in the `createStars()` function
- Default theme via `data-theme="dark"` on the `<html>` tag

## License

MIT
