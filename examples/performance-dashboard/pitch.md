# Performance Dashboard for Property Listings

**Appetite:** 1 week (5 days)

---

## Problem

**Current situation:**
Property managers with 20+ listings can't easily see which properties are performing well. They have to:
1. Open each listing individually
2. Check views, inquiries, bookings manually
3. Compare mentally or on spreadsheet
4. Takes 30-45 minutes daily

**Specific problem:**
No overview of performance across all listings. Can't quickly identify:
- Which listings need attention (low inquiries)
- Which are performing well (high conversion)
- Overall portfolio health

**Affected users:**
- Property managers with 20+ active listings
- ~15% of our user base
- But 40% of revenue (they're power users)

**Why now:**
- #1 requested feature (23 requests this quarter)
- Competitive pressure (competitors have this)
- We're losing larger customers to competitors because of this gap

---

## Appetite

**Time budget:** 1 week (5 working days)

This is NOT an estimate. We're willing to spend 5 days on this problem.

**Trade-offs accepted:**
- If scope creeps → cut features to fit 5 days
- If something is technically hard → simplify or cut
- We can always add more in v2

---

## Solution

### High-Level Approach

Add a new "Performance" tab to the dashboard that shows:

1. **Overview Card** at top
   - Total views, inquiries, bookings (last 7 days)
   - Aggregate conversion rate
   - Clear, big numbers

2. **Listings Table** below
   - One row per listing
   - Columns: Name, Photo thumb, Views, Inquiries, Bookings, Conversion %
   - Sort by any column
   - Click row → go to listing detail

**Data freshness:** Daily aggregation is fine (not real-time)

### Breadboard

```
┌─────────────────────────────────────────┐
│ Navigation: [ Dashboard | Listings |   │
│             [ Calendar | Performance ]  │ ← New tab
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│ PERFORMANCE DASHBOARD                   │
├─────────────────────────────────────────┤
│                                         │
│  ╔═══════════════════════════════════╗ │
│  ║  OVERVIEW (Last 7 Days)           ║ │
│  ║                                   ║ │
│  ║  2,450 views  │ 87 inquiries     ║ │
│  ║  12 bookings  │ 13.8% conversion ║ │
│  ╚═══════════════════════════════════╝ │
│                                         │
│  ╔═══════════════════════════════════╗ │
│  ║  TOP PERFORMING LISTINGS          ║ │
│  ╠═══════╦═══════╦══════════╦════════╣ │
│  ║ Name  ║ Views ║ Inq. ║ Conv. %  ║ │
│  ╠═══════╬═══════╬══════════╬════════╣ │
│  ║ Villa ║  450  ║   23   ║  18.2% ║ │
│  ║ Apt B ║  380  ║   12   ║  11.5% ║ │
│  ║ ...   ║  ...  ║  ...   ║  ...   ║ │
│  ╚═══════╩═══════╩══════════╩════════╝ │
│                                         │
│  [Sort by: Views ▼] [Last 7 days ▼]   │
└─────────────────────────────────────────┘

Click listing row → Existing listing detail page
```

### Key Elements

1. **Performance Tab**
   - New navigation item
   - Visible to all users
   - Shows overview + table

2. **Overview Card**
   - 4 key metrics (views, inquiries, bookings, conversion %)
   - Always last 7 days for v1
   - Big, clear numbers

3. **Listings Table**
   - One row per active listing
   - Sortable columns (default: highest conversion first)
   - Click row → navigate to listing detail
   - Shows listing photo thumbnail

4. **Data Pipeline**
   - Daily aggregation job (runs at 2 AM)
   - Stores results in `listing_stats` table
   - No real-time queries (too expensive)

---

## Rabbit Holes

### Technical Unknowns

- **Performance data location**
  - Analytics in MongoDB (page_views collection)
  - Listings in PostgreSQL (listings table)
  - **Mitigation:** Daily aggregation job copies data to PostgreSQL
    - Agent can implement this (we have similar jobs already)

- **Conversion rate calculation**
  - Inquiries → bookings is not 1:1 (inquiry might lead to booking weeks later)
  - **Mitigation:** Simple approach for v1: bookings / inquiries in same 7-day window
    - Not perfect but "good enough" for dashboard overview

### Scope Creep Risks

- **Custom date ranges:** Users will want "last 30 days", "this month", etc.
  - **Decision:** v1 = hard-coded "last 7 days" only
  - Add date picker in v2 if needed

- **Comparison periods:** "Compare to previous week"
  - **Decision:** OUT for v1 (adds complexity)

- **Export to CSV/Excel:**
  - **Decision:** OUT for v1 (can be v2 feature)

- **Filtering by property type, location, etc.:**
  - **Decision:** v1 = just sorting, no filters

---

## No Gos

**Explicitly OUT of scope for this cycle:**

- ❌ Custom date ranges (just last 7 days)
- ❌ Comparison to previous periods
- ❌ Export functionality (CSV, PDF, Excel)
- ❌ Filtering by property attributes
- ❌ Graphs/charts (just numbers for v1)
- ❌ Email notifications ("Your listing X is underperforming")
- ❌ Real-time data (daily aggregation is fine)
- ❌ Drill-down analytics (just high-level for now)

**If users ask:** Politely acknowledge and note for future consideration.

---

## Success Criteria

**This feature is successful if:**

- [ ] Property manager can see overview metrics in <2 clicks from homepage
- [ ] Table loads in <2 seconds with 100 listings
- [ ] Sorting works on all columns
- [ ] Clicking listing row navigates to detail page
- [ ] Data is accurate (matches detail pages)
- [ ] Works on mobile (responsive layout)

**Metric to track post-launch:**
- % of power users (20+ listings) who use Performance tab weekly
- Target: >50% within 2 weeks of launch

---

## Data/Integration Notes

### Data Flow

```
1. Raw analytics data
   Location: MongoDB (analytics_db.page_views)
   Fields: listing_id, viewed_at, user_id

2. Inquiry data
   Location: PostgreSQL (inquiries table)
   Fields: listing_id, created_at, status

3. Booking data
   Location: PostgreSQL (bookings table)
   Fields: listing_id, booked_at, inquiry_id

4. Daily aggregation (runs 2 AM UTC)
   → Creates/updates: PostgreSQL (listing_stats table)
   Schema:
     - listing_id (FK to listings)
     - date (DATE)
     - views (INT)
     - inquiries (INT)
     - bookings (INT)
     - conversion_rate (DECIMAL)

5. Dashboard queries
   → Only read from listing_stats (fast!)
   → JOIN with listings for name, photo
```

### Example API Response

**GET /api/dashboard/performance**

```json
{
  "overview": {
    "period": "last_7_days",
    "startDate": "2026-02-10",
    "endDate": "2026-02-16",
    "totalViews": 2450,
    "totalInquiries": 87,
    "totalBookings": 12,
    "conversionRate": 0.138
  },
  "listings": [
    {
      "id": 123,
      "name": "Villa Marina",
      "photoThumbUrl": "https://cdn.../thumb.jpg",
      "views": 450,
      "inquiries": 23,
      "bookings": 5,
      "conversionRate": 0.182
    },
    {
      "id": 124,
      "name": "Cozy Apartment Downtown",
      "photoThumbUrl": "https://cdn.../thumb.jpg",
      "views": 380,
      "inquiries": 12,
      "bookings": 2,
      "conversionRate": 0.115
    }
  ]
}
```

### Integration Points

**Frontend:**
- New route: `/dashboard/performance`
- Uses existing auth guard (must be logged in)
- Uses existing layout component
- Add tab to main navigation

**Backend:**
- New controller: `PerformanceDashboardController`
- New route: `GET /api/dashboard/performance`
- Reuses existing auth middleware
- New background job: `DailyStatsAggregationJob` (runs via cron)

**Database:**
- New table: `listing_stats`
- Indexes needed: `(listing_id, date)`, `(date)`

### DO NOT Over-Engineer

**Agent should NOT:**
- Add caching layer (not needed yet, data changes daily)
- Implement real-time updates (daily is fine)
- Add GraphQL endpoint (REST is fine)
- Create separate admin panel (reuse existing dashboard)
- Optimize prematurely (query is simple JOIN, will be fast)

---

## Open Questions

- [x] **Where does inquiry → booking attribution happen?**
  - Answer: Same 7-day window for v1 (simple approach)

- [x] **What if listing has zero views?**
  - Answer: Still show in table, conversion = 0% (or N/A)

- [x] **Mobile layout?**
  - Answer: Stack cards vertically, table becomes scrollable

- [x] **Loading state?**
  - Answer: Skeleton loaders (use existing component)

**All critical questions resolved → ready to bet.**

---

## Ready to Bet?

- [x] Problem is clear and specific (property managers, 30 min daily)
- [x] Appetite is set (1 week / 5 days)
- [x] Solution is rough but solved (overview + table)
- [x] Rabbit holes identified and mitigated (aggregation job, simple conversion calc)
- [x] Boundaries defined (No Gos listed above)
- [x] Success criteria are measurable (load time, usage %)
- [x] Technical integration is clear (data flow diagram)
- [x] No blocking open questions (all resolved)

**✅ READY FOR BETTING TABLE**

---

## Notes

**Design reference:**
- Similar to existing "Analytics" tab (consistent UI)
- Use existing table component (don't rebuild)
- Use existing card component for overview

**Post-launch ideas** (not for this cycle):
- Custom date ranges (v2)
- Comparison mode (v2)
- Export functionality (v2)
- Performance alerts ("Listing X dropped 50% in inquiries")
- Graphs/trends over time

---

_Shaped by: Sergio & Vasco | Date: 2026-02-17_
