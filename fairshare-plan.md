Here's the plan content. You can copy it and paste into a text editor (Notepad, Word, etc.) and save it to your desktop:

---

# Fairshare App Improvement Plan

## Context

Fairshare is a household cost-splitting app for two people living together. It's currently a single `index.html` file (416 lines) using vanilla JS and localStorage — no backend, no auth, no cloud sync. The UI is Swedish-only, dark mode, with partial PWA support. The user wants to: (1) make it maximally useful for couples, (2) ship it on iOS and Android, and (3) introduce free/paid tiers.

**Current file:** `/home/user/Fairshare/index.html` (all CSS, HTML, JS in one file)

---

## A. Feature Improvements (Prioritized)

### Tier 1 — High Impact (Build First)

1. **Settlement tracking ("Who owes who" + mark as paid)** — The app calculates amounts but doesn't track whether the transfer happened. Add a "Mark as settled" button with settlement history. This turns it from a calculator into a workflow tool.

2. **Expense editing** — Currently expenses can only be added or deleted. Users who mistype must delete and re-add. Add inline edit on each expense item.

3. **Per-expense split overrides** — Currently all expenses use the global income ratio. Some expenses are genuinely 50/50 or 100% one person. Add per-expense split type: "use ratio" (default), "50/50", "custom %", or "only Person X".

4. **Budget targets per category** — The 10 categories already have breakdown charts but no limits. Allow monthly budget targets per category with progress bars and warnings.

5. **Multi-month trends** — Historical data exists in `S.expenses` keyed by month, but only one month is viewable at a time. Add a trends view: total spending, per-category, and ratio changes over 6-12 months.

### Tier 2 — Strong Value-Add

6. **Receipt photo capture** — Attach a photo when adding an expense. Store compressed base64 (local) or upload to cloud storage (after backend). Drives daily usage. ✅ **(Just implemented)**

7. **Recurring expense dashboard** — Fixed expenses auto-copy monthly but there's no central view to manage, edit, or pause them.

8. **Reminders & notifications** — Monthly nudge to review expenses, settle up, or check the overview. Requires push notifications.

9. **CSV/bank import** — Swedish banks export CSV. Bulk-import would reduce manual entry dramatically.

10. **English language support (i18n)** — All strings are hardcoded Swedish. i18n unlocks Norway, Denmark, Finland, and international markets.

### Tier 3 — Nice-to-Have

11. Dark/light mode toggle
12. Custom expense categories
13. Notes/comments on expenses
14. Savings goals as a couple

---

## B. Free vs Paid Tier Structure

### Free Tier — "FairShare Bas"
- Two-person expense splitting with income-based ratio
- Swedish tax calculation
- 10 default categories
- Fixed/recurring expenses
- Month navigation (last 3 months of history)
- Basic overview with category breakdown
- Export as PNG image
- QR code settings transfer
- Guided tour / help
- Settlement tracking (current month only)

### Paid Tier — "FairShare Plus" (39 kr/month or 349 kr/year)

Price rationale: In line with Swedish consumer app pricing. 349 kr/year (~29 kr/month) provides a meaningful annual discount. Comparable to YNAB pricing adjusted for the Nordic market.

- **Unlimited history** (free: 3 months)
- **Multi-month trends and year summary** with charts
- **Budget targets per category** with alerts
- **Receipt photo capture**
- **Detailed HTML report export** (free: image only)
- **CSV bank import**
- **Per-expense split overrides**
- **Cloud sync between devices**
- **Custom categories**
- **Full settlement history**
- **Recurring expense dashboard**

### Optional: Family plan at 59 kr/month
Both partners get their own login with shared household data — removes friction of one person paying.

---

## C. Mobile App Strategy

### Recommended: Capacitor (not React Native / Flutter)

**Why Capacitor:**
- The app is already a functional web app — Capacitor wraps it into native iOS/Android shells with zero rewrite
- React Native or Flutter would require a complete rewrite (expensive for a solo dev)
- Capacitor gives native API access: camera, push notifications, biometric auth, in-app purchases
- The app continues working as a PWA for web users simultaneously

**What NOT to do:** Don't rewrite in React Native or Flutter. The ROI doesn't justify it at this stage.

---

## D. Architecture Migration Path

### Phase 1: Split the Monolith (week 1-2)

Initialize a Vite project and decompose `index.html` into modules

### Phase 2: PWA Completion (week 2)
- Add `manifest.json` (name, icons, theme, start_url)
- Add service worker for offline caching

### Phase 3: High-Priority Features (week 3-5)
- Settlement tracking with "mark as paid"
- Expense editing
- Budget targets per category
- Per-expense split overrides

### Phase 4: Backend with Supabase (week 5-8)

**Why Supabase:** PostgreSQL, built-in auth + storage + realtime, generous free tier, row-level security, no infra management.

### Phase 5: Capacitor Wrapper (week 8-9)
- Add Capacitor with iOS and Android platforms
- Add plugins for camera, notifications

### Phase 6: Paid Tier Infrastructure (week 9-10)
- RevenueCat for subscription management
- Stripe for web subscriptions

### Phase 7: Remaining Features (week 10+)
- Receipt OCR, CSV import, trends view, i18n
- Submit to App Store and Google Play

---

## Summary

The core strategy is: **refactor the monolith → add high-value features → add backend for sync/auth → wrap with Capacitor for native apps → monetize with subscriptions**. This avoids an expensive rewrite while systematically building toward a real product.