# Development Workflow: From Pitch to Production

> The complete guide to building software with AI agents using Shape Up + Claude Code

---

## Overview

This guide bridges **Shape Up methodology** (how we organize work) with **Claude Code workflow** (how AI agents build that work). By the end, you'll know exactly how to take a raw idea and ship production code with AI assistance.

**What you'll learn:**
- How to transform a Shape Up pitch into executable specs
- The Spec → Plan → Implement cycle
- TDD workflow with AI agents
- Code review automation (accessibility + quality)
- PR strategies and merge decisions
- Real-world examples at different scales

**Who this is for:**
- Developers at PILL
- Open-source contributors to shape-up-ai-native
- Teams wanting to adopt AI-native development

**Prerequisites:**
- Basic understanding of [Shape Up methodology](./shaping.md)
- Claude Code or similar AI coding assistant
- Git workflow knowledge

---

## How Shape Up + Claude Code Work Together

Shape Up gives us **what** to build and **when** to build it. Claude Code gives us **how** to build it efficiently with AI.

### The Integration Map

```mermaid
graph TD
    A[Shape Up: SHAPING] -->|pitch.md| B[Shape Up: BETTING]
    B -->|Approved| C[Claude Code: SPEC MODE]
    C -->|spec.md| D[Claude Code: PLAN MODE]
    D -->|scopes + tasks| E[Claude Code: IMPLEMENT MODE]
    E -->|Working code| F[Shape Up: SHIPPING]
    
    C -.->|Figma Design| C1[Design Specs]
    D -.->|Dependencies| D1[Task Breakdown]
    E -.->|TDD Loop| E1[Tests + Code]
    E -.->|Code Review| E2[A11y + Quality]
    E -->|PRs| E3[Human Review]
    
    style A fill:#e1f5ff
    style B fill:#e1f5ff
    style F fill:#e1f5ff
    style C fill:#fff4e1
    style D fill:#fff4e1
    style E fill:#fff4e1
```

**Key insight:** What Shape Up calls "Building" (one phase) becomes **three distinct phases** with Claude Code:

1. **Spec Mode:** Turn pitch → detailed specification
2. **Plan Mode:** Turn spec → implementation plan with scopes
3. **Implement Mode:** Turn plan → working code with tests

This additional structure gives AI agents the **incremental clarity** they need to build autonomously.

---

## Phase 1: From Pitch to Spec

**Goal:** Transform a shaped pitch into a detailed technical specification that an AI agent can execute.

### When You're Ready

You have an approved pitch from betting. It's **rough** (not detailed), **solved** (not open-ended), and **bounded** (clear scope).

Example pitch: *"Performance Dashboard - 2 week appetite - Shows aggregate stats + per-listing breakdown for property managers"*

### The `/spec` Command

Claude Code provides a `/spec` command that automates spec creation:

**Basic usage:**
```
/spec Build performance dashboard with overview card and listings table
```

**With Figma design:**
```
/spec Performance dashboard, figma: https://figma.com/file/abc123...
```

### What Happens Under the Hood

```mermaid
sequenceDiagram
    participant H as Human
    participant A as AI Agent
    participant F as Figma API
    participant G as Git

    H->>A: /spec Performance dashboard, figma: [link]
    A->>G: Check git status
    G-->>A: Clean (no uncommitted changes)
    A->>F: Extract design from Figma
    F-->>A: Colors, typography, spacing, layout
    A->>A: Parse pitch → feature_slug
    A->>G: Create branch: claude/feature/performance-dashboard
    A->>A: Draft spec from template
    A->>G: Save to _specs/performance-dashboard.md
    A->>H: ✅ Spec created! Review at _specs/performance-dashboard.md
```

### Spec Structure

The generated spec follows this structure:

```markdown
# [Feature Name]

**Branch:** claude/feature/[slug]
**Pitch Reference:** [Link to Basecamp]
**Appetite:** [Time box]
**Cycle:** [Current cycle]

---

## Feature Overview
[What this feature does, 2-3 sentences]

## User Stories
[As a... I want to... So that...]

## Technical Requirements
### Data Sources
### Components to Create
### Database Schema

## Design Specifications
[Extracted from Figma or written manually]
### [Component] Visual Specs
### Responsive Behavior

## Success Criteria
- [ ] Checklist of outcomes

## Out of Scope (v1)
- ❌ Features explicitly not included

## Implementation Notes
[Technical guidance, patterns to follow]

## Dependencies
[External APIs, libraries, existing code]

## Testing Strategy
[How to verify this works]
```

### Real Example: Performance Dashboard Spec

Let's look at a real spec excerpt:

```markdown
# Performance Dashboard

**Branch:** claude/feature/performance-dashboard
**Appetite:** 1-2 weeks

---

## Feature Overview

Build a dashboard showing aggregate performance metrics (total views, inquiries, 
bookings) and per-listing breakdown for real estate portfolio managers.

## User Stories

**As a** property manager  
**I want to** see total views, inquiries, and bookings across all listings  
**So that** I can gauge overall portfolio performance at a glance

**As a** property manager  
**I want to** see per-listing breakdown with sorting  
**So that** I can identify underperforming properties

---

## Technical Requirements

### Data Sources
- **Database:** PostgreSQL (`listings` and `bookings` tables)
- **API:** `/api/performance` endpoint (to create)
- **Auth:** Protected route (requires session)

### Components to Create
1. **OverviewCard** - Aggregate stats component
2. **ListingsTable** - Sortable table with per-listing data
3. **PerformancePage** - Container page

### Database Schema
```sql
-- Existing tables to query:
listings (id, name, views, status)
bookings (id, listing_id, created_at, status)
```

---

## Design Specifications

### OverviewCard Component

**Visual:**
- White card with rounded corners (8px border radius)
- 3 stat blocks side-by-side: Views | Inquiries | Bookings
- Each stat: Large number (32px bold) + small label (14px)
- Icons: Eye, MessageCircle, Calendar (Lucide React)

**Colors:**
- Background: `bg-white` / `bg-gray-900` (dark mode)
- Numbers: `--primary` (#C27AFF)
- Labels: `text-gray-600`

**Spacing:**
- Card padding: 24px
- Gap between stats: 32px

**States:**
- Loading: Skeleton placeholders
- Error: Red border + error message

**Responsive:**
- Desktop: 3 columns horizontal
- Mobile (< 640px): Stack vertically

[... more detailed specs ...]
```

### Your Role as Human

**Review the spec** before the agent proceeds:

✅ **Check:**
- Does it match the pitch intent?
- Are edge cases covered?
- Is the scope bounded (no creep)?
- Are success criteria measurable?
- Is technical approach sound?

❌ **Red flags:**
- Spec too detailed (over-specification)
- Solving problems not in pitch
- Missing authentication/security
- Unclear success criteria

**Provide feedback:**
```
Looks good overall! Two changes:
1. Add pagination to ListingsTable (could be 100+ listings)
2. Clarify: Should "inquiries" include both email + phone?
```

Agent updates spec → you approve → proceeds to planning.

---

## Phase 2: From Spec to Plan

**Goal:** Break the spec into independent **scopes** with clear tasks, dependencies, and estimates.

### What Makes a Good Scope?

A scope is an **independently shippable piece** of the feature.

**Bad example (tasks):**
- Create database migration ❌
- Add API endpoint ❌
- Build UI component ❌
- Write tests ❌

**Good example (scopes):**
1. ✅ **Overview Card** (complete component: code + tests + styles)
2. ✅ **Listings Table** (complete component: code + tests + sorting logic)
3. ✅ **API + Integration** (endpoint + page that uses both components)

**Why scopes matter:**
- Each scope can ship independently
- PRs are smaller and easier to review
- Progress is visible (scope complete = working feature)
- Parallelization possible (independent scopes)

### The Planning Process

Claude Code's **Plan Mode** analyzes the spec and discovers scopes:

**Triggers:**
- Human: "Create implementation plan from this spec"
- Or: Native command palette → Plan Mode

**Agent analyzes:**
1. Spec requirements
2. Existing codebase patterns
3. Dependencies between components
4. Technical complexity (unknowns)

**Agent produces:**
```markdown
# [Feature] - Implementation Plan

**Spec:** _specs/[feature].md
**Branch:** claude/feature/[slug]
**Estimated Total:** X hours/days

---

## Scopes

### Scope 1: [Name] ⛰️ (Uphill: 20%)

**What:** [Description of this independently shippable piece]

**Tasks:**
1. [Specific task]
2. [Specific task]
3. [...]

**Dependencies:** None / Depends on Scope X

**Estimated:** X hours

**Unknowns:**
- [Technical uncertainty to resolve]

**Files to Create:**
- [List of files]

---

### Scope 2: [Name] 🏔️ (Uphill: 50%)

[... same structure ...]

---

## Execution Order

[Dependency-aware ordering, what can run in parallel]

## Technical Decisions

[Key architectural choices with rationale]

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk] | [L/M/H] | [L/M/H] | [Strategy] |

## Definition of Done (per Scope)

**Scope 1:**
- [ ] Component renders correctly
- [ ] Tests pass
- [ ] CSS matches design
- [ ] Responsive
- [ ] Added to preview page
```

### Real Example: Performance Dashboard Plan

```markdown
# Performance Dashboard - Implementation Plan

**Estimated Total:** 14 hours (2 days)

---

## Scopes

### Scope 1: Overview Card ⛰️ (Uphill: 20%)

**What:** Component showing aggregate stats (views, inquiries, bookings)

**Tasks:**
1. Create OverviewCard component structure
2. Create OverviewCard.module.css with design specs
3. Write tests (renders, loading state, error state)
4. Add to preview page

**Dependencies:** None

**Estimated:** 4 hours

**Unknowns:**
- Best loading skeleton pattern (✓ Resolve: use existing pattern from Navbar)

**Files to Create:**
- `components/OverviewCard/OverviewCard.tsx`
- `components/OverviewCard/OverviewCard.module.css`
- `components/OverviewCard/index.ts`
- `tests/components/OverviewCard.test.tsx`

---

### Scope 2: Listings Table ⛰️ (Uphill: 30%)

**What:** Sortable table showing per-listing breakdown

**Tasks:**
1. Create ListingsTable component
2. Create ListingsTable.module.css
3. Write tests (renders, sorting, pagination)
4. Implement sort logic (client-side)
5. Implement pagination UI
6. Add to preview page

**Dependencies:** None

**Estimated:** 6 hours

**Unknowns:**
- Performance with 100+ rows
  - **Decision:** Start simple with client-side sort, optimize if slow

**Files to Create:**
- `components/ListingsTable/ListingsTable.tsx`
- `components/ListingsTable/ListingsTable.module.css`
- `components/ListingsTable/index.ts`
- `tests/components/ListingsTable.test.tsx`

---

### Scope 3: API + Page Integration 🏔️ (Uphill: 50%)

**What:** API endpoint + performance page combining components

**Tasks:**
1. Create `/api/performance` route
2. Implement database queries
3. Add error handling + auth check
4. Create performance page
5. Integrate OverviewCard + ListingsTable
6. Add loading/error states
7. Add to dashboard navigation

**Dependencies:** Scope 1 (OverviewCard), Scope 2 (ListingsTable)

**Estimated:** 4 hours

**Unknowns:**
- Database query performance (test with realistic data)
- Auth middleware pattern (check existing protected routes)

**Files to Create:**
- `app/api/performance/route.ts`
- `app/(dashboard)/performance/page.tsx`
- Update `components/Navbar/Navbar.tsx`

---

## Execution Order

```
Day 1 Morning:    Scope 1 (OverviewCard)          ┐
Day 1 Afternoon:  Scope 2 (ListingsTable) - start ┤ Can be parallel
Day 2 Morning:    Scope 2 (ListingsTable) - finish┘
Day 2 Afternoon:  Scope 3 (API + Integration)      ← Depends on 1 + 2
```

**Parallel work:** Scope 1 and 2 are independent (no blocking)

---

## Technical Decisions

### Data Fetching Strategy
**Decision:** Server-side fetch in page (Next.js Server Component)  
**Why:** Simpler, no client-side data lib needed, leverages App Router  
**Alternative considered:** SWR (rejected: adds complexity)

### Sorting Implementation
**Decision:** Client-side array.sort()  
**Why:** Dataset small (<100 listings), fast enough  
**Future:** If grows to 1000+, move to backend sorting

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| DB query slow (>1s) | Medium | High | Add indexes, test with realistic data |
| Sorting slow with many rows | Low | Medium | Start simple, optimize if needed |
| Design doesn't match Figma | Low | Low | Use figma-design-extractor agent |
```

### Your Role as Human

**Review the plan:**

✅ **Check:**
- Are scopes truly independent?
- Is dependency order correct?
- Are estimates realistic?
- Are unknowns identified?
- Is there a clear Definition of Done?

**Provide feedback:**
```
Plan looks solid! One concern:
- Scope 3 seems heavy (API + page + integration). Consider splitting 
  into "API Endpoint" and "Page Integration" for easier review?
```

Agent adjusts → you approve → implementation begins.

---

## Phase 3: Implementation with TDD

**Goal:** Build each scope using Test-Driven Development (Red → Green → Refactor).

### The TDD Loop

```mermaid
graph LR
    A[Write Test] -->|Run| B[Test Fails ❌]
    B -->|Write Code| C[Test Passes ✅]
    C -->|Clean Up| D[Refactor]
    D -->|Commit| E[Next Feature]
    E -->|Next Test| A
    
    style B fill:#ffcccc
    style C fill:#ccffcc
```

**Why TDD with AI agents?**
- **Tests define behavior** (agent knows what "done" looks like)
- **Prevents over-engineering** (only write code to pass tests)
- **Regression safety** (future changes won't break this)
- **Documentation** (tests show how to use the code)

### The `/component` Command

For UI components, Claude Code provides a TDD workflow command:

**Usage:**
```
/component User stats card showing total views
```

**What happens:**

```mermaid
sequenceDiagram
    participant H as Human
    participant A as Agent
    participant T as Test Runner
    participant P as Preview Page

    H->>A: /component UserStatsCard
    A->>A: Parse → ComponentName (PascalCase)
    A->>T: Create tests/components/UserStatsCard.test.tsx
    A->>T: npm test (expect FAIL)
    T-->>A: ❌ Component doesn't exist
    A->>A: Create UserStatsCard.tsx + .module.css
    A->>T: npm test (expect PASS)
    T-->>A: ✅ All tests pass
    A->>P: Add to app/(public)/preview/page.tsx
    A->>H: ✅ Component ready! View at /preview
```

### Real Example: Building OverviewCard

Let's walk through **Scope 1** implementation step-by-step.

#### Step 1: Write the Test (RED)

**Human triggers:**
```
Let's start with Scope 1. Create OverviewCard component using TDD.
```

**Agent creates test first:**

```typescript
// tests/components/OverviewCard.test.tsx
import { render, screen } from '@testing-library/react'
import { describe, it, expect } from 'vitest'
import OverviewCard from '@/components/OverviewCard'

describe('OverviewCard', () => {
  it('renders successfully with stats', () => {
    const stats = { views: 1234, inquiries: 56, bookings: 12 }
    render(<OverviewCard stats={stats} />)
    
    expect(screen.getByText('1234')).toBeInTheDocument()
    expect(screen.getByText('56')).toBeInTheDocument()
    expect(screen.getByText('12')).toBeInTheDocument()
  })

  it('displays all three stat labels', () => {
    const stats = { views: 1000, inquiries: 50, bookings: 10 }
    render(<OverviewCard stats={stats} />)
    
    expect(screen.getByText('Total Views')).toBeInTheDocument()
    expect(screen.getByText('Inquiries')).toBeInTheDocument()
    expect(screen.getByText('Bookings')).toBeInTheDocument()
  })

  it('shows loading state when stats are null', () => {
    render(<OverviewCard stats={null} loading={true} />)
    expect(screen.getByRole('status')).toBeInTheDocument()
  })

  it('handles zero values correctly', () => {
    const stats = { views: 0, inquiries: 0, bookings: 0 }
    render(<OverviewCard stats={stats} />)
    
    expect(screen.getByText('0')).toBeInTheDocument()
  })
})
```

**Agent runs test:**
```bash
npm test tests/components/OverviewCard.test.tsx
```

**Result:** ❌ FAIL (component doesn't exist yet)

#### Step 2: Write Minimal Code (GREEN)

**Agent creates component:**

```typescript
// components/OverviewCard/OverviewCard.tsx
import styles from './OverviewCard.module.css'
import { Eye, MessageCircle, Calendar } from 'lucide-react'

interface OverviewCardProps {
  stats: {
    views: number
    inquiries: number
    bookings: number
  } | null
  loading?: boolean
}

export default function OverviewCard({ stats, loading }: OverviewCardProps) {
  if (loading) {
    return (
      <div role="status" className={styles.loading} aria-label="Loading stats">
        <div className={styles.skeleton}></div>
      </div>
    )
  }

  if (!stats) return null

  return (
    <div className={styles.card}>
      <div className={styles.stat}>
        <Eye className={styles.icon} aria-hidden="true" />
        <div className={styles.number}>{stats.views.toLocaleString()}</div>
        <div className={styles.label}>Total Views</div>
      </div>
      
      <div className={styles.stat}>
        <MessageCircle className={styles.icon} aria-hidden="true" />
        <div className={styles.number}>{stats.inquiries.toLocaleString()}</div>
        <div className={styles.label}>Inquiries</div>
      </div>
      
      <div className={styles.stat}>
        <Calendar className={styles.icon} aria-hidden="true" />
        <div className={styles.number}>{stats.bookings.toLocaleString()}</div>
        <div className={styles.label}>Bookings</div>
      </div>
    </div>
  )
}
```

```css
/* components/OverviewCard/OverviewCard.module.css */
.card {
  @apply bg-white dark:bg-gray-900 rounded-lg p-6 shadow-sm;
  @apply flex gap-8;
  @apply border border-gray-200 dark:border-gray-800;
}

.stat {
  @apply flex flex-col items-center flex-1;
  @apply min-w-0; /* Allow flex shrink */
}

.icon {
  @apply text-primary w-8 h-8 mb-3;
}

.number {
  @apply text-3xl font-bold text-primary mb-1;
}

.label {
  @apply text-sm text-gray-600 dark:text-gray-400;
  @apply text-center;
}

.loading {
  @apply bg-gray-100 dark:bg-gray-800 rounded-lg p-6;
  @apply animate-pulse;
}

.skeleton {
  @apply h-24 bg-gray-200 dark:bg-gray-700 rounded;
}

/* Responsive: stack on mobile */
@media (max-width: 640px) {
  .card {
    @apply flex-col gap-6;
  }
  
  .stat {
    @apply border-b border-gray-200 dark:border-gray-800 pb-6;
  }
  
  .stat:last-child {
    @apply border-b-0 pb-0;
  }
}
```

```typescript
// components/OverviewCard/index.ts
export { default } from './OverviewCard'
```

**Agent runs test:**
```bash
npm test tests/components/OverviewCard.test.tsx
```

**Result:** ✅ PASS (all tests green!)

#### Step 3: Add to Preview Page

**Agent updates preview:**

```typescript
// app/(public)/preview/page.tsx
import OverviewCard from '@/components/OverviewCard'

export default function PreviewPage() {
  const mockStats = {
    views: 12543,
    inquiries: 234,
    bookings: 67
  }
  
  return (
    <div className="p-8 space-y-12">
      <section>
        <h2 className="text-2xl font-bold mb-4">Overview Card</h2>
        
        <div className="space-y-6">
          <div>
            <h3 className="text-lg font-semibold mb-2">Default State</h3>
            <OverviewCard stats={mockStats} />
          </div>
          
          <div>
            <h3 className="text-lg font-semibold mb-2">Loading State</h3>
            <OverviewCard stats={null} loading={true} />
          </div>
          
          <div>
            <h3 className="text-lg font-semibold mb-2">Zero Values</h3>
            <OverviewCard stats={{ views: 0, inquiries: 0, bookings: 0 }} />
          </div>
        </div>
      </section>
    </div>
  )
}
```

**Agent reports:**
```
✅ Scope 1 (OverviewCard) complete!

Created:
- components/OverviewCard/OverviewCard.tsx
- components/OverviewCard/OverviewCard.module.css
- components/OverviewCard/index.ts
- tests/components/OverviewCard.test.tsx

Tests: 4 passed (4 total)
Preview: http://localhost:3000/preview

Ready to commit and open PR.
```

#### Step 4: Commit with Semantic Message

**Agent uses `/commit-message` command:**

```bash
git status
git add components/OverviewCard tests/components/OverviewCard.test.tsx app/(public)/preview/page.tsx
git diff --staged
```

**Agent generates:**
```
✨ feat: Add OverviewCard component for performance dashboard

Implements the overview card showing aggregate stats (views, inquiries, bookings).

Features:
- Responsive design (horizontal on desktop, vertical on mobile)
- Loading state with skeleton animation
- Dark mode support
- Number formatting with locale
- Accessibility (ARIA labels, semantic HTML)

Tests:
- Renders with stats correctly
- Displays all three stat labels
- Shows loading state
- Handles zero values

Part of Scope 1 (performance-dashboard feature).
```

**Human approves → Agent commits**

#### Step 5: Open Pull Request

**Agent pushes branch:**
```bash
git push origin claude/feature/performance-dashboard
```

**Agent creates PR on GitHub:**

**Title:** `Scope 1: OverviewCard component for Performance Dashboard`

**Description:**
```markdown
## Scope: Overview Card

**Feature:** Performance Dashboard  
**Spec:** _specs/performance-dashboard.md  
**Scope:** 1 of 3

---

### What

Implements performance overview card showing aggregate stats:
- Total views (with Eye icon)
- Inquiries (with MessageCircle icon)
- Bookings (with Calendar icon)

### Why

First independently shippable scope of the performance dashboard feature. 
Property managers need to see aggregate performance at a glance.

---

### Implementation

**Components:**
- `OverviewCard` (TypeScript React component)
- CSS Modules with Tailwind @apply
- Lucide React icons
- Responsive (desktop horizontal, mobile vertical)

**States:**
- Default: Shows stats with formatted numbers
- Loading: Skeleton animation
- Null stats: Renders nothing

---

### Testing

✅ **Unit tests (4 passed):**
- Renders successfully with stats
- Displays all three stat labels
- Shows loading state
- Handles zero values

✅ **Manual testing:**
- Tested in preview page (`/preview`)
- Verified responsive behavior (320px to 1920px)
- Tested dark mode
- Verified accessibility (screen reader compatible)

---

### Screenshots

**Desktop:**
![OverviewCard desktop](https://...)

**Mobile:**
![OverviewCard mobile](https://...)

**Loading:**
![Loading state](https://...)

---

### Files Changed
- ✅ `components/OverviewCard/OverviewCard.tsx` (new)
- ✅ `components/OverviewCard/OverviewCard.module.css` (new)
- ✅ `components/OverviewCard/index.ts` (new)
- ✅ `tests/components/OverviewCard.test.tsx` (new)
- ✅ `app/(public)/preview/page.tsx` (modified)

---

### Checklist

- [x] Component renders correctly
- [x] Tests pass
- [x] CSS matches design specs
- [x] Responsive on mobile
- [x] Added to preview page
- [x] No console errors/warnings
- [x] TypeScript strict mode passes
- [x] Follows project conventions (no semicolons, CSS Modules, barrel exports)

---

### Next Steps

After merge:
- Scope 2: ListingsTable component
- Scope 3: API + Integration

---

**Ready for review!** 🚀
```

**Agent adds labels:** `scope:1`, `feature:performance-dashboard`, `ready-for-review`

### Your Role as Human Reviewer

**Review checklist:**

✅ **Code Quality**
- Does code match spec?
- Are tests meaningful (not just passing)?
- Is error handling present?
- Are edge cases covered?

✅ **Design**
- Does it match Figma design?
- Is it responsive?
- Dark mode works?

✅ **Standards**
- Follows project conventions?
- TypeScript types correct?
- Accessibility (ARIA, semantic HTML)?
- No hardcoded values that should be config?

✅ **Integration**
- Will it work with rest of system?
- No breaking changes?
- Dependencies correct?

**Provide feedback:**

**Option 1: Approve**
```
✅ LGTM! Code is clean, tests pass, design matches spec.

Merging.
```

**Option 2: Request changes**
```
Good work! Two small changes:

1. Line 23: Use `aria-label` on loading div for screen readers
2. CSS line 15: Add transition to hover state on mobile

Otherwise looks great!
```

**Agent addresses feedback → you re-review → approve → merge**

---

## Phase 4: Code Review Automation

**Goal:** Catch accessibility and quality issues before human review.

### The `/code-review` Command

Before opening PR (or after creating it), run automated review:

```
/code-review
```

### What Happens

**Two specialist agents run in parallel:**

```mermaid
graph LR
    A[Human: /code-review] --> B[Gather Git Diff]
    B --> C[a11y-reviewer Agent]
    B --> D[code-quality-reviewer Agent]
    C --> E[Merge Results]
    D --> E
    E --> F[Present Action Plan]
    F --> G{Human Approves?}
    G -->|Yes| H[Agent Implements]
    G -->|No| I[Manual Fixes]
    
    style C fill:#e1f5ff
    style D fill:#e1f5ff
```

#### Agent 1: Accessibility Reviewer

**Checks for:**
- WCAG 2.1/2.2 compliance
- Semantic HTML
- ARIA attributes
- Keyboard navigation
- Focus management
- Screen reader compatibility
- Color contrast
- Touch target sizes

**Severity levels:**
- 🔴 **Critical:** Content completely inaccessible
- 🟠 **Serious:** Major barriers (difficult to use)
- 🟡 **Moderate:** Friction but workarounds exist
- 🔵 **Minor:** Best practice improvements

**Example output:**
```markdown
### Accessibility Review

🟠 **Serious Issue**
**File:** components/OverviewCard/OverviewCard.tsx  
**Lines:** 15-17  
**WCAG Criterion:** 1.3.1 Info and Relationships

**Problem:**
Icons lack descriptive labels for screen readers. 
Users with screen readers will hear "image" instead of understanding 
what each stat represents.

**Current Code:**
```typescript
<Eye className={styles.icon} />
```

**Suggested Fix:**
```typescript
<Eye className={styles.icon} aria-hidden="true" />
<span className="sr-only">Total Views:</span>
```

**Why:**
Decorative icons should be hidden from assistive tech, with 
meaningful text provided separately. This ensures screen reader 
users get the same information as sighted users.

**Impact:** 
Affects ~15% of users (those using screen readers or keyboard-only navigation).
```

#### Agent 2: Code Quality Reviewer

**Checks 7 categories:**
1. **Clarity & Readability** - Complex logic, magic numbers
2. **Naming** - Confusing variable/function names
3. **Duplication** - Copy-paste code
4. **Error Handling** - Missing try-catch, no fallbacks
5. **Secrets & Security** - Hardcoded keys, exposed data
6. **Input Validation** - Unvalidated user input
7. **Performance** - Inefficient algorithms, memory leaks

**Severity levels:**
- 🔴 **Critical:** Security vulnerabilities, data loss, crashes
- 🟠 **High:** Bugs, missing error handling
- 🟡 **Medium:** Code clarity, duplication
- 🔵 **Low:** Minor naming, style

**Example output:**
```markdown
### Code Quality Review

🔴 **Critical Issue**
**Category:** Secrets & Security  
**File:** app/api/performance/route.ts  
**Line:** 12

**Problem:**
Database connection string is hardcoded in the file. This exposes 
credentials in version control.

**Current Code:**
```typescript
const db = postgres('postgresql://user:pass@localhost:5432/db')
```

**Suggested Fix:**
```typescript
const db = postgres(process.env.DATABASE_URL!)
```

**Why:**
Environment variables keep secrets out of code. Never commit credentials.

---

🟡 **Medium Issue**
**Category:** Duplication  
**File:** components/ListingsTable/ListingsTable.tsx  
**Lines:** 45-52, 67-74

**Problem:**
Same sorting logic duplicated for each column.

**Suggested Fix:**
```typescript
function sortByColumn<T>(data: T[], key: keyof T, order: 'asc' | 'desc'): T[] {
  return [...data].sort((a, b) => {
    if (order === 'asc') return a[key] > b[key] ? 1 : -1
    return a[key] < b[key] ? 1 : -1
  })
}
```

**Why:**
DRY (Don't Repeat Yourself). One function is easier to test and maintain.
```

### Combined Action Plan

After both reviews complete, agent merges findings:

```markdown
## Code Review Summary

### Accessibility (a11y-reviewer)
🟠 **1 Serious Issue**
🟡 **2 Moderate Issues**

### Code Quality (code-quality-reviewer)
🔴 **1 Critical Issue**
🟡 **1 Medium Issue**

---

## Action Plan

**Priority Order:**

1. 🔴 **CRITICAL:** Move database connection string to environment variable
   - File: `app/api/performance/route.ts:12`
   - Fix: Use `process.env.DATABASE_URL`
   - Create `.env.local` with `DATABASE_URL=...`

2. 🟠 **SERIOUS:** Add screen reader labels to stat icons
   - File: `components/OverviewCard/OverviewCard.tsx:15-17`
   - Fix: Add `aria-hidden="true"` to icons, add `<span className="sr-only">` labels

3. 🟡 **MODERATE:** Add aria-label to sorting buttons
   - File: `components/ListingsTable/ListingsTable.tsx:89`
   - Fix: Add `aria-label="Sort by name"`

4. 🟡 **MEDIUM:** Extract duplicate sorting logic to helper function
   - File: `components/ListingsTable/ListingsTable.tsx:45-74`
   - Fix: Create `sortByColumn()` utility

---

## Questions

- Should the loading skeleton have a "Loading..." text for screen readers?
- Database URL pattern: Should we support multiple databases (read/write replicas)?

---

**Implement this action plan now? (yes/no)**
```

### Human Decision

**Option 1: Yes (let agent fix)**
```
yes
```

Agent implements all fixes → commits → updates PR

**Option 2: No (manual fixes)**
```
no - I'll handle #1 and #4 manually. You can do #2 and #3.
```

Agent implements partial fixes → human does rest

**Option 3: Defer**
```
Good catches! Let's defer #4 (duplication) to a future refactor. 
Fix #1-#3 now.
```

Agent implements specified fixes

---

## Phase 5: PR Strategy

**Goal:** Keep PRs small, reviewable, and independently mergeable.

### When to Open a PR

**✅ Open PR when:**
- Scope is complete (all Definition of Done items checked)
- Tests pass locally
- No obvious bugs
- Code follows conventions
- Feature works in preview/localhost

**❌ Don't open PR when:**
- Scope is half-done (unless explicitly WIP)
- Tests failing
- Code doesn't compile
- Depends on another unmerged PR (unless stacked)

### PR Size Guidelines

**Ideal PR:**
- 200-400 lines changed
- 1 scope per PR
- 15-30 min review time
- Self-contained (no external dependencies)

**Too small (<50 lines):**
- Consider batching related changes
- Exception: Critical hotfixes

**Too large (>800 lines):**
- Split into multiple scopes
- Use stacked PRs if dependent

### PR Template

**Title format:**
```
[Scope X]: [Brief description] for [Feature]
```

**Examples:**
- ✅ `Scope 1: OverviewCard component for Performance Dashboard`
- ✅ `Scope 2: ListingsTable with sorting for Performance Dashboard`
- ❌ `Add stuff` (too vague)
- ❌ `Performance Dashboard implementation` (too broad)

**Description structure:**
```markdown
## Scope: [Name]

**Feature:** [Feature name]
**Spec:** [Link to spec]
**Scope:** [X of Y]

---

### What
[What this PR does, 2-3 sentences]

### Why
[Why this is needed, business/user value]

---

### Implementation
[Technical approach, key decisions]

---

### Testing
✅ **Unit tests:** [X passed]
✅ **Manual testing:** [What you tested]

---

### Screenshots
[Visual proof it works]

---

### Files Changed
[List with checkmarks]

---

### Checklist
- [ ] Definition of Done items

---

### Next Steps
[What comes after this PR]
```

### Stacked PRs (Advanced)

When scopes have dependencies, use stacked PRs:

**Example:**
- PR #1: `Scope 1: OverviewCard` (base: `main`)
- PR #2: `Scope 2: ListingsTable` (base: `main`)
- PR #3: `Scope 3: API + Integration` (base: `main`, depends on #1 and #2)

**Workflow:**
1. Open all 3 PRs simultaneously
2. Mark PR #3 as "Draft" (depends on #1 and #2)
3. Review and merge #1 and #2 independently
4. Once #1 and #2 merged → mark #3 as "Ready for review"
5. Review and merge #3

**Benefits:**
- Reviewers can work in parallel
- Faster feedback loop
- Clear dependency visualization

---

## Real-World Examples

Let's see this workflow at three different scales.

### Example 1: Small Scope (4 hours)

**Feature:** "Add export CSV button to Performance Dashboard"

**Appetite:** 4 hours

#### Pitch (condensed)
```markdown
# CSV Export Button

**Appetite:** 4 hours (small batch)

**Problem:** Property managers want to analyze data in Excel

**Solution:** Add "Export CSV" button to Performance Dashboard that 
downloads current view as CSV file

**Rabbit Holes:** 
- Don't build custom CSV formatter (use library)
- Don't add date range filters (future scope)

**No Gos:**
- PDF export
- Email delivery
- Scheduled exports
```

#### Spec → Plan → Implement

**Spec created in 15 min:**
- Add button to PerformancePage
- Use `json2csv` library
- Include current table data
- File name: `performance-export-YYYY-MM-DD.csv`

**Plan created in 10 min:**
- **Single scope** (no breakdown needed - it's simple)
- Estimated: 4 hours

**Implementation (TDD):**

1. **Write test** (30 min):
```typescript
it('exports CSV when button clicked', async () => {
  const { user } = setup()
  const button = screen.getByRole('button', { name: /export csv/i })
  await user.click(button)
  
  // Check download triggered
  expect(mockDownload).toHaveBeenCalledWith(
    expect.stringContaining('performance-export-'),
    expect.stringContaining('text/csv')
  )
})
```

2. **Implement button** (1.5 hours):
```typescript
// components/ExportButton.tsx
import { Download } from 'lucide-react'
import { json2csv } from 'json-2-csv'

export default function ExportButton({ data }: { data: any[] }) {
  function handleExport() {
    const csv = json2csv(data)
    const blob = new Blob([csv], { type: 'text/csv' })
    const url = URL.createObjectURL(blob)
    const a = document.createElement('a')
    a.href = url
    a.download = `performance-export-${new Date().toISOString().split('T')[0]}.csv`
    a.click()
    URL.revokeObjectURL(url)
  }

  return (
    <button onClick={handleExport} className="btn btn-secondary">
      <Download className="w-4 h-4 mr-2" aria-hidden="true" />
      Export CSV
    </button>
  )
}
```

3. **Add to page** (30 min):
```typescript
// app/(dashboard)/performance/page.tsx
<div className="flex justify-between items-center mb-4">
  <h2>Listings Breakdown</h2>
  <ExportButton data={data.listings} />
</div>
```

4. **Code review** (30 min):
   - a11y: ✅ No issues
   - quality: 🟡 Add try-catch around json2csv

5. **Fix + commit** (20 min)

6. **Open PR** (10 min)

7. **Human review + merge** (15 min)

**Total time:** 3.5 hours (under appetite ✅)

**Result:** Feature shipped same day

---

### Example 2: Medium Scope (2 days)

**Feature:** "User Authentication with Email + Password"

**Appetite:** 2 days

#### Pitch (condensed)
```markdown
# User Authentication

**Appetite:** 2 days

**Problem:** App currently has no user accounts. Need to protect 
dashboard from public access.

**Solution:** 
- Email/password signup and login
- Session stored in httpOnly cookies
- Protected routes (middleware)

**Rabbit Holes:**
- Don't build OAuth (future)
- Don't build password reset (future)
- Use existing session library (next-auth or similar)

**No Gos:**
- Social login
- 2FA
- User profiles
```

#### Spec → Plan

**Spec created in 45 min**

**Plan created in 30 min:**

**Scopes:**
1. **Auth API Routes** (signup, login, logout endpoints)
2. **Auth Forms** (signup page, login page)
3. **Protected Routes** (middleware + session checks)

**Estimated total:** 12 hours (1.5 days)

#### Implementation

**Day 1 Morning: Scope 1 (Auth API Routes)**

1. Write tests for `/api/auth/signup`:
```typescript
it('creates user with valid email and password', async () => {
  const res = await POST('/api/auth/signup', {
    email: 'test@example.com',
    password: 'SecurePass123!'
  })
  
  expect(res.status).toBe(201)
  expect(res.body.user.email).toBe('test@example.com')
})

it('rejects weak passwords', async () => {
  const res = await POST('/api/auth/signup', {
    email: 'test@example.com',
    password: '123'
  })
  
  expect(res.status).toBe(400)
  expect(res.body.error).toContain('Password must be')
})
```

2. Implement signup endpoint (2 hours)
3. Write tests for `/api/auth/login` (30 min)
4. Implement login endpoint (1.5 hours)
5. Write tests for `/api/auth/logout` (15 min)
6. Implement logout endpoint (30 min)
7. Code review (30 min) → fix issues
8. Commit + open PR #1

**Human reviews PR #1** (async, while agent continues)

**Day 1 Afternoon: Scope 2 (Auth Forms)**

1. Write tests for SignupForm component (45 min)
2. Implement signup page (2 hours)
3. Write tests for LoginForm (30 min)
4. Implement login page (1.5 hours)
5. Add form validation (30 min)
6. Code review (30 min) → fix issues
7. Commit + open PR #2

**Human reviews PR #2** (async)

**Day 2 Morning: Scope 3 (Protected Routes)**

1. Write tests for middleware (1 hour)
2. Implement session middleware (2 hours)
3. Protect dashboard routes (1 hour)
4. Add redirect logic (unauthorized → /login) (30 min)
5. Code review (30 min) → fix issues
6. Commit + open PR #3 (depends on #1 and #2)

**Day 2 Afternoon: Integration + Polish**

1. Human merges PR #1 ✅
2. Human merges PR #2 ✅
3. Agent rebases PR #3 on main
4. Human reviews PR #3
5. Human requests change: "Add loading state to login form"
6. Agent implements change (30 min)
7. Human merges PR #3 ✅

8. **Integration testing** (agent + human together):
   - Test signup flow end-to-end
   - Test login flow
   - Test protected route redirects
   - Test logout

9. **Deploy to staging** (1 hour human work)
10. **Validate in staging** (30 min human)
11. **Deploy to production** ✅

**Total time:** 1.5 days development + 0.5 days review/deploy = **2 days total** (on appetite ✅)

---

### Example 3: Large Scope (1 week)

**Feature:** "Real-time Collaboration on Pitch Documents"

**Appetite:** 1 week (5 days)

#### Pitch (condensed)
```markdown
# Real-time Pitch Collaboration

**Appetite:** 1 week

**Problem:** Team members edit pitches in Basecamp comments, causing 
version conflicts and lost work.

**Solution:** 
- Real-time collaborative editor (like Google Docs)
- Show who's currently editing
- Auto-save drafts
- Conflict resolution

**Rabbit Holes:**
- Use existing CRDT library (don't build from scratch)
- Start with text-only (no rich formatting v1)
- Use WebSockets (not polling)

**No Gos:**
- Rich text formatting (v2)
- Version history (v2)
- Comments/annotations (v2)
```

#### Spec → Plan

**Spec created in 2 hours** (complex feature, needs research)

**Plan created in 1.5 hours:**

**Scopes:**
1. **WebSocket Infrastructure** (server + connection handling)
2. **CRDT Document Store** (Yjs integration + persistence)
3. **Collaborative Editor Component** (UI with live cursors)
4. **Presence System** (show who's online)
5. **Auto-save + Conflict Resolution** (draft handling)

**Estimated total:** 32 hours (4 days dev + 1 day integration/testing)

#### Implementation (Week View)

**Monday:**
- Scope 1: WebSocket Infrastructure (6 hours)
  - Write tests for WebSocket server
  - Implement connection handling
  - Add auth to WebSocket connections
  - Code review + fix issues
  - PR #1 opened
  
- Scope 2: CRDT Document Store (2 hours start)
  - Research Yjs library
  - Write tests for document initialization

**Tuesday:**
- Scope 2: CRDT Document Store (finish - 6 hours)
  - Implement Yjs document creation
  - Add persistence to database
  - Write tests for sync logic
  - Code review + fix issues
  - PR #2 opened (depends on #1)

- Scope 3: Collaborative Editor Component (2 hours start)
  - Write tests for editor rendering
  - Setup editor UI skeleton

**Wednesday:**
- Scope 3: Collaborative Editor Component (finish - 6 hours)
  - Implement editor with Yjs binding
  - Add live cursor tracking
  - Style editor UI
  - Code review + fix issues
  - PR #3 opened (depends on #1 and #2)

- Scope 4: Presence System (2 hours start)
  - Write tests for presence tracking

**Thursday:**
- Scope 4: Presence System (finish - 4 hours)
  - Implement "who's online" indicator
  - Add user avatars
  - Code review + fix issues
  - PR #4 opened (depends on #1)

- Scope 5: Auto-save + Conflict Resolution (4 hours start)
  - Write tests for auto-save
  - Implement draft persistence

**Friday:**
- Scope 5: Auto-save + Conflict Resolution (finish - 2 hours)
  - Implement conflict resolution UI
  - Code review + fix issues
  - PR #5 opened

- **Integration Day** (6 hours - agent + human together)
  - Human reviews and merges PRs #1-#5
  - Agent fixes any integration issues
  - End-to-end testing with 3 simultaneous users
  - Performance testing (100+ concurrent connections)
  - Fix bugs discovered during testing
  - Deploy to staging
  - Final validation in staging
  - Deploy to production ✅

**Total time:** 4 days development + 1 day integration = **5 days** (on appetite ✅)

**Result:** Production-ready real-time collaboration shipped in one cycle

---

## Commands Reference

Quick reference for all workflow commands.

### `/spec` - Create Specification

**Purpose:** Transform pitch into detailed technical spec

**Syntax:**
```
/spec [feature description]
/spec [feature description], figma: [figma-url]
```

**Examples:**
```
/spec Build user profile page with avatar upload
/spec Dashboard cards for analytics, figma: https://figma.com/file/abc...
```

**What it does:**
1. Checks git status (aborts if dirty)
2. Parses feature → slug
3. Extracts Figma design (if URL provided)
4. Creates branch: `claude/feature/[slug]`
5. Generates spec from template
6. Saves to `_specs/[slug].md`

**Output:**
```
✅ Spec created!

Branch: claude/feature/user-profile-page
Spec file: _specs/user-profile-page.md
Title: User Profile Page with Avatar Upload

Next: Review spec, then ask me to create implementation plan.
```

---

### `/component` - TDD Component Creation

**Purpose:** Create UI component using Test-Driven Development

**Syntax:**
```
/component [ComponentName or description]
```

**Examples:**
```
/component UserProfileCard
/component Card showing user stats and avatar
```

**What it does:**
1. Parses → ComponentName (PascalCase)
2. Creates test file first
3. Runs test (expect fail)
4. Creates component files
5. Runs test (expect pass)
6. Adds to preview page

**Output:**
```
✅ Component created!

Files:
- components/UserProfileCard/UserProfileCard.tsx
- components/UserProfileCard/UserProfileCard.module.css
- components/UserProfileCard/index.ts
- tests/components/UserProfileCard.test.tsx

Tests: 3 passed
Preview: http://localhost:3000/preview

Ready to commit.
```

---

### `/code-review` - Automated Review

**Purpose:** Run accessibility + quality review on current changes

**Syntax:**
```
/code-review
```

**What it does:**
1. Gathers git diff (unstaged + staged)
2. Invokes two agents in parallel:
   - `a11y-reviewer` (accessibility)
   - `code-quality-reviewer` (code quality)
3. Merges results into action plan
4. Asks permission to implement fixes

**Output:**
```
## Code Review Summary

### Accessibility
🟠 1 Serious Issue
🟡 2 Moderate Issues

### Code Quality
🔴 1 Critical Issue

## Action Plan
1. 🔴 Move API key to environment variable
2. 🟠 Add aria-labels to icon buttons
3. 🟡 Extract duplicate sorting logic

Implement this plan now? (yes/no)
```

---

### `/commit-message` - Generate Semantic Commit

**Purpose:** Generate conventional commit message from diff

**Syntax:**
```
/commit-message
```

**What it does:**
1. Runs `git status`
2. Runs `git diff --staged`
3. Analyzes changes
4. Generates semantic commit message

**Output:**
```
Suggested commit message:

✨ feat: Add user profile page with avatar upload

Implements user profile view with editable fields and image upload.

Features:
- Avatar upload with preview
- Editable name, email, bio fields
- Form validation
- Auto-save drafts

Tests: 5 unit tests added

Commit with this message? (yes/no)
```

**Commit types:**
- ✨ `feat:` - New feature
- 🐛 `fix:` - Bug fix
- 🔨 `refactor:` - Code refactoring
- 📝 `docs:` - Documentation
- 🎨 `style:` - Styling/formatting
- ✅ `test:` - Tests
- ⚡ `perf:` - Performance
- 🔧 `chore:` - Build/tooling

---

## Troubleshooting

Common problems and solutions.

### Problem: Spec Too Detailed

**Symptom:** Spec is 20+ pages, specifies every variable name

**Why it happens:** Over-specification. Agent has too much detail, loses flexibility.

**Solution:**
```
The spec should be "rough, solved, bounded" (Shape Up principle).

Good level of detail:
✅ "Table should be sortable by any column"
❌ "Create sortByColumn(data, key, order) function with QuickSort algorithm"

Rule of thumb: Spec = WHAT and WHY. Code = HOW.
```

**Human action:** Ask agent to simplify spec, remove implementation details.

---

### Problem: Scope Creep During Implementation

**Symptom:** Agent adds features not in spec (e.g., "I also added dark mode toggle")

**Why it happens:** Agent infers features from context or prior knowledge.

**Solution:**
```
Stop! This wasn't in the spec.

Original scope: User profile page
You added: Dark mode toggle (not requested)

Revert the dark mode changes. Stick to spec only.

If dark mode is needed, we'll add it as a separate scope.
```

**Prevention:** In spec, add explicit "Out of Scope" section.

---

### Problem: Tests Pass But Feature Broken

**Symptom:** Tests green, but manual testing shows bugs

**Why it happens:** Tests are too shallow (only happy path).

**Solution:**
```
Tests need to cover edge cases:

Current tests:
✅ User can upload avatar
❌ Missing: What if file too large?
❌ Missing: What if file wrong type (PDF)?
❌ Missing: What if network fails during upload?

Add these test cases:
1. test('rejects files larger than 5MB')
2. test('only accepts image formats')
3. test('shows error message on upload failure')
```

**Prevention:** In TDD, write edge case tests before implementation.

---

### Problem: PR Too Large to Review

**Symptom:** PR has 2000+ lines changed, takes 2+ hours to review

**Why it happens:** Scope too large, or multiple scopes in one PR.

**Solution:**
```
This PR mixes 3 scopes:
1. User profile page
2. Avatar upload system
3. Image processing backend

Split into 3 PRs:
- PR #1: User profile page (UI only, with mock data)
- PR #2: Avatar upload (API + storage)
- PR #3: Integration (connect UI to API)

Close this PR, open 3 smaller ones.
```

**Prevention:** Plan scopes smaller (200-400 lines each).

---

### Problem: Merge Conflicts

**Symptom:** Can't merge PR because main branch changed

**Why it happens:** Long-lived branch, main moved forward

**Solution:**
```bash
# Update your branch with latest main
git checkout claude/feature/user-profile
git fetch origin
git rebase origin/main

# Resolve conflicts (agent can help)
# Then force-push
git push origin claude/feature/user-profile --force-with-lease
```

**Prevention:** Merge PRs quickly (< 24 hours), keep scopes small.

---

### Problem: Tests Failing in CI But Pass Locally

**Symptom:** Local tests green, GitHub Actions red

**Why it happens:** Environment differences (timezone, database, dependencies)

**Solution:**
```
Common causes:

1. Timezone issues:
   - Use UTC in tests: new Date('2024-01-01T00:00:00Z')

2. Database state:
   - Clean database before each test (beforeEach hook)

3. Async timing:
   - Use waitFor() instead of fixed delays

4. Missing environment variables:
   - Check .env.example matches CI secrets
```

**Debug:** Check CI logs, reproduce locally with `CI=true npm test`.

---

### Problem: Agent Ignores Code Review Feedback

**Symptom:** Human requests changes, agent says "done" but didn't actually fix

**Why it happens:** Miscommunication or agent misunderstood feedback

**Solution:**
```
Be specific in feedback:

Bad feedback:
❌ "Fix the accessibility issues"

Good feedback:
✅ "Add aria-label='Close dialog' to the X button on line 45 
   of components/Modal/Modal.tsx"

Show exact file, line number, and expected code.
```

**Verification:** After agent claims fix, check the diff yourself.

---

## Metrics: Measuring Success

How to track if this workflow is working.

### Cycle Metrics

**Track these per 6-week cycle:**

| Metric | Target | Why It Matters |
|--------|--------|----------------|
| **Scopes Completed** | 80%+ of planned | Are we shipping what we committed to? |
| **Scopes Cut** | <20% | Are we overcommitting? |
| **PR Review Time** | <24 hours | Are we bottlenecking on human review? |
| **Tests Passing Rate** | >95% | Is code quality maintained? |
| **Critical Bugs in Production** | 0-1 per cycle | Are we catching issues before shipping? |
| **Rollbacks** | 0 | Are we shipping broken code? |

**How to measure:**
```bash
# Scopes completed
git log --since="6 weeks ago" --grep="Scope.*:" --oneline | wc -l

# Average PR review time
gh pr list --state merged --limit 100 --json createdAt,mergedAt \
  | jq '.[] | (.mergedAt | fromdateiso8601) - (.createdAt | fromdateiso8601)' \
  | awk '{sum+=$1; count++} END {print sum/count/3600 " hours"}'
```

---

### Code Quality Metrics

**Track these continuously:**

| Metric | Target | Tool |
|--------|--------|------|
| **Test Coverage** | >80% | Vitest coverage report |
| **TypeScript Strict Mode** | 100% passing | `tsc --noEmit` |
| **Accessibility Score** | >90 (Lighthouse) | Lighthouse CI |
| **Build Time** | <30 seconds | CI logs |
| **Bundle Size** | <200KB (gzipped) | Next.js build output |

**How to measure:**
```bash
# Test coverage
npm test -- --coverage

# TypeScript errors
npx tsc --noEmit

# Bundle size
npm run build
# Check .next/analyze output
```

---

### Velocity Metrics

**Track these to understand team pace:**

| Metric | Calculation | Insight |
|--------|-------------|---------|
| **Average Scope Duration** | Total hours / Scopes completed | How long does typical scope take? |
| **Spec → Code Time** | Time from spec approval to first PR | How fast do we go from idea to code? |
| **Review → Merge Time** | Time from PR open to merge | Where are review bottlenecks? |
| **Bug Fix Rate** | Bugs fixed / Bugs reported | Are we keeping up with issues? |

**Example tracking (Basecamp):**
```
Project: Q1 Cycle 3
Hill Chart: 
  - Scope 1: Downhill (100%) - 4 hours
  - Scope 2: Downhill (100%) - 6 hours
  - Scope 3: Uphill (40%) - 3 hours so far

Average scope duration: 5 hours
Scopes completed: 2 of 3 (67%)
Days remaining: 28 days
```

---

### Comparison: Before vs After AI Agents

**Typical PILL project metrics:**

| Metric | Before AI | After AI | Improvement |
|--------|-----------|----------|-------------|
| Scopes/cycle | 3-4 | 6-8 | +100% |
| PR review time | 2-3 days | <1 day | -66% |
| Test coverage | ~60% | >80% | +33% |
| Bugs in production | 3-5/cycle | 0-1/cycle | -80% |
| Documentation | Often missing | Auto-generated | +∞ |
| Accessibility issues | Found after launch | Caught in review | 100% earlier |

---

## Appendices

### Appendix A: Templates

All templates are available in `/templates/`:

**Pitch Template:**
```
/templates/pitch-template.md
```
Structure for shaping pitches (Shape Up format)

**Spec Template:**
```
/templates/spec-template.md  (auto-used by /spec command)
```
Technical specification format

**Scope Template:**
```
/templates/scope-template.md
```
Individual scope tracking (Basecamp todo)

**Cycle Kickoff Template:**
```
/templates/cycle-kickoff.md
```
Starting a new 6-week cycle

**Usage:**
```bash
# Copy template
cp templates/pitch-template.md pitches/my-new-feature.md

# Edit in your editor
code pitches/my-new-feature.md
```

---

### Appendix B: Basecamp Integration

**How Shape Up artifacts map to Basecamp:**

```
Basecamp Project: "Q1 Cycle 3"
│
├── 📋 To-Do List: "Performance Dashboard"
│   ├── ☐ Spec [assigned to Vasco]
│   │   └── 📎 Attachment: _specs/performance-dashboard.md
│   │
│   ├── ☐ Scope 1: Overview Card [assigned to Vasco]
│   │   └── 🔗 Link: GitHub PR #123
│   │
│   ├── ☐ Scope 2: Listings Table [assigned to Vasco]
│   │   └── 🔗 Link: GitHub PR #124
│   │
│   └── ☐ Scope 3: API + Integration [assigned to Vasco]
│       └── 🔗 Link: GitHub PR #125
│
├── 📊 Hill Chart: "Performance Dashboard"
│   ├── Scope 1: ████████████████░░░░ (Downhill, 80%)
│   ├── Scope 2: ██████████░░░░░░░░░░ (Uphill, 50%)
│   └── Scope 3: ████░░░░░░░░░░░░░░░░ (Uphill, 20%)
│
└── 💬 Message Board
    └── "Performance Dashboard Shipped! 🚀"
        [Screenshot + demo link]
```

**Automation:**

The Basecamp skill (in `~/.openclaw/skills/basecamp/`) can auto-update:

```bash
# Create project for new cycle
bc create-project "Q1 Cycle 4" "6-week cycle starting Feb 17"

# Add feature todo list
bc create-todolist PROJECT_ID "Performance Dashboard"

# Add scope todos
bc create-todo TODOLIST_ID "Scope 1: Overview Card"

# Mark scope complete
bc complete-todo TODO_ID

# Post ship message
bc create-message PROJECT_ID "Feature Shipped" "Performance Dashboard is live! ..."
```

**Agent can do this automatically** when you say:
```
Update Basecamp: Scope 1 is complete
```

---

### Appendix C: Tool Setup

**Required tools for this workflow:**

#### 1. Claude Code (or similar AI coding assistant)

**Options:**
- **Claude Code Masterclass** snippets (this guide's approach)
- **Cursor IDE** with Claude
- **GitHub Copilot** + custom prompts
- **OpenClaw** + Claude API (PILL's setup)

**Cost:** $20-200/month depending on usage

#### 2. Git + GitHub

**Setup:**
```bash
# Configure git
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Generate SSH key for GitHub
ssh-keygen -t ed25519 -C "you@example.com"
cat ~/.ssh/id_ed25519.pub
# Add to GitHub → Settings → SSH Keys

# Test connection
ssh -T git@github.com
```

#### 3. Node.js + Package Manager

**Install:**
```bash
# Install Node.js (use version in project's .nvmrc)
brew install node

# Or use nvm for version management
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 20
nvm use 20
```

#### 4. Testing Framework

**For Next.js/React projects:**
```bash
npm install -D vitest @testing-library/react @testing-library/user-event jsdom
```

**vitest.config.mts:**
```typescript
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./vitest.setup.ts'],
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './'),
    },
  },
})
```

#### 5. Code Quality Tools

**Prettier (formatting):**
```bash
npm install -D prettier
```

**.prettierrc:**
```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

**ESLint (linting):**
```bash
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

#### 6. Figma Access (optional)

**For design extraction:**

1. Install Figma Desktop app
2. Install Figma MCP server:
```bash
npm install -g @modelcontextprotocol/server-figma
```

3. Configure in `~/.openclaw/config/mcp.json`:
```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-figma"],
      "env": {
        "FIGMA_PERSONAL_ACCESS_TOKEN": "your-token-here"
      }
    }
  }
}
```

Get token: Figma → Settings → Personal Access Tokens

---

## Credits & Resources

**This workflow is based on:**

### Claude Code Masterclass
- **Course:** https://netninja.dev/p/claude-code-masterclass
- **Author:** Shaun Pelling (Net Ninja)
- **Repository:** https://github.com/iamshaunjp/Claude-Code-Masterclass

The `/spec`, `/component`, and `/code-review` commands, plus the agent patterns (a11y-reviewer, code-quality-reviewer, figma-design-extractor) come directly from this excellent course.

**Highly recommended** for anyone wanting to dive deeper into AI-native development workflows.

---

### Shape Up Methodology
- **Book:** https://basecamp.com/shapeup
- **Authors:** Ryan Singer (Basecamp)
- **Key concepts:** Appetites, pitches, scopes, hill charts, cycles

Our implementation adapts Shape Up for AI agent collaboration.

---

### PILL Resources
- **This repository:** https://github.com/madebypill/shape-up-ai-native
- **Contributing:** See [CONTRIBUTING.md](../CONTRIBUTING.md)
- **Setup guide:** [Agent Setup Guide](./agent-setup-guide.md)
- **Basecamp integration:** [Basecamp Implementation](./basecamp-implementation.md)

---

## Next Steps

**Now that you understand the workflow:**

1. **Set up your tools** → See [Appendix C](#appendix-c-tool-setup)
2. **Try a small scope** → Follow [Example 1](#example-1-small-scope-4-hours)
3. **Review existing specs** → See `/examples/` in this repo
4. **Join the community** → Contribute to shape-up-ai-native

**For PILL team members:**
- Read [Basecamp Implementation](./basecamp-implementation.md) for PILL-specific setup
- Check Basecamp → Shaping Team for active pitches
- Ask Sergio or Vasco for onboarding

---

**Questions or feedback?** Open an issue on GitHub or message in Discord.

**Happy building!** 🚀

---

<div style="display: flex; justify-content: space-between; padding: 2rem 0; border-top: 2px solid #eee; margin-top: 3rem;">
  <div>
    ← Previous: <a href="./building.md">Building</a>
  </div>
  <div>
    Next: <a href="./tools.md">Tools & Skills</a> →
  </div>
</div>
