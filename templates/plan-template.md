# [Feature Name] - Implementation Plan

<!--
  PLAN TEMPLATE - Spec → Task Breakdown
  
  Purpose: Transform a technical spec into executable scopes with tasks, dependencies, and estimates.
  
  When to use: After spec has been approved and before implementation starts
  
  Who fills this: AI agent (Plan Mode) or human developer during kickoff
  
  Inspired by: Claude Code Masterclass (Plan Mode) + Shape Up (Scopes & Hill Charts)
  Learn more: https://netninja.dev/p/claude-code-masterclass
-->

**Spec:** `_specs/[feature-slug].md`  
**Branch:** `claude/feature/[feature-slug]`  
**Cycle:** [e.g., 2026-Q1-C3]  
**Estimated Total:** [X hours / Y days]  
**Status:** 🟡 Planning → ✅ Approved → 🚀 In Progress

---

## 📊 Executive Summary

<!-- One-paragraph overview of implementation approach -->

[2-3 sentences summarizing the implementation strategy, number of scopes, and key technical decisions]

**Example:**
> This feature will be implemented in 3 independent scopes: OverviewCard component, ListingsTable component, and API + Page integration. The first two scopes can be built in parallel (no dependencies), while the third scope combines them. Total estimated time: 14 hours (~2 days) with one agent.

---

## 🎯 Scopes

<!-- 
  Scopes are INDEPENDENT, INTEGRATED parts of the project.
  Good scope: Can be built, tested, and integrated separately.
  Bad scope: Just a task list without clear integration point.
  
  Hill Chart Position (Shape Up concept):
  - Uphill (figuring out): 🏔️ High % = more unknowns
  - Downhill (executing): ⛰️ Low % = mostly known work
-->

### Scope 1: [Scope Name] 🏔️ (Uphill: [0-100]%)

**What:** [1-2 sentence description of what this scope delivers]

**Why This is a Scope:** [How it integrates independently, what it delivers on its own]

**Tasks:**
1. [ ] [Specific, actionable task]
2. [ ] [Specific, actionable task]
3. [ ] [Specific, actionable task]
4. [ ] [Specific, actionable task]
5. [ ] [Specific, actionable task]

**Dependencies:** 
- None / [List dependencies on other scopes or external factors]

**Estimated Time:** [X hours]

**Uphill Work (Unknowns):** 
- [What needs to be figured out? What are we uncertain about?]
- **Resolution:** [How we'll resolve these unknowns]

**Files to Create/Modify:**
```
[List of file paths that will be touched]
```

**Definition of Done:**
- [ ] [Testable criterion]
- [ ] [Integration point validated]
- [ ] [Tests pass]

---

**Example: Small Scope (2-4 hours)**

### Scope 1: Overview Card Component ⛰️ (Uphill: 20%)

**What:** Create reusable OverviewCard component that displays aggregate performance stats (views, inquiries, bookings) with icons and loading states.

**Why This is a Scope:** Delivers a standalone, testable component that can be previewed independently and later integrated into the dashboard page.

**Tasks:**
1. [ ] Write component tests first (TDD: renders, loading, error states)
2. [ ] Create OverviewCard.tsx with TypeScript interface
3. [ ] Create OverviewCard.module.css with responsive design
4. [ ] Implement loading skeleton pattern
5. [ ] Add Lucide React icons (Eye, MessageCircle, Calendar)
6. [ ] Create barrel export (index.ts)
7. [ ] Add to preview page for visual QA
8. [ ] Run tests and ensure 100% pass

**Dependencies:** 
- None (can start immediately)

**Estimated Time:** 4 hours

**Uphill Work (Unknowns):**
- Best skeleton loading pattern (minimal figuring needed)
- **Resolution:** Use existing pattern from other components in codebase

**Files to Create/Modify:**
```
components/OverviewCard/
├── OverviewCard.tsx          (new)
├── OverviewCard.module.css   (new)
└── index.ts                  (new)

tests/components/
└── OverviewCard.test.tsx     (new)

app/(public)/preview/page.tsx (modify: add demo)
```

**Definition of Done:**
- [ ] Component renders correctly with mock stats
- [ ] All 3 tests pass (renders, loading, error)
- [ ] CSS matches Figma design specs
- [ ] Responsive on mobile (tested at 320px width)
- [ ] Visible in preview page at /preview
- [ ] TypeScript strict mode passes
- [ ] No console errors or warnings

---

**Example: Medium Scope (4-8 hours)**

### Scope 2: Listings Table Component 🏔️ (Uphill: 40%)

**What:** Build sortable table component showing per-listing performance breakdown with client-side sorting and pagination.

**Why This is a Scope:** Delivers an independent, reusable table that can handle any dataset with sorting/pagination, testable in isolation before integration.

**Tasks:**
1. [ ] Write table component tests (renders, sorting, pagination, edge cases)
2. [ ] Create ListingsTable.tsx with TypeScript interfaces
3. [ ] Create ListingsTable.module.css (table styling, hover states)
4. [ ] Implement sort logic (client-side, case-insensitive)
5. [ ] Implement pagination UI (10 items per page)
6. [ ] Add sort indicators (arrows) to column headers
7. [ ] Add status badge styling (Active=green, Inactive=gray)
8. [ ] Handle empty state (no listings message)
9. [ ] Add to preview page with mock data (50+ rows)
10. [ ] Run tests and performance check (sorting 100+ items)

**Dependencies:**
- None (can run in parallel with Scope 1)

**Estimated Time:** 6 hours

**Uphill Work (Unknowns):**
- Sorting performance with 100+ items (needs testing)
- **Resolution:** Implement simple array.sort first, optimize if slow (likely fine)
- Best pagination pattern (some research needed)
- **Resolution:** Study existing patterns in codebase, use simple offset-based

**Files to Create/Modify:**
```
components/ListingsTable/
├── ListingsTable.tsx          (new)
├── ListingsTable.module.css   (new)
└── index.ts                   (new)

tests/components/
└── ListingsTable.test.tsx     (new)

app/(public)/preview/page.tsx  (modify: add table demo)
```

**Definition of Done:**
- [ ] Table renders with mock data (50+ rows)
- [ ] Sorting works on all 5 columns (Name, Views, Inquiries, Bookings, Status)
- [ ] Pagination displays 10 items per page
- [ ] Tests pass for sorting, pagination, empty state
- [ ] Responsive on mobile (horizontal scroll works)
- [ ] Visible in preview page
- [ ] Performance tested with 100 rows (<100ms sort time)

---

**Example: Large Scope (6-12 hours)**

### Scope 3: API Endpoint + Page Integration 🏔️ (Uphill: 50%)

**What:** Create `/api/performance` endpoint with database queries, build performance dashboard page, and integrate OverviewCard + ListingsTable components.

**Why This is a Scope:** Delivers the final working feature by connecting data layer (API) with UI layer (components), making the feature shippable.

**Tasks:**

**API Endpoint:**
1. [ ] Create `/api/performance/route.ts` file structure
2. [ ] Implement authentication check (session validation)
3. [ ] Write database query for aggregate stats (SUM, COUNT)
4. [ ] Write database query for per-listing breakdown (JOIN, GROUP BY)
5. [ ] Add error handling (try/catch, 401, 500 responses)
6. [ ] Test API with curl/Postman (auth, success, error cases)
7. [ ] Write API integration tests

**Dashboard Page:**
8. [ ] Create `app/(dashboard)/performance/page.tsx`
9. [ ] Implement server-side data fetching (Server Component)
10. [ ] Integrate OverviewCard component (pass stats prop)
11. [ ] Integrate ListingsTable component (pass listings prop)
12. [ ] Add loading.tsx for loading state
13. [ ] Add error.tsx for error boundary
14. [ ] Style page layout (title, spacing, responsive)

**Navigation:**
15. [ ] Add "Performance" link to dashboard navbar
16. [ ] Test navigation flow (click link → page loads)

**Testing:**
17. [ ] Run full integration test (auth → API → page → components)
18. [ ] Test with realistic data (50+ listings)
19. [ ] Test error scenarios (DB down, unauthenticated)
20. [ ] Performance test (page load time <2s)

**Dependencies:**
- ⚠️ **BLOCKS ON:** Scope 1 (OverviewCard) and Scope 2 (ListingsTable)
- Must be completed last (needs both components)

**Estimated Time:** 8 hours

**Uphill Work (Unknowns):**
- Database query performance (major unknown)
- **Resolution:** Test with realistic data (100 listings), add indexes if needed
- Auth middleware pattern (some research needed)
- **Resolution:** Study existing protected API routes (`/api/bookings`)
- Next.js App Router data fetching best practices (minor learning)
- **Resolution:** Follow official Next.js docs for Server Components

**Files to Create/Modify:**
```
app/api/performance/
├── route.ts                   (new)
└── route.test.ts              (new, optional)

app/(dashboard)/performance/
├── page.tsx                   (new)
├── loading.tsx                (new)
└── error.tsx                  (new)

components/Navbar/
└── Navbar.tsx                 (modify: add Performance link)
```

**Definition of Done:**
- [ ] `/api/performance` returns correct JSON structure `{ stats, listings }`
- [ ] API enforces authentication (401 if not logged in)
- [ ] API handles errors gracefully (500 with error message)
- [ ] Performance page renders with real data from API
- [ ] OverviewCard displays correct aggregate numbers
- [ ] ListingsTable displays all listings with sorting
- [ ] Loading state shows during data fetch
- [ ] Error boundary catches and displays errors
- [ ] Navigation link works (navbar → /performance)
- [ ] Page loads in <2s with 100 listings
- [ ] All integration tests pass

---

## 🔀 Dependencies & Execution Order

<!-- Visual dependency graph -->

```
Timeline (2 days with 1 agent):

Day 1 Morning    ┌──────────────────┐
                 │ Scope 1          │  (4 hours)
                 │ OverviewCard     │
                 └──────────────────┘

Day 1 Afternoon  ┌──────────────────┐
Day 2 Morning    │ Scope 2          │  (6 hours)
                 │ ListingsTable    │
                 └──────────────────┘

Day 2 Afternoon  ┌──────────────────┐
                 │ Scope 3          │  (8 hours)
                 │ API + Page       │  ⚠️ Depends on Scope 1 + 2
                 └──────────────────┘
                           │
                           ▼
                      🚀 Feature Complete
```

**Parallel Work Possible:**
- ✅ Scope 1 and Scope 2 can run **in parallel** (no dependencies)
- ❌ Scope 3 must run **after** Scope 1 and 2 (needs components)

**Critical Path:**
Scope 2 (longest) → Scope 3 (blocks shipping) = **14 hours minimum**

---

## 🎨 Hill Chart Estimates

<!-- Shape Up concept: visualize where scopes are (uphill=figuring out, downhill=executing) -->

```
              Uphill (Figuring It Out)  │  Downhill (Making It Happen)
                                        │
    100%                                │                          0%
      │                                 │                            │
      │                                 │                            │
      │            Scope 3 🏔️          │                            │
      │             (50%)               │                            │
      │                                 │        Scope 1 ⛰️         │
      │         Scope 2 🏔️             │         (20%)             │
      │          (40%)                  │                            │
      │                                 │                            │
      ├─────────────────────────────────┼────────────────────────────┤
    Unknowns                            │                        Knowns
```

**Interpretation:**
- **Scope 1 (20% uphill):** Mostly known work, minimal unknowns
- **Scope 2 (40% uphill):** Some unknowns (sorting performance, pagination pattern)
- **Scope 3 (50% uphill):** Most unknowns (DB query perf, auth pattern, integration complexity)

**Implication:** Start with Scope 3 unknowns **research** early (study existing auth, test DB query) to move it downhill faster.

---

## 🛠️ Technical Decisions

<!-- Document key architectural choices made during planning -->

### 1. Data Fetching Strategy

**Decision:** Server-side fetch in page component (Next.js Server Component)

**Why:**
- Simpler than client-side data fetching (no SWR/React Query needed)
- Better performance (data ready on server, no loading spinner)
- Leverages Next.js caching and revalidation
- Matches existing codebase pattern (other dashboard pages)

**Alternatives Considered:**
- ❌ Client-side with SWR: Adds library dependency, more complex, slower UX
- ❌ Client-side with useEffect: Worst option (loading flicker, extra roundtrip)

**Code Example:**
```typescript
// app/(dashboard)/performance/page.tsx
async function getPerformanceData() {
  const res = await fetch('http://localhost:3000/api/performance', {
    cache: 'no-store', // or cache: 'force-cache' with revalidate
  })
  return res.json()
}

export default async function PerformancePage() {
  const data = await getPerformanceData()
  return <OverviewCard stats={data.stats} />
}
```

---

### 2. Sorting Implementation

**Decision:** Client-side sorting (JavaScript `array.sort()`)

**Why:**
- Dataset is small (<100 listings expected)
- No backend changes needed (simpler)
- Instant feedback (no API roundtrip)
- Works offline (cached data)

**Alternatives Considered:**
- ❌ Server-side sorting: Over-engineering for small dataset, adds API complexity
- ❌ Database ORDER BY: Requires API changes, slower UX (network roundtrip)

**Future Consideration:** If listings grow to 1000+, migrate to server-side sorting

**Code Example:**
```typescript
const sortedListings = [...listings].sort((a, b) => {
  if (sortKey === 'name') return a.name.localeCompare(b.name)
  return a[sortKey] - b[sortKey] // numeric sort
})
```

---

### 3. Component Testing Approach

**Decision:** Vitest + React Testing Library (unit tests) + manual integration testing

**Why:**
- Matches project conventions (already using Vitest)
- Fast feedback loop (unit tests run in <1s)
- Integration tests cover happy path (manual for now)
- No E2E complexity needed for v1

**Coverage Target:** 80%+ on components

**Alternatives Considered:**
- ❌ E2E with Playwright: Overkill for v1, slow, flaky
- ❌ No tests: Risky for component reusability

---

### 4. Performance Optimization Strategy

**Decision:** Start simple, optimize if needed (measure first)

**Why:**
- Avoid premature optimization
- Deliver faster (no upfront perf work)
- Real user data will guide optimization
- Likely fast enough with 100 listings

**Performance Targets:**
- API response: <500ms (p95)
- Page load: <2s (desktop)
- Sort operation: <100ms (100 items)

**Optimization Plan (if needed):**
1. Add database indexes (`CREATE INDEX ON bookings(listing_id, status)`)
2. Implement Redis caching for aggregate stats (5-minute TTL)
3. Add pagination to API (if listings >100)
4. Use React.memo on table rows (if re-renders slow)

---

## ⚠️ Risks & Mitigations

<!-- Identify potential blockers and how to handle them -->

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Database query slow (>1s with 100 listings) | Medium | High | Add composite indexes, test with realistic data early |
| Sorting slow with 100+ rows | Low | Medium | Measure first, optimize if >100ms (likely fine) |
| Design doesn't match Figma | Low | Low | Use Figma design extractor agent during Scope 1/2 |
| Auth middleware pattern unclear | Low | High | Study existing protected routes (`/api/bookings`) first |
| Scope creep (user requests filters) | Medium | Medium | Defer to v2, document in "Out of Scope" |
| Dependencies block progress (Scope 3 waiting) | Medium | Medium | Start Scope 1 and 2 in parallel to minimize wait |

**Biggest Risk:** Database query performance (Scope 3)

**Mitigation Steps:**
1. Research existing DB query patterns in codebase (Day 1)
2. Write test query with realistic data (50-100 listings) early
3. Profile query execution time (`EXPLAIN ANALYZE`)
4. Add indexes proactively if query >200ms
5. Have backup plan: implement pagination if still slow

---

## 📋 Definition of Done (Overall)

<!-- What "shipped" means for this feature -->

**All Scopes Complete:**
- [ ] All scope-specific DoD checklists ✅
- [ ] All PRs merged to main branch
- [ ] All tests passing (unit + integration)
- [ ] No regression bugs (existing features work)

**Production Ready:**
- [ ] Feature deployed to staging environment
- [ ] Manual testing completed (checklist below)
- [ ] Performance validated (meets targets)
- [ ] Accessibility tested (keyboard nav, screen reader)
- [ ] Security reviewed (auth checks, no vulnerabilities)
- [ ] Documentation updated (if needed)

**Manual Testing Checklist:**
- [ ] Load /performance page → displays data ✅
- [ ] All stats match database query ✅
- [ ] Sort each column → works correctly ✅
- [ ] Pagination → shows 10 per page ✅
- [ ] Responsive on mobile (320px) ✅
- [ ] Loading state shows during fetch ✅
- [ ] Error state shows when API fails ✅
- [ ] Unauthenticated user → redirected to login ✅
- [ ] Navigation link works (navbar → performance) ✅
- [ ] No console errors or warnings ✅

**Success Metrics (Post-Launch):**
- [ ] Page load time <2s (confirmed in analytics)
- [ ] No user-reported bugs in first week
- [ ] Feature used by >50% of active users in first month

---

## 📝 Notes & Open Questions

<!-- Capture anything that needs clarification or follow-up -->

### Open Questions (Resolve Before Implementation)

- [ ] **Q:** What should happen when a listing has 0 bookings? Show "0" or "—"?
  - **A:** [To be decided]
  
- [ ] **Q:** Should inactive listings appear in the table?
  - **A:** [To be decided]
  
- [ ] **Q:** Do we need real-time updates or is static data on page load OK?
  - **A:** Static is fine for v1 (refresh to update)

### Implementation Notes

- Prefer simplicity over optimization (optimize later if needed)
- Use existing patterns (check other dashboard pages for reference)
- Ask for clarification if spec unclear (don't assume)
- Document any deviations from this plan (with reasoning)

### Future Enhancements (Post-v1)

- 📅 Date range filters (v2)
- 📊 Export to CSV (v2)
- 🔄 Real-time updates via WebSocket (v3)
- 📈 Comparison with previous periods (v2)

---

## 🔗 References

- **Spec Document:** `_specs/performance-dashboard.md`
- **Basecamp Project:** [Link to Cycle project]
- **Figma Design:** [Link]
- **Claude Code Masterclass:** [Plan Mode lesson](https://netninja.dev/p/claude-code-masterclass)
- **Shape Up Book:** [Scopes & Hill Charts](https://basecamp.com/shapeup/3.2-chapter-10)

---

## 🤖 Agent Guidance

<!-- Instructions for AI agents executing this plan -->

This plan is ready for **Claude Code Implement Mode**.

**Execution Strategy:**
1. **Start with research:** Study existing auth patterns and test DB query (de-risk Scope 3)
2. **Build Scope 1 and 2 in parallel** (if multiple agents) or sequentially (if solo)
3. **Use TDD workflow:** `/component` command enforces test-first development
4. **Open PR per scope:** Don't wait to merge everything at once
5. **Request human review:** After each scope completes
6. **Merge incrementally:** Keep main branch deployable

**Commands to Use:**
- `/component [name]` → TDD workflow for UI components
- `/code-review` → Before merging Scope 3 (critical path)
- `/commit-message` → Semantic commit messages
- Preview page → Visual QA for components

**Communication:**
- Update this plan if scope changes (mark tasks ✅ as completed)
- Flag blockers immediately (don't wait)
- Ask questions early (don't assume)

---

**Plan Status:** 🟡 Draft → Human Review → ✅ Approved → 🚀 Execute

---

_Template created for Shape Up AI Native workflow._  
_Inspired by [Claude Code Masterclass](https://netninja.dev/p/claude-code-masterclass) Plan Mode & Shape Up scopes._
