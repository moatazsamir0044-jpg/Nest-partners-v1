# Nest Ambassadors — Google Form Build Specs

**BirdNest · Internal · Confidential**
*Paste-ready field specs for both intake forms · companion to [`06-referral-tracking-sheet.xlsx`](./06-referral-tracking-sheet.xlsx)*

---

## How the two forms fit together

Both forms exist to put a **timestamp** on a referral claim before the client's booking conversation starts — that timestamp is the attribution proof described in `02-quick-win-action-plan.md` (CX Option A). They write into the same `Referral Tracking` tab of the tracking workbook, just tagged with a different **Attribution Source**.

| | Form A — Ambassador Pre-Submission | Form B — Client WhatsApp Referral |
|---|---|---|
| **Who fills it** | The employee, before the client contacts BirdNest | The client themselves, after messaging BirdNest's WhatsApp |
| **Trigger** | Employee has a lead in mind | Client's WhatsApp message is answered with the form link (set this as a WhatsApp Business **Quick Reply**, e.g. `/referral`) |
| **Attribution Source value** | `Employee Pre-Submitted` | `Client WhatsApp Form` |
| **Sequencing rule** | Submitted before any CRM record exists | Sent and filled **before** the reservation/rental conversation proceeds |

Build both in Google Forms, then under **Responses → Link to Sheets**, point each at the `Referral Tracking` tab of the same workbook (Google Forms appends new rows; it does not overwrite existing ones).

---

## Form A — Ambassador Pre-Submission

**Form title:** Nest Ambassadors — Submit a Referral
**Description:** "Submit this before your contact reaches out to BirdNest — your submission time is what proves the referral is yours."

| # | Field | Type | Options / validation |
|---|---|---|---|
| 1 | Ambassador Name | Dropdown | Populated from the `Staff List` tab — keep in sync when staff changes |
| 2 | Department | Short answer, **read-only via description** | Or a second dropdown mirroring Staff List; do not let ops correct this manually — it should match the roster |
| 3 | Client Name | Short answer | Required |
| 4 | Client Phone | Short answer | Required; regex validation `^01[0-9]{9}$` (Egyptian mobile format) or adjust to house standard |
| 5 | Client Email | Short answer | Optional |
| 6 | Product Track | Multiple choice | `Sahel Reservation` / `Furnishing Package` |
| 7 | How do you know this person? | Short answer | Required — feeds dispute resolution per the CX rule |

**Do not add a manual timestamp field.** Google Forms auto-captures submission time in the linked sheet's response — that is the attribution source of truth. Never let an Ambassador backdate or edit a submission.

---

## Form B — Client WhatsApp Referral

**Form title:** BirdNest — Confirm Your Referral
**Description:** "Thanks for reaching out! Please confirm a few details below so we can credit the right person — this takes under a minute, then we'll continue your reservation."

| # | Field | Type | Options / validation |
|---|---|---|---|
| 1 | Your Name | Short answer | Required (maps to Client Name) |
| 2 | Your Phone Number | Short answer | Required (maps to Client Phone) |
| 3 | Your Email | Short answer | Optional |
| 4 | Who referred you? (employee name) | Dropdown | Same list as Form A's Ambassador dropdown — **dropdown, not free text**, so names can't be misspelled or invented |
| 5 | What are you interested in? | Multiple choice | `Sahel Reservation` / `Furnishing Package` |
| 6 | How do you know this person? | Short answer | Required — same dispute-resolution purpose as Form A |

**Keep this to 6 fields.** Anything beyond confirming the referral belongs in the normal reservation flow that follows — a longer form risks losing the booking, not just the attribution.

**Fraud guard (manual, zero-build):** when a Form B response lands, Commercial Ops pings the named Ambassador in the WhatsApp Ambassadors group to confirm they recognize the client before moving the row to `Valid`. This replaces the CRM-timestamp check that Form A gets for free, since the timestamp here only proves *when the client claimed it*, not that the employee actually made the referral.

---

## WhatsApp Business setup (zero engineering)

1. In WhatsApp Business (the free app), go to **Business tools → Quick replies**.
2. Create a quick reply, e.g. shortcut `/referral`, with the message:
   > "Thanks for reaching out! To make sure the right person gets credit, please fill this quick form first: **[Form B link]**. Once submitted, we'll pick up your reservation right away."
3. Whoever answers the number sends `/referral` the moment a client claims a referral — before discussing dates, pricing, or availability.
4. Pin the same link as a WhatsApp **Business catalog / About** shortcut if volume grows, so clients can self-serve it without waiting for a reply.

No API, no chatbot, no integration — this is available in the free WhatsApp Business app today.
