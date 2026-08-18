# DEP Payment & Claim Workflow — Internal Controls Review

Review of "Bank Payment Process Flow (DEP)" draft v1 · 18 August 2026
Scope: supplier payments and staff/member claims · Systems assumed: AutoCount, bank portal with maker–checker, WhatsApp

## 1. Verdict

The four-lane design already encodes the control that matters most — separation of maker and
checker — and routing every payment through a payment voucher in AutoCount is the right anchor.
Three things will still fail a review:

1. **WhatsApp is doing the job of a system of record**, and it cannot. No request number, no
   immutable trail, compressed and expiring attachments, no enforced fields.
2. **Segregation of duties is drawn but not enforced.** "Checker review and approve online" appears
   in both the Maker lane and the Checker lane, which reads as the Maker checking their own work.
3. **The flow ends at filing.** Nothing captures the bank reference, confirms the money arrived,
   reconciles to the statement, or handles a payment the bank returns.

Tally: 4 critical, 5 major, 2 notation.

## 2. Keep

- Maker–checker on release, enforced by the bank portal rather than by policy alone.
- Completeness gate before approval — protects approver attention.
- A payment voucher per payment in AutoCount, tying request to ledger.
- Bank transfer only, no cheques.

## 3. Findings, ranked by audit exposure

### Critical

**01 — WhatsApp is the system of record.** Messages can be deleted for everyone, media expires,
photos are recompressed until an invoice number is unreadable. No request ID links invoice → PV →
bank reference. Nothing enforces that the payee account or amount was stated. The same invoice can
be sent twice in two chats and paid twice. Payee bank details and claimants' personal data in a
group chat on personal phones is a data-protection exposure.
*Fix:* split the channel from the record. Intake moves to a structured form writing to a Payment
Register, attachments into a folder under one request ID (`PR-2026-0001`). WhatsApp keeps carrying
notifications and links only.

**02 — Maker and checker are not held apart.** Delete the checker box from the Maker lane. Write
down who is Maker, who is Checker, and each one's standing alternate. Never share portal
credentials or tokens. Replace "Treasurer *or* Assistant Treasurer" with a rule: the Assistant acts
only on SLA breach or recorded leave, and the register captures which one approved.

**03 — No approval limits and no budget check.** A RM200 stationery bill and a RM80,000 contract
payment take the same path and the same one signature; nothing asks whether the payment is
budgeted. *Fix:* a written approval matrix by amount (§6), plus a budget line and an
over-budget/resolution-reference field.

**04 — Payee bank details are never verified independently.** This is how small organisations
actually lose money: a real invoice with a substituted account. Because the Checker reviews what the
Maker keyed rather than what was approved, a changed account passes both pairs of eyes.
*Fix:* a beneficiary master list; call-back verification on any new payee or changed details, using
a number from your own records, logged by name and date; the Checker verifies against the approved
request and the master, not the Maker's screen.

### Major

**05 — The flow ends at filing, not at the bank statement.** No bank reference, no advice, no
notification to requester or payee, no reconciliation, and no path at all for a failed or returned
transfer. *Fix:* add stages 6 and 7 (§4).

**06 — Claims are treated as supplier payments.** As drawn, the Treasurer can submit and approve
their own mileage claim, and no policy limits are tested. *Fix:* a claim variant — claimant never
in their own approval chain; screening checks the claim against written policy.

**07 — Rejections and returns have no state and no log.** "Documents complete? → No" loops back to
START; "Reject and notify requester" has no exit. *Fix:* the status model in §7; Returned is
distinct from Rejected, and a second return escalates instead of looping.

**08 — One path for payments that are not ad-hoc.** EPF, SOCSO, EIS, monthly tax deductions, SST,
rent, utilities and standing instructions have no requester. Forcing them through the ad-hoc path
means the path gets bypassed or deadlines get missed. *Fix:* three named variants (§8).

**09 — Conditional bookkeeping and duplicated filing.** "Record transaction if Maker didn't issue
PV" is a step that eventually happens to nobody, and two parallel files will diverge. *Fix:* the PV
is always raised in AutoCount before release; go digital-first, with the PDF on the PV as the
record. Retain seven years, retrievable in Malaysia.

### Notation

**10 — Numbering, lanes and colours disagree.** Step 5 used twice; "Submit for approval" sits in
the Treasurer lane but is coloured as a requester activity; END sits in the Checker lane although
filing is the last activity, in the Maker lane.

**11 — A dead cheque branch is still drawn.** Delete it from the DEP flow; if another entity issues
cheques, that is a separate variant document.

## 4. Recommended flow — seven stages on one spine

Every stage reads from and writes to a single register row. Stages 3 and 5 are hard gates.

| # | Stage | Owner | Must check | Evidence produced | SLA |
|---|-------|-------|------------|-------------------|-----|
| 1 | Request | Requester | Mandatory fields; supporting document; declaration | Register row, request ID, attachments | — |
| 2 | Screen | Finance admin | Legibility; invoice not already in register; payee on master; budget line and GL code; e-Invoice / WHT flags | Screening note, coding, duplicate-check result | 1 wk day |
| 3 | **Approve — gate** | Treasurer + 2nd per matrix | Merit; within budget or resolution attached; within authority limit | Approval record: name, limit applied, timestamp | 2 wk days |
| 4 | Prepare | Maker | PV raised first; payee and account keyed from the *approved request* | PV number, payment queued | 1 wk day |
| 5 | **Release — gate** | Checker | Payee, account, bank, amount, PV against approved request and master | Release record, bank reference | 1 wk day |
| 6 | Post | Maker / accounts | Advice attached; posting matches PV; requester and payee notified; invoice marked paid | Advice PDF on PV, ledger entry, status Paid | 1 wk day |
| 7 | Reconcile | Treasurer | Every debit ties to a released request; no payment without PV; no PV without approval | Reconciliation, exception report, signature | Monthly |

Two legitimate backward paths, both logged with a reason: stage 2 → stage 1 (returned) and
stage 5 → stage 4 (sent back on mismatch).

## 5. Segregation of duties

Four roles: Requester (raises), Approver/Treasurer (authorises), Maker (keys in), Checker
(releases).

| Overlap | Allowed? | Compensating control |
|---|---|---|
| Requester also keys the payment | Yes | Checker verifies payee and account against the original invoice and the master, never against the Maker's screen |
| Treasurer approves, then keys | Yes | Checker must have no involvement in the request; monthly exception report reviewed by a second office-bearer |
| Maker also releases | **Never** | Two portal user IDs, two devices, no shared token |
| Approver also releases | **Never** | Collapses four eyes to two; if headcount forces it, the payment waits |
| Requester also releases | **Never** | A person paying their own request |
| Claimant anywhere in own chain | **Never** | An office-bearer's claim goes to the President or committee |

Additional control regardless of headcount: the bank statement should reach one person who is
neither Maker nor Checker.

## 6. Approval matrix (template — set the bands yourself)

| Amount per payment | Approver(s) at stage 3 | Additional requirement | Checker at stage 5 |
|---|---|---|---|
| Up to RM 1,000 | Treasurer or Assistant | Within budget line | Any other authorised releaser |
| RM 1,001 – 10,000 | Treasurer + one committee member | Within budget line | Not either approver |
| RM 10,001 – 50,000 | Treasurer + President | Quotation comparison on file | Not either approver |
| Above RM 50,000 | Treasurer, President, committee | Committee resolution referenced | Not any approver |
| Any amount, over budget | As above + committee | Resolution or budget revision referenced | As above |
| New payee / changed details | Approver as above | Call-back logged: who called, which number, when | Checker confirms against master |

Align the bands to the bank mandate and the constitution, then adopt by resolution.

## 7. Status model

Spine: **Submitted → Approved → In payment → Paid → Closed**

Backward states (mandatory reason, second return escalates):
- Returned to requester — incomplete / resubmitted
- Sent back to maker — mismatch found / corrected

Exit states:
- **Rejected** (terminal) — declined at the approval gate, logged
- **Cancelled** (terminal) — withdrawn, PV voided
- **Returned by bank** — funds came back; PV reversed or held, re-issued only after payee details
  are re-verified. The draft has no answer for this state at all.

Closed is terminal. Every transition logged with actor, timestamp and reason.

## 8. Variants

| Variant | Applies to | How it differs | Control that replaces the standard one |
|---|---|---|---|
| Recurring & statutory | EPF, SOCSO, EIS, MTD, SST, rent, utilities, standing instructions | Schedule approved once a year; no per-payment approval | Variance check at stage 2 — new payee, amount above tolerance or changed account drops back into the standard flow |
| Emergency | Genuine same-day need out of hours; penalty deadline | Verbal/WhatsApp approval from Treasurer plus one office-bearer | Form raised within one working day, written ratification within three, every instance named in the monthly exception report |
| Staff / member claim | Reimbursements, mileage, per-diem, petty cash, advances | Claim form with policy limits printed; receipt threshold; submission deadline | Claimant never in own chain; advances cleared before a new claim; stage 2 checks against written policy |

## 9. The register — build this first

Mandatory before approval marked ‡.

`request_id`‡ · `date_submitted`‡ · `requester`‡ · `type`‡ · `payee_name`‡ · `payee_bank`/`account`‡ ·
`amount`‡ · `invoice_no`‡ · `invoice_date`‡ · `due_date`‡ · `description`‡ · `gl_code`/`cost_centre` ·
`budget_line`/`over_budget` · `einvoice_status` · `wht_applicable` · `attachments`‡ ·
`screened_by`/`at` · `approved_by`/`at` (+ matrix band applied) · `maker`/`at` ·
`checker`/`released_at` · `pv_no` · `bank_reference` · `status`‡ · `status_reason` · `reconciled_on`

## 10. Tooling

| Layer | Start with | Upgrade to when volume justifies |
|---|---|---|
| Intake | Microsoft/Google Forms — mandatory fields, upload, identity from login | Power Automate + SharePoint list, or Zoho Creator, for routing by amount |
| Register | The form's response sheet plus status and stage columns | List or database with per-stage permissions |
| Notification | WhatsApp, manually, with the request ID and link | Automated on status change |
| Approval evidence | Approver's own submission, timestamped and attributable | Native approval workflow — check the AutoCount edition first |
| Documents | One folder per request ID; `PR-2026-0001_Payee_Amount.pdf` | Attached to the PV in AutoCount as the single record |
| Payment | Portal maker–checker, two user IDs, two devices | Bulk payment file generated from approved requests |

WhatsApp may carry a notification, a link and a status. It must not carry the approval decision, the
bank account number, or the only copy of an invoice.

## 11. Compliance checkpoints to capture at screening

- **e-Invoice** — whether a valid e-Invoice was received, whether the case is self-billed (typically
  foreign suppliers and certain commissions), or outside the mandate for now. Confirm the current
  phase against the entity's turnover band; the rollout is staged.
- **Withholding tax** — payments to non-residents (technical fees, royalties, interest, certain
  services and rentals) attract withholding at rates depending on payment type, remittable within a
  month of paying or crediting. Late means a penalty *and* a disallowed deduction. The only reliable
  moment to catch it is before release.
- **SST** — whether service tax applies and the supplier's registration number is on the invoice.
- **Record retention** — seven years, retrievable in Malaysia; digital-first needs a real backup.
- **Personal data** — payee and claimant bank details belong in a permissioned folder, not a chat.
- **Statutory deadlines** — EPF, SOCSO, EIS, MTD belong on the recurring schedule with due dates.

**Independence.** If the practice acts as Maker for an entity it also audits, that is a self-review
and management-responsibility threat and is not curable by disclosure. Decide per client which side
of the line you are on and place the Maker role accordingly.

## 12. Sequence

**Week 1–2 — make the trail exist.** Name the roles and split portal credentials · adopt the
approval matrix by resolution · build the register and form · start the beneficiary master from the
last twelve months · redraw the diagram (delete the cheque branch and the duplicated checker box,
renumber).

**Week 3–6 — close the loop.** Add stages 6 and 7 · write the one-page exception report (payments
without a PV, PVs without approval, emergency payments, new payees, returned payments, requests past
SLA) · fix the filing convention · publish the claim policy · approve the recurring schedule for the
year.

**Month 2–3 — make it fast.** Automate status notifications · batch payment runs on one or two fixed
days · report quarterly to the committee (value paid, days from request to payment, exceptions
raised and cleared) · only then evaluate a workflow tool.

## 13. To confirm

1. What is DEP? Read here as the entity or fund code; society vs company vs client fund changes the
   approvers and the governing statute.
2. How many people are actually available? §5 changes materially at three versus five.
3. Which bank portal? Maker–checker limits, bulk upload format and audit-log export all differ.
4. Which AutoCount edition and version? Decides whether a separate form and sheet are needed at all.
5. Volume — payments per month, and how many are claims. Under ~30 a month a spreadsheet register is
   genuinely right for a long time.
6. Own practice, one client, or a template to roll out? If a service offering, build it as a
   template with a per-client approval matrix and the independence question answered per engagement.

---

Findings are ranked by audit exposure, not by effort. Amounts in the approval matrix are
illustrative bands to be set by the entity. Tax and compliance points are checkpoints to build into
the workflow — confirm current rates, thresholds and timelines with LHDN and against the entity's
own circumstances before relying on them.
