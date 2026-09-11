# WORKFLOWS.md

## Purpose

This document defines how Combat Academy OS behaves over time.

- `vision.md` = what we are building
- `PRODUCT-MAP.md` = what the product can do
- `DOMAIN-MODEL.md` = what the system is made of
- `WORKFLOWS.md` = how the system behaves

V1/V2/V3 prioritization is intentionally deferred.

---

## 1. Academy Onboarding

**Actors:** platform admin, academy owner/manager.

**Trigger:** owner creates an academy.

**Flow**
1. Create/sign in to platform account.
2. Create Academy.
3. Enter academy identity, contact, address, timezone, currency and locale.
4. Create first Location and optional Rooms.
5. Select disciplines.
6. Assign owner.
7. Create initial Website and Theme.
8. Open dashboard and onboarding checklist.

**Created/updated:** Academy, Location, Room, Person/User relationship, AcademyMembership, Website, Theme.

**Events:** `AcademyCreated`, `LocationCreated`, `WebsiteCreated`, `AcademyOwnerAssigned`.

**Edge cases:** abandoned onboarding, duplicate academy/domain, multiple initial locations.

---

## 2. Website Creation and Publishing

**Actors:** owner, manager, website administrator.

**Flow**
1. Select template.
2. Configure theme, logo, typography, navigation and branding.
3. Configure pages and sections.
4. Reuse canonical academy data for programs, coaches, schedule and locations.
5. Preview.
6. Save draft.
7. Publish.

**Principle:** website presentation data is separate from operational data, but both use the same canonical academy records.

**Events:** `WebsiteDraftUpdated`, `WebsitePublished`, `WebsiteUnpublished`.

**Edge cases:** archived referenced data, broken media, concurrent editing, failed publication.

---

## 3. Domain Setup

1. Academy chooses a domain or connects an existing one.
2. System verifies ownership/DNS.
3. Domain is activated.
4. HTTPS/SSL is provisioned.
5. Website becomes available at the domain.

**Events:** `DomainAdded`, `DomainVerified`, `DomainActivated`, `DomainVerificationFailed`.

---

## 4. Lead Capture

**Trigger:** visitor submits an inquiry.

1. Match or create Person.
2. Create Lead.
3. Record source: website, social, referral, advertising, walk-in, etc.
4. Put lead into academy pipeline.
5. Assign staff.
6. Follow up.

Typical pipeline:

`New → Contacted → Qualified → Trial Booked → Trial Attended → Converted / Lost`

**Automation:** acknowledgement, staff alert, follow-up reminders.

**Events:** `LeadCreated`, `LeadStageChanged`.

---

## 5. Trial Booking

**Actors:** prospect, receptionist, coach, system.

1. Prospect selects eligible program/class.
2. System shows available trial slots.
3. Prospect selects slot.
4. System validates capacity and eligibility.
5. TrialBooking is created.
6. Confirmation and reminders are sent.

**Alternative paths:** class full, trial unavailable, cancellation, rescheduling.

**Events:** `TrialBooked`, `TrialRescheduled`, `TrialCancelled`.

---

## 6. Trial Attendance

1. Student arrives.
2. Staff/coach checks them in.
3. Trial becomes attended, no-show, cancelled, or other configured status.
4. Coach can add notes.
5. CRM updates.
6. Follow-up action becomes available.

**Automation**
- attended → thank-you + membership follow-up
- no-show → rescheduling message + staff task

**Events:** `TrialAttended`, `TrialNoShow`.

---

## 7. Trial → Student Conversion

1. Staff confirms conversion.
2. Lead becomes converted.
3. StudentProfile is created/activated if necessary.
4. Program is selected.
5. Enrollment is created.
6. Membership plan is selected.
7. Membership is created.
8. Payment is requested/recorded.
9. Student becomes active.

Core funnel:

`Lead → Trial → Student → Enrollment → Membership → Payment`

**Events:** `LeadConverted`, `ProgramEnrollmentCreated`, `MembershipCreated`.

---

## 8. Student Registration

1. Search for an existing Person first.
2. Create Person only if needed.
3. Create/activate StudentProfile.
4. Collect required information.
5. Add guardian relationship where applicable.
6. Collect waivers/documents.
7. Set student status.

**Critical rule:** do not create duplicate identities when the same person already exists.

---

## 9. Program Enrollment

1. Student selects a Program.
2. Validate eligibility/prerequisites/capacity.
3. Create ProgramEnrollment.
4. Record start date.
5. Attach class preferences where applicable.
6. Student becomes visible in relevant operational contexts.

**Events:** `ProgramEnrollmentCreated`, `ProgramEnrollmentChanged`, `ProgramEnrollmentEnded`.

**Edge cases:** multiple programs, prerequisite failure, capacity limits, program transfer.

---

## 10. Membership Purchase

1. Select MembershipPlan.
2. Calculate price.
3. Apply configured discounts/taxes/promotions.
4. Create Membership.
5. Generate Invoice where applicable.
6. Collect/record Payment.
7. Activate Membership when requirements are satisfied.
8. Calculate renewal/expiration.
9. Send confirmation.

Typical lifecycle:

`Draft → Pending Payment → Active → Frozen → Expired / Cancelled`

**Important:** academy memberships are completely different from the academy's SaaS subscription.

---

## 11. Payment and Invoice

**Flow**
1. Billing obligation is created.
2. Invoice is generated where applicable.
3. Payment is made/recorded.
4. Record amount, method, date, reference and status.
5. Update invoice balance.
6. Update membership/account state where appropriate.
7. Generate receipt.

Possible methods: cash, card, bank transfer, online payment, academy-defined methods.

**Principle:** Invoice = billing obligation; Payment = money received/payment transaction.

**Events:** `InvoiceCreated`, `PaymentRecorded`, `PaymentFailed`, `InvoicePaid`, `RefundRecorded`.

---

## 12. Class Scheduling

1. Create ClassDefinition.
2. Associate discipline/program.
3. Assign coach.
4. Assign location/room.
5. Configure recurrence.
6. Generate ClassOccurrences.
7. Show schedule to staff/students.

**Critical distinction**

`BJJ Fundamentals — Monday 19:00` = ClassDefinition.

`September 14, 2026 — 19:00` = ClassOccurrence.

This allows individual cancellation, substitution, attendance and historical reporting without damaging the recurring definition.

---

## 13. Attendance

1. ClassOccurrence opens.
2. System displays expected participants.
3. Coach/reception records attendance.
4. Status can be Present, Absent, Late, Excused, or academy-defined.
5. Save AttendanceRecord.
6. Update student history and analytics.

**Automation:** absence follow-up, low-attendance alerts, streaks, coach reports.

**Events:** `AttendanceRecorded`, `AttendanceUpdated`.

---

## 14. Class Cancellation

1. Authorized user cancels one occurrence.
2. Record reason.
3. Update attendance context.
4. Notify affected students.
5. Update public schedule.
6. Optionally offer replacement/make-up.

**Important:** cancelling one occurrence must not delete the recurring ClassDefinition.

**Event:** `ClassCancelled`.

---

## 15. Coach / Staff Onboarding

1. Owner/manager creates or invites Person.
2. Create StaffProfile and CoachProfile when relevant.
3. Assign roles.
4. Assign permissions/location scope.
5. Invite user.
6. User accepts and links User account.
7. Coach can be assigned to classes.

Removing access must not destroy historical classes, attendance or other records.

**Events:** `StaffInvited`, `StaffActivated`, `StaffRoleChanged`, `StaffDeactivated`.

---

## 16. Student Progression

1. Configure ProgressionSystem.
2. Define Rank and optional RankMarker records.
3. Assign student to progression system.
4. Coach evaluates progress.
5. Authorized coach awards promotion.
6. Record previous rank, new rank, date, coach and notes.
7. Update current progression.
8. Optionally generate certificate/notification.

**Principle:** progression is historical. Promotions remain immutable historical records.

**Events:** `PromotionAwarded`, `StudentRankChanged`.

---

## 17. Competition Management

1. Create Competition.
2. Configure Divisions.
3. Identify eligible athletes.
4. Register athletes.
5. Track registration/check-in/weigh-in as applicable.
6. Record matches/results.
7. Update athlete competition history.

Possible results include win, loss, draw, submission, decision, points, DQ and custom results.

**Events:** `CompetitionCreated`, `CompetitionRegistrationCreated`, `CompetitionResultRecorded`.

---

## 18. Academy Events

Events are broader than normal classes.

Examples:
- seminars
- open mats
- belt promotions
- tournaments
- camps
- social events
- staff meetings

**Flow:** create → configure eligibility/capacity → open registration → register participants → event occurs → record attendance/results → close/archive.

---

## 19. Membership Renewal

1. System detects upcoming expiration.
2. Reminder sequence begins.
3. Student receives renewal prompt.
4. Staff sees renewal opportunity.
5. Student renews.
6. New membership period is created.
7. Payment is recorded.

Possible configurable reminders: 30, 14, 7 days before expiration, expiration day, post-expiration.

**Events:** `MembershipRenewalDue`, `MembershipRenewed`, `MembershipExpired`.

---

## 20. Membership Freeze

1. Student requests freeze.
2. System checks academy policy.
3. Authorized staff approves/rejects.
4. Freeze period is recorded.
5. Membership becomes Frozen.
6. Expiration/end date is adjusted according to policy.
7. Notifications are sent.
8. Membership resumes automatically or manually according to configuration.

Handle maximum duration, paid/unpaid freeze, overlapping expiration and multiple requests.

---

## 21. Membership Cancellation

1. Cancellation requested.
2. System checks policy.
3. Authorized user approves.
4. Membership becomes cancelled at the effective date.
5. Future billing may stop.
6. Credits/refunds handled according to policy.
7. Historical enrollment remains.

Never erase attendance, payments, progression or competition history merely because membership ended.

---

## 22. Website Customization Request

For changes beyond the standard editor:

`Submitted → Reviewing → Quoted → Approved → In Progress → Delivered`

The academy describes the request; platform team evaluates feasibility, optionally quotes it, builds it, presents preview, obtains approval and publishes.

This creates a paid customization/service layer without turning the core product into an uncontrolled Wix/Webflow clone.

---

## 23. Communication

1. Staff or automation selects audience.
2. System resolves recipients.
3. Select template.
4. Populate variables.
5. Queue message.
6. Provider sends it.
7. Record delivery status.
8. Retry/failures are surfaced.

Potential channels: email, SMS, push, in-app, integrations.

Must respect consent, notification preferences, permissions and applicable legal requirements.

---

## 24. Automation Engine

Core model:

`Trigger → Conditions → Actions`

Example:

`Membership expires in 7 days`
→ student is active
→ send renewal message
→ create staff task

Other examples:

- `TrialNoShow → reschedule message → follow-up task`
- `LeadCreated → acknowledgement → receptionist notification`
- `StudentCreated → welcome message → missing-document request`
- low attendance → retention task

Requirements:
- idempotency
- execution history
- failure handling
- retries
- pause/disable
- permission boundaries
- tenant isolation

---

## 25. Multi-Location Operations

1. Academy contains multiple Locations.
2. Users receive organization-wide or location-scoped access.
3. Programs/classes are assigned to locations.
4. Students may enroll at one or multiple locations.
5. Staff can work across locations.
6. Reports can be filtered by location or aggregated.

Example: a location manager can manage Location A but cannot change organization-wide SaaS settings or manage Location B.

---

## 26. Guardian Portal

Where a guardian relationship exists:

1. Guardian receives controlled access.
2. Guardian views permitted student information.
3. Guardian can potentially view schedule/attendance, manage permitted payments/memberships, register for events and manage permitted documents.

Access must be based on explicit guardian relationship and policy.

---

## 27. Student Portal

Potential capabilities:

- profile
- schedule
- class/trial/event booking
- attendance
- membership
- invoices/payments
- progression
- competition history
- messages
- permitted documents

It should consume the same canonical operational data as the dashboard.

---

## 28. Coach Portal

Potential capabilities:

- assigned schedule
- class rosters
- attendance
- relevant student information
- progression evaluations
- authorized promotions
- competition records
- permitted communication

Financial/admin access should not be granted automatically.

---

## 29. Analytics

Aggregate operational events from:

- CRM
- trials
- students
- enrollments
- memberships
- payments
- attendance
- progression
- competitions
- website
- communication

Metrics include:

**Growth:** leads, trials, conversion, new students.

**Retention:** active members, churn, renewals, expirations, attendance trends.

**Finance:** revenue, outstanding invoices, revenue by program/location.

**Training:** attendance, class utilization, coach workload, program participation.

**Marketing:** lead sources and website conversion.

---

## 30. Search

Search should work across academy data while respecting permissions.

Examples:

`Ahmed` → student / guardian / lead / staff.

`BJJ Fundamentals` → discipline / program / class / website content.

Tenant isolation and authorization must apply to search results.

---

## 31. Audit Logging

Sensitive actions should produce AuditLog records.

Examples:
- permission/role changes
- membership cancellation
- payment modification/refund
- promotion
- archive/delete
- website publication
- financial configuration changes

Record:
- actor
- action
- target
- timestamp
- metadata
- tenant/academy context

Audit records should not be silently rewritten.

---

## 32. Archiving and Historical Data

Prefer archive/deactivation over destructive deletion for operationally important records.

When a student leaves:

`Active → Archived/Inactive`

Historical attendance, payments, memberships, progression and competition records remain.

Apply the same principle to coaches, classes, programs, membership plans and locations.

---

## 33. Academy SaaS Subscription

This is the platform's commercial relationship with the academy, not a student's membership.

Flow:

1. Academy selects SaaSPlan.
2. Subscription starts.
3. Billing cycle begins.
4. Entitlements/features are checked.
5. Renewal occurs.
6. Failed payment triggers recovery.
7. Upgrade/downgrade/cancellation changes entitlement.

Events:
- `SaaSSubscriptionCreated`
- `SaaSSubscriptionRenewed`
- `SaaSPaymentFailed`
- `SaaSSubscriptionCancelled`

---

## 34. SaaS Upgrade / Downgrade

### Upgrade

1. Academy chooses higher plan.
2. Calculate price/proration where applicable.
3. Enable new entitlements.
4. Update subscription/billing.

### Downgrade

1. Academy requests lower plan.
2. Check whether current usage exceeds target limits.
3. Warn where necessary.
4. Apply downgrade according to billing policy.
5. Restrict features without casually deleting data.

Feature availability should be controlled centrally through entitlements/feature flags rather than scattered UI conditionals.

---

## 35. Cross-Domain Event Examples

### Payment

```text
PaymentRecorded
  ↓
Finance updates balance
  ↓
Membership may activate
  ↓
Receipt notification
  ↓
Analytics updates
  ↓
Dashboard KPI updates
```

### Trial

```text
TrialAttended
  ↓
CRM updates
  ↓
Conversion opportunity
  ↓
Automation follow-up
  ↓
Analytics funnel update
```

### Promotion

```text
PromotionAwarded
  ↓
Progression updates
  ↓
Student portal updates
  ↓
Optional notification/certificate
  ↓
Optional public athlete profile update
```

---

## 36. Transaction Boundaries

Core state changes should be transactional.

Example:

```text
Transaction:
  Create Payment
  Update Invoice Balance
  Commit

After commit:
  Publish PaymentRecorded
  Send receipt
  Update analytics
```

An external email/SMS failure should not roll back a valid financial transaction.

---

## 37. Idempotency

Retryable operations must be safe to repeat.

Especially:

- payment webhooks
- external webhooks
- automation execution
- notifications
- event handlers
- integrations

Example: receiving the same payment webhook three times must not create three payments.

---

## 38. Temporal / Historical Rules

The system must preserve what was true at the time.

Examples:

- old ranks remain historically correct
- past occurrences retain their actual coach
- past payments retain original amounts
- historical invoices do not change merely because today's prices changed
- website publication has revision/history
- attendance remains attached to the occurrence that happened

---

## 39. Core Workflow Principles

1. One source of truth for operational data.
2. Preserve historical records.
3. Separate identity, roles and relationships.
4. Keep enrollment, membership, payment and attendance distinct.
5. Separate recurring class definitions from occurrences.
6. Separate website presentation from operational data.
7. Enforce permissions server-side.
8. Emit domain events for important state changes.
9. External failures must not corrupt internal business state.
10. Retried operations must be idempotent.
11. Enforce tenant isolation at every data boundary.
12. Keep automation configurable and controlled.
13. Dashboard, portals and website should consume consistent canonical data.
14. Support simple academies without preventing sophisticated operations.

---

## 40. Planning Sequence

The product specification now has four layers:

```text
VISION
What are we building?
        ↓
PRODUCT MAP
What can it do?
        ↓
DOMAIN MODEL
What is it made of?
        ↓
WORKFLOWS
How does it behave?
        ↓
ARCHITECTURE
How will we implement it?
        ↓
FEATURE MATRIX
What capabilities exist and how are they grouped?
        ↓
ROADMAP
What do we build first?
```

No V1/V2/V3 prioritization is made here. The full product behavior is mapped first; prioritization comes after architecture and feature analysis.
