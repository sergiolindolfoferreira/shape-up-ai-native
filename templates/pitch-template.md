# [Feature Name]

**Appetite:** [1-2 days / 1 week / 2 weeks]

---

## Problem

**Current situation:**
[Describe what users do today, what's painful]

**Specific problem:**
[The exact problem we're solving, for whom]

**Why now:**
[Why this matters, what's the cost of not solving it]

---

## Appetite

**Time budget:** [X days/weeks]

**This is NOT an estimate.** It's how much time we're willing to spend on this problem.

**Trade-offs accepted:**
- If it takes longer → we cut scope or cancel
- If it takes less → great, we ship early

---

## Solution

### High-Level Approach

[Describe the solution in 2-3 paragraphs. What does it look like? How does it work?]

### Breadboard (or Fat Marker Sketch)

```
[Draw ASCII breadboard or describe UI flow]

Example:
[ Main Screen ]
   |
   +-- [ Action Button ] --> [ New Screen ]
   |                             |
   |                             +-- [ Form Fields ]
   |                             +-- [ Save Button ] --> back to Main
   |
   +-- [ List View ]
         - Item 1
         - Item 2
```

Or link to sketch/whiteboard photo.

### Key Elements

1. **[Element 1]:** What it does
2. **[Element 2]:** What it does  
3. **[Element 3]:** What it does

---

## Rabbit Holes

**Potential risks we've identified:**

### Technical Unknowns

- **[Risk 1]:** [Description]
  - **Mitigation:** [How we'll handle it]

- **[Risk 2]:** [Description]
  - **Mitigation:** [How we'll handle it]

### Scope Creep Risks

- Watch out for: [Feature X that users might ask for]
  - **Decision:** Out of scope for v1

---

## No Gos

**Explicitly OUT of scope for this cycle:**

- ❌ [Feature A] — Nice to have, but not essential
- ❌ [Feature B] — Too complex, future version
- ❌ [Feature C] — Different use case
- ❌ [Feature D] — Low user demand

**If users ask for these:** Politely defer to future consideration.

---

## Success Criteria

**What does "done" look like?**

- [ ] [Specific, measurable outcome 1]
- [ ] [Specific, measurable outcome 2]
- [ ] [Specific, measurable outcome 3]

**NOT:** "Users are happy" (too vague)
**YES:** "Users can export data in <2 clicks, works for 100+ records"

---

## Data/Integration Notes

(For AI agents - be specific)

**Data sources:**
- [Where data lives - database, API, etc.]

**Data transformations:**
- [What needs to happen to data]

**Integration points:**
- [What existing code this touches]
- [APIs to call]
- [Dependencies]

**Example API response:**
```json
{
  "field1": "value",
  "field2": 123
}
```

---

## Open Questions

(Answer these before betting)

- [ ] **Question 1:** [Description]
  - Answer: [Resolved or "still investigating"]

- [ ] **Question 2:** [Description]
  - Answer: [Resolved or "still investigating"]

**If any critical questions remain → keep shaping, don't bet yet.**

---

## Ready to Bet?

- [ ] Problem is clear and specific
- [ ] Appetite is set (days/weeks)
- [ ] Solution is rough but solved
- [ ] Rabbit holes identified and mitigated
- [ ] Boundaries defined (No Gos listed)
- [ ] Success criteria are measurable
- [ ] Technical integration is clear
- [ ] No blocking open questions

**If all checked → ready for betting table.**

---

## Notes

[Any additional context, references, or decisions made during shaping]
