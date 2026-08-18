# DEP Payment & Claim Workflow — Internal Controls Review

Review of "Bank Payment Process Flow (DEP)" draft v1 · 18 August 2026

- **Entity:** Dhamma Earth Penang — Buddhist society registered with ROS
- **Governed by:** Societies Act 1966 and the society's own constitution
- **Scope:** supplier payments, member claims, dana & honorariums, cash collections
- **Systems assumed:** AutoCount, bank portal with maker–checker, WhatsApp

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

Then there is what the draft does not cover at all. A society spends money that was given to it
for stated purposes, so **which fund a payment comes out of is as important as who approved it** —
and nothing in the flow records a fund. The Treasurer's authority is not the committee's to set; it
is whatever the constitution grants, and internal policy may be stricter but never looser. And for a
temple the larger exposure is not the bank transfer at all — it is the dana box, the event collection
and the petty cash tin, none of which appear in the diagram.

Tally: 6 critical, 6 major, 2 notation.

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

**02 — Restricted funds are not tracked at all.** The request carries an invoice, a payee and an
amount, but never a fund. Donors give for a purpose — building fund, sangha dana, food offerings,
dhamma school, welfare, a named event. Money given for one purpose and spent on another is the
failure that costs a society its standing with its own members, and a flow recording only payee and
amount cannot detect it. At the AGM someone will ask what is left in the building fund; without a
fund tag that figure is reconstructed by hand, if it can be answered at all.
*Fix:* a mandatory fund field on every request; a fund register with opening balance, receipts,
payments and closing balance per fund; and a screening rule that a payment is released only against a
fund whose purpose covers it and whose balance bears it. Carry the fund in AutoCount as a project or
department code (§9).

**03 — Cash never enters the workflow.** The flow starts at a payment request and ends at a bank
transfer. Money coming *in* is absent, and so is any cash paid out. For a temple that is where the
exposure actually sits: dana boxes, offering trays, event and retreat collections, book and CD sales,
food stall takings, red packets and the petty cash tin — no dual control, no signed count sheet, no
pre-numbered receipt, no rule that collections are banked intact. Unlike a bad bank payment,
uncounted cash leaves nothing to reconcile against; the loss is undetectable by design.
*Fix:* §10. Two people at every collection point, a signed count sheet, banked intact, and no expense
ever paid out of a collection. This is the part to implement first, ahead of anything bank-side.

**04 — Maker and checker are not held apart.** Delete the checker box from the Maker lane. Write
down who is Maker, who is Checker, and each one's standing alternate. Never share portal
credentials or tokens. Replace "Treasurer *or* Assistant Treasurer" with a rule: the Assistant acts
only on SLA breach or recorded leave, and the register captures which one approved.

**05 — Authority is not anchored to the constitution.** A RM200 offering-tray purchase and a
RM80,000 renovation payment take the same path and the same one signature. There is no threshold at
which a second approver, a committee resolution or a general meeting is required — and for a society
that is not merely weak: the Treasurer's spending authority is not the committee's to invent, it is
whatever the constitution grants, so a flow that ignores the constitution can authorise payments the
society had no power to make. Nothing asks "is this budgeted?" either.
*Fix:* read the constitution's finance clauses first, then write the matrix inside them (§6).
Stricter than the constitution is always allowed; looser needs an amendment lodged with ROS, not a
committee decision to ignore it. Add a budget line and an over-budget/resolution-reference field.

**06 — Payee bank details are never verified independently.** This is how small organisations
actually lose money: a real invoice with a substituted account. Because the Checker reviews what the
Maker keyed rather than what was approved, a changed account passes both pairs of eyes.
*Fix:* a beneficiary master list; call-back verification on any new payee or changed details, using
a number from your own records, logged by name and date; the Checker verifies against the approved
request and the master, not the Maker's screen.

### Major

**07 — The flow ends at filing, not at the bank statement.** No bank reference, no advice, no
notification to requester or payee, no reconciliation, and no path at all for a failed or returned
transfer. *Fix:* add stages 6 and 7 (§4).

**08 — Claims are treated as supplier payments.** As drawn, the Treasurer can submit and approve
their own mileage claim, and no policy limits are tested. *Fix:* a claim variant — claimant never
in their own approval chain; screening checks the claim against written policy.

**09 — No rule for paying a member or a member's business.** Societies buy from their own members
constantly — a committee member's printing shop, a member's catering, a relative's contracting.
Usually nothing is wrong with it and it is often the cheapest option. But with no declared interest
and no abstention on record it is indistinguishable afterwards from self-dealing, and the person it
hurts most is the honest committee member who has to answer the question years later.
*Fix:* a related-party flag on the request; the interested person declares and abstains, and the
abstention is minuted. Comparative quotations above a stated amount. A standing register of committee
members' business interests, refreshed at the first meeting after each AGM.

**10 — Rejections and returns have no state and no log.** "Documents complete? → No" loops back to
START; "Reject and notify requester" has no exit. *Fix:* the status model in §7; Returned is
distinct from Rejected, and a second return escalates instead of looping.

**11 — One path for payments that are not ad-hoc.** EPF, SOCSO, EIS, monthly tax deductions, SST,
rent, utilities and standing instructions have no requester. Forcing them through the ad-hoc path
means the path gets bypassed or deadlines get missed. *Fix:* three named variants (§8).

**12 — Conditional bookkeeping and duplicated filing.** "Record transaction if Maker didn't issue
PV" is a step that eventually happens to nobody, and two parallel files will diverge. *Fix:* the PV
is always raised in AutoCount before release; go digital-first, with the PDF on the PV as the
record. Retain seven years, retrievable in Malaysia.

### Notation

**13 — Numbering, lanes and colours disagree.** Step 5 used twice; "Submit for approval" sits in
the Treasurer lane but is coloured as a requester activity; END sits in the Checker lane although
filing is the last activity, in the Maker lane.

**14 — A dead cheque branch is still drawn.** Delete it from the DEP flow; if another entity issues
cheques, that is a separate variant document.

## 4. Recommended flow — seven stages on one spine

Every stage reads from and writes to a single register row. Stages 3 and 5 are hard gates.

| # | Stage | Owner | Must check | Evidence produced | SLA |
|---|-------|-------|------------|-------------------|-----|
| 1 | Request | Requester | Mandatory fields; supporting document; declaration | Register row, request ID, attachments | — |
| 2 | Screen | Finance admin | Legibility; invoice not already in register; payee on master; **fund named and balance sufficient**; objective class; related party declared; budget line and GL code; e-Invoice / WHT flags | Screening note, coding, duplicate-check result | 1 wk day |
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

| Workflow role | At Dhamma Earth Penang | May never also be |
|---|---|---|
| Requester | Any member, volunteer or event/project coordinator — and the Treasurer for routine bills | — |
| Approver | Treasurer, plus President or a second committee member per the matrix band | The Checker on that payment |
| Maker | Assistant Treasurer, or the admin/accounts person if there is one | The Checker on that payment |
| Checker | President, or another authorised signatory named in the bank mandate | The requester or an approver of that payment |
| Outside the flow | The honorary auditors elected at the AGM — they examine, never approve or release | Committee members, if the constitution says so — check the clause |

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

## 6. Approval matrix, nested inside the constitution

Read the society's finance clauses first. Almost every constitution fixes a petty cash ceiling for
the Treasurer, names the signatories, and sets a figure above which the committee or a general
meeting must decide. The matrix has to sit *inside* those limits. Stricter than the constitution is
always allowed; looser is not a committee decision — it needs an amendment lodged with ROS.

| Payment | Approver(s) at stage 3 | Additional requirement | Checker at stage 5 |
|---|---|---|---|
| Petty cash, up to RM 200 | Treasurer alone, from the imprest float | Voucher plus receipt; never from a collection | Reviewed at the monthly float reconciliation |
| Up to RM 1,000 | Treasurer, or Assistant in their absence | Within an approved budget line | Any other authorised signatory |
| RM 1,001 – 5,000 | Treasurer + President | Within an approved budget line | Signatory who is neither approver |
| RM 5,001 – 20,000 | Treasurer, President + committee resolution | Minuted resolution referenced; quotations compared | Signatory who is neither approver |
| Above RM 20,000, or any building/capital item | As the constitution requires — committee, or a general meeting | Check whether a general meeting is required before relying on a committee vote | Signatory who is no approver |
| From a restricted fund, any amount | Approver per band, fund named | Fund purpose covers it; fund balance bears it | Checker confirms the fund on the PV |
| To a member, committee member, or their business | Approver per band, interested person abstaining | Interest declared, abstention minuted; quotations above a stated figure | Checker confirms the declaration is on file |
| Dana or honorarium to a visiting teacher | Within the annual dana policy — no further approval | Outside the policy, a fresh committee resolution | Two people sign the voucher if handed over in cash |
| New payee / changed bank details | Approver per band | Call-back logged: who called, which number, when | Checker confirms against beneficiary master |
| Any amount, over budget | As above + committee | Resolution or budget revision referenced | As above |

**Three tests before the committee adopts it.** (1) No band grants more than the constitution
already allows. (2) No band lets one person carry a payment from request to release alone. (3) The
bank mandate's own limits match the matrix — if the mandate says "any two signatories" with no
ceiling while the matrix requires a resolution above RM 20,000, the portal will happily release a
payment the policy forbids. Fix the mandate at the same time, and minute the adoption.

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
| Dana & honorarium | Dana to invited monks and nuns, honorariums and travel for visiting dhamma teachers, offerings at Kathina and other observances | Categories and amounts set once a year in a committee-adopted dana policy rather than negotiated per visit; often needed in cash and close to the date | Within the policy no further approval; outside it, a resolution. Every disbursement gets a voucher, and where handed over in cash two committee members sign it as having done so. Travel and accommodation at actual cost against documents |
| Staff / member claim | Reimbursements, mileage, per-diem, petty cash, advances | Claim form with policy limits printed; receipt threshold; submission deadline | Claimant never in own chain; advances cleared before a new claim; stage 2 checks against written policy |

## 9. Restricted funds

A society's money is not one pot. Nearly every ringgit arrives with a purpose attached by the person
who gave it, and the committee holds it as steward of that purpose rather than owner of the money.
That makes the fund as material to a payment as the approval is — and it is the field the draft flow
does not have.

| Fund | Typical source | May pay for | Hard rule |
|---|---|---|---|
| General | Subscriptions, unearmarked dana, hall bookings | Any purpose within the objects | The only fund that should carry administration and overheads |
| Building / renovation | Appeals, named gifts, item sponsorship | Construction, renovation, fittings, related professional fees | Never operating costs — not even temporarily without a minuted resolution |
| Sangha dana | Offerings for monks and nuns, Kathina, requisites | Dana, requisites, medical needs, travel for invited sangha | Spent per the adopted dana policy |
| Dhamma education | Course contributions, book and material sponsorship | Teachers, materials, venue for classes and retreats | Course surpluses stay in this fund unless donors were told otherwise |
| Welfare / charity | Appeals for a named cause, relief drives | Assistance under the approved criteria | Beneficiary approval is a separate decision from payment approval |
| Event-specific | Table sales, event sponsorship, retreat fees | That event's costs | Surplus disposed of as announced to donors, or by resolution if nothing was said |

**Four rules that make the fund column mean something.**

1. **One payment, one fund.** If a cost genuinely spans two funds, split the request and let each
   carry its own approval. Do not average it and do not decide later.
2. **No fund goes negative.** Lending between funds is a committee decision with a minuted repayment
   date, not a bookkeeping entry someone makes to get a payment out of the door.
3. **Surplus goes where the appeal said it would.** If the appeal said nothing, the committee
   resolves and tells the members — before the next AGM, not at it.
4. **Administration is charged to general funds** unless the donor agreed otherwise in writing.
   Quietly loading overheads onto the building fund is the most common way a restricted fund is
   misused by people who never intended to misuse anything.

Carry the fund as a project or department code in AutoCount on *both* receipts and payments, and the
statement of funds becomes a report you run: `fund` · `opening_balance` · `receipts` · `payments` ·
`transfers_in_out` (minuted resolution only) · `closing_balance`. That answers the question members
actually ask at the AGM.

## 10. Cash & donations

Everything above protects money already in the bank. For a temple the exposure sits earlier — the
dana box, the offering tray, the event table, the petty cash tin. Implement this section first.

| Collection point | Minimum control |
|---|---|
| Dana / donation boxes | Sealed and numbered. Opened only by two people together, on a stated schedule — never ad-hoc. Count sheet signed by both, box re-sealed, new seal number recorded. Banked intact |
| Offering tray, loose collection | Counted by two immediately after the event, before anyone goes home. Count sheet signed by both. Nothing paid out of it |
| Named donations | Pre-numbered official receipt issued, donor name and fund recorded. Receipt books controlled and the full sequence accounted for, including cancelled and spoiled receipts |
| Event fees, table sales, retreat contributions | Reconcile collections to the registration or seating list, so there is an independent expectation of what should have come in |
| Book, CD, item sales | Stock counted at open and close; takings reconciled to units sold |
| Online transfers, QR, DuitNow | Reconcile the bank credit list to the donor register weekly; chase unidentified credits while the donor still remembers |
| Petty cash | Fixed imprest float in a locked tin, one named custodian, replenished only against vouchers, no IOUs. Counted monthly by someone other than the custodian |

**Bank intact.** No expense is ever paid out of a collection, however convenient — the moment cash is
spent before it is banked, the link between what was given and what was recorded is gone and cannot
be rebuilt. **Dual control at every count.** One person alone with an unopened box is not a weakness
you can compensate for later; there is nothing to reconcile against.

None of this implies anything about anyone's integrity, and it is worth saying so when introducing
it. A count sheet with two signatures protects the volunteers who carry the box at least as much as
it protects the society, and it lets the committee answer a donor's question with a document rather
than a recollection. Where dana is handed to sangha directly, the record can travel with the
handover — two members signing a voucher — without intruding on the offering itself.

## 11. The register — build this first

Mandatory before approval marked ‡.

`request_id`‡ · `date_submitted`‡ · `requester`‡ · `type`‡ · `payee_name`‡ · `payee_bank`/`account`‡ ·
`amount`‡ · `invoice_no`‡ · `invoice_date`‡ · `due_date`‡ · `description`‡ · **`fund`‡** ·
**`objective_class`** (religious objects / welfare / administration / fundraising) ·
**`related_party`** · **`resolution_ref`** · `gl_code`/`cost_centre` ·
`budget_line`/`over_budget` · `einvoice_status` · `wht_applicable` · `attachments`‡ ·
`screened_by`/`at` · `approved_by`/`at` (+ matrix band applied) · `maker`/`at` ·
`checker`/`released_at` · `pv_no` · `bank_reference` · `status`‡ · `status_reason` · `reconciled_on`

## 12. Tooling

| Layer | Start with | Upgrade to when volume justifies |
|---|---|---|
| Intake | Microsoft/Google Forms — mandatory fields, upload, identity from login | Power Automate + SharePoint list, or Zoho Creator, for routing by amount |
| Register | The form's response sheet plus status and stage columns | List or database with per-stage permissions |
| Notification | WhatsApp, manually, with the request ID and link | Automated on status change |
| Approval evidence | Approver's own submission, timestamped and attributable | Native approval workflow — check the AutoCount edition first |
| Fund tracking | A fund column on the register, reconciled monthly to the bank | Project or department codes in AutoCount on both receipts and payments, so the statement of funds is a report |
| Documents | One folder per request ID; `PR-2026-0001_Payee_Amount.pdf` | Attached to the PV in AutoCount as the single record |
| Payment | Portal maker–checker, two user IDs, two devices | Bulk payment file generated from approved requests |

WhatsApp may carry a notification, a link and a status. It must not carry the approval decision, the
bank account number, or the only copy of an invoice.

## 13. Compliance & governance

Each item is only cheap if the payment flow captures it at the moment of payment. Treat these as
checkpoints to confirm for DEP's own circumstances, not as advice to rely on — rates, thresholds,
forms and deadlines move.

- **ROS annual return & accounts** — accounts prepared, examined by the honorary auditors, adopted at
  the AGM, and the annual return lodged with the Registrar within the period the Societies Act allows
  after the AGM (commonly cited as 60 days; confirm the current form and deadline). The register plus
  a statement per fund turns this from a month of archaeology into a week of assembly.
- **Committee changes** — must be notified to ROS, and the bank mandate and portal users have to
  change at the same time (§14). The notification usually gets done; the portal usually does not.
- **Income tax status** — income of a religious institution not operating for profit and established
  for religious worship or the advancement of religion is exempt under Schedule 6 of the ITA; confirm
  which paragraph and conditions apply to DEP. Do not assume it stretches to unrelated commercial
  activity such as hall rental to outside parties or trading — which is why the objective
  classification on each payment is worth capturing.
- **Donation deductibility (s.44(6))** — if DEP holds approval, or wants to apply, the approval
  carries annually tested conditions on how much income must be spent on the society's objects and
  how much may go to administration. Those ratios are only reportable if every payment was classified
  by objective when it was made. Confirm the current guideline and figures with LHDN.
- **Withholding tax on visiting teachers** — fees, honorariums or service payments to a non-resident
  for services performed in Malaysia can attract withholding, remittable within one month of paying
  or crediting, with a penalty for late remittance. Note the trap: **DEP's own tax exemption does not
  remove the duty to withhold** — the tax belongs to the recipient's income, not the society's.
  Whether a genuine dana carrying no service obligation is a payment for services at all is a real
  grey area. Get it confirmed before the teacher arrives, not after the money has gone.
- **e-Invoice** — a society under the announced turnover exemption is likely outside the obligation
  to *issue*; confirm DEP's position. Two purchase-side duties can still bite: obtaining valid
  supporting documents from suppliers, and issuing a *self-billed* e-Invoice where required, which
  typically covers foreign suppliers and acquisitions from individuals not carrying on a business —
  precisely the honorarium, freelance helper and small-vendor case.
- **SST** — whether service tax applies to what the society buys, and to anything it charges for.
- **Records** — seven years, retrievable in Malaysia. Digital-first is better, but the backup cannot
  be one person's laptop and certainly not a WhatsApp history.
- **Personal data** — donor lists, payee accounts and claimants' details are personal data, and the
  donor register is among the most sensitive things the society holds. Permissioned folder, not a
  group chat.
- **Statutory deadlines** — if DEP has employees (resident teacher, caretaker, admin staff), EPF,
  SOCSO, EIS and monthly tax deductions have fixed dates and belong on the recurring schedule.

**Where your own firm sits.** If the practice keeps DEP's books or acts as Maker, no partner or staff
member should also be one of the honorary auditors examining those accounts — and if a partner sits
on the committee, the firm should not be examining them either. Any fee the society pays the firm is
a related-party payment under §6: declared, minuted, and approved by someone with no interest in it.
None of this stops you helping; it decides which seat you help from.

## 14. Handover at the AGM

Committees turn over, and handover is where controls quietly lapse — most often because the outgoing
Treasurer still has portal access eighteen months later. Adopt this as a checklist signed by the
incoming and outgoing office-bearers together.

| Item | When | Why |
|---|---|---|
| Unbanked collections banked; petty cash counted jointly and the count signed | Before the AGM | Nobody should inherit an unexplained shortfall, or be blamed for one |
| Bank mandate updated; outgoing signatories removed | Within days of the AGM | An ex-office-bearer who can still authorise payments is the most common finding of all |
| Portal user IDs created for incoming, **deleted** for outgoing; tokens returned | Same week | Deactivated accounts get reactivated; deleted ones do not. Check today whether any past member is still a user |
| Register, fund statements, beneficiary master and receipt books handed over against a signed list | At handover | The incoming Treasurer should inherit a position, not a shoebox |
| Donation box keys and seals reissued, new seal numbers recorded | At handover | Old keys in circulation defeat the dual-control rule in §10 |
| AutoCount and cloud logins reassigned; shared passwords changed | Same week | Shared credentials make every control here unattributable |
| Approval matrix and dana policy re-adopted or confirmed; interests register refreshed | First committee meeting | A new committee has not agreed to the old committee's limits until it says so |
| ROS notified of office-bearer changes | Per the Act | Statutory, and it dates the mandate change for the bank |

## 15. Sequence

Do not start with the bank flow. The first fortnight is about cash and authority, because those are
the two places where a gap cannot be repaired retrospectively.

**Week 1–2 — close what cannot be fixed later.** Dual control on every collection point: print the
count sheets, set the opening schedule, seal and number the boxes, announce the bank-intact rule ·
convert petty cash to a fixed imprest float and count it jointly today · read the constitution's
finance clauses and write the approval matrix inside them, adopt by resolution, fix the bank mandate
to match · name Maker and Checker with alternates, split portal credentials, delete any portal user
who is no longer an office-bearer · open the fund register with an opening balance per fund, minuting
any part that is an estimate.

**Week 3–6 — build the record.** Build the register and form (§11) · tag funds in AutoCount as
project or department codes, on receipts as well as payments · start the beneficiary master from the
last twelve months and set the call-back rule · add stages 6 and 7, reconciling bank and each fund ·
write the one-page exception report (payments without a PV, PVs without approval, emergency payments,
new payees, returned payments, related-party payments, any fund overdrawn, requests past SLA) · adopt
the claim policy and the dana policy, approve the recurring schedule, redraw the diagram.

**Month 2–3 — make it routine.** Produce the statement of funds monthly rather than once a year ·
fix a weekly payment run, which respects the fact that office-bearers are volunteers with day jobs ·
automate status notifications · assemble the honorary auditors' pack from what the workflow already
produces · adopt the handover checklist ahead of the next AGM, while nobody is under time pressure.

## 16. To confirm

The first two would let the templates in §6 and §9 be replaced with something specific to DEP.

1. **The constitution's finance clauses** — petty cash ceiling, who the signatories are, the figure
   above which the committee or a general meeting must decide, and whether the honorary auditors may
   be committee members.
2. **Which funds exist today, and does anyone know the balances?** If the answer is "roughly", then
   reconstructing opening fund balances is a small project of its own.
3. **Does DEP hold s.44(6) approval**, or is it applying? Either way the spending ratios drive the
   objective classification.
4. **How much comes in as cash** versus transfer, and how many collection points — boxes, events,
   sales? That decides how much of §10 is needed immediately.
5. **How many people are genuinely available and willing?** §5 changes materially between three and
   five.
6. **Which bank, and does the portal do maker–checker with per-user limits?** Related: is any past
   committee member still an active portal user right now?
7. **Which AutoCount edition and version?** Whether it carries project/department codes for funds,
   and whether it holds document approvals natively.

---

Findings are ranked by audit exposure, not by effort. Amounts in the approval matrix and the fund
structure in §9 are shapes to be set by the committee within the society's constitution — not
recommendations of specific figures, and the constitution prevails over anything here. Tax, ROS and
e-Invoice points are checkpoints to build into the workflow: confirm current rates, thresholds, forms
and deadlines with LHDN and the Registrar of Societies, and against DEP's own circumstances, before
relying on them.
