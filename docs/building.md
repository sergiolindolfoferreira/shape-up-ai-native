# Building with AI Agents

> The agent + human workflow during a cycle

---

## Three Gates — The Non-Negotiable Process

Before any code is written, the human must validate two documents. No exceptions.

```
PITCH (approved in Betting)
  │
  ├─► SPEC   → agent creates spec.md   → ⏸️ GATE 1: Human approves
  │
  ├─► PLAN   → agent creates plan.md   → ⏸️ GATE 2: Human approves
  │
  └─► IMPLEMENT → agent writes code → PRs → ⏸️ GATE 3: Human reviews & merges
```

### Why Three Gates?

| Gate | What it catches | Cost of skipping |
|---|---|---|
| **Spec** | Wrong problem, wrong scope, missing constraints | Agent builds the wrong thing entirely |
| **Plan** | Risky architecture, missing pieces, wrong breakdown | Agent builds the right thing the wrong way |
| **PR Review** | Bugs, edge cases, code quality | Broken features in production |

**The agent is fast. Gates are cheap. Skipping them is expensive.**

### Gate Rules

- **Agent never starts coding without Gate 2 approval**
- **Agent never spawns sub-agents to implement without explicit Plan approval**
- **If human says "just go ahead with everything"** → agent must confirm: *"Confirm you're approving Spec + Plan + Implement without intermediate review?"*
- Gates are sequential — no parallelising Spec and Plan

---

## The Core Loop

```
Agent builds → Creates PR → Human reviews → Merges or requests changes
↑                                                                      ↓
└──────────────────────── Repeat until shipped ────────────────────────┘
```

**Continuous integration replaces end-of-cycle integration.**

---

## Roles & Responsibilities

### AI Agent
- **Implements** code based on pitch + scope
- **Creates branches** (`agent/feature-name`)
- **Writes tests** (unit, integration)
- **Opens PRs** when scope completes
- **Responds** to review feedback
- **Does NOT merge** (humans gate production)

### Human Reviewer
- **Reviews PRs** (code quality, correctness, tests)
- **Tests locally** (run, verify behavior)
- **Approves or requests changes**
- **Merges** approved work
- **Deploys** to production
- **Validates** in production

---

## Daily Workflow

### Agent's Day

**Morning:**
1. Check for new review feedback
2. Address requested changes
3. Continue on current scope

**Throughout day:**
4. Implement scope incrementally
5. Run tests locally
6. Commit frequently (atomic commits)

**End of day:**
7. If scope complete → open PR
8. If scope in progress → commit + push (work-in-progress)

**No meetings, no standups needed.**

### Human's Day

**Morning:**
1. Check for new PRs
2. Prioritize by age/importance

**Throughout day:**
3. Review 2-3 PRs (15-30 min each)
4. Test locally if non-trivial
5. Approve or request changes
6. Merge approved PRs

**End of day:**
7. Deploy accumulated merges (if stable)
8. Check cycle progress

**Time commitment:** 1-2 hours/day for review

---

## Scopes, Not Tasks

Shape Up principle: Work is organized by **scopes** (parts that can ship independently), not tasks.

### What's a Scope?

**Scope = independently shippable piece of the project**

Example project: Performance Dashboard

**Bad (tasks):**
- Create database migration
- Add API endpoint
- Build frontend component
- Write tests
- Add documentation

**Good (scopes):**
1. **Overview Card** (shows aggregate stats)
2. **Listings Table** (shows per-listing breakdown)
3. **Performance Query Optimization** (indexes + caching)

Each scope:
- Can be built independently
- Has clear definition of "done"
- Delivers user value
- Generates 1-3 PRs

### Discovering Scopes

**Not decided upfront (unlike Scrum tasks)**

Agent discovers scopes as it digs in:

**Day 1:** "I see three main pieces here..."
**Day 3:** "Actually, the table needs to split into two scopes..."
**Day 5:** "Performance optimization is its own scope"

**Dynamic, not fixed.**

---

## Hill Charts (Optional)

**Original Shape Up uses Hill Charts** to show progress:

```
Uphill (figuring it out) → Peak → Downhill (just execution)
```

**With AI agents:**
- Less useful (agents don't report uncertainty well)
- Human tracks: # scopes completed / # total scopes

**Alternative: Simple Progress**

```
Dashboard Feature:
- [x] Overview Card (merged)
- [x] Listings Table (merged)
- [ ] Performance optimization (in PR #45)
- [ ] Mobile responsive (not started)

Progress: 50% (2/4 scopes done)
```

---

## Pull Request Best Practices

### Agent's PR Description

**Template:**

```markdown
## Scope: [Name]

**What:** Brief description of what this implements

**Why:** How it relates to the pitch

**Testing:**
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Tested manually (describe what you checked)

**Screenshots/Demo:** (if UI change)

**Notes:** Any decisions made, tradeoffs, or context for reviewer
```

### Human's Review Checklist

- [ ] Code matches pitch intent (not gold-plating)
- [ ] Tests exist and pass
- [ ] No obvious bugs or edge cases missed
- [ ] Follows project conventions (naming, structure)
- [ ] Doesn't break existing functionality
- [ ] Documentation updated (if needed)
- [ ] Runs locally without errors

**If any fails:** Request changes (be specific)

---

## Handling Issues

### Agent Gets Stuck

**Symptom:** No PRs for 2+ days, scope not progressing

**Action:**
1. Check in: "What's blocking you?"
2. Agent explains issue
3. Human decides:
   - Provide more context (clarify pitch)
   - Simplify scope (cut complexity)
   - Re-assign to human (too hard for agent)

**Don't let it fester.** 2 days = intervention needed.

### PR Needs Major Changes

**Symptom:** Review reveals fundamental misunderstanding

**Action:**
1. Close PR (don't try to patch)
2. Clarify pitch/scope
3. Agent starts fresh
4. Better to restart than patch bad foundation

### Scope Creep Detected

**Symptom:** PR adds features not in pitch

**Action:**
1. Request changes: "This is out of scope"
2. Agent removes extra features
3. Remind: Stick to pitch boundaries

**Don't merge scope creep** (even if code is good)

### Can't Ship in Time

**Symptom:** Day 6 of 7, only 40% done

**Action (Circuit Breaker):**
1. Stop adding scopes
2. Finish what's mergeable
3. Cut the rest
4. Ship what's done
5. Re-shape remainder for future cycle (if still important)

**Don't extend by default.**

---

## Integration Strategy

### Continuous vs. End-of-Cycle

**Original Shape Up:** Build separately, integrate late

**AI Native:** Integrate continuously

**Why:**
- Can't wait 1-2 weeks to discover integration issues
- Agents produce code fast (need feedback faster)
- PRs enable asynchronous integration

**How:**
1. Merge approved PRs daily
2. Run full test suite after each merge
3. Deploy to staging continuously
4. Deploy to production at end of cycle (or mid-cycle if stable)

### Integration Conflicts

**If merge conflicts:**
1. Agent resolves (rebase on main)
2. Re-run tests
3. Update PR
4. Human re-reviews (if significant changes)

**If breaking tests:**
1. Fix immediately (top priority)
2. Don't merge more until fixed
3. All other PRs wait

**Main branch always green.**

---

## Deployment

### Staging
- Deploy continuously (every merge)
- Automated (CI/CD pipeline)
- Always matches `main` branch

### Production
- Deploy deliberately (human-triggered)
- Typical cadence: End of cycle
- Can deploy mid-cycle if needed (hotfixes, early wins)

**Never auto-deploy to production** (human gate)

---

## Communication

### Async by Default

**Agent doesn't need synchronous communication:**
- Pitch provides context
- PRs provide updates
- Comments provide feedback

**No daily standups needed.**

### When to Sync

**Rare situations:**
- Agent truly stuck (after 2 days)
- Major technical decision needed
- Scope needs re-shaping mid-cycle

**Default: async. Exception: sync.**

---

## End of Cycle

### Last Day Checklist

- [ ] All approved PRs merged
- [ ] Main branch passes all tests
- [ ] Staging deployed and tested
- [ ] Production deployed (if shipping)
- [ ] Incomplete work noted (for future shaping)

### What Ships

**Ships:**
- All completed scopes (merged to main)
- Tested and validated functionality

**Doesn't ship:**
- Incomplete scopes (PR still open)
- Unmerged code
- "90% done" work

**Binary: Merged or not merged. No "almost shipped."**

### Cool-down Starts

- 1-2 days
- Agent can: Fix small bugs, improve docs, refactor
- Human: Review cycle, prepare next betting table

---

## Anti-Patterns

### ❌ Review Bottleneck

**Problem:** 20 PRs waiting, human reviews 2/week

**Fix:** Bet less next cycle (match review capacity)

### ❌ Merge Without Review

**Problem:** "Agent is reliable, I'll just merge it"

**Why it fails:** Agents make confident mistakes

**Fix:** Always review (even if quick)

### ❌ Mega PRs

**Problem:** 3000-line PR with 5 scopes

**Why it fails:** Impossible to review properly

**Fix:** 1 scope = 1 PR (or split large scopes)

### ❌ WIP PRs

**Problem:** PR marked "Work in Progress" for 5 days

**Why it fails:** Not actually ready for review

**Fix:** Only open PR when scope is done

### ❌ Stale PRs

**Problem:** PR open for 7+ days, no review

**Why it fails:** Agent can't progress, work wasted

**Fix:** Review within 24-48 hours (or bet less next time)

---

**Next:** [Tools for AI-Native Shape Up](tools.md)

---

## Navigation

**← Previous:** [Betting](betting.md) (How to prioritize and commit)

**→ Next:** [Tools](tools.md) (Basecamp, GitHub setup)

**📚 Ready to Start?**
- [Create Your AI Programmer](agent-setup-guide.md) - Complete setup guide
- [Set Up Basecamp](basecamp-implementation.md) - Configure your workflow

**🎯 Back to:** [README](../README.md) (Table of contents)

---

## Repository Structure

Every repo using Shape Up AI Native **must always** have this structure:

```
.claude/
  commands/       → Custom Claude Code slash commands
  agents/         → Claude Code sub-agents
_specs/
  <feature-slug>/
    spec.md       → Gate 1 document (human approves before Plan)
_plans/
  <feature-slug>/
    plan.md       → Gate 2 document (human approves before code)
```

### Branch Naming Convention

| Branch | Purpose |
|---|---|
| `spec/<slug>` | PR opened for Spec review (Gate 1) |
| `plan/<slug>` | PR opened for Plan review (Gate 2) |
| `feature/<slug>` | PR opened for implementation review (Gate 3) |

### Why this structure?

- **`_specs/` and `_plans/`** — underscore prefix keeps them visually grouped and separate from source code. Easy to find, easy to audit.
- **`.claude/`** — standard location for Claude Code customizations. Consistent across all repos.
- **Branch naming** — makes the current gate immediately visible in GitHub's PR list.
