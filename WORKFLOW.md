# Workflow Quick Reference

> Day-to-day guide for building with Shape Up + AI agents

**Variant:** Strict Gates (quality-critical teams)  
**For detailed guide:** See [docs/development-workflow.md](docs/development-workflow.md)

---

## The Complete Flow

```
SHAPING (Define pitch)
   ↓
BETTING (Approve feature)
   ↓
SPEC Phase → 🔴 GATE 1 (review + approval) → spec.md committed
   ↓
PLAN Phase → 🔴 GATE 2 (review + approval) → plan.md committed
   ↓
IMPLEMENT Phase:
   ├─ Component 1 → /code-review → commit → push
   ├─ Component 2 → /code-review → commit → push
   └─ Component N → /code-review → commit → push
   ↓
Single PR created (all scopes)
   ↓
🔴 GATE 3 (final review)
   ↓
Merge to main → ✅ COMPLETE
```

---

## Phase 1: SPEC (Specification)

### Command
```bash
/spec "Feature description" [figma: URL]
```

### What Happens
1. Agent checks git status (must be clean)
2. Agent creates branch: `claude/feature/<english-slug>`
3. Agent generates spec from template
4. Agent saves to `_specs/<slug>.md`
5. Agent commits + pushes
6. **🔴 GATE: Agent STOPS and WAITS**

### Expected Agent Message
```
✅ Spec created: _specs/performance-dashboard.md
   Branch: claude/feature/performance-dashboard

Please review the spec. I will WAIT for explicit approval before 
proceeding to planning phase.

Reply "approved, proceed to plan" when ready.
```

### Your Response
- ✅ **"Approved, proceed to plan"** → Agent continues
- ⚠️ **"Change X, then proceed"** → Agent edits → waits again

### Critical Rule
**Agent does NOT proceed without explicit "proceed to plan"**

---

## Phase 2: PLAN (Implementation Plan)

### What Happens
1. Agent analyzes spec
2. Agent creates scopes (independently shippable pieces)
3. Agent estimates effort per scope
4. Agent identifies dependencies
5. Agent saves to `_plans/<slug>.md`
6. Agent commits + pushes
7. **🔴 GATE: Agent STOPS and WAITS**

### Expected Agent Message
```
✅ Plan created: _plans/performance-dashboard.md

Scopes:
1. OverviewCard component (4h)
2. ListingsTable component (6h)
3. API + Integration (4h)

Total: 14 hours (~1.5 days)

Please review. I will WAIT for explicit approval before starting 
implementation.

Reply "approved, start implementation" when ready.
```

### Your Response
- ✅ **"Approved, start implementation"** → Agent begins coding
- ⚠️ **"Split scope 3 into two"** → Agent adjusts → waits again

### Critical Rule
**Agent does NOT start coding without explicit "start implementation"**

---

## Phase 3: IMPLEMENT (Build with TDD)

### For Each Component/Module

**1. Agent implements using TDD:**
   - Write test first (RED 🔴)
   - Implement code (GREEN 🟢)
   - Refactor if needed
   - Component works in preview

**2. Agent runs code review:**
   ```bash
   /code-review
   ```
   - Spawns `a11y-reviewer` + `code-quality-reviewer`
   - Gets action plan
   - Fixes all issues found

**3. Agent commits:**
   ```bash
   /commit-message
   ```
   - Generates semantic commit message
   - Commits with emoji prefix
   - Example: `✨ feat: Add OverviewCard component`

**4. Agent pushes:**
   ```bash
   git push origin claude/feature/<slug>
   ```

**5. Agent continues to next scope**

### Key Difference
**Standard:** Code review before PR  
**Strict Gates:** Code review before **each commit**

This ensures every commit is production-quality.

---

## Phase 4: PULL REQUEST (Single PR)

### When All Scopes Complete

**Agent creates PR with:**
- All scopes together
- All commits (one per component/module)
- Complete feature

**PR Structure:**

**Title:**
```
[Feature Name]
```

**Description:**
```markdown
## Feature: [Name]

**Spec:** _specs/<slug>.md  
**Plan:** _plans/<slug>.md  
**Appetite:** X days  
**Actual:** Y days

### Scopes Implemented
✅ Scope 1: [Name] (Xh)
✅ Scope 2: [Name] (Yh)
✅ Scope 3: [Name] (Zh)

### Testing
✅ Unit tests: X passed
✅ Manual testing: Verified
✅ Code review: Clean

### Files Changed
[List with descriptions]
```

---

## Phase 5: REVIEW & MERGE

### 🔴 GATE 3: Human Review

**Check:**
- [ ] Does feature match pitch intent?
- [ ] Do all scopes work together?
- [ ] Tests pass?
- [ ] Design matches spec?
- [ ] Responsive?
- [ ] Accessible?

### Response Options

**✅ Approve & Merge:**
```
LGTM! Merging now.
```

**⚠️ Request Changes:**
```
Good work! One change needed:
[Specific change with file/line]
```

Agent implements → you re-review → merge

### After Merge
✅ **Workflow complete!**  
Feature is in `main` branch.

---

## Commands Reference

### During Development

```bash
# Create spec from pitch
/spec "Feature description" [figma: URL]

# Create component (TDD workflow)
/component ComponentName "description"

# Review current changes
/code-review

# Generate commit message
/commit-message
```

### Git Commands

```bash
# Check status
git status

# Stage changes
git add .

# Commit (after /commit-message)
git commit -m "✨ feat: Add feature"

# Push to remote
git push origin claude/feature/<slug>
```

---

## Branch Naming

**Always use English slugs:**

✅ **Correct:**
```
claude/feature/performance-dashboard
claude/feature/budget-templates
claude/feature/user-profile
```

❌ **Wrong:**
```
claude/feature/painel-desempenho  (Portuguese)
claude/feature/template-orcamentos (Mixed)
```

**Why:** Git branches are international. Commits can be any language.

---

## Commit Message Format

Use Conventional Commits with emoji:

```
<emoji> <type>: <description>

<optional body>
```

**Types:**
- ✨ `feat:` - New feature
- 🐛 `fix:` - Bug fix
- 🔨 `refactor:` - Refactoring
- 📝 `docs:` - Documentation
- 🎨 `style:` - Styling/formatting
- ✅ `test:` - Tests
- ⚡ `perf:` - Performance
- 🔧 `chore:` - Build/tooling

**Examples:**
```bash
✨ feat: Add OverviewCard component for performance dashboard

Displays aggregate stats (views, inquiries, bookings) with icons.
Responsive design, dark mode support, loading state.
```

```bash
🐛 fix: Prevent duplicate listings in table

Race condition caused duplicates. Added unique key validation.
```

---

## File Structure

```
project/
├── .claude/
│   └── commands/
│       ├── spec.md              # /spec command
│       ├── component.md         # /component command (TDD)
│       ├── code-review.md       # /code-review command
│       └── commit-message.md    # /commit-message command
│
├── _specs/
│   ├── template.md              # Spec template
│   └── <slug>.md                # Feature specs
│
├── _plans/
│   ├── template.md              # Plan template
│   └── <slug>.md                # Feature plans
│
├── components/
│   └── <ComponentName>/
│       ├── <ComponentName>.tsx
│       ├── <ComponentName>.module.css
│       └── index.ts
│
├── tests/
│   └── components/
│       └── <ComponentName>.test.tsx
│
└── WORKFLOW.md                  # ← Quick reference (this file)
```

---

## Common Patterns

### Pattern 1: Simple Feature (1 scope, 4h)

```
Day 1:
09:00 - /spec "Add export CSV button"
09:15 - 🔴 GATE 1: "Approved, proceed to plan"
09:20 - Plan created
09:25 - 🔴 GATE 2: "Approved, start implementation"
09:30 - Implement → /code-review → commit → push
12:30 - Create PR
13:00 - 🔴 GATE 3: Review & merge
13:15 - ✅ Complete
```

**Total:** Half day

---

### Pattern 2: Medium Feature (3 scopes, 14h)

```
Day 1:
09:00 - /spec
09:30 - 🔴 GATE 1: Approved
09:45 - Plan (3 scopes)
10:00 - 🔴 GATE 2: Approved
10:15 - Scope 1 → review → commit → push
13:00 - Scope 2 → review → commit → push

Day 2:
09:00 - Scope 3 → review → commit → push
12:00 - Create PR
12:30 - 🔴 GATE 3: Review & merge
13:00 - ✅ Complete
```

**Total:** 1.5 days

---

### Pattern 3: Complex Feature (5 scopes, 3 days)

```
Day 1: Spec + Plan + Scope 1-2
Day 2: Scope 3-4
Day 3: Scope 5 + PR + Review
```

---

## Troubleshooting

### Problem: Agent proceeds without approval

**Solution:**
```
Stop! I did not approve yet.

Please wait for explicit approval before proceeding.
Revert any work done after the spec/plan.
```

---

### Problem: Code review finds critical issues

**Solution:**
```
Fix the critical issues first, then proceed.

Do not commit until /code-review shows no critical issues.
```

---

### Problem: PR too large to review

**Solution:**
```
This should not happen with scope breakdown.

If it does: Split into smaller scopes and re-plan.
```

---

## Quality Standards

Every commit must pass:

- ✅ **Tests:** All tests passing
- ✅ **TypeScript:** No type errors
- ✅ **Code Review:** No critical/serious issues
- ✅ **Accessibility:** WCAG 2.1 Level AA minimum
- ✅ **Conventions:** Follow project style guide

---

## When to Use This Workflow

**✅ Use Strict Gates when:**
- Quality is critical (healthcare, finance, security)
- Team is small (2-4 people, tight coordination)
- Features are small-medium (1-4 scopes, 1-3 days)
- You have time for synchronous reviews
- Mistakes are costly to fix post-merge

**❌ Consider flexible variant when:**
- Team is distributed (async reviews needed)
- Features are large (5+ scopes, 1+ weeks)
- Speed matters more than perfection
- Developers are experienced (can self-regulate)

See [Workflow Variants](docs/development-workflow.md#workflow-variants) for alternatives.

---

## Resources

**Documentation:**
- [Development Workflow (complete guide)](docs/development-workflow.md)
- [Agent Setup Guide](docs/agent-setup-guide.md)
- [Shape Up Principles](docs/principles.md)

**Templates:**
- [Spec Template](templates/spec-template.md)
- [Plan Template](templates/plan-template.md)
- [PR Template](templates/pr-template.md)

**Examples:**
- [Simple Feature Example](examples/simple-feature/)
- [Medium Feature Example](examples/medium-feature/)
- [Complex Feature Example](examples/complex-feature/)

---

## Updates

**2026-02-19:** Initial workflow quick reference (Variant B: Strict Gates)

---

**Questions?** See [complete guide](docs/development-workflow.md) or open an issue.
