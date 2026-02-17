# Performance Dashboard - Complete Pitch Example

This is a complete, well-shaped pitch ready to hand to an AI agent.

**Live example:** [View in Basecamp](https://3.basecamp.com/5517580/buckets/46149894/messages/9588362237)

---

## 📋 Pitch: Performance Dashboard

**Appetite:** 1 week

---

## Problem

Users can't quickly identify where performance bottlenecks are in the application. When there's slowness, the support team receives complaints but has no immediate visibility into:

- Which endpoints are slow
- How many users were affected
- What the real business impact is

Currently, they need to access multiple tools (logs, APM, analytics) to piece together the puzzle. This wastes time and delays resolution of critical problems.

---

## Solution (Breadboard)

### Interface principal:

```
┌─────────────────────────────────────────┐
│  Performance Dashboard                   │
├─────────────────────────────────────────┤
│                                          │
│  [Last 24h] [7 days] [30 days]          │
│                                          │
│  ┌─────────────────────────────────┐    │
│  │ Slowest Endpoints               │    │
│  │                                 │    │
│  │ GET /api/properties  2.3s 🔴    │    │
│  │ POST /api/search     1.8s 🟡    │    │
│  │ GET /api/users       0.8s 🟢    │    │
│  └─────────────────────────────────┘    │
│                                          │
│  ┌─────────────────────────────────┐    │
│  │ Error Rate Trend                │    │
│  │        ▁▂▃█▆▄▃▂▁                │    │
│  │ 0.5%  ─────────────  2.1%       │    │
│  └─────────────────────────────────┘    │
│                                          │
│  Affected Users: 247                    │
│  Business Impact: €1,240 lost revenue   │
└─────────────────────────────────────────┘
```

### Flow:

1. Dashboard automatically loads data from last 24h
2. User can change period (7d, 30d)
3. Clicking an endpoint shows details (stack traces, examples)
4. Automatic alert when threshold exceeded

### Backend:

- Leverage existing APM data
- New API endpoint: `GET /api/performance/summary`
- 5-minute cache (doesn't need real-time)
- Simple aggregation (average, p95, count)

---

## Rabbit Holes (what NOT to do)

❌ **Don't build metrics system from scratch**
   → Use data we already have in APM/existing logs

❌ **Don't create complete alerting system**
   → Just show data, alerting comes later

❌ **Don't do real-time updates**
   → Manual refresh or 5min cache is sufficient

❌ **Don't optimize performance queries**
   → That's a separate project, here we just show what we have

---

## No-Gos (out of scope)

🚫 **History > 30 days**
   - Old data isn't critical for immediate troubleshooting
   - Adds storage complexity

🚫 **Drill-down to individual logs**
   - Dashboard is overview, not deep debugging tool
   - Links to existing tools (Sentry, DataDog) are enough

🚫 **Environment comparison**
   - Focus on production only
   - Staging/dev aren't priority for this cycle

🚫 **Per-user threshold customization**
   - Fixed values for entire team
   - Advanced configuration waits for V2

---

## Ready to Bet?

✅ Problem is clear and urgent  
✅ Solution is rough but solvable  
✅ Rabbit holes identified and addressed  
✅ Boundaries well defined  
✅ 1 week appetite is realistic  

**Next step:** Bet on next cycle and start working!

---

## Why This Works for AI Agents

This pitch demonstrates key properties for autonomous agent execution:

### 1. Clear Problem → No Feature Creep

The agent knows exactly what user pain we're solving. Won't add "nice to have" features that bloat scope.

### 2. Fixed Appetite → No Infinite Optimization

Agent has 1 week. Won't spend 3 days bikeshedding color schemes or optimizing queries that work fine.

### 3. Rough Solution → Room for Implementation

Breadboard shows structure, not pixel-perfect mockups. Agent can make implementation decisions within boundaries.

### 4. Rabbit Holes → Avoids Technical Dead-Ends

"Don't build metrics from scratch" prevents agent from attempting complex infrastructure that's out of scope.

### 5. No-Gos → Clear Boundaries

Agent knows what's explicitly out. Won't implement 90-day history or per-user settings thinking they're required.

---

## How to Use This Template

When shaping your own pitch:

1. **Copy this structure** (Problem, Solution, Rabbit Holes, No-Gos)
2. **Replace content** with your specific feature
3. **Set appetite** (1-2 days, 1 week, or 2 weeks max)
4. **Draw breadboard** (ASCII art or hand sketch)
5. **Identify risks** (what could derail this?)
6. **Define boundaries** (what's explicitly out?)
7. **Verify checklist** (see [Shaping Checklist](../../docs/shaping.md#shaping-checklist))

**Then:** Post in Basecamp, bet on it, assign to agent, ship it! 🚀

---

**See also:**
- [Shaping Guide](../../docs/shaping.md)
- [Betting Guide](../../docs/betting.md)
- [Building Guide](../../docs/building.md)
