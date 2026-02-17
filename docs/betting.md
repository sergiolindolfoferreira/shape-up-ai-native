# Betting When Agents Move Fast

> How to prioritize and commit when build speed isn't the bottleneck

---

## The New Bottleneck

**Shape Up (2019):** Build capacity was the constraint.
→ Bet on what team can build in 6 weeks.

**Shape Up AI Native (2026):** Review capacity is the constraint.
→ Bet on what you can review, test, and deploy.

**Your capacity = how much work you can validate, not build.**

---

## The Betting Table

**When:** At the start of each cycle (during cool-down)

**Who:** Shapers + decision makers (usually founders, CTOs, product leads)

**Duration:** 1-2 hours

**Input:** Shaped pitches ready to bet on

**Output:** Commitment for next cycle

---

## What's Different with AI

### 1. **Capacity = Review Time**

Calculate your review capacity:

```
Example team:
- 1 senior dev (can review 3 PRs/day)
- 1 cycle = 5 working days
- Capacity = 15 PRs/cycle

Therefore:
- Bet on work that generates ~15 PRs
- Or: 3 medium features (5 PRs each)
- Or: 1 large feature (15 PRs)
```

**Don't bet more than you can review.**

### 2. **Parallel Work is Possible**

AI agents can work simultaneously:
- Agent A on Feature X
- Agent B on Feature Y
- Agent C on Bug fixes

**But:** You still review serially.

**Implication:** Parallelism helps if review is non-blocking.

### 3. **Smaller Bets = More Bets**

Old: 1 team, 1 big bet per 6 weeks

New: 1 reviewer, 3-5 small bets per 1-2 weeks

**More shots on goal** (but same total capacity)

---

## Betting Process

### Step 1: Review Shaped Pitches (30 min)

For each pitch, ask:

#### **Does the problem matter?**
- Will this move the needle?
- Is this the right time?
- What happens if we don't build it?

#### **Is the appetite right?**
- Too ambitious? (cut scope or split)
- Too cautious? (could we do more?)

#### **Is the solution attractive?**
- Does it solve the problem elegantly?
- Are there obvious rabbit holes?
- Will users understand it?

#### **Can we review it?**
- How many PRs will this generate?
- Do we have review capacity?
- Who reviews (domain knowledge needed)?

#### **Are dependencies clear?**
- Does it require other work first?
- Are integrations specified?
- Any external blockers?

### Step 2: Choose the Bets (20 min)

Options for a 1-week cycle:

**Option A: One Big Bet**
- 1 feature consuming full review capacity
- Deep, complex work
- Example: New payment integration

**Option B: Mix of Medium + Small**
- 2 medium features + 2 small improvements
- Diversified risk
- Example: Dashboard (5 days) + 2 bug fixes (1 day each)

**Option C: Many Small Bets**
- 5-10 small improvements/fixes
- High velocity, low risk
- Example: Polish cycle before launch

**Don't overthink it.** Choose what fits capacity.

### Step 3: Commit (5 min)

For each chosen bet:
- Assign to agent (or human + agent pair)
- Post kickoff message (pitch + scope)
- **Commit: No interruptions for cycle duration**

### Step 4: Decline the Rest (5 min)

Pitches not chosen:
- **No backlog!** (Shape Up principle)
- Communicate decision (with rationale)
- Important ideas will come back

**"We're not doing this now" ≠ "We'll never do this"**

---

## Special Situations

### Production Mode (Stable Product)

**Context:** Mature product, known architecture, predictable work

**Betting:**
- Standard 1-2 week cycles
- Mix of features + improvements
- Occasional bug fix cycles

**Example:**
```
Cycle 12 (1 week):
- Performance dashboard (big)
- Fix 3 reported bugs (small)
```

### R&D Mode (New Product)

**Context:** Early product, architecture uncertain, many unknowns

**Betting:**
- Shorter cycles (3-5 days)
- Spike work to reduce unknowns
- More hands-on human involvement

**Example:**
```
Cycle 1 (3 days):
- Spike: Authentication approach
- Outcome: Decision document for future work
```

### Cleanup Mode (Pre-Launch)

**Context:** Feature complete, need polish before launch

**Betting:**
- Many small bets (15-30 items)
- Focus on bugs, UI polish, edge cases
- No new features

**Example:**
```
Cycle Pre-Launch (1 week):
- 10 UI polish items
- 8 bug fixes
- 5 copy improvements
```

---

## Betting Anti-Patterns

### ❌ Betting More Than Review Capacity

**Problem:** Agent builds 10 features, you review 2, other 8 sit waiting

**Fix:** Calculate review capacity first, bet accordingly

### ❌ No Appetite on Bets

**Problem:** "Build dashboard (no deadline)"

**Why it fails:** Scope creeps indefinitely

**Fix:** Every bet has explicit appetite (1-2 days, 1 week, 2 weeks max)

### ❌ Hidden Backlog

**Problem:** "Betting table" is just rubber-stamping a backlog

**Why it fails:** Not actually choosing, just executing a queue

**Fix:** Each cycle: fresh consideration, real choices, decline some pitches

### ❌ Interrupting Cycles

**Problem:** "This urgent thing just came up..."

**Why it fails:** Breaks commitment, nothing ships

**Fix:** Real emergencies only (production down). Everything else waits.

### ❌ Extending by Default

**Problem:** "It's 90% done, just one more day..."

**Why it fails:** Circuit breaker doesn't work, sunk cost fallacy

**Fix:** Default is cancel, not extend. Re-shape for next cycle if still important.

---

## The Circuit Breaker

**Rule:** If work doesn't ship in its appetite, it's cancelled by default.

**Why this matters with AI:**
- Agents can build indefinitely (no natural stopping point)
- "Just needs more polish" can go on forever
- Sunk cost fallacy is worse when agent built quickly

**When to extend (rare):**
1. Scope was shaped correctly
2. Agent delivered 90%+ of value
3. Remaining work is <20% of original appetite
4. Team explicitly agrees to extension

**When to cancel (default):**
- Anything else
- Then: Re-shape for future cycle if still important

**Example:**
```
Bet: Performance dashboard (1 week)
Day 7: Agent built 60% (overview card done, table incomplete)

Decision: CANCEL
- What shipped (overview card): Keep it
- What didn't (table): Cancel or re-shape as new 3-day bet
```

---

## Betting Template

Use this at betting table:

```markdown
## Cycle N Bets

**Cycle duration:** 1 week (Feb 24-28)
**Review capacity:** 15 PRs
**Reviewer:** Sergio

### Chosen Bets

1. **Performance Dashboard** (big bet, 1 week)
   - Appetite: 5 days
   - Estimated PRs: 8-10
   - Agent: Vasco
   - Reviewer: Sergio
   - Kickoff: [link to pitch]

2. **Fix mobile menu bugs** (small bet, 1 day)
   - Appetite: 1 day
   - Estimated PRs: 2-3
   - Agent: Vasco
   - Reviewer: Sergio
   - Kickoff: [link to issue list]

**Total estimated PRs:** 10-13 (within capacity)

### Declined This Cycle

- Export to Excel feature (good idea, not now)
- Advanced filtering (wait for user feedback first)
- Email notifications (v2 feature)
```

---

## Measuring Success

Track over time:

### Cycle Completion Rate
**Goal:** >80% of bets ship

If <80%: 
- Shaping quality issue? (rabbit holes missed)
- Appetite too tight? (need more time)
- Review bottleneck? (need more review capacity)

### Review Turnaround Time
**Goal:** PRs reviewed within 1 day

If slower:
- Too many bets (over capacity)
- PRs too large (break into smaller)
- Reviewer availability issue

### Circuit Breaker Rate
**Goal:** <20% of bets hit circuit breaker

If >20%:
- Shaping quality poor
- Appetites unrealistic
- Agent capability mismatch

---

## Betting Checklist

Before committing to a cycle:

- [ ] Each bet has clear appetite (days/weeks)
- [ ] Total estimated PRs ≤ review capacity
- [ ] Reviewer assigned and available
- [ ] Kickoff materials ready (pitches linked)
- [ ] Declined pitches communicated
- [ ] No hidden "will fit in if time" items
- [ ] Circuit breaker rule understood by all

**If any fails:** Re-evaluate the bets.

---

**Next:** [Building with AI Agents](building.md)
