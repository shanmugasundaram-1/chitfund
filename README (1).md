# Sri Lakshmi Chit Funds — Digital Chit Management Platform

A role-based web application for running a chit fund business end to end: chit groups, subscribers, auctions, collections, dividends, prize payments, ledger and reports, all in one dashboard. It also includes a separate customer portal where subscribers can view their own chit details.

The UI is fully bilingual (**English / தமிழ்**) and works in a **demo mode**: all data comes from built-in mock data, so no backend setup is needed to try it out.

---

## Features

### Admin / Staff Panel
- **Dashboard**: KPIs, collection overview, outstanding ageing, and alerts
- **Chit Groups**: chit value, members, duration, commission %, auction method, grace period and penalty rate
- **Subscribers**: KYC status, ticket number, nominee, prize status, outstanding and future liability
- **Auctions**: open auction and sealed bid, bid entry, winner selection, approval workflow
- **Collections & Payments**: cash, UPI, bank transfer, cheque and auto-debit, with receipts
- **Dividends & Prize Payments**: dividend pool distribution and prize approval flow
- **Outstanding Tracking**: overdue members with penalty calculation
- **Ledger & Reconciliation**: transaction types include subscription, dividend, prize payment, commission, penalty, adjustment, reversal and refund
- **Reports**
- **Chit Calculator**: monthly contribution, prize amount, commission, dividend, future liability, net cash flow, plus **IRR / XIRR**
- **Branches, Staff & Roles, Documents, Notifications, Audit Trail, Settings**

### Customer Portal
- My Chit (summary, prize and dividend details)
- Payment history and receipts
- Auction details
- Account statement

---

## User Roles

| Role | Access |
|---|---|
| Super Admin | All modules, branches, staff and settings |
| Branch Manager | Branch dashboard, subscribers, collections, auctions, approvals |
| Accountant | Payments, ledger, reconciliation, financial reports |
| Collection Staff | Assigned subscribers, collections, outstanding tracking |
| Auction Staff | Auctions, eligible members, bids and results |
| Customer | Own chit, payments, receipts, auction status, statements |

Menu items are shown or hidden based on the selected role (see `src/config/navigation.ts`).

---

## Tech Stack

- **React 18** + **TypeScript**
- **Vite 5**
- **Tailwind CSS 3**
- **lucide-react** (icons)
- Custom i18n layer (English and Tamil)
- React Context for language and role state, persisted in `localStorage`

> `@supabase/supabase-js` is listed in `package.json` but is **not used yet**. The app currently runs on mock data only.

---

## Getting Started

**Prerequisites:** Node.js 18+ and npm

```bash
# 1. Install dependencies
npm install

# 2. Start the dev server
npm run dev

# 3. Open the URL shown in the terminal (usually http://localhost:5173)
```

On the login screen, pick any role to enter demo mode. No username or password is required.

### Other Scripts

| Command | Description |
|---|---|
| `npm run build` | Production build into `dist/` |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | TypeScript type check |

---

## Project Structure

```
src/
├── App.tsx                  # Page routing by role
├── main.tsx
├── components/
│   ├── Layout.tsx           # Admin layout, sidebar, top bar
│   ├── Table.tsx            # Table and KPI card components
│   └── ui.tsx               # Shared UI (Card, Button, Modal, Input, StatusBadge...)
├── config/
│   └── navigation.ts        # Sidebar items and role-based access
├── context/
│   └── AppContext.tsx       # Language and role state
├── data/
│   └── mockData.ts          # Demo data (chits, subscribers, auctions, payments...)
├── i18n/
│   └── translations.ts      # English and Tamil strings
├── pages/
│   ├── LoginPage.tsx
│   ├── DashboardPage.tsx
│   ├── ChitGroupsPage.tsx
│   ├── SubscribersPage.tsx
│   ├── AuctionPage.tsx
│   ├── PaymentsPage.tsx
│   ├── CollectionsPage.tsx  # Collections, Outstanding, Ledger
│   ├── AdminPages.tsx       # Dividends, Prize Payments, Reconciliation, Reports,
│   │                        # Calculator, Branches, Staff, Documents,
│   │                        # Notifications, Audit Trail, Settings
│   └── CustomerPortal.tsx
├── types/
│   └── index.ts             # TypeScript types and enums
└── utils/
    ├── calculations.ts      # Chit maths, IRR / XIRR
    ├── format.ts            # INR and date formatting
    └── cn.ts                # className helper
```

Imports use the `@/` alias, which maps to `src/`.

---

## How the Chit Maths Works

Implemented in `src/utils/calculations.ts`:

| Value | Formula |
|---|---|
| Monthly contribution | `chitValue / members` |
| Prize amount | `chitValue − auctionDiscount` |
| Foreman commission | `chitValue × commission% / 100` |
| Dividend pool | `auctionDiscount − commission` |
| Dividend per member | `dividendPool / members` |
| Future liability | `(duration − auctionMonth) × monthlyContribution` |
| Penalty | `outstanding × penaltyRate × (overdueDays / 30) / 100` |

IRR and XIRR are solved with Newton–Raphson iteration.

---

## Current Limitations

- Runs on **mock data**: changes are not saved to a database
- No real authentication; role selection is for demo purposes only
- Supabase client is installed but not connected

## Roadmap Ideas

- Connect Supabase (auth, database, row-level security per role)
- Real file upload for KYC documents
- Receipt and statement PDF generation
- SMS / WhatsApp payment reminders

---

## Author

**Shanmuga Sundaram A (Broly)**
- GitHub: [shanmugasundaram-1](https://github.com/shanmugasundaram-1)
- LinkedIn: [shanmuga-sundaram](https://linkedin.com/in/shanmuga-sundaram-0aa178420)
