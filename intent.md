# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17 and is history only; nothing in this intent is approved on its authority.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. (`01-approved-prd.md`: "The fictional clinic receives avoidable calls from patients checking whether results are ready.")

## Proposed outcome

- **I-2** The patient is notified when a result is ready for their review in the existing authenticated portal. (`01-approved-prd.md`: "Notify the patient when a result is ready for their review in the existing authenticated portal.")
- **I-3** "Ready" means a doctor has reviewed **and** released the result. A completed laboratory upload alone does not make a result ready. (`01-approved-prd.md`: "A doctor must review and release the result before any notification is eligible."; `02-current-meeting.md`, product owner: `"Ready" must not mean that the laboratory upload merely finished.`)

### How success will be observed

- **I-4** Status-only calls ("is my result ready?"), counted separately from clinical questions and compared against a measured baseline. (`01-approved-prd.md`: "Measure reduction in results-status calls"; `02-current-meeting.md`, support lead: "count status-only calls separately from clinical questions; compare against a measured baseline.")
- **I-5** Delivery failures of the notification. (`01-approved-prd.md`: "Measure reduction in results-status calls and delivery failures.")
- **I-6** No baseline or numeric target is approved yet; both will be agreed during discovery. (`01-approved-prd.md`: "The baseline and numerical target will be agreed during discovery; none is approved yet.")

## Affected users and systems

- **I-7** Patients: the only approved recipients of the notification. (`01-approved-prd.md`: "Notify the patient")
- **I-8** Doctors: their review and release of a result is the gate for any notification. (`01-approved-prd.md` approved constraints)
- **I-9** Support staff: handle the results-status calls and count status-only calls separately from clinical questions. (`02-current-meeting.md`, support lead)
- **I-10** The existing authenticated portal and its existing result route, where the patient views the result. (`01-approved-prd.md` scope: "Use the existing portal result route."; `02-current-meeting.md`, engineering lead: "the existing portal path is adequate.")
- **I-11** The notification sending process and its notification logs (channel still open, see OPEN-1).

## Out of scope

- **X-1** Portal redesign. (`01-approved-prd.md` scope)
- **X-2** A new or rebuilt patient dashboard: a separate project. (`02-current-meeting.md`, engineering lead: "A new patient dashboard is a separate project.")
- **X-3** Appointment scheduling. (`01-approved-prd.md` scope)
- **X-4** Clinical interpretation of results. (`01-approved-prd.md` scope)
- **X-5** Notifying third parties. (`01-approved-prd.md` scope) The referring doctor is not approved as a recipient and is handled separately as OPEN-2.
- **X-6** Excluded because they appear only in the superseded request: notifying as soon as the lab uploads a result, putting the test result in an SMS, copying the referring doctor, rebuilding the patient dashboard, and a 90% call-reduction target in one week.

## Constraints

- **C-1** Review before notification: no notification is eligible until a doctor has reviewed and released the result. (`01-approved-prd.md`)
- **C-2** Message content: the notification only says that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, clinical measurement or treatment detail in the message or in the notification logs. (`01-approved-prd.md`; `02-current-meeting.md`, privacy owner: "no sensitive content in messages or operational logs.")
- **C-3** Access: authentication happens in the existing portal. No results are attached and no public result link is introduced. (`01-approved-prd.md`)
- **C-4** Duplicate prevention: no duplicate notifications for the same released result version. (`01-approved-prd.md`)
- **C-5** Delivery failures are handled without sending the patient duplicate messages. (`01-approved-prd.md`)
- **C-6** Recipients: only the patient. No other recipient may be added until its authorization is decided (see OPEN-2). (`01-approved-prd.md`; `02-current-meeting.md`, clinical owner)

## Open questions and owners

- **OPEN-1** Channel (SMS, email or portal push). Owners: product owner and privacy owner, who decide after the consent and delivery review. (`01-approved-prd.md` unresolved decisions; `02-current-meeting.md`, privacy owner: "Consent and the actual channel are still under review.")
- **OPEN-2** Whether to notify the referring doctor. Owners: clinical owner and privacy owner, who must decide whether that recipient is authorized. Not approved by the PRD, and not approved by the superseded request either. (`01-approved-prd.md` unresolved decisions; `02-current-meeting.md`, clinical owner: "do not infer approval for referring-doctor notifications from the old request.")

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
