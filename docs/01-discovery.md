# Split-the-Bill for Bulgaria: Discovery

> Status: exploration, v0.1 (Oct 2026). Items marked **[VERIFY]** are assumptions that
> need confirmation from a lawyer, tax advisor, payment provider or POS vendor before we build on them.

## 1. The problem

10 friends eat at a restaurant. The bill comes as **one** fiscal receipt for the table.
Today one person pays the venue, and the other nine pay that person back in cash, by Revolut or by bank transfer.

What goes wrong:

| Pain | Who feels it |
|---|---|
| One person fronts a large amount and chases people for days | The "organizer" |
| Working out who owes what (shared pizza, 3 beers vs 1 water, tip) | Everyone |
| Waiter runs 10 card payments or does cash maths at the table | Staff / venue |
| The table stays occupied longer, so fewer tables turn over | Venue |
| Small debts are never repaid, which causes friction | Friend groups |

The goal is that **each person pays for what they consumed, directly, at the table, and the venue receives the full bill.**

## 2. Actors

- **Guests**: payers. Some are tech-savvy, some only have cash, some are tourists with foreign cards.
- **Organizer**: often the person who would have fronted the money.
- **Waiter**: wants it fast. Their tips are at stake.
- **Venue owner**: cares about fees, cash flow, fiscal compliance, and not changing their POS.
- **POS / restaurant-software vendor**: holds the bill data, e.g. Microinvest (Restaurant Pro, 40k+ customers, 130+ resellers in BG), Barsy, and many smaller local systems. **[VERIFY]**: which vendors expose an API.
- **Payment provider / acquirer**: BORICA, banks, myPOS, Stripe, Revolut Business, etc.
- **Regulators**: NRA/НАП (fiscal rules, Наредба Н-18) and BNB (payment licensing, PSD2).

## 3. Real-life scenarios the solution must handle

Simple equal split is easy. These cases decide whether the product is usable:

1. **Shared items**: one pizza for 3 people, a bottle of wine for 4, or "the table" salad.
2. **Paying for someone else**: a birthday guest, a partner, or "I'll get his drink".
3. **Tips**: per person or one tip for the table, by percentage or a fixed amount.
4. **People leave early**: they pay their part and go while the bill is still open.
5. **Items added after someone has paid**: another round arrives.
6. **Voids, corrections and discounts**: the waiter removes an item, or happy hour applies to some items only.
7. **Mixed payment**: 7 pay by phone and 3 pay cash to the waiter.
8. **Someone does not pay or a card fails**: who covers the gap? The table cannot be closed.
9. **Rounding**: €47.00 split 3 ways is €15.67 + €15.67 + €15.66. The cents must always add up.
10. **Invoice (фактура) for one person's share**: very common in Bulgaria for business meals.
11. **Refunds and chargebacks** on one share, not the whole bill.
12. **No smartphone, or a dead battery**.
13. **Foreign cards and tourists**: the Black Sea and ski resorts are important for this.

## 4. What exists elsewhere

| Model | Examples | How it works |
|---|---|---|
| Debt tracking after the meal | Splitwise, Tricount, Revolut "split bill" | One pays, the app tracks and requests repayment. No venue involvement. |
| Pay-at-table QR | sunday (FR/UK/US), Qerko (CZ), me&u (AU/UK), HungryPay, MOBI, Foodics | QR on the table opens the live bill from the POS. Each guest selects items and pays the venue. |
| Group virtual card | Cino (UK) | Each friend funds their share onto a one-off virtual card in the app, and the card pays the venue's normal terminal. No venue integration. |
| Terminal-level split | Worldline "Split Bill" (Greece, 2025) | The card terminal itself splits the amount over several cards. |
| **Bulgaria** | **PossePay** (Sofia, 2024, Elevate S4 winner) | QR pay-at-table with item selection and tips. First client was a shisha bar in Sofia. **[VERIFY]** current traction. |

**Takeaway:** the problem is solved abroad. In Bulgaria the market is open but **not empty**. PossePay exists, and acquirers like Worldline could bring terminal-level split. We need a clear angle.

## 5. Bulgarian context that matters

- **Euro since 1 Jan 2026 and SEPA membership.** One currency for tourists and locals. SEPA Instant (TIPS) is available.
- **Blink**: instant transfers within 10 seconds, 24/7. **blinkP2P** allows payment to a phone number in EUR and is joining EuroPA, the cross-border P2P network. This is a cheap and fast rail for repaying friends.
- **Revolut** is extremely widespread among younger Bulgarians. That is how people solve the problem today.
- **Fiscal rules (Наредба Н-18)**: every sale in a venue must be registered on a fiscal device and reported to НАП. A card payment made **from a phone at the table** sits between "in-store card payment", which needs a fiscal receipt, and "remote payment", which in some cases can use another document. **[VERIFY with a tax advisor]** This is the single biggest legal question for the venue-integrated model. In practice, the safest path is that **the venue's POS stays the system of record** and issues the receipt(s). Our system only tells the POS which part was paid and how.
- **Holding money for others (the "vault" idea)** is a regulated payment service or e-money activity under PSD2 / BNB rules. We cannot do this without a licence. We can do it **through a licensed partner**: an EU Banking-as-a-Service provider or an e-money institution. **[VERIFY]**
- **Tips by card**: check the current rules on how card tips are recorded and paid to staff. **[VERIFY]**
- **POS fragmentation**: many venues run local, often offline, Windows-based systems installed by resellers. Integration is per vendor, and sometimes per installation.

## 6. Solution models

### Model A: Smart debt settlement (no venue integration, no licence)
The organizer pays as today. The app makes repayment painless:
- Photograph the bill, then OCR turns it into items. Or the organizer types the total.
- A shared link lets everyone tap the items they had. Shared items are split automatically, and the tip is added.
- Each person gets a one-tap **Revolut link / Blink P2P / IBAN QR (EPC QR code)** with the exact amount.
- The app tracks who has paid.

**Pros:** buildable in weeks, no regulation, no venues needed, good for validating demand.
**Cons:** someone still fronts the money. The bill-photo OCR must handle Bulgarian receipts.

### Model B: Group "vault" / virtual card (no venue integration, licensed partner needed)
This is the user's vault idea in its most practical form, and it is how Cino works:
1. The organizer opens a "tab" in the app and shares the link.
2. Each friend claims items and **pays their share into the tab** (card, Apple/Google Pay).
3. Once fully funded, the app issues a **single-use virtual card** for the exact total.
4. The organizer taps their phone on the venue's **normal card terminal**. The venue receives one normal payment and prints one normal fiscal receipt.

**Pros:** works in **every** venue with a card terminal from day 1. The venue does nothing and pays nothing extra. The fiscal question disappears because, for the venue, it is a normal card sale.
**Cons:** requires a BaaS / card-issuing partner. The partner holds the funds and issues the cards. Costs include card-issuing fees plus pay-in fees on every share. Shortfall risk: someone does not pay, so either block until funded or let the organizer cover the gap. Refunds come back to the card and must be redistributed.

### Model C: Pay-at-table with venue integration (the "full" product)
This is the sunday / Qerko / PossePay model:
1. A QR code on the table, or on the printed bill, opens a web page. No app install is needed.
2. The page shows the **live itemised bill from the POS**.
3. Each guest selects items or a share, adds a tip, and pays with card / Apple Pay / Google Pay.
4. The money goes to the **venue's merchant account** through a payment-facilitator setup such as Stripe Connect or an acquirer partner, so we never hold the funds.
5. Each payment is pushed back to the POS. The POS records the partial payment and issues the fiscal document. When the balance is 0, the table closes.

**Pros:** best experience. Strong value for venues: faster table turnover, less waiter time, and usually higher tips. This is the business with recurring revenue.
**Cons:** you need a **POS integration per vendor**, venue sales one by one, and a fiscal-compliance setup that is validated per POS. Chicken-and-egg: guests only use it where venues have it.

### Model D: Partner with an acquirer for terminal-level split
Pitch split-at-terminal to a BG acquirer (e.g. a bank, myPOS or BORICA ecosystem) as in Worldline Greece. This is more a B2B partnership than a product of our own.

### Comparison

| | A: Settlement | B: Vault/card | C: Pay-at-table | D: Terminal |
|---|---|---|---|---|
| Removes "someone fronts money" | No | **Yes** | **Yes** | **Yes** |
| Needs venue onboarding | No | **No** | Yes, per venue | Via acquirer |
| Needs POS integration | No | No | **Yes** | No |
| Needs licence / financial partner | No | **Yes** (BaaS) | PSP / payfac | Acquirer |
| Fiscal complexity for us | None | Low | **High** | Low |
| Time to first users | Weeks | Months | Months+ | Long |
| Who pays us | Consumers (hard) | Consumers / fee per tab | **Venues** (SaaS + % fee) | Acquirer revenue share |

## 7. Recommended direction (for discussion)

A phased approach where each step de-risks the next:

**Phase 0: Validate (2–4 weeks)**
- Interview about 15 venue owners/managers (Sofia, Plovdiv, Varna; mix of restaurant, bar and café) and about 30 guests.
- Talk to 2–3 POS vendors (Microinvest first, because of market share) about API access and fiscal handling of partial payments.
- Get a written opinion from a tax advisor on Н-18 and phone-based payments at the table.
- Contact 2–3 EU BaaS / card-issuing providers about Model B feasibility and pricing in Bulgaria.
- Study PossePay's current product and traction: what has and has not worked.

**Phase 1: Consumer MVP = Model A (about 4–6 weeks)**
- Web app with no install: create a tab, scan or enter the bill, share a link, claim items, and handle tip and rounding.
- Pay the organizer back via Revolut link / Blink P2P / EPC-QR bank transfer, then track status.
- This builds the **core engine** that every later model reuses: bill model, item claiming, shared-item splitting, rounding, real-time sync. It also gets users quickly.

**Phase 2: Choose based on Phase 0 findings**
- If a BaaS partner is viable, then **Model B**. The organizer no longer fronts money, and it works everywhere.
- If a POS vendor is cooperative and venues show interest, then pilot **Model C** in 3–5 venues on that vendor.
- Both can coexist: B for "any venue", C for "partner venues".

## 8. Core technical building blocks (shared by all models)

- **Bill model**: items (name, unit price, qty, VAT group), modifiers, discounts, service charge, tip. Money is stored in **integer cents (EUR)** and never as floats.
- **Allocation engine**: assigns items or fractions of items to participants. Shared items are split equally or by custom weights. A deterministic rounding rule (largest remainder) ensures **the sum of shares always equals the bill total**.
- **Real-time session**: everyone at the table sees the same bill live (WebSockets). Concurrent claims of the same item need conflict handling, e.g. optimistic locking with versioned bill state.
- **Ledger**: an append-only record of who owes and who paid, with every state change auditable. This is critical for refunds and disputes.
- **Payment adapters**: Model A uses link generation (Revolut, EPC-QR, Blink). B uses a BaaS pay-in plus card issuing. C uses PSP + payfac split.
- **POS adapters (Model C)**: one adapter per vendor behind a common interface (`getOpenBill(table)`, `registerPartialPayment(...)`, `closeBill(...)`). Expect some vendors to need an on-premise agent next to the POS.
- **OCR (Model A)**: parse photos of Bulgarian receipts in Cyrillic, with the fiscal receipt layout.
- **Web-first**: PWA, Bulgarian + English, Apple Pay / Google Pay first, and 3-D Secure (SCA) handled by the PSP.
- **Compliance**: GDPR (minimal personal data: a guest can be anonymous), PSD2 via the partner, and fiscal compliance via the POS.

## 9. Key risks

1. **Fiscal/legal ambiguity** of phone payments at the table (Model C). Mitigation: tax opinion first, and keep the POS as the system of record.
2. **POS integration effort and vendor cooperation** (Model C).
3. **Unit economics**: 10 small transactions cost more in fixed fees than one large one. We must decide who pays: the venue or the guest as a small convenience fee.
4. **Partner dependency** (Model B): BaaS pricing, onboarding of a Bulgarian entity, KYC requirements on the organizer.
5. **Competition**: PossePay locally, and international players (sunday, Qerko) entering after euro adoption.
6. **Habit**: "I'll just Revolut you" is free and familiar. We must be clearly faster than that.

## 10. Open questions

- Who is the first customer we want to win: **friend groups (B2C)** or **venues (B2B)**?
- Target segment: casual restaurants, bars/clubs (high group spend, many rounds), cafés, or tourist resorts?
- Is a licensed financial partner (Model B) acceptable, or do we want to stay out of handling money completely?
- Do we have, or can we get, an introduction to Microinvest or another POS vendor?
- Business model preference: venue SaaS fee, per-transaction %, consumer fee, or a mix?

## Sources

- PossePay (Capital.bg via AUBG): https://www.aubg.edu/aubg-today/possepay-split-my-bill-capital-bg/ ; https://www.capital.bg/biznes/fintech/2024/01/15/4575756_possepay_razdeli_mi_smetkata/
- Cino (TechCrunch, 2025): https://techcrunch.com/2025/03/04/cino-has-cracked-bill-splitting-at-the-moment-of-payment-raises-seed
- Worldline Split Bill, Greece: https://worldline.com/en-gr/home/top-navigation/media-relations/press-release/pr-2025_10_01_01
- BORICA, instant payments: https://www.borica.bg/en/latest/novini/instant-payments-in-bulgaria-3-years-after-the-first-transaction_en
- Bulgaria joins EuroPA (BTA): https://www.bta.bg/en/news/economy/1126599-bulgarians-to-be-able-to-send-money-across-europe-as-bulgaria-joins-european-pa
- Payments as of 01.01.2026: https://ibank.bg/en/news/important-information-regarding-payments-as-of-01-01-2026
- Наредба Н-18 and online payments: https://kik-info.com/novini/novini-i-akcenti/Promenite-za-onlayn-magazinite-pri-plashtaniya-s-karti.154920.php ; https://www.mypos.com/bg-bg/blog/biznes-narachnik/kasova-belezhka-onlain-magazin-plashtane-virtualen-pos
- Microinvest: https://en.wikipedia.org/wiki/Microinvest
- Pay-at-table examples: https://www.hungryhungry.com/en/blogs/customers-split-bill-themselves-without-staff ; https://www.mobihq.com/in-venue/pay-at-table ; https://www.foodics.com/portfolio/pay-at-table/
