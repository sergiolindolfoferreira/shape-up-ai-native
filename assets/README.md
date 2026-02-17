# 📊 Workflow Diagrams

Visual representations of the **Shape Up AI Native** workflow.

## 📁 Files

### `workflow-diagram.md` ⭐ (Easiest to use)
**Markdown with embedded Mermaid** - Best for quick preview and direct inclusion in documentation.

**Usage:**
- View directly on GitHub (auto-renders the Mermaid diagram)
- Copy the entire content to paste into other docs
- Link from README: `![Workflow](assets/workflow-diagram.md)`

---

### `workflow-diagram.mmd` (For developers)
**Raw Mermaid flowchart code** - For editing and customization.

**Usage in Markdown:**
````markdown
```mermaid
(paste contents from workflow-diagram.mmd)
```
````

**Edit & Preview:**
- [Mermaid Live Editor](https://mermaid.live/)
- VS Code: Install "Mermaid Preview" extension
- GitHub: Automatically renders in `.md` files

---

### `workflow-diagram.txt`
**ASCII art diagram** - Universal fallback, works everywhere (CLI, plain text, terminals).

**Usage:**
- View directly: `cat assets/workflow-diagram.txt`
- Embed in docs using code blocks:
  ````markdown
  ```
  (paste ASCII content here)
  ```
  ````
- Perfect for README files on platforms without Mermaid support

---

## 🎨 Diagram Overview

Both diagrams illustrate the complete **Shape Up AI Native** workflow:

### 🔨 **SHAPING** (Basecamp)
- **Input:** Problem, ideas, user needs
- **Process:** Define appetite, sketch solution, identify rabbit holes
- **Output:** `pitch.md`

### 🎲 **BETTING** (Basecamp)
- **Input:** All pitches from shaping
- **Process:** Review, commit resources, assign teams
- **Output:** Approved pitch → kick-off

### 🚀 **BUILDING** (GitHub + Claude Code)

#### 📝 **SPEC MODE**
- **Input:** Approved pitch
- **Process:** Create detailed technical specification
- **Output:** `spec.md`

#### 📋 **PLAN MODE**
- **Input:** `spec.md`
- **Process:** Break into scopes, tasks, dependencies
- **Output:** `plan.md` + `scopes/`

#### 💻 **IMPLEMENT MODE**
- **Input:** `plan.md`
- **Process:** TDD, code reviews, pull requests
- **Output:** Working features + tests

### 📦 **SHIPPING** (Azure DevOps)
- **Input:** Completed features
- **Process:** Final review, merge, CI/CD pipeline
- **Output:** 🎉 Production release

---

## 🎯 Design Philosophy

The diagrams emphasize:
- **Visual clarity** - Understand the flow at a glance
- **Phase differentiation** - Colors separate Shape Up (methodology) from Claude Code (execution)
- **Artifacts** - Every phase produces tangible outputs
- **Feedback loop** - Learnings flow back to shaping

---

## 🔄 Updating the Diagrams

### Mermaid (`.mmd`)
1. Edit the file directly
2. Preview using:
   - [Mermaid Live Editor](https://mermaid.live/)
   - VS Code extensions (Mermaid Preview, Markdown Preview Enhanced)
   - GitHub's built-in renderer

### ASCII (`.txt`)
1. Edit using a monospace font editor
2. Use Unicode box-drawing characters: `┌ ┐ └ ┘ │ ─ ├ ┤ ┬ ┴ ┼ ╭ ╮ ╰ ╯`
3. Preserve exact spacing for alignment

---

## 📝 Contributing

When improving the diagrams:
- Keep them **horizontally narrow** (~80 chars for ASCII)
- Maintain **consistent styling** (colors, borders, spacing)
- Test rendering on **GitHub preview** before committing
- Update this README if adding new diagram formats

---

**Created:** 2026-02-17  
**Version:** 1.0  
**Maintainer:** Shape Up AI Native Project
