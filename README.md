# BILLTRAP — Everyday Bill Overcharge & Increase Analyzer

**BILLTRAP** is a standalone, zero-backend web application built with pure **HTML5, CSS3, and Vanilla JavaScript**. It helps individuals analyze utility, mobile, internet, insurance, and everyday household statements to identify unusual price increases, possible duplicate charges, newly introduced fees, and recurring cost creep before contacting a service provider.

---

## Included Standalone Pages & Files

- **`billtrap.html`** — **Bill Analyzer**: Add, edit, and delete statements with itemized charge descriptions; compare bills against previous statements from the same provider or category; adjust the unusual-increase alert threshold; inspect line-by-line duplicate and new-fee flags; filter/sort statements; and clear or restore sample data.
- **`history.html`** — **Spending History**: Search and filter historical statements; view interactive native SVG monthly spending charts; compare spending across categories; identify consecutive recurring increases and annualized 12-month cost estimates; edit/delete historical records; and export CSV files.
- **`action-center.html`** — **Action Center**: Triage flagged warnings (`Needs Review`, `Reviewed`, `Resolved`, `Dismissed`); explicitly distinguish **Suspected Issues (Unverified)** from **Confirmed Billing Errors**; generate and copy customizable provider clarification/dispute letters; complete a provider contact checklist; log contact dates, reference numbers, and credit outcomes; and export `.txt` or `.csv` findings reports.
- **`README.md`** — Project setup, usage, calculation rules, and privacy guide.
- **`prompts.txt`** — Master prompt and structured implementation prompts.

---

## Setup & How to Run

### Option 1: Open Directly in Any Browser (Recommended — Zero Build Tools)
No server, installation, or internet API keys are required:
1. Download or locate the project folder on your computer.
2. Double-click **`billtrap.html`** (or open `billtrap.html`, `history.html`, or `action-center.html` in Chrome, Edge, Firefox, or Safari).
3. Use the left sidebar navigation links to move seamlessly between **Bill Analyzer**, **Spending History**, and **Action Center**.

> **Browser Note for `file://` Usage:** Modern desktop browsers share `localStorage` across files opened from the same folder. If strict private-browsing settings restrict local file storage, run any static file server (e.g., `npx vite` or `python3 -m http.server 3000`) and open `http://localhost:3000/billtrap.html`.

---

## Shared `localStorage` Schema

All three standalone pages read and write the same `localStorage` keys so updates on any page immediately appear on the others:

| Key | Purpose |
| :--- | :--- |
| `billtrap_bills_v1` | Array of statement objects (`id`, `provider`, `category`, `billingPeriod`, `billingCycle`, `totalAmount`, `lineItems`, `notes`, `isSample`, `createdAt`) |
| `billtrap_actions_v1` | Map of finding IDs to review status (`unreviewed`, `reviewed`, `resolved`, `dismissed`), classification (`suspected`, `confirmed_error`, `verified_valid`), and provider contact logs |
| `billtrap_settings_v1` | User preferences (`thresholdPercent`, `comparisonMode`, `sampleInitialized`) |
| `billtrap_checklist_v1` | Persistent provider contact preparation checklist items |

---

## Deterministic Analysis Rules

1. **Absolute Change**: `currentBill.totalAmount - previousBill.totalAmount`
2. **Percentage Change**: `((currentBill.totalAmount - previousBill.totalAmount) / previousBill.totalAmount) * 100` (computed only when `previousBill.totalAmount > 0` to prevent division-by-zero errors).
3. **Annualized Increase Estimate**: `positiveMonthlyIncrease * 12` (shown only when `billingCycle === "monthly"`).
4. **Possible Duplicate Line Items**: Normalizes descriptions (lowercased, punctuation stripped, whitespace collapsed) and checks for 2+ entries on the same bill with identical normalized text and positive dollar amounts.
5. **Non-Accusatory Guidance**: Warnings are labeled `"Review recommended"`, `"Possible duplicate"`, or `"Recurring increase"` and never claim fraud or verified provider wrongdoing until confirmed by the user.

---

## Data & Privacy Notice

- **Sample Data**: Initial statements for *NorthStar Energy*, *Pulse Wireless*, *Veridian Fiber*, and *HarborShield Insurance* are fictional sample records clearly labeled `Sample data`. Use **Clear Sample Data** or **Restore Sample Data** anytime on `billtrap.html`.
- **Browser-Local Storage**: All records stay locally in your browser's `localStorage`. Browser storage is **not** a secure cloud backup and will be lost if you clear site data. Export CSV/report backups regularly.
- **No Sensitive Credentials**: Never enter bank login credentials, SSNs, or payment card numbers.
