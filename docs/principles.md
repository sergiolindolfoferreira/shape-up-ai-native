# Principles of Shape Up AI Native

> What changes, what stays, and why it matters

---

## The Core Insight

**Shape Up (2019)** solved a fundamental problem: how to ship meaningful work predictably without burning out teams.

**Shape Up AI Native (2026)** adapts these solutions for a new reality: AI agents can build 10-100x faster than humans, but humans remain the bottleneck for validation, deployment, and strategic decisions.

---

## What Changes

### 1. **Cycle Length: 6 weeks → 1-2 weeks**

**Why it changed:**
- AI agents build features in days, not weeks
- Humans can't wait 6 weeks to validate
- Feedback loops need to be tighter
- Deployment cadence increases

**What it looks like:**
```
Old: 6 weeks building → 1 week cool-down → repeat
New: 1-2 weeks building → 1-2 days review → repeat
```

### 2. **Cool-down: 1 week → 1-2 days**

**Why it changed:**
- Agents don't need rest (they're not burning out)
- Human review time is the new constraint
- Betting decisions happen faster
- Bug fixes can't wait a full week

**What it looks like:**
- Day 1: Review what shipped, deploy final pieces
- Day 2: Bet on next cycle, shape upcoming work
- (Optional: Day 3-4 for complex review/fixes)

### 3. **Builder Role: Human → AI Agent + Human**

**Why it changed:**
- AI agents write code faster and more consistently
- Humans shift from writing to reviewing
- Integration happens continuously (not end-of-cycle)
- Pull Requests become the collaboration interface

**What it looks like:**
```
Old workflow:
  Designer + Programmer → build together → ship

New workflow:
  Shaper → defines pitch
  AI Agent → implements in branches
  Human → reviews PRs, tests, merges
  Human → deploys to production
```

### 4. **Integration: End-of-cycle → Continuous**

**Why it changed:**
- Can't wait 6 weeks to discover integration issues
- Agents can produce broken code quickly
- Need human feedback loop every 1-2 days
- PRs enable asynchronous review

**What it looks like:**
- Agent creates PR after each scope completes
- Human reviews when available (non-blocking)
- Merge approved work continuously
- Ship accumulated work at cycle end

---

## What Stays (and why it's more important!)

### 1. **Shaping Before Building** ⭐

**Why it's critical:**
- **Garbage in = garbage out at 100x speed**
- AI agents without context build the wrong thing efficiently
- Appetite prevents infinite perfectionism
- Boundaries prevent scope creep

**Shaping = 10 min of thinking that saves 10 hours of rework**

### 2. **Appetite, Not Estimates**

**Why it matters:**
- "How long will this take?" → still unknowable
- "How much time do we want to spend?" → still the right question
- Fixed time, variable scope → prevents endless polish
- Circuit breaker → kills runaway projects

**AI makes estimation worse, not better** (can build wrong thing very quickly)

### 3. **Betting, Not Backlogs**

**Why it's essential:**
- **Human review capacity is the bottleneck**
- Can't review 50 projects simultaneously
- Need to choose: 1 big bet or 3 small bets per cycle
- Important ideas come back (don't need infinite backlog)

**Agents don't eliminate prioritization—they amplify its importance**

### 4. **Circuit Breaker**

**Why it's crucial:**
- Agents can get stuck in rabbit holes too
- If it doesn't ship in one cycle → re-shape or kill
- Prevents sunk cost fallacy at AI speed
- Forces hard conversations about scope

**Default: cancel, not extend** (exactly like original Shape Up)

### 5. **Autonomy with Guardrails**

**Why it works:**
- Agents work best with clear context (the pitch)
- Don't micromanage (let agent discover how)
- Define scopes, not tasks
- Trust the process

**Agents are even more autonomous than humans** (don't need meetings!)

---

## New Principles for AI Age

### 1. **Review Budget = Cycle Capacity**

**Your bottleneck is review time, not build time.**

If you can review:
- 3 small features per week → that's your capacity
- 1 large feature per cycle → that's your capacity

**Don't bet more than you can review.**

### 2. **Continuous Integration is Mandatory**

**Can't wait until end of cycle to discover problems.**

- PRs should be small (1 scope = 1 PR)
- Review happens throughout cycle
- Merge continuously, ship at end
- Early integration catches issues early

### 3. **Shaping Quality > Quantity**

**One well-shaped project > three vague ideas.**

Agents amplify:
- Good shapes → great outcomes
- Bad shapes → expensive mistakes

**Invest time in shaping** (it's now the highest-leverage activity)

### 4. **Human Judgment is the Product**

**Agents build, humans decide.**

Non-negotiable human decisions:
- What problem to solve (betting)
- How much time to spend (appetite)
- When scope is "good enough" (scope hammering)
- Whether to ship (quality gate)

**Automate execution, not judgment.**

---

## Anti-Patterns to Avoid

### ❌ "Let the agent figure it out"

**Problem:** Vague shaping → wasted cycles

**Fix:** Shape properly (problem, appetite, solution, boundaries)

### ❌ "Review everything at the end"

**Problem:** Miss issues until too late

**Fix:** Continuous PRs, review throughout cycle

### ❌ "No appetite, just build until done"

**Problem:** Infinite scope creep at AI speed

**Fix:** Set appetite, enforce circuit breaker

### ❌ "Backlog of 100 agent tasks"

**Problem:** Can't review that much work

**Fix:** Bet on what fits review capacity

### ❌ "Trust the agent blindly"

**Problem:** Agents make confident mistakes

**Fix:** Always review, test, validate before deploy

---

## The Meta-Principle

**Shape Up was always about managing risk at fixed time horizons.**

AI changes the SPEED, not the PRINCIPLES:
- Risk of shipping nothing → still exists
- Risk of building wrong thing → amplified
- Risk of scope creep → much worse
- Risk of burnout → shifts from builders to reviewers

**Shape Up AI Native:** Same risk management, adapted for AI velocity.

---

## Summary Table

| Principle | Original | AI Native | Why |
|-----------|----------|-----------|-----|
| **Cycle length** | 6 weeks | 1-2 weeks | AI is faster |
| **Cool-down** | 1 week | 1-2 days | Less needed |
| **Shaping** | Critical | **MORE critical** | Garbage in/out |
| **Appetite** | Fixed time | Still fixed time | Prevents creep |
| **Betting** | Essential | **MORE essential** | Review bottleneck |
| **Circuit breaker** | Kill at 6wks | Kill at 1-2wks | Same logic, faster |
| **Autonomy** | Teams decide | Agents decide (more) | Less coordination |
| **Integration** | Late cycle | Continuous | Can't wait |
| **Review** | Implicit | **Explicit gate** | New bottleneck |

---

**Next:** [How to Shape Work for AI Agents](shaping.md)
