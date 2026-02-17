# [Feature Name]

<!-- 
  SPEC TEMPLATE - Pitch → Technical Specification
  
  Purpose: Transform a shaped pitch into a detailed technical spec that AI agents can use to plan and implement.
  
  When to use: After a pitch has been approved in betting (start of build cycle)
  
  Who fills this: AI agent (via /spec command) or human developer
  
  Inspired by: Claude Code Masterclass (Spec Mode)
  Learn more: https://netninja.dev/p/claude-code-masterclass
-->

**Branch:** `claude/feature/[feature-slug]`  
**Pitch Reference:** [Link to Basecamp pitch]  
**Appetite:** [1-2 weeks / 2-4 weeks / 6 weeks]  
**Cycle:** [e.g., 2026-Q1-C3]  
**Status:** 🟡 In Progress

---

## 📋 Feature Overview

<!-- High-level description of what we're building and why -->

[2-3 sentences explaining the feature, its purpose, and the problem it solves]

**Example:**
> Build a performance dashboard showing aggregate metrics and per-listing breakdown for real estate portfolio. Property managers need to quickly assess portfolio health and identify underperforming listings without manual spreadsheet work.

---

## 👤 User Stories

<!-- Focus on outcomes, not implementation details -->

**As a** [user type]  
**I want to** [action]  
**So that** [benefit/outcome]

**Example:**

**As a** property manager  
**I want to** see total views, inquiries, and bookings across all listings  
**So that** I can gauge overall portfolio performance at a glance

**As a** property manager  
**I want to** see per-listing breakdown with sortable columns  
**So that** I can identify which properties need attention

---

## 🔧 Technical Requirements

### Data Sources

<!-- What data do we need and where does it come from? -->

- **Database:** [e.g., PostgreSQL, Firestore, external API]
- **Existing Tables/Collections:** [list relevant schemas]
- **New Tables/Collections:** [if creating new data structures]
- **APIs:** [external services, endpoints to create]
- **Authentication:** [auth requirements, permissions needed]

**Example:**
- **Database:** PostgreSQL (existing `listings` and `bookings` tables)
- **API:** `/api/performance` endpoint (to create)
- **Auth:** Protected route (requires active session)

### Components to Create

<!-- List UI components, pages, or modules needed -->

1. **[ComponentName]** - [Brief description]
2. **[ComponentName]** - [Brief description]
3. **[PageName]** - [Container/layout description]

**Example:**
1. **OverviewCard** - Aggregate stats display (views, inquiries, bookings) with icons
2. **ListingsTable** - Sortable table showing per-listing breakdown
3. **PerformancePage** - Dashboard layout integrating both components

### Database Schema / Data Model

<!-- Detail any schema changes, new types, or data structures -->

```sql
-- Existing tables used:
[Table definitions or references]

-- New tables/columns needed:
[DDL statements or structure definitions]

-- Key queries/aggregations:
[Example queries that will power the feature]
```

**Example:**
```sql
-- Existing tables (no changes):
listings (id, name, views, status)
bookings (id, listing_id, created_at, status)

-- Aggregation query for overview stats:
SELECT 
  (SELECT SUM(views) FROM listings) as total_views,
  (SELECT COUNT(*) FROM bookings WHERE status = 'inquiry') as total_inquiries,
  (SELECT COUNT(*) FROM bookings WHERE status = 'confirmed') as total_bookings
```

---

## 🎨 Design Specifications

<!-- Visual and UX requirements, ideally extracted from Figma -->

### [ComponentName] Component

**Visual Design:**
- **Layout:** [Description of arrangement]
- **Colors:** [Palette with hex codes or CSS variables]
- **Typography:** [Font sizes, weights, line heights]
- **Spacing:** [Padding, margins, gaps]
- **Icons:** [Source and specific icons used]
- **Visual Effects:** [Shadows, borders, animations]

**Component States:**
- **Default:** [Normal state appearance]
- **Loading:** [Skeleton/spinner pattern]
- **Error:** [Error messaging and styling]
- **Empty:** [Zero-state handling]
- **Hover/Active:** [Interactive state changes]

**Responsive Behavior:**
- **Desktop (>1024px):** [Layout description]
- **Tablet (768-1023px):** [Adaptations]
- **Mobile (<768px):** [Mobile-specific changes]

**Example:**

### OverviewCard Component

**Visual Design:**
- **Layout:** Card with 3 stat blocks arranged horizontally
- **Colors:** 
  - Background: `bg-white` (light) / `bg-gray-900` (dark)
  - Numbers: `--primary` (#C27AFF)
  - Labels: `text-gray-600`
- **Typography:**
  - Stat numbers: 32px, bold
  - Labels: 14px, regular
- **Spacing:**
  - Card padding: 24px
  - Gap between stats: 32px
- **Icons:** Lucide React (Eye, MessageCircle, Calendar) - 24px size
- **Visual Effects:**
  - Border radius: 8px
  - Subtle shadow: `shadow-sm`

**Component States:**
- **Default:** Shows all three stats with icons
- **Loading:** Skeleton placeholders (animated pulse)
- **Error:** Red border + error message text

**Responsive Behavior:**
- **Desktop:** 3 columns, equal width
- **Mobile:** Stack vertically with full-width blocks

---

## 🧪 Testing Strategy

<!-- What tests need to be written and what to validate -->

### Unit Tests

- [ ] [Component] renders successfully
- [ ] [Component] handles loading state
- [ ] [Component] handles error state
- [ ] [Component] displays correct data
- [ ] [Function] returns expected output

**Example:**
- [ ] OverviewCard renders with stats prop
- [ ] OverviewCard shows loading skeleton when `loading={true}`
- [ ] OverviewCard displays all three stat blocks
- [ ] ListingsTable sorts correctly by each column

### Integration Tests

- [ ] [API endpoint] returns correct data structure
- [ ] [API endpoint] handles authentication
- [ ] [API endpoint] handles errors gracefully
- [ ] [Page] integrates components correctly

**Example:**
- [ ] `/api/performance` returns `{ stats, listings }` structure
- [ ] `/api/performance` returns 401 when not authenticated
- [ ] Performance page renders both components with real data

### Manual Testing Checklist

- [ ] Visual design matches Figma
- [ ] Responsive on mobile (test 320px width)
- [ ] Accessibility (keyboard navigation, screen reader)
- [ ] Performance (page loads in <2s with realistic data)
- [ ] Edge cases (empty data, 100+ items, network error)

---

## 🔒 Security Considerations

<!-- Security, auth, and data privacy requirements -->

- [ ] Authentication/authorization checks
- [ ] Input validation and sanitization
- [ ] No sensitive data exposed in client
- [ ] Rate limiting (if applicable)
- [ ] HTTPS required
- [ ] CORS configured correctly

**Example:**
- [ ] All API endpoints check session authentication
- [ ] User can only see data for their own account
- [ ] No API keys or secrets in client-side code
- [ ] Database queries use parameterized statements (no SQL injection)

---

## ⚡ Performance Requirements

<!-- Performance targets and optimization notes -->

**Targets:**
- API response time: [e.g., <500ms for p95]
- Page load time: [e.g., <2s on 3G]
- Time to Interactive: [e.g., <3s]
- Database query time: [e.g., <200ms]

**Optimizations:**
- [ ] [Specific optimization, e.g., "Add index on listings.views"]
- [ ] [Caching strategy if needed]
- [ ] [Pagination for large datasets]

**Example:**
- API response time: <500ms (p95)
- Page load: <2s on desktop, <3s on mobile
- Optimizations:
  - [ ] Add composite index on `bookings(listing_id, status)`
  - [ ] Cache aggregate stats for 5 minutes (Redis)
  - [ ] Implement pagination if listings >100

---

## 📦 Dependencies

<!-- External libs, APIs, or internal modules needed -->

### External Dependencies
- [ ] [Package name] - [Purpose and version]
- [ ] [API/Service] - [What it's used for]

### Internal Dependencies
- [ ] [Existing module/component] - [How it's used]
- [ ] [Existing service/util] - [What it provides]

**Example:**

### External Dependencies
- None (uses existing Next.js, React, Lucide React)

### Internal Dependencies
- `@/lib/auth` - Session management (existing)
- `@/lib/db` - Database connection (existing)
- `@/components/Navbar` - Add navigation link

---

## ✅ Success Criteria

<!-- Specific, measurable outcomes that define "done" -->

- [ ] [Specific testable criterion]
- [ ] [User can complete X action]
- [ ] [Performance meets Y threshold]
- [ ] [Tests achieve Z% coverage]

**Example:**
- [ ] OverviewCard renders with correct aggregate numbers
- [ ] ListingsTable displays all listings with accurate data
- [ ] Sorting works on all 5 columns (Name, Views, Inquiries, Bookings, Status)
- [ ] Page loads in <1s with 100 listings
- [ ] Responsive design works on 320px mobile width
- [ ] Unit tests achieve 80%+ coverage
- [ ] Manual testing checklist 100% complete

---

## 🚫 Out of Scope (v1)

<!-- Explicitly list what we're NOT doing in this iteration -->

- ❌ [Feature or complexity explicitly excluded]
- ❌ [Future enhancement to consider later]
- ❌ [Alternative approach we decided against]

**Why:** [Brief reasoning for scope decisions]

**Example:**
- ❌ Date range filters (future v2 - adds complexity)
- ❌ Export to CSV (future v2 - separate scope)
- ❌ Real-time updates via WebSocket (future v3 - over-engineering)
- ❌ Comparison with previous periods (future v2 - needs historical data tracking)

**Why:** Keep v1 simple and ship fast. These features require additional data structures and UX complexity that can be added later based on user feedback.

---

## 📝 Implementation Notes

<!-- Technical guidance, code patterns, and architectural decisions for agents/developers -->

### API Endpoint Pattern

```typescript
// app/api/performance/route.ts
import { NextResponse } from 'next/server'
import { getSession } from '@/lib/auth'
import { db } from '@/lib/db'

export async function GET(request: Request) {
  // 1. Auth check
  const session = await getSession()
  if (!session) {
    return new Response('Unauthorized', { status: 401 })
  }
  
  // 2. Query database
  const stats = await db.query(`...`)
  const listings = await db.query(`...`)
  
  // 3. Return JSON
  return NextResponse.json({ stats, listings })
}
```

### Component Structure Pattern

```
components/
├── OverviewCard/
│   ├── OverviewCard.tsx          # Main component file
│   ├── OverviewCard.module.css   # Scoped styles (CSS Modules)
│   └── index.ts                  # Barrel export
└── ListingsTable/
    ├── ListingsTable.tsx
    ├── ListingsTable.module.css
    └── index.ts

tests/
└── components/
    ├── OverviewCard.test.tsx     # Vitest + React Testing Library
    └── ListingsTable.test.tsx
```

### Project-Specific Conventions

<!-- Adapt this section to your codebase standards -->

- ❌ **NO semicolons** (enforced by linter)
- ✅ **CSS Modules** for component styling (not inline Tailwind)
- ✅ **TypeScript strict mode** (all types defined)
- ✅ **Barrel exports** (`index.ts` re-exports)
- ✅ **Server Components** by default (Next.js App Router)
- ✅ **"use client"** only when needed (interactivity)

### Data Fetching Strategy

**Decision:** [Chosen approach]  
**Why:** [Reasoning]  
**Alternatives Considered:** [What we decided against and why]

**Example:**

**Decision:** Server-side fetch in page component (Server Component)  
**Why:** Simpler, leverages Next.js caching, no client-side data lib needed  
**Alternatives Considered:** 
- SWR/React Query (rejected: adds complexity for static dashboard)
- Client-side fetch in useEffect (rejected: slower, worse UX)

---

## 🔗 References

<!-- Links to related resources -->

- **Basecamp Pitch:** [Link]
- **Figma Design:** [Link]
- **Related Docs:** [Links to existing documentation]
- **API Docs:** [If using external APIs]
- **Claude Code Masterclass:** [Spec Mode lesson](https://netninja.dev/p/claude-code-masterclass)

---

## 🤖 Agent Guidance

<!-- Instructions for AI agents using this spec -->

This spec is optimized for **Claude Code Spec → Plan → Implement workflow**.

**Next Steps:**
1. Human reviews this spec and approves (or requests changes)
2. Agent enters **Plan Mode** to break down into scopes
3. Agent enters **Implement Mode** using TDD workflow per scope

**Plan Mode Hints:**
- Likely scopes: OverviewCard component, ListingsTable component, API + Page integration
- OverviewCard and ListingsTable are independent (can be built in parallel)
- API + Page depends on both components (must be last)

**Implement Mode Hints:**
- Use `/component` command for UI components (enforces TDD)
- Use `/code-review` before merging critical PRs
- Use `/commit-message` for semantic commit messages
- Reference `preview` page for visual QA

---

**Spec Status:** 🟡 Draft → Human Review → ✅ Approved → 🚀 Ready for Planning

---

_Template created for Shape Up AI Native workflow._  
_Inspired by [Claude Code Masterclass](https://netninja.dev/p/claude-code-masterclass) Spec Mode._
