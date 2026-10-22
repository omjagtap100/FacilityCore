# FacilityCore — Business Requirements, Product Phase 1

This is the build document. Where an older Word brief still mentions a vendor directory, a payment gateway, or a guard login, this document wins. The companion drawings are `docs/data-model-and-flows.md` and `docs/erd.md`. The click-through screens are under `docs/wireframes/`.

The server will be a .NET API. This document does not choose folders, class names, or libraries. Those follow the layout you supply next.

Money is integer paise. A positive amount means the flat owes the society. A negative amount is an advance. Dates are stored in UTC and shown in Asia/Kolkata. The worked society is Palm Court CHS, Mumbai.

## 1. What the product is

FacilityCore is software for a cooperative housing society in the Mumbai region. The committee raises a monthly maintenance bill, the resident pays the society’s own UPI ID, and the treasurer confirms the bank reference. The same product also keeps complaints, notices, a yes/no poll, society papers, and an emergency SMS.

One installation serves many societies. Each society is a tenant. Every bill, payment, complaint, notice, and file belongs to exactly one society. A person signed into Palm Court must not see Lakeview. A query that can return a second society’s row is a defect.

The first release may run one society. The data and the access checks are still multi-tenant from the first row.

There are three surfaces:

| Surface | Who | Shape |
| --- | --- | --- |
| Resident phone | Owner, tenant, or an extra phone on a flat | Android, portrait |
| Committee phone | Secretary, treasurer, facility manager | Android, portrait, same sign-in family as the resident |
| Staff desk | FacilityCore staff, called system admin | Website, wide page, not a phone |

Owners and renters do not get a separate app. The committee does not get a desktop product in this phase. System admin is not a role inside a society and is not a phone screen.

FacilityCore does not hold the society’s bank login, does not take a cut of the maintenance, and does not move the money. The resident pays the society. The treasurer looks at the bank and confirms.

## 2. Who is involved

| Person | How they exist in the product | What they are for |
| --- | --- | --- |
| System admin | A role with no society | Creates the society and names the committee. Does not touch dues. |
| Secretary | One open role per society. The older brief called this Builder/Admin. | Flats, charges, settings, bills (with the treasurer), cash and cheque (with the treasurer), waivers, notices, polls, documents, handover. |
| Treasurer | One open role per society | The only person who confirms or rejects a reported UPI payment. Shares bills, cash, cheque, defaulters, reports, and history with the secretary. |
| Facility manager | One open role per society | Complaints. Receives the SOS SMS. Does not move money. |
| Resident | An occupancy on a flat, not a committee role | Own bills, own payments, own complaints, notices, one vote if they are the bill payer, SOS. |
| Duty guard | A mobile number on the society, not a user | Receives the SOS SMS. Cannot sign in. |
| Bill payer | One open occupancy flag per flat | The only phone that is reminded and the only phone that votes. |

A phone number is one person across the product. That person may be the secretary of one society and a resident of another, or the owner of two flats. Each sign-in token carries one role and, except for system admin, one society. The token cannot call another role’s actions. Choosing a second flat or a second role means a new token.

Replacing a secretary, treasurer, or facility manager closes the old assignment and opens a new one. Bills, payments, waivers, and the audit keep the person who actually did the old action. A later rename does not rewrite that history.

## 3. What this phase includes

Nine build steps, in this order. The order is dependency, not a calendar.

1. **Foundation.** System admin onboards a society and grants committee access. People sign in with a phone code. Language, society rules, flats, opening balances, wings, categories, parking, and handover.
2. **Bills.** The charge list, funds, levies, non-occupancy, parking charges, a draft bill run, send, and a recorded correction. A second run does not create a second bill.
3. **Collection.** UPI intent and QR, the resident’s reference, treasurer confirm or reject, a block on a reused reference, cash, cheque, bounce, advance, refund, and a payment posted to the wrong flat.
4. **Accounts.** Statement, receipt PDF, move-out, no-dues certificate, tenancy papers, interior-work deposit.
5. **Discipline.** Late fees, waiver, SMS and in-app reminders, share on WhatsApp by the resident, defaulter list for the committee only.
6. **Helpdesk.** Complaint with one photo, status, reminders, reopen, and a category list the secretary can extend.
7. **Community.** Yes/no poll, SOS, notice board, document vault, compliance dates.
8. **Reports.** Dashboard figures, Excel, immutable history, April–March export, backup, speed, privacy.
9. **Ready to extend.** Flags for later work exist and are off. A priced-service payment cannot reduce dues.

## 4. What this phase refuses

These are not hidden screens and not empty menus. They are absent.

- Local vendors, a phonebook, click-to-call, and any “within 2 km” directory.
- A services shop, a booking calendar, and a visitor or gate screen.
- Face scan, Aadhaar face match, boom barriers, cameras, and a guard login.
- A card or net-banking gateway, a commission, NACH, and any FacilityCore fee on the maintenance.
- GST returns, payroll, redevelopment, and HRA rent receipts.
- Automatic WhatsApp. The resident may open WhatsApp themselves and press send.
- Posting defaulter names to other residents or to a lobby display.
- Applying a provider’s payment to a maintenance bill or to a late fee.
- Editing or deleting a payment, a ledger line, or a history row.

Later work may use rows that exist now: a provider-payment row that is not maintenance, a consent row that stays empty, vehicle and parking rows for a future gate, and quantity and rate on a charge line for metered water. None of those have a screen in this phase.

## 5. Society lifecycle

**BR-001.** System admin creates a society with a name, a city, and an address. The society starts as Draft.

**BR-002.** Draft means the secretary can sign in and load flats, charges, and opening balances. Residents cannot sign in. Bills are not sent to residents while the society is Draft.

**BR-003.** System admin marks the society Active when the secretary is ready. Residents with an occupancy can then sign in.

**BR-004.** System admin can suspend a society. Suspension revokes every session for that society at once. Nobody in that society can sign in until it is Active again. System admin can still open the society record.

**BR-005.** The staff website lists every society and shows counts of societies, active, draft, and suspended. A resident, a secretary, a treasurer, and a facility manager never see that list.

**BR-006.** System admin grants access by mobile number and one role: Secretary, Treasurer, or Facility Manager. The first person required is the secretary. System admin does not type each resident, does not confirm a payment, and does not send bills.

**BR-007.** One society has at most one open secretary, one open treasurer, and one open facility manager. Granting the same role to a new mobile closes the previous assignment.

**BR-008.** Creating a society also creates, switched off, the four later flags: services, gate, face check, and automatic WhatsApp. It seeds the complaint categories, the notice types, and the message texts in English, Hindi, and Marathi.

**BR-009.** Staff actions that change a society or a role write an audit row: society created, role granted, role removed, society suspended. The audit cannot be edited.

Acceptance: a second society’s flats do not appear on Palm Court’s list. A resident token presented while the society is Draft or Suspended is refused. A system-admin token that tries to confirm a payment is refused.

## 6. Sign-in, language, and the privacy notice

**BR-010.** Sign-in is a mobile number and a six-digit SMS code. There is no password. The code is stored only as a hash, expires, and can be used once.

**BR-011.** The code is sent by the same SMS path as reminders. The society cannot switch OTP off.

**BR-012.** The first successful sign-in asks for English, Hindi, or Marathi, then shows what data the product keeps, and records that the person continued. Profile can change the language later. Another language is a new value, not a new release. Gujarati is not in the first three, but the language field is a code so it can be added without a migration.

**BR-013.** After the code is accepted, the server loads open committee roles and open occupancies. If the society is suspended, sign-in stops. If the person is only a resident and the society is not Active, sign-in stops. If the person has more than one role or more than one society, they choose one. The token is bound to that choice.

**BR-014.** Logout revokes that token. A new role needs a new code and a new token.

**BR-015.** The committee sign-in and the resident sign-in are the same OTP. The screens after the code differ by the role on the token.

**BR-016.** Guards have no account in this phase.

Acceptance: six wrong or expired digits do not open a session. A token issued as treasurer cannot waive a late fee. A token issued as secretary cannot confirm a UPI payment.

## 7. Society rules

The secretary saves these. They apply to the whole society.

| Rule | Value in this phase |
| --- | --- |
| Society name | Free text. Palm Court CHS in the sample. |
| Bill date | Day of the month the bill is raised. Default the 1st. |
| Payment window | An integer from 5 to 10 inclusive. Default 10. This is the only grace. There is no second grace on top of it. |
| Late-fee type | Simple or Growing. The starting selection is Simple. |
| Daily rate | Stored as parts per million. 500 means 0.05% per day. |
| Cap | Optional. Empty until the committee has read the bye-laws. When set, the fee stops at that amount. |
| UPI ID | A VPA. The save is refused unless the value contains `@`. Sample `palmcourt@okbank`. |
| Grievance contact | A name shown on the resident profile. Sample Mrs. Deshmukh. |
| Duty-guard mobile | A phone used only for the emergency SMS. It is not a login. |

**BR-017.** The product never asks for a UPI PIN, a bank password, or a card number.

**BR-018.** A window outside 5 to 10, or a UPI ID without `@`, is rejected and the previous saved values stay.

**BR-019.** Before a pilot, the committee reads its bye-laws and confirms the late-fee type and the cap. Many Maharashtra society bye-laws allow simple interest up to a yearly limit. The product does not invent that percentage. Growing remains available and is covered by the paisa test in section 16.

**BR-020.** The secretary can hand the secretary role to another mobile. That transfer closes the old assignment, opens the new one, revokes the old secretary’s session, and writes an audit row. Ledger signing and the right to send notices and polls move with the role. Past rows keep the old actor.

Acceptance: saving `palmcourt@okbank` succeeds. Saving `palmcourt-okbank` fails. Saving a window of 15 fails. After handover, the old phone cannot open settings, and a waiver they made last month still shows their name.

## 8. Flats, people, wings, and parking

**BR-021.** A flat belongs to one wing (or tower) and one category. The flat’s identity inside a society is the wing plus the number. `B-1204` in Wing B is one flat. The same number in Wing A is a different flat.

**BR-022.** Categories start as 1RK, 1BHK, 2BHK, 3BHK, penthouse, and shop. The secretary can add a label. A shop can carry a different charge from a flat in the same wing.

**BR-023.** The secretary can add one flat by hand or import a sheet. A second save of the same wing and number does not create a second flat and does not create a second opening line.

**BR-024.** People on a flat are owner, tenant, or extra. Each has a mobile. The owner is the bill payer until the secretary switches that flag to the tenant. Exactly one open bill payer exists on the flat.

**BR-025.** One mobile can be on several flats. Each flat has its own bill and its own balance. Extra numbers can view, pay, and complain. Only the bill payer is reminded. Only the bill payer votes.

**BR-026.** A phone that has not yet completed OTP can still be stored at import, so a reminder has somewhere to go. The first OTP is what sets the language.

**BR-027.** Changing a person’s phone updates that person. It does not rewrite old ledger or audit rows. Replacing a person ends the occupancy and starts another. History stays on the old person.

**BR-028.** The import sheet carries, at least: wing, flat number, category, owner, owner phone, tenant, tenant phone, who the bill payer is, extra phones, opening balance, and the as-of date. Before save, the screen shows the society total of the opening balances so the secretary can match it to the paper register.

**BR-029.** The opening line is the first line of the statement. It is positive when the flat owes and negative when the flat is in advance. Late fees do not run on that old balance unless the secretary opts in for that flat. Charge lines of new bills always can accrue.

**BR-030.** A parking slot has a wing, a label, and a vehicle type of car, bike, or a label the society adds. It may be linked to one flat. A vehicle has a plate and a sticker number and belongs to one flat. It may sit in one slot. Residents of that flat see their own vehicles. There is no barrier, no camera, and no guard login. The register exists so a parking charge, and a later gate, use the same rows.

**BR-031.** The secretary can mark a flat as let out. That flag is what allows a non-occupancy charge. It does not by itself create the charge.

Sample flat: Wing B, number 1204, category 2BHK, owner Anita Kulkarni, bill payer Anita Kulkarni, monthly total ₹4,850.

Acceptance: importing the same file twice leaves one B-1204 and one opening line. A tenant who is not the bill payer can open the bill and cannot cast the poll. A resident of A-101 cannot read B-1204’s agreement.

## 9. Charges

A charge item is the society’s price list. A sent bill copies the lines. Editing the price list later does not change a bill already sent.

**BR-032.** Kinds are monthly, annual, fund, levy, parking, and non-occupancy.

**BR-033.** A monthly item is added on every bill run. It may apply to every flat, or only to one category, so a 2BHK and a shop can differ. Sample lines for a 2BHK: base maintenance ₹3,200, lift ₹450, sinking fund ₹100.

**BR-034.** An annual item is added only in the month the secretary chooses, such as April.

**BR-035.** A fund item (sinking fund, repair fund) may appear every month. Reports can total funds apart from ordinary maintenance.

**BR-036.** A levy is one month and a chosen set of flats (tanker water, a festival collection, a special repair). Flats that were not chosen do not get the line.

**BR-037.** Non-occupancy ships inactive. The bill run adds it only when the secretary has saved a rate and the flat is marked let out. The software does not ship a legal percentage.

**BR-038.** A parking charge either points at one slot, in which case only that slot’s flat gets the line, or it names a vehicle type, in which case the flat is charged once for each linked slot of that type.

**BR-039.** Quantity and rate on a charge stay empty in this phase. A later metered-water charge can fill them without a new bill table. The bill line copies them when they exist.

**BR-040.** The secretary can add a charge without a new release. Names such as CCTV, security salaries, and common electricity are data, not a fixed menu.

Acceptance: changing base maintenance from ₹3,200 to ₹3,400 after March has been sent leaves March’s line at ₹3,200 and uses ₹3,400 only on a later unsent draft.

## 10. Bills

**BR-041.** On the bill day the system drafts one bill per flat for that month from the charge list: monthly items, an annual item if this is its month, levy targets for this month, an active non-occupancy line on a let-out flat, and the parking lines.

**BR-042.** One flat has at most one bill for a given year and month. Running the draft twice does not create a second bill and does not duplicate lines. A failed run can be repeated.

**BR-043.** Residents cannot read a draft. The sample draft is March 2026, 120 flats, ₹6,12,000, and B-1204 at ₹4,850.

**BR-044.** Secretary or treasurer sends the bills. Send stamps the send time, sets the due date, posts one charge line to the flat’s ledger for each bill line, and messages the bill payer if that message is switched on.

**BR-045.** The due date is the last day of the payment window, counting the bill day as day 1. A bill raised on 1 March with a 10-day window is payable through 10 March. Late fees start on 11 March, the day after the due date. A 5-day window is payable through day 5 and the fee starts on day 6. The window is the only grace.

**BR-046.** A sent bill is never edited in place. A correction is a new adjustment line plus a ledger row. The person, the time, and the reason are required. The original lines stay.

**BR-047.** Bill status is one of: Draft, Sent, Partially paid, Awaiting confirmation, Paid. Overdue is not a status and not a second balance. It is calculated: the flat still owes, the due date has passed, and the payment is not sitting in Awaiting confirmation.

**BR-048.** Status moves in this order. A reported UPI payment makes the bill Awaiting confirmation. A zero balance makes it Paid. Some credit and a remaining balance makes it Partially paid. Otherwise, once sent, it stays Sent.

Acceptance: two draft runs for March produce one bill for B-1204. A resident token that asks for drafts receives none. An adjustment without a reason is refused.

## 11. The ledger

The flat’s balance is the sum of its ledger. Interior-work deposits are not in that sum. A provider payment is not in that sum.

**BR-049.** Ledger kinds are: opening, charge, late fee, payment, adjustment, waiver, bounce, bounce charge, and refund.

**BR-050.** A payment row is never deleted. A mistake is a reversing ledger row and, where needed, a new payment row. The history screen cannot be edited.

**BR-051.** Each ledger row stores the person who caused it and a copy of their name at that moment, plus the reason where the rule requires one.

**BR-052.** Incoming money is allocated to late fees first, then to the oldest unpaid bill. Money beyond the dues is an advance (the balance goes negative) or a recorded refund. The next bill consumes an advance.

**BR-053.** A short payment leaves a positive balance. The bill stays Partially paid.

## 12. Paying

### UPI, which is the resident’s normal path

**BR-054.** Pay now opens the phone’s UPI app with the society VPA, the society name as the payee, and the exact amount due. The UPI note carries the flat number so the bank statement is easier to match. From Home, the UPI app is at most three taps away. Pay now opens in under two seconds on a normal connection.

**BR-055.** If no UPI app opens, the resident sees a QR and the UPI ID. The amount and the flat number are still shown.

**BR-056.** When the amount is above the usual per-payment UPI limit, the screen says so and points to a part payment or to paying the rest with the treasurer. No card gateway is added.

**BR-057.** After paying, the resident reports the bank reference (UTR) and may attach one screenshot. The payment is then Reported. The bill shows Awaiting confirmation. The late-fee job adds nothing while that payment is Reported.

**BR-058.** A reference that already sits on a Reported or Confirmed payment in that society cannot be submitted again. A reference that was only Rejected can be sent again. Two flats submitting one live reference: the second is refused.

**BR-059.** Only the treasurer confirms or rejects. Confirm writes the negative ledger row, allocates it, stops the late fee, and makes a receipt. Reject stores a reason, tells the resident, and the days spent waiting count as late days. The reference may be sent again after a reject.

**BR-060.** A field is kept for a future automatic bank match. It stays empty. Matching in this phase is the treasurer looking at the bank.

Sample waiting rows: B-1204 Anita Kulkarni ₹4,850 reference `123456789012`; A-202 ₹4,850; C-104 ₹5,200.

### Cash and cheque

**BR-061.** Secretary or treasurer records cash or cheque with the flat, the amount, the date, and the cheque number when the mode is cheque. The payment is Confirmed at once. The resident receives a receipt. Cash may have no reference. A cheque number is stored and is not under the UPI-uniqueness rule.

**BR-062.** A bounced cheque posts the amount back as a positive bounce. An optional bounce charge may be added. Late fees still use the original due date. The original payment row stays.

**BR-063.** A payment posted to the wrong flat is not dragged onto the right flat. The ledger is reversed with a reason, and a new payment is written on the right flat, pointing at the one it corrects. The original row stays.

Acceptance: the secretary’s token cannot confirm the UPI reference `123456789012`. The treasurer’s confirm of ₹4,850 on a ₹4,850 bill leaves B-1204 at zero and stops the fee. Rejecting it with no reason is refused. Submitting `123456789012` again while it is Reported is refused.

## 13. Late fees and reminders

**BR-064.** The late-fee job considers bills past the due date that are not paid. If a payment on that bill is still Reported, the job skips it. On rejection, the skipped days accrue.

**BR-065.** Simple: late fee = unpaid principal × daily rate × late days. Growing: amount due = principal × (1 + daily rate) raised to the power of late days. The fee posted each day is only the increase since the previous day, so a rerun on the same day does not charge twice.

**BR-066.** A cap, when the society has set one, stops the fee. Amounts are kept to the paisa and shown with two decimals.

**BR-067.** The opening balance is part of the principal only when that flat was opted in. New charge lines accrue.

**BR-068.** Only the secretary can waive a late fee, and only with a written reason. The waiver is its own ledger row and an audit row. The treasurer cannot waive.

**BR-069.** The bill payer is messaged, in their language, when the bill is sent, three days before the due date, on the due date, when the late fee starts, and once a week while a balance remains. The society can switch any of those reminders off. OTP cannot be switched off.

**BR-070.** Secretary and treasurer can open and export the flats with an overdue balance. A resident token that asks for that list is refused. The product does not publish the names to other residents.

Acceptance is the paisa test in section 16, plus: a reported payment on day 2 adds no fee on day 3; a rejection on day 4 then accrues the waiting days; a waiver without a reason is refused.

## 14. Statement, receipt, move-out, papers, and the work deposit

**BR-071.** The resident’s statement is one list: opening balance, charges, late fees, payments, adjustments, and the current balance. It is per flat.

**BR-072.** A receipt PDF is in the resident’s language and shows the society, the flat, the mode, the amount, and the balance. It can be downloaded in the app.

**BR-073.** Every bill and every receipt has a share control that opens WhatsApp with the text already written. The resident sends it. The product does not call the WhatsApp Business API in this phase.

**BR-074.** The secretary records a move-out or sale date. Dues stay on the flat. They do not move to the person and they do not disappear.

**BR-075.** A no-dues certificate PDF can be created only when the flat’s ledger sum is zero. It names the flat, the date, and the society. A positive balance disables it.

**BR-076.** The secretary or the owner can store a rental agreement and a police-verification file, each with a date. FacilityCore does not perform police verification. It only keeps the file the society already has. The secretary and the people on that flat can read it. Any other flat’s token cannot.

**BR-077.** A resident submits interior-work dates. The secretary approves or refuses. A refundable deposit is recorded on its own list as collected and, later, refunded. It is not income, not a maintenance charge, and not a base for late fees. Collecting or refunding it leaves the maintenance balance unchanged. The sample on B-1204 is 14 April to 30 April and a deposit of ₹10,000.

Acceptance: sharing a receipt does not create a message row. A no-dues certificate for B-1204 while ₹4,850 is open is refused. After the ledger is zero, the certificate is created. The ₹10,000 deposit does not appear in the maintenance balance.

## 15. Complaints

**BR-078.** A resident raises a complaint on their flat: a category, an optional photo, and an optional one-line place such as “bathroom”.

**BR-079.** Categories are seeded per society: plumbing, electrical, lift, drainage choke, seepage, water supply, DG and power backup, parking, noise, and common-area cleaning. The secretary can add one. Adding one does not need a release.

**BR-080.** Status is Pending, In progress, or Resolved. The secretary and the facility manager can see the society’s complaints and move the status. The guard is not notified. Each move is appended. The old status event is not overwritten.

**BR-081.** The resident is told on each status change.

**BR-082.** Still pending after 3 days: a reminder to the facility manager. Still pending after 7 days: a reminder to the secretary. The committee wireframe text that says day 8 is not the rule. The rule is day 7.

**BR-083.** Within 7 days of Resolved, the resident can mark it not fixed. That appends a Pending event and clears the resolved time. After 7 days it stays resolved.

Acceptance: a complaint on B-1204 is invisible to A-101. A resident token cannot set the status to Resolved. Reopening on day 8 after resolution is refused.

## 16. Notices, polls, the vault, and SOS

**BR-084.** The secretary publishes a notice of type water, tanker, lift, meeting, or general. Title and body are stored per language. The resident home shows the text for that resident’s language. A notice does not create a bill and does not collect a vote.

**BR-085.** The secretary sends a yes/no question with a closing time. One vote per flat, cast by the open bill payer. A second vote from the same flat is refused. A vote after the close is refused. A person who is not the bill payer is refused. Results update as votes arrive, and the resident can see that their flat has voted. The sample question is whether to approve the lift AMC, closing 20 March, with a running tally of 62 yes and 21 no.

**BR-086.** The secretary uploads society documents: bye-laws, occupancy certificate, fire NOC, lift licence, insurance, and meeting minutes. Any resident of that society can read them. A resident of another society cannot.

**BR-087.** The secretary records a date for lift AMC, fire NOC, building insurance, and the AGM. The committee dashboard lists items due within 30 days. This reminds the committee. It does not book a contractor. The sample shows the fire NOC inside that window.

**BR-088.** The resident home has a clear SOS control. The resident picks medical, fire, or security. An SMS goes to the open secretary, the open facility manager, and the duty-guard number, with the flat number and a callback phone. The handoff to the SMS provider is tested against 10 seconds. The guard still has no login.

Acceptance: a notice in Hindi is what a Hindi resident sees, even if the secretary also stored English. A tenant who is not the bill payer cannot vote. An SOS does not create a complaint and does not create a bill.

## 17. Reports, history, backup, and privacy

**BR-089.** The committee dashboard for a chosen month shows collected, pending, overdue, late fees, and the total still awaiting confirmation. The sample month is March 2026: collected ₹3,20,000, pending ₹1,10,000, overdue ₹42,500, and ₹18,400 waiting. Overdue excludes drafts and excludes payments still reported.

**BR-090.** The same figures can be taken for the Indian financial year, April through March. Fund kinds are totalled apart from ordinary maintenance. The export lists bills, receipts, adjustments, outstanding, and those fund totals. It is a pack for the society’s chartered accountant. It is not a balance sheet and not a GST return.

**BR-091.** Those reports download to Excel. The sample funds on the month view are sinking fund ₹12,000 and repair fund ₹12,000.

**BR-092.** Money history lists every bill, adjustment, payment, confirm, reject, and waiver with who and when. The list cannot be edited or deleted. A resident does not see other flats’ history.

**BR-093.** A backup runs every day, is kept 30 days, and a restore is tested before the pilot. This is an operations duty, not a screen.

**BR-094.** Main screens open in under 3 seconds on a normal mobile connection. A bill already opened can be read with no connection. Pay, complaints, and SOS need a connection. The resident app targets Android 8 or newer. The staff website runs in current Chrome and Edge. The availability target for the core is 99.5%.

**BR-095.** The product keeps name, phone, flat, and the files the society uploaded. Data is protected in transit and at rest. Phone numbers are hidden from other residents. Database and files are hosted in an Indian region.

**BR-096.** Profile shows the grievance contact. A resident can ask to see their data. That request is stored for the grievance contact. The society can ask for an export of its own data. Both requests are recorded.

**BR-097.** With the four flags off, the resident payload has no booking block, no visitor block, and no vendor block. Home shows the current bill, Pay now, SOS, helpdesk, polls, and notices.

**BR-098.** Any attempt to apply a provider payment to a bill, a ledger row, or a late fee is refused. The provider’s UPI ID is a different field from the society’s UPI ID.

## 18. The paisa test

This test is the definition of the late-fee arithmetic. Principal ₹3,000. Daily rate 0.05% (500 parts per million). No cap.

| Late day | Simple fee | Simple amount due | Growing amount due |
| --- | --- | --- | --- |
| 1 | ₹1.50 | ₹3,001.50 | ₹3,001.50 |
| 10 | ₹15.00 | ₹3,015.00 | ₹3,015.03 |

Day 1 does not tell Simple from Growing. Day 10 does. The growing figure is principal × (1.0005) raised to the 10th power, kept to the paisa: ₹3,015.03. A rerun of day 10 posts no further fee. A cap of ₹10.00 would stop the simple fee at ₹10.00 even on day 10.

Allocation check, separate from the rate check: a flat owing ₹15 late fee and ₹3,000 principal receives ₹1,000. The ₹15 fee is cleared first. ₹985 goes to the principal. The balance left is ₹2,015 of principal and no fee.

## 19. What each role may do

An empty cell is refused.

| Action | System admin | Secretary | Treasurer | Facility manager | Resident |
| --- | --- | --- | --- | --- | --- |
| Create a society, set Draft, Active, or Suspended, grant or remove a committee role | yes | | | | |
| Flats, import, opening balances, charges, settings, handover, notices, polls, documents | | yes | | | |
| Send bills, record cash or cheque, defaulter list, reports, money history | | yes | yes | | |
| Confirm or reject a reported UPI payment | | | yes | | |
| Waive a late fee, and only with a reason | | yes | | | |
| See and move complaints | | yes | | yes | own flat, raise and reopen |
| Vote, read notices and society documents, SOS, own statement | | | | | yes |
| Apply a provider payment to dues | no | no | no | no | no |

While the society is Draft, the secretary’s column still works and the resident’s column does not. While Suspended, every column except system admin stops.

## 20. Messages

Texts are data, one body per language, with a platform default and an optional society override. The secretary can switch a reminder off. The keys in this phase are:

| Key | When |
| --- | --- |
| OTP | Sign-in. Cannot be switched off. |
| Bill sent | Secretary or treasurer sends the month. |
| Reminder before | Three days before the due date. |
| Reminder due | On the due date. |
| Late started | The first day a fee is posted. |
| Weekly balance | Once a week while the flat still owes. |
| Complaint status | Each status change, including reopen. |
| Payment rejected | Treasurer rejects, including the reason. |
| SOS | The three recipients in BR-088. |

Channels in this phase are SMS and in-app. Share-on-WhatsApp is not one of these rows. SMS uses a DLT-registered sender. The body is in the recipient’s language.

Only the bill payer receives the money messages. Extra phones on the flat do not.

## 21. Screens this phase must cover

The HTML under `docs/wireframes/` is the accepted layout. Behaviour that disagrees with this document follows this document. The wireframes are English; every string they show must also exist in Hindi and Marathi.

Resident, in order: choose language, mobile number, code, data notice, home when an amount is due, home while a payment is waiting, home when nothing is due, bill breakup, UPI reference, statement, receipt, complaints, new complaint, complaint detail, notices, poll, profile, emergency.

Committee, in order: sign in, code, overview, flats, one flat, import, charges, review bills, confirm payments, record a payment, late fees, complaints, polls, notices, documents, reports, history, settings.

Staff website: society list, new society, society access.

There is no vendor screen, no visitor screen, and no face-scan screen on any of the three.

## 22. Worked month for Palm Court

Use this as the fixture. Do not invent a second society’s numbers into Palm Court’s totals.

- Society Palm Court CHS, Mumbai, UPI `palmcourt@okbank`, bill day the 1st, window 10 days, late fee Simple at 0.05% per day, cap empty, grievance contact Mrs. Deshmukh.
- March 2026 bill for B-1204, Anita Kulkarni, 2BHK: base ₹3,200, lift ₹450, sinking fund ₹100, and the rest of the catalogue to a bill of ₹4,850. Due 10 March.
- She pays by UPI and reports reference `123456789012`. The bill waits. The fee does not grow. The treasurer confirms. The balance is zero. A receipt exists. The same reference cannot be used by A-202.
- If the treasurer rejects that reference, she is told, the days since 11 March count, and she may submit a new reference.
- A-101 is a shop with ₹12,000 overdue. It appears on the defaulter list for the secretary and the treasurer and does not appear on any resident’s home.
- Fire NOC is inside the 30-day reminder. The dashboard shows that date. No work order is created.
- A poll asks the bill payers to approve the lift AMC and closes on 20 March. One vote per flat.
- A water-shutdown notice is on the resident home in that resident’s language.

## 23. Acceptance scenarios

These are the checks a build is held to. Each one is pass or fail.

1. Staff create Palm Court as Draft, grant a secretary, and a resident OTP is refused.
2. Staff mark it Active. The resident OTP succeeds. Staff suspend it. That resident’s existing token stops working.
3. The secretary imports flats. The preview total matches the sum of the opening column. Saving twice does not double the flats or the opening lines.
4. A 2BHK and a shop on the same wing receive different base amounts when the charge is limited to a category.
5. March is drafted twice. B-1204 has one March bill. The resident cannot open it until send.
6. Send posts the charge lines, sets due 10 March for a 1 March bill with a 10-day window, and messages only the bill payer.
7. Pay now carries `palmcourt@okbank`, the amount due, and `B-1204` in the note. With no UPI app, the QR and the VPA are shown.
8. Two residents submitting one new reference: the second is refused. After a reject, the same reference can be submitted again.
9. Treasurer confirm clears late fees first, then the oldest bill, writes a receipt, and stops the fee. Secretary confirm is refused.
10. A cheque bounce restores the debit from the original due date. A payment on the wrong flat is reversed and rewritten, and both payment rows still exist.
11. The section 18 table matches to the paisa for Simple and Growing, including ₹3,015.03 on growing day 10, and a same-day rerun posts zero.
12. A waiver without a reason is refused. A resident token asking for the defaulter list is refused.
13. No-dues is refused at ₹4,850 and allowed at zero. A ₹10,000 work deposit does not change that balance.
14. A Hindi resident sees the Hindi notice. A non-payer cannot vote. A second vote from the flat is refused.
15. SOS handoff is at or under 10 seconds in the test, and the guard has no way to sign in.
16. The April–March export totals sinking fund and repair fund apart from ordinary maintenance, and the file is not labelled a balance sheet.
17. A provider-payment row cannot be allocated to B-1204’s bill.
18. With all four flags off, no API response used by the resident home contains a vendor, a booking, or a visitor.

## 24. What comes after this phase, and what never gets a table

| Later work | May use | Must not do in this phase |
| --- | --- | --- |
| Priced home services the resident pays directly | The provider-payment row and the services flag | Show a catalogue, take a booking, or reduce dues |
| A society cost recovered through the bill | A levy charge | Open a second ledger |
| Automatic WhatsApp | Consent and the message texts, flag off | Call the WhatsApp API |
| A gate, after a poll | Vehicles, slots, and the tenancy file, flag off | Visitor codes, guard login, barriers |
| Face check, after a privacy review | Consent, flag off | Store a face template |
| Metered water | Quantity and rate on the charge line | Add a meter register yet |

Held out with no extension point: the vendor directory, a gateway, commission, NACH, GST, payroll, redevelopment, HRA receipts, Aadhaar face match, and boom barriers.

## 25. Words

| Word | Meaning |
| --- | --- |
| Society | One tenant. Palm Court is one. Lakeview is another. |
| System admin | FacilityCore staff. Not a member of the society. |
| Secretary | The committee role the older brief called Builder/Admin. |
| Bill payer | The one occupancy that is reminded and that votes. |
| VPA | The society UPI ID, such as `palmcourt@okbank`. |
| UTR | The bank reference the resident types after paying. |
| Reported | The resident says they paid. The treasurer has not decided. Late fees wait. |
| Confirmed | The treasurer matched it to the bank. The ledger moves. |
| Rejected | The treasurer refused it, with a reason. The waiting days count. |
| Window | The 5 to 10 days after the bill day, including the bill day, before a fee starts. |
| Simple | Fee = principal × daily rate × late days. |
| Growing | Amount due = principal × (1 + daily rate) ^ late days. |
| Paise | The stored unit. ₹4,850 is 485000 paise. |
| Advance | A negative balance. The flat has paid more than it owes. |
| Draft society | Secretary may work. Residents may not. |
| Suspended | Every session for that society is dead. |
| Flag | A switch for a later product. All four start off. |

## 26. Trace

| Step | Requirements |
| --- | --- |
| 1 Foundation | BR-001–BR-031 |
| 2 Bills | BR-032–BR-048 |
| 3 Collection | BR-049–BR-063 |
| 4 Accounts | BR-071–BR-077 |
| 5 Discipline | BR-064–BR-070, section 18 |
| 6 Helpdesk | BR-078–BR-083 |
| 7 Community | BR-084–BR-088 |
| 8 Reports | BR-089–BR-096 |
| 9 Ready to extend | BR-097, BR-098, section 24 |
