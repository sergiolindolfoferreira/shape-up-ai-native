# Implementing Shape Up AI Native in Basecamp

> Complete step-by-step guide to set up and use Shape Up AI Native in your Basecamp account

---

## Overview

This guide shows you how to implement the Shape Up AI Native methodology in Basecamp, from initial setup to shipping your first AI-built feature.

**What you'll create:**
- 🔨 Shaping project (private workspace)
- 📋 Pitches (Message Board)
- 🎲 Betting Table (Card Table with 5 columns)
- 🚀 Cycle Projects (one per cycle)
- ✅ Tracking system (To-Dos + Hill Chart)

**Time required:** ~30 minutes initial setup

---

## Part 1: Initial Setup

### Step 1: Create the Shaping Project

1. Go to your Basecamp home
2. Click **"Make a new project"**
3. Name: **"🔨 Shaping"**
4. Description: **"Shape work before betting"**
5. Set to **Private** (only shapers should access)
6. Add team members: Product leads, CTOs, senior devs

**Why private?** Shaping is where you explore ideas before committing. Keep it separated from client-facing work.

---

### Step 2: Configure the Shaping Project

Inside your new **🔨 Shaping** project:

#### Enable these tools:
- ✅ **Message Board** (rename to "Pitches")
- ✅ **Card Table** (rename to "Betting Table")
- ✅ **Docs & Files** (for templates and references)
- ✅ **Chat** (for quick shaping discussions)

#### Disable these tools:
- ❌ To-dos (not needed in shaping project)
- ❌ Schedule (not needed in shaping project)
- ❌ Automatic Check-ins (too formal for shaping)

![Basecamp Shaping Project Overview](../assets/basecamp-shaping-overview.jpg)

*Your Shaping project should look like this: Pitches, Betting Table, Docs & Files, Chat*

---

### Step 3: Set Up the Betting Table

Open the **Card Table** (now called "Betting Table") and create **5 columns**:

1. **To Shape** (default "Triage" column)
   - Raw ideas
   - Not ready for betting
   - Early-stage exploration

2. **Shaped** (new column - add it)
   - Color: Purple
   - Pitches ready to bet on
   - Complete problem + solution + boundaries

3. **In Progress** (new column - add it)
   - Color: Orange
   - Current cycle work
   - **Reuse every cycle** (don't create "Cycle 1", "Cycle 2")

4. **Done** (default column)
   - Shipped features
   - Completed and deployed
   - Historical record

5. **Not Now** (default column)
   - Declined pitches
   - Backburner ideas
   - Maybe revisit later

![Basecamp Betting Table](../assets/basecamp-betting-table.jpg)

*The Betting Table with all 5 columns and example cards*

**Important:** The "In Progress" column is reused every cycle. Don't create separate columns per cycle.

---

## Part 2: Shaping Work

### Step 4: Create Your First Pitch

Go to **Message Board → Pitches** and create a new message.

**Use this template:**

```markdown
# [Feature Name]

**Appetite:** [1-2 days | 1 week | 2 weeks]

---

## Problem

[Describe the user pain in 3-5 sentences]

- What's broken or missing?
- Who is affected?
- What's the real cost?

---

## Solution (Breadboard)

[Show the interface/flow at high level - use text sketch or diagram]

**Example:**
┌─────────────────────────────────┐
│  Dashboard                       │
├─────────────────────────────────┤
│  [Metric 1]  [Metric 2]         │
│  [Chart showing trend]           │
│  [Action button]                 │
└─────────────────────────────────┘

**Flow:**
1. User opens dashboard
2. Sees key metrics at a glance
3. Can drill down to details

---

## Rabbit Holes (what NOT to do)

❌ **Don't [technical risk 1]**
   → Instead, [simpler approach]

❌ **Don't [technical risk 2]**
   → Instead, [simpler approach]

---

## No-Gos (out of scope)

🚫 **[Feature/aspect 1]**
   - Why it's out: [reason]

🚫 **[Feature/aspect 2]**
   - Why it's out: [reason]

---

## Ready to Bet?

- [ ] Problem is clear and urgent
- [ ] Solution is rough but solvable
- [ ] Rabbit holes identified
- [ ] Boundaries well defined
- [ ] Appetite is realistic

**Next:** Move to Betting Table when ready.
```

**Real example:** [Performance Dashboard Pitch](../examples/performance-dashboard/pitch.md)

![Complete Pitch Example](../assets/basecamp-pitch-example.jpg)

*A well-shaped pitch with all 5 key elements*

---

### Step 5: Add Card to Betting Table

After creating the pitch:

1. Go to **Betting Table**
2. Click **"+ Add a card"** in the **"To Shape"** column
3. Title: Same as pitch name
4. Content: Short summary + link to full pitch
5. Click **Save**

**Example card content:**
```
Appetite: 1 week

View full pitch → [link]

Dashboard showing slow endpoints, error rates, and business impact.
```

---

## Part 3: Betting

### Step 6: Move Cards Through Workflow

**During shaping:**
- Card starts in **"To Shape"**
- As you shape it, update the pitch
- When pitch is complete → move to **"Shaped"**

**During betting meeting:**
- Review all cards in **"Shaped"**
- Decide: Bet on it or not?
- If YES → move to **"In Progress"**
- If NO → move to **"Not Now"**

**Workflow:**
```
Raw idea → To Shape
            ↓ (after shaping)
         Shaped
            ↓ (betting decision)
      In Progress  OR  Not Now
            ↓ (after shipping)
         Done
```

**Key principle:** Only bet on what you can review and deploy in one cycle.

---

### Step 7: Betting Meeting (Start of Cycle)

**When:** During cool-down (between cycles)

**Attendees:** Shapers + decision makers

**Agenda:**
1. Review **"Shaped"** column (10-15 min per pitch)
2. Discuss appetite vs. review capacity
3. Decide what to bet on
4. Move cards to **"In Progress"**
5. Archive cards to **"Not Now"** (with reason)

**For AI agents, ask:**
- ❓ Can we review ~X PRs this cycle?
- ❓ Is the pitch clear enough for autonomous work?
- ❓ Are rabbit holes identified?
- ❓ Do we have capacity to validate this?

---

## Part 4: Building (During Cycle)

### Step 8: Create Cycle Project

For each bet, create a **new project**:

**Naming:** `🚀 Cycle N - [Feature Name]`

**Examples:**
- `🚀 Cycle 1 - Performance Dashboard`
- `🚀 Cycle 2 - CSV Export`

**Inside the project:**

#### Enable these tools:
- ✅ **Message Board** (for kickoff post)
- ✅ **To-dos** (for scopes)
- ✅ **Hill Chart** (optional - for tracking progress)
- ✅ **Chat** (for quick updates)

#### Disable these:
- ❌ Schedule (not needed)
- ❌ Automatic Check-ins (too formal)
- ❌ Email Forwards (noise)

---

### Step 9: Post Kickoff Message

In the **Message Board**, create a kickoff post:

**Title:** `Cycle N: [Feature Name]`

**Template:**

```markdown
# 🚀 Cycle N: [Feature Name]

**Duration:** [Start date] - [End date]
**Appetite:** [X days/weeks]
**Builder:** [AI Agent name]
**Reviewer:** [Human name(s)]

---

## The Pitch

[Link to shaped pitch in Shaping project]

**TL;DR:** [2-3 sentence summary]

---

## Scopes (initial)

These will evolve during work:

1. **[Scope 1 name]**
   - [Brief description]

2. **[Scope 2 name]**
   - [Brief description]

3. **[Scope 3 name]**
   - [Brief description]

---

## How to Track

- **GitHub PRs:** [Link to repo/filter]
- **Basecamp To-Dos:** This project (lists below)
- **Hill Chart:** See above (as scopes emerge)

---

## Questions?

Post in Chat or comment here.
```

---

### Step 10: Create To-Do Lists (Scopes)

**Important:** Don't create all tasks upfront. Discover scopes as you work.

**When agent starts working:**
1. Agent identifies natural scopes (chunks of work)
2. Create **one To-Do list per scope**
3. Add high-level tasks (not micro-tasks)

**Example:**

**To-Do List 1: "Overview Card"**
```
[ ] API endpoint /api/performance/summary
[ ] Frontend component DashboardOverview
[ ] Tests passing
[ ] PR merged
```

**To-Do List 2: "Endpoints List"**
```
[ ] Backend: Aggregate endpoint metrics
[ ] Frontend: EndpointsList component
[ ] Color coding by threshold
[ ] Tests passing
[ ] PR merged
```

**To-Do List 3: "Business Impact"**
```
[ ] Calculate affected users
[ ] Revenue loss estimation
[ ] Trend chart component
[ ] Tests passing
[ ] PR merged
```

**Key:** Tasks are **outcomes**, not micro-steps. Agent decides HOW.

---

### Step 11: Track with Hill Chart (Optional)

If you enabled Hill Chart:

1. Add each **scope** as a point on the chart
2. **Uphill** (left side) = figuring it out, discovering unknowns
3. **Downhill** (right side) = executing, finishing up

**Move scopes across the hill as work progresses:**
- Start: Far left (unknown)
- Middle: Top of hill (figured out, starting execution)
- End: Far right (done, shipped)

**Update weekly or when scope changes significantly.**

---

## Part 5: Shipping

### Step 12: Complete the Cycle

**When cycle ends:**

1. ✅ Review what shipped
2. ✅ Deploy to production
3. ✅ Move card from **"In Progress"** → **"Done"** in Betting Table
4. ✅ Archive the Cycle Project
5. ✅ Start cool-down (1-2 days)

**If work didn't ship:**
- ⚠️ Respect the circuit breaker
- ⚠️ Move card to **"Not Now"** (note: needs re-shaping)
- ⚠️ Don't extend the cycle
- ⚠️ Learn why (scope too big? unclear pitch?)

---

### Step 13: Cool-Down

**Duration:** 1-2 days (with AI agents, less than traditional 1 week)

**Activities:**
- 🔄 Review last cycle (what went well, what didn't)
- 🧹 Cleanup work (refactoring, docs, small bugs)
- 🔨 Shape new pitches
- 🎲 Prepare for next betting meeting

**Don't:**
- ❌ Start new cycle work
- ❌ Let cycle work bleed into cool-down
- ❌ Skip cool-down (prevents burnout)

---

## Part 6: Ongoing Workflow

### Continuous Shaping

**While cycles run:**
- Keep adding raw ideas to **"To Shape"**
- Work on shaping during cool-down
- Aim to have 3-5 shaped pitches ready at all times

**Shaping cadence:**
- Spend ~20% of time shaping (one day per week)
- Rest of time: reviewing agent work

---

### Betting Cadence

**Frequency:** Start of every cycle (every 1-2 weeks)

**Meeting length:** 1-2 hours

**Attendees:** Stay small (2-4 people max)

**Output:** Clear commitment for next cycle

---

### Review Capacity Planning

**Calculate your capacity:**

```
Example:
- You can review 3 PRs/day
- Cycle = 5 working days
- Capacity = 15 PRs/cycle

Therefore: Bet on work that generates ~15 PRs
```

**Adjust appetite based on review capacity, not build speed.**

---

## Common Mistakes to Avoid

### ❌ Creating Cycle Columns in Betting Table

**Wrong:** "Cycle 1", "Cycle 2", "Cycle 3" columns

**Right:** Reuse "In Progress" for every cycle

**Why:** Keeps Betting Table clean, history lives in archived Cycle Projects

---

### ❌ Betting on Unshaped Work

**Wrong:** "Let's just start and figure it out"

**Right:** Shape first, then bet

**Why:** Agent will confidently build the wrong thing at 100x speed

---

### ❌ Creating All Tasks Upfront

**Wrong:** 50-item checklist before starting

**Right:** Discover scopes during work, add tasks as you go

**Why:** You don't know the real work until you start

---

### ❌ Extending Cycles

**Wrong:** "We're 80% done, let's take 3 more days"

**Right:** Respect the circuit breaker, re-shape if needed

**Why:** Teaches better shaping, prevents scope creep

---

### ❌ Skipping Cool-Down

**Wrong:** Start next cycle immediately

**Right:** Take 1-2 days to breathe, clean up, shape

**Why:** Prevents burnout, ensures quality shaping

---

## Summary Checklist

### Initial Setup (one-time)
- [ ] Created "🔨 Shaping" project (private)
- [ ] Set up Message Board "Pitches"
- [ ] Created Betting Table with 5 columns
- [ ] Added shapers to project

### Every Pitch
- [ ] Problem clearly defined
- [ ] Appetite set (1-2 days, 1 week, or 2 weeks)
- [ ] Solution roughed out (breadboard)
- [ ] Rabbit holes identified
- [ ] Boundaries defined (No-Gos)
- [ ] Card added to Betting Table

### Every Betting Meeting
- [ ] Reviewed shaped pitches
- [ ] Calculated review capacity
- [ ] Made betting decisions
- [ ] Moved cards to "In Progress" or "Not Now"

### Every Cycle
- [ ] Created Cycle Project
- [ ] Posted kickoff message
- [ ] Discovered scopes during work
- [ ] Created To-Do lists per scope
- [ ] Agent created PRs
- [ ] Human reviewed + merged PRs
- [ ] Shipped to production
- [ ] Moved card to "Done"
- [ ] Archived Cycle Project

### Every Cool-Down
- [ ] Reviewed last cycle
- [ ] Shaped new pitches
- [ ] Prepared for betting
- [ ] Cleanup work completed

---

## Next Steps

1. **Set up your Basecamp** following this guide
2. **Shape your first pitch** using the template
3. **Create your AI agent** following [Agent Setup Guide](agent-setup-guide.md)
4. **Run your first cycle** and iterate

---

## Additional Resources

- [Shaping Guide](shaping.md) - Deep dive into shaping
- [Betting Guide](betting.md) - How to bet when agents move fast
- [Building Guide](building.md) - Agent + human workflow
- [Complete Example](../examples/performance-dashboard/pitch.md) - Performance Dashboard

---

**Questions?** Open an issue or discussion in the repository.

**Using this successfully?** Share your experience as a case study!
