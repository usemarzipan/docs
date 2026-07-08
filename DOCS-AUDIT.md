# Marzipan Docs Audit

**Generated:** 2026-07-08 · **App reference:** `marzipan-app` `origin/main` (HEAD `34922812`, 2026-07-03) · **Docs branch:** `develop`

This is the working list of what needs to change in the documentation to bring it in line with the app. The docs were last meaningfully updated in **March 2026**; the app has ~5 months of features on top of that, and several large feature areas (Insights, Markets, POS, Rewards, Forms, Messages) have never been documented at all.

Everything below was verified against the app source (Vue pages, models, routes) — not just the changelog. Accuracy caveats (features that exist in code but should **not** be documented as available) are called out in **§5**.

Legend: 🔴 inaccurate/misleading today · 🟠 empty stub · 🟢 missing entirely (no page) · ✏️ needs an update

---

## 1. Existing pages that are now inaccurate (fix first) 🔴

These pages are live and actively wrong or misleading.

| Page | Problem |
|------|---------|
| `settings/overview.mdx` | General settings no longer has **Currency** or **Timezone** fields (currency is now governed by **Markets**). Payments section describes "stored live/test keys toggled by test mode" — Stripe is now **Stripe Connect with separate live/test connected accounts**. Missing every newer settings area: Markets, Inventory, Shipping, Discounts, Roles, Team, Notifications, Tax, Rewards, Events, API scopes. |
| `settings/test-mode.mdx` | Doesn't mention that test mode also switches **integrations** to test mode, or the **purge test data / purge-preview** actions. Stripe test mode is a separate Connect account, not a stored test key. |
| `settings/email.mdx` | Missing **inbound email (MX → reply inbox)** and the subdomain warning, **DMARC** record, provider **routing rules**, 60s auto-polling, and "no email sends until verified". |
| `dashboard.mdx` | Badly outdated. Says "in the future you'll see website analytics" — it already exists. Missing: setup checklist, dashboard stats + chart + **market filter**, **revenue-protection tiles** (failed payments, pending cancellations), today's tasks, test-mode exclusion. |
| `orders/overview.mdx` | Missing the **Market** column/filter (multi-market), the **POS** channel, and header actions (**Order Insights**, **Bulk shipment update**). |
| `orders/create.mdx` | Missing **market auto-detection** from billing country, Delivery vs **Collection** wording, product **options** and sale/subscriber price badges, phone-required warning. Clarify POS orders come from the till app, not this form. |
| `orders/details.mdx` | Most outdated order page. Missing the **Actions menu** (resend, cancel, refund, duplicate, assign walk-in customer), **Attribution** panel, draft/hold flows (**process payment, mark as free, redeem points, edit items, release hold**), **bundle constituent items** in edit, **points redeemed** rows, **Tap to Pay / card-present** payment display, and the **5s activity polling** note. |
| `orders/fulfilment.mdx` | Missing **partial shipment** (per-item quantities), the **courier list**, tracking/cost/comments fields, the **Bulk shipment update** (CSV) tool, and the **Update shipment** modal. |
| `orders/payments.mdx` | Generic and partly invented ("print invoices and packing slips", "update order status" buttons — not in code). Broken link to `/orders/fulfillment` (spelling; file is `fulfilment`). Missing real payment flows: process payment (saved card / manual types), refunds with reasons, mark as free, redeem points, Tap to Pay. |
| `web-components/customisation.mdx` | Tab structure out of date. Add the new **Labels** tab (consolidated text labels incl. Sales Closed, Back in Stock), **product image** options (thumbnail position, zoom overlay, image radius), **event display** options (list/calendar view, occurrence count), **primary button text colour + hover**, and **subscription frequency wording** (worded vs interval). |
| `api-reference/introduction.mdx` | Still says "beta" and covers only the storefront `/v1` API. Doesn't mention the separate **`/admin/v1` mobile/admin API** (2FA login, orders, POS, customers, devices/push, Stripe Terminal). The `openapi.json` is incomplete even for `/v1` (missing visits, forms, events, back-in-stock, rewards, messages). |

---

## 2. Empty stub pages to write 🟠

These pages exist in nav but contain only frontmatter.

| Page | Feature is… | Notes |
|------|-------------|-------|
| `products/overview.mdx` | Shipped, mature | Product types are now only **Physical** and **Bundle** (subscriptions & events moved to their own areas). GTIN/barcode field, per-market pricing tabs, per-market visibility, private attributes, location inventory, overselling, max-per-customer, low-stock badges, tier exclusivity, upsell vs related products, reviews/awards, SEO. Large — candidate for a small group of pages. |
| `attributes/overview.mdx` | Shipped | Types (Text/Textarea/Select/Number), Required, **Private** (hidden from storefront/API), the "quick attribute" value-assignment UI on products/collections. |
| `collections/overview.mdx` | Shipped | **Manual only** — no automatic/condition collections. Name/Description/Published, drag-order products, collection-level attributes. |
| `subscriptions/overview.mdx` | Shipped, mature | Packages vs subscriptions; three types (**Fixed / Variable / Pick & Mix**); fixed vs per-item pricing; renewals (manual renewal, edit renewal date), change frequency, skip/add items, transfer, cancel/reactivate; failed-renewal handling; the batch **Shipments** fulfilment workflow; Subscriber tag. Large — candidate for a group. |
| `events/overview.mdx` | Shipped, mature | Event types; one-time vs **recurring** (daily/weekly/monthly, repeat-on days, all-day, multi-day); occurrences (edit one vs all, preserve customised times); tickets/capacity; **deposits + balance**; **waitlist**; **QR check-in**; attendees table + CSV export; what3words. Large — candidate for a group. |
| `discounts/overview.mdx` | Shipped | Types (fixed / percent / free shipping / **free item**); auto-apply vs code; **market requirement** (required for fixed amount); limits; conditions (cart item type, customer subscription/tags, products/product types, shipping methods, min spend); sale/subscriber exclusions. |
| `customers/overview.mdx` | Shipped, mature | List + filters (incl. **RFM segments**), Segments/Export/Insights; the tabbed **profile** (overview, activity, orders, subscriptions, events, emails, rewards), addresses, payment methods, tags, marketing consent, archive. Large — candidate for a group. |
| `cms/overview.mdx` | Shipped + headless API | Pages (templates, section fields, editor types, SEO, publish, preview tokens), Posts, Collections, Website (templates + CSS/JS files). |
| `flows/overview.mdx` | **Beta, feature-flagged** | Keep the `BETA` tag. Trigger → conditions → actions graph; only 2 live triggers (`order.paid`, `cms.page_updated`); actions (webhook, send email, update status, integrations); versions & executions + retry. See §5 — document as beta only, or defer. |

---

## 3. Entirely missing feature areas (no page, no nav) 🟢

Shipped features with zero documentation. Each needs a new page (and a nav group in `docs.json`).

**Commerce / operations**
- **POS & Tap to Pay** — sell in person via the mobile companion app; Quick Pay, walk-in customer, identify customer (subscriber pricing), Stripe Terminal / Tap to Pay on iPhone, receipts, assigning a walk-in order to a customer. (Orders group.)
- **Markets & multi-currency** (Settings) — markets, currency, exchange rate, rounding, countries, per-market payment provider, currency switcher, market resolution/precedence, per-market pricing & visibility.
- **Inventory** (Settings) — location-based inventory (one-way initialise), locations, inventory rules, stock movements & reasons, bulk adjustment + CSV, low-stock badges vs alert emails (real-time vs digest), LCB stock sync.
- **Shipping** (Settings) — methods (flat/free/pickup/custom), weight vs volume bands, countries & built-in zones (UK + SA), min order value, collection conditions.
- **Roles & Team** (Settings) — granular `resource.action` permissions, system roles, team members/invites, per-member notification opt-ins.
- **Integrations** (Settings) — install flow; catalogue (London City Bond, Robert Guy, Mailchimp, ScrubBill, DPD, APC, Fathom).

**Customer / marketing**
- **Rewards / Loyalty** — tiers, perks (naming-based; see §5), earning rates + min spend, points calculation, redemption rates + max cap, checkout confirmation, notifications, insights.
- **Marketing → Segments** — segment builder (full condition reference), AND/OR grouping, view/export members; **Mailchimp** sync (tags vs merge fields); email **Templates** (Unlayer). *Omit Campaigns/Automations — unreleased.*
- **Messages** — two-way email inbox; conversation list, filters, reply/assign/flag/close; inbound setup (MX) + status banner.
- **Tasks** — tasks with priority/due/assignee, multiple-assignee filter; **@mentions** to customers/orders/subscriptions/events and how they appear as activity.
- **Forms** — form builder (field types, per-field config), form types (Contact/Newsletter/Custom), notifications + **auto-reply template**, submissions view.

**Analytics / content**
- **Insights / Reports** — the single biggest gap. Fully built, zero docs. Reports: Revenue, Channels, Geographic, Cart Recovery, Customers (RFM/CLV), Rewards, Products, Subscriptions (+status/billing/additional-items), Orders, Attribution/UTM, Events, Web Analytics. All with date range, period comparison, market filter.
- **Media library** — central asset store (upload, drag-drop, bulk alt text, edit/delete, unlink); reused by products, labels, share images, CMS, rich-text editor.

---

## 4. Changelog ✏️

`changelog/2026.mdx` stops at the **March 2026** update. Add entries for **April–July 2026** covering (all confirmed on `origin/main`):

- **Point of Sale & Tap to Pay on iPhone** — in-person selling via the mobile app, Stripe Terminal card capture, walk-in customers, POS receipts, assign walk-in to customer.
- **Events** — all-day events, multi-day weekly schedules, recurring-event edit propagation with preserved customised occurrence times, attendees CSV export + price column + clickable orders, ticket QR codes in emails.
- **Products** — GTIN/barcode field with live validation.
- **Subscriptions** — manual renewal for fixed subscriptions, cancelled/expired date-range filter.
- **Markets** — cart follows resolved market precedence; renewals priced in the customer's account market; per-market payment provider always shown.
- **Orders** — bundle constituent items shown when editing an order; live activity polling on the order page; shipping-date picker blocks past dates + logs LCB hold reschedules.
- **Forms** — customisable auto-reply via editable notification template.
- **Web Components** — unified **Labels** tab; subscription billing-frequency style (worded vs interval); product image options; customisable button text colour with hover.
- **Mobile app** — admin/mobile API and iOS push notifications.
- **Platform** — upgraded to Laravel 12 (internal; optional to mention).

---

## 5. Accuracy caveats — do NOT document these as working

Verified in code — these look shipped from the changelog but are not merchant-ready:

- **Marketing Campaigns & Automations** — pages exist but are unrouted / "coming soon". Document Segments/Templates only.
- **Rewards perks** — a perk is a category label + free-text name/description. There is **no structured per-type config** (no % field, no product/event picker). "Exclusive products" has no merchant UI. Express perk detail via naming, not settings.
- **Tasks "pinned notes"** — pinning is a Customer/Order/Subscription **notes** feature, not a Tasks feature.
- **Events "calendar view"** — the list/calendar toggle is a **storefront web-component** setting (`events.default_view`), not an admin view. The admin events list has no calendar.
- **Segment → Mailchimp sync** pushes **tags/membership**, not merge fields. The merge-field sync (reward tier, points, address) is the separate per-customer Mailchimp sync.
- **GDPR request** in the customer archive modal is disabled ("available in a future update").
- **Customer type** can be set at customer creation but **not** edited from the profile.
- **Order shipping-date "future override warning"** — not present in order code; the picker only blocks past dates.
- **Flows** — beta, feature-flagged to allowlisted tenants, only 2 live triggers. Document as beta or defer.

---

## 6. Proposed nav (`docs.json`) additions

New/expanded Documentation-tab groups (order roughly follows the app sidebar):

- **Orders**: + `orders/pos`
- **Products**: + pricing / inventory / bundles-options (or keep as one page)
- **Subscriptions**: + packages / managing / shipments
- **Events**: + creating / recurring / attendees
- **Customers**: + profile
- **Rewards** *(new group)*: overview, tiers, perks, earning-and-redemption, insights
- **Marketing** *(new group)*: segments, mailchimp, templates
- **Messages** *(new group)*: overview, inbound-setup
- **Tasks** *(new group)*: overview
- **Forms** *(new group)*: overview, builder, notifications, submissions
- **Insights** *(new group)*: overview + per-report pages
- **Media** *(new group)*: overview
- **Settings**: + markets, inventory, shipping, discounts, roles-and-team, integrations
- **API Reference**: + admin/mobile API

Media, share images, CMS and rich-text all draw from the same Media library — cross-link rather than duplicate.
