# Codebase Visualizer - Multi-Channel Publishing Guide

All files are ready. Follow these steps on your local machine to publish everywhere.

---

## Step 1: Push to GitHub

```bash
cd /path/to/codebase-visualizer

# Commit the new files (if not already committed)
git add .claude-plugin/ skills/ package.json
git commit -m "Add plugin structure for multi-channel distribution"

# Create GitHub repo and push
gh repo create belugatempo-dot/codebase-visualizer --public --source=. --push

# Or if you prefer manual:
# 1. Go to https://github.com/new
# 2. Create "codebase-visualizer" (public)
# 3. Then:
git remote add origin https://github.com/belugatempo-dot/codebase-visualizer.git
git push -u origin main
```

After this step, users can install via:
```
/plugin marketplace add belugatempo-dot/codebase-visualizer
/plugin install codebase-visualizer@codebase-visualizer-marketplace
```

---

## Step 2: Publish to npm

```bash
cd /path/to/codebase-visualizer

# Login to npm (one-time)
npm login

# Publish
npm publish --access public
```

If the name `codebase-visualizer` is taken, use a scoped name:
```bash
# Edit package.json: change "name" to "@belugatempo-dot/codebase-visualizer"
npm publish --access public
```

After this step, users can install via:
```bash
npm install -g codebase-visualizer
```

---

## Step 3: Submit to Anthropic Official Skills Repo

```bash
# Fork the official repo
gh repo fork anthropics/skills --clone

# Copy your skill files into the fork
cp -r /path/to/codebase-visualizer/SKILL.md skills-repo/codebase-visualizer/
cp -r /path/to/codebase-visualizer/assets skills-repo/codebase-visualizer/
cp -r /path/to/codebase-visualizer/references skills-repo/codebase-visualizer/
cp -r /path/to/codebase-visualizer/examples skills-repo/codebase-visualizer/

# Commit and push
cd skills-repo
git checkout -b add-codebase-visualizer
git add codebase-visualizer/
git commit -m "Add codebase-visualizer skill"
git push -u origin add-codebase-visualizer

# Create PR
gh pr create --title "Add codebase-visualizer skill" --body "## Summary
- Transforms any codebase into an interactive single-file HTML guide
- Up to 7 views: Story Mode, Architecture, How-to-Use, Data Flow, Quality & Security, Key Features, Database Schema
- Bilingual EN/CN support, modern/light theme toggle
- Zero dependencies, fully self-contained HTML output

## Demo
See examples/excalidraw-guide.html for a live example."
```

---

## Step 4: Submit to Community Registries

### 4a. awesome-claude-skills (GitHub awesome list)

```bash
# Fork and clone
gh repo fork travisvn/awesome-claude-skills --clone
cd awesome-claude-skills

# Edit README.md to add your skill under the appropriate section:
# | [codebase-visualizer](https://github.com/belugatempo-dot/codebase-visualizer) | Transform any codebase into an interactive visual HTML guide with architecture diagrams, story mode, and bilingual CN/EN support |

git checkout -b add-codebase-visualizer
git add README.md
git commit -m "Add codebase-visualizer skill"
git push -u origin add-codebase-visualizer
gh pr create --title "Add codebase-visualizer" --body "Interactive codebase visualization skill with 4 views and bilingual support."
```

### 4b. skillsmp.com

1. Go to https://skillsmp.com
2. Click "Submit a Skill"
3. Fill in:
   - Name: `codebase-visualizer`
   - GitHub URL: `https://github.com/belugatempo-dot/codebase-visualizer`
   - Description: Transform any codebase into an interactive visual HTML guide
   - Tags: codebase, visualizer, architecture, diagram, bilingual

### 4c. claudemarketplaces.com

1. Go to https://claudemarketplaces.com
2. Submit your plugin/marketplace URL
3. GitHub URL: `https://github.com/belugatempo-dot/codebase-visualizer`

---

## Verification Checklist

After publishing, verify each channel:

- [ ] **GitHub**: `https://github.com/belugatempo-dot/codebase-visualizer` is accessible
- [ ] **Plugin install**: `/plugin marketplace add belugatempo-dot/codebase-visualizer` works
- [ ] **npm**: `npm info codebase-visualizer` shows your package
- [ ] **Anthropic PR**: PR is open at `anthropics/skills`
- [ ] **awesome-claude-skills PR**: PR is open
- [ ] **skillsmp.com**: Listing is visible
- [ ] **claudemarketplaces.com**: Listing is visible
