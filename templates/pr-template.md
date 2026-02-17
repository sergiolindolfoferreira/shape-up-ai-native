# [Scope Name or Feature Component]

<!--
  PR TEMPLATE - Pull Request Description
  
  Purpose: Provide context for code reviewers (humans and AI agents) to understand what changed and why.
  
  When to use: When opening a PR for a completed scope or feature increment
  
  Who fills this: AI agent (during implementation) or human developer
  
  Inspired by: Claude Code Masterclass (Implement Mode + Code Review agents)
  Learn more: https://netninja.dev/p/claude-code-masterclass
  
  💡 TIP: This template works with GitHub PR templates - save as `.github/pull_request_template.md`
-->

## 📦 What

<!-- Concise summary of what this PR delivers (1-2 sentences) -->

[Describe the scope/component/feature being added or changed]

**Scope:** [e.g., Scope 1 of 3: OverviewCard Component]  
**Related Spec:** `_specs/[feature-slug].md`  
**Related Plan:** `_plans/[feature-slug].md` (or comment link)  
**Jira/Basecamp:** [Link to task/todo]

---

## 🎯 Why

<!-- Explain the problem this solves or the value it adds -->

[1-2 paragraphs explaining the business/user need and why this change is important]

**User Story:**
> As a [user type], I want to [action] so that [benefit].

**Example:**

**What:**  
Implements OverviewCard component showing aggregate performance stats (total views, inquiries, bookings) with icons and loading states.

**Why:**  
Property managers need a quick visual summary of portfolio performance without manually calculating totals from spreadsheets. This card provides at-a-glance metrics that update automatically from the database.

**User Story:**
> As a property manager, I want to see aggregate stats at the top of the dashboard so that I can quickly assess overall performance without scrolling through individual listings.

---

## 🔧 Changes

<!-- Technical summary of what files/modules were modified -->

### Components Added
- [ ] `components/[ComponentName]/[ComponentName].tsx` - [Brief description]
- [ ] `components/[ComponentName]/[ComponentName].module.css` - [Styling approach]
- [ ] `components/[ComponentName]/index.ts` - Barrel export

### Tests Added
- [ ] `tests/components/[ComponentName].test.tsx` - [What scenarios covered]

### Pages/APIs Modified
- [ ] `app/[route]/page.tsx` - [What changed]
- [ ] `app/api/[endpoint]/route.ts` - [What endpoint does]

### Other Changes
- [ ] `[file path]` - [Description]

**Example:**

### Components Added
- [x] `components/OverviewCard/OverviewCard.tsx` - Main component with TypeScript interface, props: `stats`, `loading`
- [x] `components/OverviewCard/OverviewCard.module.css` - Responsive grid layout, CSS Modules with Tailwind @apply
- [x] `components/OverviewCard/index.ts` - Barrel export

### Tests Added
- [x] `tests/components/OverviewCard.test.tsx` - Covers rendering, loading state, error state, and stat display

### Preview Page Modified
- [x] `app/(public)/preview/page.tsx` - Added OverviewCard demo with mock data and loading state example

---

## ✅ Testing

<!-- Describe how this was tested and what scenarios were covered -->

### Unit Tests
- [x] All tests pass (`npm test [component].test.tsx`)
- [x] Code coverage: [X]% (minimum 80% for components)
- [x] No console errors or warnings

**Test Scenarios Covered:**
- [x] [Scenario 1, e.g., "Component renders with valid stats prop"]
- [x] [Scenario 2, e.g., "Loading state displays skeleton"]
- [x] [Scenario 3, e.g., "Error state shows error message"]
- [x] [Edge case, e.g., "Handles null/undefined data gracefully"]

### Manual Testing
- [x] Tested in development environment (`npm run dev`)
- [x] Visual QA in preview page (`/preview`)
- [x] Responsive testing (mobile 320px, tablet 768px, desktop 1024px+)
- [x] Browser testing (Chrome, Firefox, Safari)
- [x] Accessibility testing (keyboard navigation, screen reader)

**Manual Test Checklist:**
- [x] Component renders correctly
- [x] Design matches Figma specs
- [x] Responsive behavior works
- [x] No visual regressions
- [x] No TypeScript errors (`npm run type-check`)
- [x] No linting errors (`npm run lint`)

### Integration Testing (if applicable)
- [ ] API endpoint tested with curl/Postman
- [ ] Auth checks validated (401 on unauthenticated)
- [ ] Database queries tested with realistic data
- [ ] Error handling works (500 on failures)

**Example:**

### Unit Tests
- [x] All tests pass (`npm test OverviewCard.test.tsx`)
- [x] Code coverage: 92% (3 tests)
- [x] No console errors or warnings

**Test Scenarios Covered:**
- [x] Component renders successfully with stats prop
- [x] Loading skeleton displays when `loading={true}`
- [x] All three stat blocks render (views, inquiries, bookings)
- [x] Lucide icons render correctly (Eye, MessageCircle, Calendar)

### Manual Testing
- [x] Tested at `http://localhost:3000/preview`
- [x] Mobile (320px): Stats stack vertically ✅
- [x] Tablet (768px): Stats in 3-column grid ✅
- [x] Desktop (1024px+): Stats in 3-column grid with spacing ✅
- [x] Dark mode: Colors adapt correctly ✅
- [x] Keyboard tab order: Logical and accessible ✅

---

## 📸 Screenshots / Demo

<!-- Visual evidence of the changes (especially for UI components) -->

### Before (if modifying existing feature)
[Screenshot or description of previous state]

### After
[Screenshot of new component/page]

**Desktop View:**
![Desktop screenshot](https://via.placeholder.com/800x400?text=Desktop+View)

**Mobile View:**
![Mobile screenshot](https://via.placeholder.com/375x667?text=Mobile+View)

**Loading State:**
![Loading state](https://via.placeholder.com/800x400?text=Loading+State)

**Dark Mode:**
![Dark mode](https://via.placeholder.com/800x400?text=Dark+Mode)

> 💡 **Tip for AI agents:** Use browser screenshot tools or describe visual appearance if images not available.

---

## 🔍 Code Review Checklist

<!-- Self-review checklist before requesting human review -->

### General
- [ ] Code follows project conventions (no semicolons, CSS Modules, TypeScript strict)
- [ ] No commented-out code or debug console.logs
- [ ] No hardcoded values (secrets, URLs, magic numbers extracted to constants)
- [ ] Meaningful variable and function names
- [ ] No unnecessary complexity (YAGNI principle)

### TypeScript
- [ ] All types explicitly defined (no implicit `any`)
- [ ] Interfaces used for component props
- [ ] Strict mode enabled and passing
- [ ] No `@ts-ignore` or `@ts-expect-error` (or justified with comment)

### React
- [ ] Components use proper hooks (no hooks in loops/conditions)
- [ ] Props destructured consistently
- [ ] No prop drilling (context/state management if needed)
- [ ] Keys used correctly in lists
- [ ] Memo/useMemo/useCallback used appropriately (no premature optimization)

### Styling
- [ ] CSS Modules used (not inline Tailwind classes)
- [ ] Responsive design tested (mobile, tablet, desktop)
- [ ] Dark mode support (if applicable)
- [ ] Accessibility contrast ratios meet WCAG AA (minimum 4.5:1)

### Testing
- [ ] Tests cover happy path and edge cases
- [ ] Tests are readable and maintainable
- [ ] No flaky tests (consistent pass/fail)
- [ ] Test coverage meets minimum threshold (80%+)

### Performance
- [ ] No unnecessary re-renders (React DevTools profiler checked)
- [ ] Images optimized (next/image used for Next.js)
- [ ] No blocking API calls in components (loading states)
- [ ] Database queries optimized (indexes, no N+1 queries)

### Security
- [ ] No sensitive data in client-side code (API keys, secrets)
- [ ] Input validation on all user inputs
- [ ] Auth checks on protected routes/API endpoints
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (sanitized user inputs)

### Accessibility
- [ ] Semantic HTML used (`<button>`, `<nav>`, `<main>`, etc.)
- [ ] ARIA labels on icon buttons and interactive elements
- [ ] Keyboard navigation works (tab order, Enter/Space for actions)
- [ ] Focus indicators visible (no `outline: none` without alternative)
- [ ] Screen reader tested (VoiceOver/NVDA/JAWS)
- [ ] Color not sole indicator (icons, text, patterns also used)

---

## 🤖 AI Code Review

<!-- Optional: Results from automated code review agents -->

### Run Code Review

To request automated review from a11y and code quality agents:

```bash
# In Claude Code chat:
/code-review
```

### Accessibility Review (a11y-reviewer)

**Status:** ✅ Passed / ⚠️ Issues Found / ❌ Blockers

**Summary:**
- 🟢 No critical issues
- 🟡 [X] moderate issues
- 🔵 [X] minor suggestions

**Issues to Address:**
1. [Issue description with file/line]
2. [Issue description with file/line]

### Code Quality Review (code-quality-reviewer)

**Status:** ✅ Passed / ⚠️ Issues Found / ❌ Blockers

**Summary:**
- 🟢 No critical issues
- 🟡 [X] moderate issues
- 🔵 [X] minor suggestions

**Issues to Address:**
1. [Issue description with file/line]
2. [Issue description with file/line]

**Example:**

### Accessibility Review (a11y-reviewer)

**Status:** ⚠️ Minor Issues Found

**Summary:**
- 🟢 No critical issues
- 🟢 No serious issues
- 🔵 1 minor suggestion

**Issues to Address:**
1. **Consider adding aria-label to stat icons** (OverviewCard.tsx:15-20)
   - **Suggestion:** While decorative icons don't require labels, adding descriptive aria-labels improves screen reader experience
   - **Fix:** `<Eye aria-label="Total views icon" />`

### Code Quality Review (code-quality-reviewer)

**Status:** ✅ Passed

**Summary:**
- Clean, readable code
- Good separation of concerns
- Proper TypeScript typing
- Tests comprehensive

**Positive Observations:**
- Well-named variables and functions
- CSS Modules properly scoped
- Loading state handled gracefully

---

## 📋 Deployment Checklist

<!-- Pre-merge checklist for production readiness -->

### Pre-Merge
- [ ] All CI/CD checks passing (tests, lint, type-check, build)
- [ ] No merge conflicts with main branch
- [ ] Branch up-to-date with main (`git pull origin main`)
- [ ] Code review approved by [human reviewer name]
- [ ] AI code review passed (no blockers)

### Post-Merge
- [ ] Deployed to staging environment
- [ ] Smoke tested in staging (feature works end-to-end)
- [ ] No regressions detected (existing features still work)
- [ ] Performance validated (meets targets)
- [ ] Ready for production deploy

---

## 🚀 Deployment Notes

<!-- Special instructions for deploying this change -->

**Environment Variables:**
- None required for this change

**Database Migrations:**
- None required for this change

**Breaking Changes:**
- None (backwards compatible)

**Feature Flags:**
- None (feature always enabled)

**Rollback Plan:**
- If issues found: revert commit `[commit SHA]`
- No data migration needed (safe to rollback)

**Example (if applicable):**

**Database Migrations:**
- Run migration: `npm run migrate:up performance_tables`
- Rollback: `npm run migrate:down performance_tables`

**Environment Variables:**
- Add `PERFORMANCE_CACHE_TTL=300` to `.env` (staging + production)

---

## 📝 Notes

<!-- Additional context, decisions, or follow-up items -->

### Technical Decisions
- **[Decision 1]:** [Why this approach was chosen]
- **[Decision 2]:** [Trade-offs considered]

### Known Limitations (to address in future PRs)
- [ ] [Limitation 1, e.g., "No real-time updates - requires page refresh"]
- [ ] [Limitation 2, e.g., "Pagination not implemented - shows all items"]

### Follow-Up Tasks
- [ ] [Future enhancement, e.g., "Add date range filters (Scope 4)"]
- [ ] [Optimization, e.g., "Implement Redis caching if load increases"]

### Questions for Reviewers
- [ ] [Specific question about implementation choice]
- [ ] [Request for feedback on approach]

**Example:**

### Technical Decisions
- **CSS Modules over Tailwind classes:** Chosen for better scoping and readability, follows project convention
- **Client-side component (no Server Component):** Needs interactivity (loading state), so marked with "use client"

### Known Limitations
- [ ] No error retry mechanism (shows error, user must refresh page)
- [ ] Loading skeleton is basic (could use better animation)

### Follow-Up Tasks
- [ ] Implement retry logic with exponential backoff (Scope 5)
- [ ] Add Storybook stories for all component states (documentation task)

### Questions for Reviewers
- Should the loading skeleton match the exact layout or be simpler?
- Do we need animation on stats numbers (count-up effect)?

---

## 🔗 References

- **Spec:** `_specs/performance-dashboard.md`
- **Plan:** `_plans/performance-dashboard.md`
- **Basecamp Task:** [Link]
- **Figma Design:** [Link]
- **Related PRs:** 
  - #123 (Scope 1: OverviewCard) ← this PR
  - #124 (Scope 2: ListingsTable) - in progress
  - #125 (Scope 3: API + Integration) - planned

---

## ✍️ PR Metadata

<!-- Auto-filled by GitHub or AI agent -->

**Type:** ✨ Feature / 🐛 Fix / 🔨 Refactor / 📝 Docs / 🎨 Style / ✅ Test / ⚡ Perf

**Priority:** 🔴 High / 🟡 Medium / 🟢 Low

**Size:** 🐭 XS (<50 lines) / 🐱 S (50-200) / 🐶 M (200-500) / 🐘 L (500-1000) / 🦕 XL (>1000)

**Breaking Changes:** ✅ Yes / ❌ No

**Requires Migration:** ✅ Yes / ❌ No

---

**Example:**

**Type:** ✨ Feature  
**Priority:** 🟡 Medium  
**Size:** 🐱 S (~180 lines)  
**Breaking Changes:** ❌ No  
**Requires Migration:** ❌ No

---

## 👥 Reviewers

<!-- Tag relevant reviewers -->

**Human Reviewers:**
- @[lead-developer] - Code review
- @[design-lead] - Design QA (optional)

**AI Reviewers:**
- 🤖 a11y-reviewer (accessibility)
- 🤖 code-quality-reviewer (code quality)

**Suggested Review Focus:**
- Component reusability and prop design
- CSS Module scoping and naming
- Test coverage and edge cases
- Accessibility (keyboard nav, screen reader)

---

_This PR is part of the **[Feature Name]** feature (Cycle [X])._  
_Created using Shape Up AI Native workflow._  
_Automated review via [Claude Code Masterclass](https://netninja.dev/p/claude-code-masterclass) agents._

---

<!-- 
  🤖 AI AGENT INSTRUCTIONS:
  
  When creating a PR automatically:
  1. Fill out all sections based on implementation context
  2. Run /code-review before submitting
  3. Include actual screenshots (use browser tools)
  4. Cross-reference spec and plan documents
  5. Mark all completed checklist items
  6. Tag appropriate human reviewers
  7. Include commit SHA for rollback reference
  
  When reviewing a PR:
  1. Check that all checklist items are completed
  2. Verify screenshots match Figma design
  3. Validate test coverage meets threshold
  4. Ensure no security issues (secrets, XSS, SQL injection)
  5. Confirm accessibility standards met (WCAG AA)
  6. Check performance impacts (bundle size, render time)
  
  Communication:
  - Use clear, concise language
  - Provide specific file/line references
  - Suggest concrete fixes (not vague descriptions)
  - Be constructive (acknowledge good work too)
-->
