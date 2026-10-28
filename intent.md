# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md`
(discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17
and is history only: nothing in this intent is approved on its authority.

## Problem

- **P-1** The fictional clinic receives avoidable calls from patients checking whether their results are
  ready. (PRD: "receives avoidable calls from patients checking whether results are ready.")
- **P-2** Today patients have no signal that a reviewed result is waiting for them, so they phone to ask.

## Proposed outcome

- **I-1** The patient is notified when a result is ready for their review in the existing authenticated
  portal. (PRD: "Notify the patient when a result is ready for their review in the existing authenticated
  portal.")
- **I-2** "Ready" means a doctor has reviewed **and** released the result. Completion of the laboratory
  upload alone never makes a result ready. (PRD constraint; product owner, 2026-09-20: "'Ready' must not
  mean that the laboratory upload merely finished.")
- **I-3** The message only says that an update is available and directs the patient to the existing
  authenticated portal.

**How success will be observed**

- **S-1** Results-status calls go down: status-only calls are counted separately from clinical questions
  and compared against a measured baseline. (Support lead, 2026-09-20.)
- **S-2** Delivery failures are measured and handled without sending the patient duplicate messages.
  (PRD: "Measure reduction in results-status calls and delivery failures.")
- **S-3** No numeric target is approved. The baseline and any numerical target will be agreed during
  discovery (PRD); this intent sets none.

## Affected users and systems

- **U-1** Patients: receive the update-available notification and view the result in the portal.
- **U-2** Doctors: their review and release of a result is the gate for any notification.
- **U-3** Support staff: field status-only calls and provide the call counts used as the success measure.
- **SYS-1** Existing authenticated patient portal and its existing result route (engineering lead: "the
  existing portal path is adequate").
- **SYS-2** Result review/release workflow, which supplies the released result version.
- **SYS-3** Notification delivery and its operational logs (channel still open, see OPEN-1).

## Scope

In scope: notifying the patient through the existing portal result route, preventing duplicate
notifications for the same released result version, and handling delivery failures without duplicate
patient messages.

## Out of scope

- **X-1** Portal redesign. (PRD scope.)
- **X-2** A new or rebuilt patient dashboard. It is a separate project (engineering lead, 2026-09-20).
- **X-3** Appointment scheduling. (PRD scope.)
- **X-4** Clinical interpretation of results. (PRD scope.)
- **X-5** Notifying third parties. The referring doctor is not approved as a recipient; that is a
  separate open decision (OPEN-2), not part of this scope.
- **X-6** Everything only the superseded request asked for: notifying on lab upload, putting the test
  result in the message, copying the referring doctor, rebuilding the dashboard and its call-reduction
  target.

## Constraints

- **C-1** A doctor must review and release the result before any notification is eligible.
- **C-2** The message and the notification logs contain no diagnosis, test name, test result, clinical
  measurement or treatment detail: only that an update is available, with direction to the portal.
  (PRD; privacy owner: "no sensitive content in messages or operational logs.")
- **C-3** Access to the result is authenticated in the existing portal. No attached results and no
  public result link.
- **C-4** No duplicate notifications for the same released result version; delivery failures are
  handled without duplicate patient messages.
- **C-5** Use the existing portal result route; no new dashboard or portal redesign.
- **C-6** Recipients are limited to the patient. No other recipient is approved until its open decision
  is resolved by its owners.

## Open questions

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner** and **privacy owner**, after
  the consent and delivery review. (PRD; privacy owner: "Consent and the actual channel are still under
  review.")
- **OPEN-2 Notification to the referring doctor.** Owners: **clinical owner** and **privacy owner**, who
  must decide recipient authorization. Not approved by the PRD, and approval must not be inferred from
  the old request (clinical owner, 2026-09-20).
- **OPEN-3 Baseline and numeric target** for status-only calls: to be agreed during discovery (PRD). No
  value is assumed here.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
