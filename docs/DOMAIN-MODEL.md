# Combat Academy OS --- Domain Model

**Status:** Working Domain Specification\
**Version:** 0.1\
**Purpose:** Define the conceptual entities, relationships, ownership,
lifecycles, and invariants that underpin the complete Combat Academy OS.

------------------------------------------------------------------------

# 1. Domain Modeling Principles

The domain model is the foundation of the platform.

The system should be modeled around **real academy concepts**, not
around screens or database tables.

Primary principles:

1.  An academy is the primary tenant.
2.  People and users are different concepts.
3.  Training structure is hierarchical but flexible.
4.  Commercial state is separated from training state.
5.  Website content should reuse academy domain data.
6.  Martial-art progression must be configurable.
7.  Permissions must be independent from UI roles.
8.  Historical records should be preserved rather than overwritten.
9.  Every academy-owned record must be tenant-scoped.
10. Domain events should eventually connect independent modules.

------------------------------------------------------------------------

# 2. High-Level Domain Graph

``` text
PLATFORM
│
├── Platform User
├── SaaS Plan
├── Subscription
└── Academy
     │
     ├── Locations
     │    └── Rooms
     │
     ├── People
     │    ├── Users
     │    ├── Students
     │    ├── Staff
     │    └── Guardians
     │
     ├── Disciplines
     │    └── Programs
     │         └── Classes
     │
     ├── Enrollments
     ├── Attendance
     │
     ├── Membership Plans
     │    └── Memberships
     │         └── Payments
     │
     ├── Leads
     │    └── Trials
     │
     ├── Progression Systems
     │    └── Ranks
     │         └── Promotions
     │
     ├── Competitions
     │    └── Divisions
     │         └── Registrations
     │              └── Results
     │
     ├── Events
     │
     ├── Website
     │    ├── Pages
     │    ├── Sections
     │    ├── Theme
     │    ├── Media
     │    └── Domain
     │
     ├── Communications
     ├── Automations
     ├── Tasks
     ├── Documents
     └── Audit Logs
```

------------------------------------------------------------------------

# 3. Tenant / Academy

## Entity: Academy

The academy is the primary tenant boundary.

### Core attributes

-   `id`
-   `name`
-   `legal_name`
-   `description`
-   `logo_media_id`
-   `phone`
-   `email`
-   `whatsapp`
-   `timezone`
-   `currency`
-   `default_language`
-   `status`
-   `created_at`
-   `updated_at`

### Relationships

``` text
Academy
 ├── has many Locations
 ├── has many People
 ├── has many Disciplines
 ├── has many Programs
 ├── has many Classes
 ├── has many MembershipPlans
 ├── has many Leads
 ├── has many Competitions
 ├── has many Events
 └── has one primary Website
```

### Invariants

-   Academy ID is required for tenant-owned records.
-   An academy cannot access another academy's records.
-   Deleting an academy should normally be a controlled
    archival/suspension operation rather than destructive deletion.

------------------------------------------------------------------------

# 4. Location

## Entity: Location

Represents a physical academy branch or training location.

### Attributes

-   `id`
-   `academy_id`
-   `name`
-   `address`
-   `city`
-   `country`
-   `latitude`
-   `longitude`
-   `phone`
-   `email`
-   `opening_hours`
-   `status`

### Relationships

``` text
Academy
  └── has many Locations

Location
  └── has many Rooms
```

------------------------------------------------------------------------

# 5. Room

## Entity: Room

Represents a training area within a location.

Examples:

-   Main Mat
-   Boxing Room
-   Cage
-   Private Training Room

### Attributes

-   `id`
-   `location_id`
-   `name`
-   `capacity`
-   `description`
-   `status`

### Invariants

A room belongs to exactly one location.

------------------------------------------------------------------------

# 6. Person

## Entity: Person

Represents the real human identity.

This should be separate from authentication.

### Attributes

-   `id`
-   `first_name`
-   `last_name`
-   `display_name`
-   `date_of_birth`
-   `photo_media_id`
-   `email`
-   `phone`
-   `address`
-   `created_at`
-   `updated_at`

### Relationships

A person may have several academy relationships:

``` text
Person
 ├── StudentProfile
 ├── StaffProfile
 └── GuardianProfile
```

A person can therefore be both:

``` text
Student + Coach
```

without duplicating identity information.

------------------------------------------------------------------------

# 7. User Account

## Entity: User

Represents authentication and access to the platform.

### Attributes

-   `id`
-   `person_id`
-   `email`
-   `authentication_provider`
-   `status`
-   `last_login_at`
-   `created_at`

### Important distinction

``` text
Person ≠ User
```

A student can exist without a login.

A staff member can exist without a login.

A user account grants digital access to a person.

------------------------------------------------------------------------

# 8. Academy Membership / User Relationship

A user needs an academy-specific relationship.

Conceptually:

``` text
User
  ↓
AcademyMembership
  ↓
Academy
```

### Attributes

-   `id`
-   `academy_id`
-   `user_id`
-   `status`
-   `joined_at`

Roles and permissions are attached to this relationship rather than
globally to the person.

This allows one person to have different roles in different academies.

------------------------------------------------------------------------

# 9. Role

## Entity: Role

Examples:

-   Owner
-   Manager
-   Coach
-   Reception
-   Accountant
-   Marketing
-   Student

A role is a named collection of permissions.

------------------------------------------------------------------------

# 10. Permission

## Entity: Permission

Fine-grained capability.

Examples:

``` text
students.read
students.create
students.update
students.delete

payments.read
payments.write

website.read
website.write
website.publish

attendance.read
attendance.write

progression.read
progression.write
```

### Relationship

``` text
Role
 └── has many Permissions
```

Custom roles should eventually be supported.

------------------------------------------------------------------------

# 11. Student Profile

## Entity: StudentProfile

Connects a person to the academy as a student.

### Attributes

-   `id`
-   `academy_id`
-   `person_id`
-   `student_number`
-   `enrollment_date`
-   `status`
-   `home_location_id`
-   `notes`
-   `created_at`
-   `updated_at`

### Relationships

``` text
Student
 ├── belongs to Person
 ├── belongs to Academy
 ├── has many Enrollments
 ├── has many Memberships
 ├── has many Payments
 ├── has many AttendanceRecords
 ├── has many Promotions
 ├── has many CompetitionRegistrations
 └── may belong to Guardian relationships
```

------------------------------------------------------------------------

# 12. Guardian Relationship

## Entity: GuardianRelationship

Connects a person acting as guardian to a student.

### Attributes

-   `guardian_person_id`
-   `student_id`
-   `relationship_type`
-   `is_primary`
-   `can_manage`
-   `created_at`

Example:

``` text
Parent
 ├── Child A
 └── Child B
```

------------------------------------------------------------------------

# 13. Staff Profile

## Entity: StaffProfile

Represents non-student personnel.

### Attributes

-   `id`
-   `academy_id`
-   `person_id`
-   `staff_number`
-   `job_title`
-   `start_date`
-   `status`
-   `notes`

Staff can optionally also have specialized profiles.

------------------------------------------------------------------------

# 14. Coach Profile

## Entity: CoachProfile

A specialized staff relationship.

### Attributes

-   `id`
-   `staff_profile_id`
-   `bio`
-   `photo_media_id`

### Relationships

``` text
Coach
 ├── teaches Disciplines
 ├── assigned to Programs
 ├── teaches Classes
 └── may manage Student Progression
```

------------------------------------------------------------------------

# 15. Discipline

## Entity: Discipline

Represents a martial art/combat sport.

Examples:

-   BJJ
-   Judo
-   Wrestling
-   Boxing
-   MMA
-   Muay Thai

### Attributes

-   `id`
-   `academy_id`
-   `name`
-   `description`
-   `image_media_id`
-   `progression_system_id`
-   `status`
-   `public_visibility`

A discipline is academy-owned, even when its name matches a globally
common discipline.

------------------------------------------------------------------------

# 16. Program

## Entity: Program

Represents a structured training offering.

Example:

``` text
BJJ
├── Kids BJJ
├── Adults Beginners
├── Adults Advanced
└── Competition Team
```

### Attributes

-   `id`
-   `academy_id`
-   `discipline_id`
-   `name`
-   `description`
-   `age_min`
-   `age_max`
-   `skill_min`
-   `skill_max`
-   `capacity`
-   `status`
-   `public_visibility`

### Relationships

``` text
Discipline
  └── has many Programs

Program
  ├── has many Classes
  ├── has many Enrollments
  └── may be included in Membership Plans
```

------------------------------------------------------------------------

# 17. Class

## Entity: Class

Represents a scheduled training offering.

A class should conceptually separate:

1.  The recurring class definition.
2.  The actual scheduled occurrence.

This distinction becomes important later.

------------------------------------------------------------------------

# 18. Class Definition

## Entity: ClassDefinition

Example:

``` text
Adults BJJ
Monday
19:00–20:30
Coach Ahmed
```

### Attributes

-   `id`
-   `academy_id`
-   `program_id`
-   `location_id`
-   `room_id`
-   `name`
-   `capacity`
-   `duration`
-   `status`

### Relationships

``` text
ClassDefinition
 ├── has primary Coach
 ├── has assistant Coaches
 └── generates ClassOccurrences
```

------------------------------------------------------------------------

# 19. Class Occurrence

## Entity: ClassOccurrence

Represents one actual session.

Example:

``` text
September 10
19:00–20:30
Adults BJJ
```

### Attributes

-   `id`
-   `class_definition_id`
-   `starts_at`
-   `ends_at`
-   `status`
-   `cancellation_reason`
-   `notes`

### Why this matters

A recurring class can exist indefinitely while individual occurrences
can be:

-   Cancelled
-   Rescheduled
-   Replaced
-   Modified

------------------------------------------------------------------------

# 20. Enrollment

## Entity: ProgramEnrollment

Connects a student to a program.

### Attributes

-   `id`
-   `student_id`
-   `program_id`
-   `location_id`
-   `start_date`
-   `end_date`
-   `status`

### Relationship

``` text
Student
  └── enrolls in Program
```

Enrollment is training participation.

It is not the same thing as a payment or membership.

------------------------------------------------------------------------

# 21. Attendance

## Entity: AttendanceRecord

Connects a student to a specific class occurrence.

### Attributes

-   `id`
-   `class_occurrence_id`
-   `student_id`
-   `status`
-   `checked_in_at`
-   `marked_by_user_id`
-   `notes`

### States

``` text
Present
Absent
Late
Excused
```

### Invariant

A student should normally have at most one attendance record per class
occurrence.

------------------------------------------------------------------------

# 22. Membership Plan

## Entity: MembershipPlan

Defines a commercial product.

Example:

``` text
All Access Monthly
450 MAD
Unlimited classes
```

### Attributes

-   `id`
-   `academy_id`
-   `name`
-   `description`
-   `price`
-   `currency`
-   `billing_interval`
-   `duration`
-   `status`
-   `public_visibility`

### Access configuration

A plan may grant access to:

-   Disciplines
-   Programs
-   Locations
-   Number of classes

------------------------------------------------------------------------

# 23. Membership

## Entity: Membership

Represents a student's actual commercial membership.

### Attributes

-   `id`
-   `academy_id`
-   `student_id`
-   `membership_plan_id`
-   `start_date`
-   `end_date`
-   `status`
-   `auto_renew`
-   `freeze_period`
-   `notes`

### Important distinction

``` text
MembershipPlan
=
What the academy sells

Membership
=
What a specific student owns
```

------------------------------------------------------------------------

# 24. Payment

## Entity: Payment

Represents money received or recorded.

### Attributes

-   `id`
-   `academy_id`
-   `student_id`
-   `membership_id`
-   `invoice_id`
-   `amount`
-   `currency`
-   `method`
-   `status`
-   `paid_at`
-   `reference`
-   `notes`

### Methods

Potentially:

-   Cash
-   Bank transfer
-   Card
-   Online provider
-   Other

------------------------------------------------------------------------

# 25. Invoice

## Entity: Invoice

Represents an amount due.

### Attributes

-   `id`
-   `academy_id`
-   `student_id`
-   `membership_id`
-   `number`
-   `subtotal`
-   `discount`
-   `tax`
-   `total`
-   `currency`
-   `status`
-   `issued_at`
-   `due_at`

Payment and invoice are separate concepts.

------------------------------------------------------------------------

# 26. Lead

## Entity: Lead

Represents a prospective customer before becoming a student.

### Attributes

-   `id`
-   `academy_id`
-   `person_id` (optional)
-   `name`
-   `email`
-   `phone`
-   `source`
-   `discipline_id`
-   `program_id`
-   `status`
-   `assigned_to_user_id`
-   `notes`
-   `created_at`

### Lifecycle

``` text
New
 ↓
Contacted
 ↓
Trial Booked
 ↓
Trial Attended
 ↓
Converted
```

Alternative:

``` text
Lost
```

------------------------------------------------------------------------

# 27. Trial

## Entity: TrialBooking

Represents a prospect's trial experience.

### Attributes

-   `id`
-   `academy_id`
-   `lead_id`
-   `class_occurrence_id`
-   `program_id`
-   `scheduled_at`
-   `status`
-   `notes`

### Lifecycle

``` text
Requested
 ↓
Confirmed
 ↓
Attended
 ↓
Follow-up
 ↓
Converted / Lost
```

------------------------------------------------------------------------

# 28. Conversion

Conversion should be represented as a relationship/history rather than
simply changing a lead's label.

Conceptually:

``` text
Lead
 ↓
Trial
 ↓
StudentProfile
 ↓
Membership
```

The original lead/trial history should remain available for analytics.

------------------------------------------------------------------------

# 29. Progression System

## Entity: ProgressionSystem

Defines how a discipline tracks advancement.

Examples:

``` text
BJJ Belt System
Judo Kyu/Dan System
Custom Level System
```

### Attributes

-   `id`
-   `academy_id`
-   `name`
-   `type`
-   `description`

------------------------------------------------------------------------

# 30. Rank

## Entity: Rank

Represents a level inside a progression system.

Examples:

``` text
White Belt
Blue Belt
Purple Belt
```

or:

``` text
White
Yellow
Orange
Green
```

### Attributes

-   `id`
-   `progression_system_id`
-   `name`
-   `order`
-   `metadata`

------------------------------------------------------------------------

# 31. Rank Marker

Some systems use stripes, grades, or levels inside a rank.

## Entity: RankMarker

### Attributes

-   `id`
-   `rank_id`
-   `name`
-   `order`
-   `type`

Examples:

``` text
Stripe 1
Stripe 2
Stripe 3
Stripe 4
```

The system must not require markers.

------------------------------------------------------------------------

# 32. Student Progression

## Entity: StudentProgression

Represents a student's current state within a discipline's progression
system.

### Attributes

-   `id`
-   `student_id`
-   `discipline_id`
-   `progression_system_id`
-   `rank_id`
-   `rank_marker_id`
-   `effective_date`

A student may have different progression states in different
disciplines.

------------------------------------------------------------------------

# 33. Promotion

## Entity: Promotion

Represents a historical advancement.

### Attributes

-   `id`
-   `student_id`
-   `discipline_id`
-   `previous_rank_id`
-   `previous_marker_id`
-   `new_rank_id`
-   `new_marker_id`
-   `awarded_by_coach_id`
-   `awarded_at`
-   `notes`
-   `certificate_media_id`

### Invariant

Historical promotions should not be overwritten.

------------------------------------------------------------------------

# 34. Competition

## Entity: Competition

Represents a competition event.

### Attributes

-   `id`
-   `academy_id`
-   `name`
-   `organizer`
-   `location`
-   `starts_at`
-   `registration_deadline`
-   `website`
-   `notes`

------------------------------------------------------------------------

# 35. Competition Division

## Entity: CompetitionDivision

Defines a competition category.

Potential dimensions:

-   Discipline
-   Age
-   Gender/category
-   Weight
-   Rank
-   Ruleset

The model should remain flexible because competition structures vary
substantially by sport and organizer.

------------------------------------------------------------------------

# 36. Competition Registration

## Entity: CompetitionRegistration

Connects a student to a competition/division.

### Attributes

-   `id`
-   `competition_id`
-   `division_id`
-   `student_id`
-   `status`
-   `registration_fee`
-   `notes`

------------------------------------------------------------------------

# 37. Competition Match / Result

Potential entities:

``` text
CompetitionMatch
CompetitionResult
```

Possible data:

-   Athlete
-   Opponent
-   Round
-   Result
-   Method
-   Time
-   Points
-   Decision
-   Medal

The exact model should be refined after researching the major
competition formats we intend to support.

------------------------------------------------------------------------

# 38. Event

## Entity: AcademyEvent

General academy event.

Examples:

-   Seminar
-   Open mat
-   Workshop
-   Camp
-   Belt ceremony
-   Social event

An event may have:

-   Registration
-   Capacity
-   Payment
-   Attendance
-   Website visibility
-   Notifications

Competitions remain a specialized domain rather than simply being
generic events.

------------------------------------------------------------------------

# 39. Website

## Entity: Website

Represents an academy's public digital presence.

### Attributes

-   `id`
-   `academy_id`
-   `name`
-   `status`
-   `template_id`
-   `published_at`

### Relationships

``` text
Website
 ├── has Pages
 ├── has Theme
 ├── has Domains
 ├── uses Media
 └── consumes Academy data
```

------------------------------------------------------------------------

# 40. Website Page

## Entity: Page

Examples:

-   Home
-   About
-   Programs
-   Coaches
-   Schedule
-   Pricing
-   Contact

### Attributes

-   `id`
-   `website_id`
-   `slug`
-   `title`
-   `status`
-   `seo_metadata`

------------------------------------------------------------------------

# 41. Website Section

## Entity: Section

A page is composed of configurable sections.

Examples:

-   Hero
-   Program Grid
-   Coach Grid
-   Schedule
-   Pricing
-   Testimonials
-   Gallery
-   Map
-   FAQ
-   CTA

### Attributes

-   `id`
-   `page_id`
-   `type`
-   `position`
-   `enabled`
-   `configuration`

------------------------------------------------------------------------

# 42. Website Theme

## Entity: Theme

Defines presentation.

Potential configuration:

-   Colors
-   Fonts
-   Button styles
-   Border radius
-   Spacing
-   Navigation
-   Layout style

Themes should be configuration rather than hard-coded per academy.

------------------------------------------------------------------------

# 43. Website Template

## Entity: WebsiteTemplate

Defines a reusable starting design.

Example:

``` text
BJJ Academy
MMA Academy
Kids Academy
Competition Academy
```

Templates should map to the same underlying content model.

------------------------------------------------------------------------

# 44. Custom Domain

## Entity: Domain

Represents a public domain attached to an academy website.

Potential attributes:

-   `id`
-   `academy_id`
-   `website_id`
-   `hostname`
-   `status`
-   `verification_status`
-   `ssl_status`
-   `renewal_date`
-   `provider`

------------------------------------------------------------------------

# 45. Media

## Entity: MediaAsset

Central file/media object.

Potential types:

-   Image
-   Video
-   Document
-   Logo
-   Certificate

### Attributes

-   `id`
-   `academy_id`
-   `type`
-   `storage_key`
-   `filename`
-   `mime_type`
-   `size`
-   `metadata`
-   `created_at`

Media access must respect tenant boundaries and permissions.

------------------------------------------------------------------------

# 46. Communication

## Entity: Message

Represents a communication sent by or through the platform.

Potential channels:

-   In-app
-   Email
-   SMS
-   WhatsApp
-   Push

### Attributes

-   `id`
-   `academy_id`
-   `channel`
-   `sender`
-   `recipient`
-   `template_id`
-   `status`
-   `sent_at`

------------------------------------------------------------------------

# 47. Notification

## Entity: Notification

Represents a user-facing notification.

Examples:

-   Membership expiring
-   Trial booked
-   Class cancelled
-   Promotion awarded

Notifications should be separate from messages because an in-app
notification does not necessarily imply an external message.

------------------------------------------------------------------------

# 48. Automation

## Entity: Automation

Defines:

``` text
Trigger
 ↓
Conditions
 ↓
Actions
```

Example:

``` text
Trigger:
Membership expires in 3 days

Condition:
Membership is active

Actions:
Send WhatsApp
Send email
Create task
```

Potential components:

-   Trigger
-   Condition
-   Action
-   Execution
-   Status

------------------------------------------------------------------------

# 49. Task

## Entity: Task

Represents internal work.

### Attributes

-   `id`
-   `academy_id`
-   `title`
-   `description`
-   `assignee_id`
-   `priority`
-   `status`
-   `due_at`
-   `related_entity`
-   `created_by`

Tasks can relate to:

-   Leads
-   Students
-   Memberships
-   Events
-   Competitions

------------------------------------------------------------------------

# 50. Document

## Entity: Document

Represents a stored academy document.

Examples:

-   Waiver
-   Registration form
-   Policy
-   Contract
-   Certificate

Documents can be:

-   Academy-wide
-   Staff-specific
-   Student-specific

Access must be permission-controlled.

------------------------------------------------------------------------

# 51. Audit Log

## Entity: AuditLog

Records important changes.

### Attributes

-   `id`
-   `academy_id`
-   `actor_user_id`
-   `action`
-   `entity_type`
-   `entity_id`
-   `previous_data`
-   `new_data`
-   `created_at`

Audit logs should be append-oriented.

------------------------------------------------------------------------

# 52. Subscription

## Entity: Subscription

Represents an academy's relationship with the SaaS product.

``` text
Academy
 ↓
Subscription
 ↓
SaaS Plan
```

Potential attributes:

-   `id`
-   `academy_id`
-   `plan_id`
-   `status`
-   `start_date`
-   `renewal_date`
-   `cancelled_at`

------------------------------------------------------------------------

# 53. SaaS Plan

## Entity: SaaSPlan

Defines platform-level commercial packaging.

Potential limits:

-   Number of students
-   Number of staff
-   Number of locations
-   Storage
-   Messages
-   Features

The plan system should support feature flags.

------------------------------------------------------------------------

# 54. Feature Flag

## Entity: FeatureFlag

Controls feature availability.

Potential scope:

-   Global
-   Plan
-   Academy
-   User

Examples:

`text competition_module automation_engine advanced_analytics custom_domains`

------------------------------------------------------------------------

# 55. Domain Events

The platform should eventually emit domain events.

Examples:

`text StudentCreated TrialBooked TrialAttended MembershipCreated MembershipExpiring MembershipExpired PaymentRecorded ClassCancelled AttendanceRecorded PromotionAwarded CompetitionRegistrationCreated`

Events allow modules to react without becoming tightly coupled.

Example:

`text MembershipExpiring        │        ├── Notification        ├── Automation        └── Task`

------------------------------------------------------------------------

# 56. Core Business Invariants

The following rules should guide implementation.

## Tenant isolation

A user can only access records permitted by their academy relationship
and permissions.

## Student identity

A person's identity should not be duplicated merely because they have
multiple academy roles.

## Membership versus enrollment

Training enrollment and commercial membership are separate concepts.

## Recurring class versus occurrence

A recurring class definition is not the same as a specific training
session.

## Historical progression

Promotions are historical records and should not be rewritten.

## Website consistency

Public dynamic content should derive from the academy's canonical data.

## Permission enforcement

Authorization must happen at the domain/API layer, not only by hiding UI
elements.

## Auditability

Important financial, permission, progression, and operational changes
should be traceable.

------------------------------------------------------------------------

# 57. State Machines to Define Later

Several entities require explicit state machines.

### Student

``` text
Lead
Trial
Active
Inactive
Frozen
Suspended
Expired
Archived
```

### Membership

``` text
Pending
Active
Expiring
Frozen
Overdue
Cancelled
Expired
```

### Lead

``` text
New
Contacted
Trial Booked
Trial Attended
Converted
Lost
```

### Trial

``` text
Requested
Confirmed
Attended
No-show
Follow-up
Converted
Lost
```

### Payment

``` text
Pending
Paid
Failed
Refunded
Partially Refunded
Cancelled
```

### Website

``` text
Draft
Published
Suspended
```

These should be finalized before implementation.

------------------------------------------------------------------------

# 58. Important Relationship Rules

## Academy → Location

One academy can have many locations.

## Location → Room

One location can have many rooms.

## Academy → Person

An academy can have many people through role-specific relationships.

## Discipline → Program

One discipline can have many programs.

## Program → Class

One program can have many class definitions.

## Class Definition → Occurrence

One recurring class can generate many occurrences.

## Student → Program

A student can enroll in multiple programs.

## Student → Membership

A student can have historical and current memberships.

## Student → Attendance

A student can have many attendance records.

## Student → Progression

A student can have progression in multiple disciplines.

## Student → Competition

A student can register for many competitions.

## Academy → Website

An academy should normally have one primary public website, with the
model remaining extensible.

------------------------------------------------------------------------

# 59. Important Modeling Distinctions

These distinctions should remain explicit.

``` text
Person
≠
User
```

``` text
Discipline
≠
Program
```

``` text
Program
≠
Class
```

``` text
Class Definition
≠
Class Occurrence
```

``` text
Enrollment
≠
Membership
```

``` text
Membership
≠
Payment
```

``` text
Lead
≠
Student
```

``` text
Trial
≠
Membership
```

``` text
Rank
≠
Promotion
```

``` text
Competition
≠
Competition Registration
```

``` text
Website
≠
Website Template
```

These distinctions prevent many future architectural problems.

------------------------------------------------------------------------

# 60. Proposed Aggregate Boundaries

The final implementation should investigate the following aggregate
boundaries:

``` text
Academy
People
Training
Memberships
Finance
CRM
Progression
Competitions
Events
Website
Communication
Automation
Platform Billing
```

The exact DDD aggregate design should be finalized during architecture
design rather than prematurely treating every entity as an independent
service.

------------------------------------------------------------------------

# 61. Suggested Initial Bounded Contexts

A mature system can be logically divided into:

``` text
Identity & Access
Academy Management
People
Training
Attendance
Memberships
Finance
CRM
Progression
Competitions
Events
Website
Communication
Automation
Analytics
Platform Billing
```

These do not necessarily need to become separate microservices.

A modular monolith is likely a better initial architectural direction.

------------------------------------------------------------------------

# 62. Data Ownership Principle

Each domain should have clear ownership.

Example:

``` text
Student Profile
→ People domain

Class
→ Training domain

Membership
→ Membership domain

Payment
→ Finance domain

Promotion
→ Progression domain

Lead
→ CRM domain

Website
→ Website domain
```

Other modules may reference the entity but should not silently become
its owner.

------------------------------------------------------------------------

# 63. Cross-Domain Example

A trial-to-membership journey may cross several domains:

``` text
CRM
Lead
 ↓
Trial
 ↓
People
Student
 ↓
Training
Program Enrollment
 ↓
Memberships
Membership
 ↓
Finance
Payment
 ↓
Communication
Confirmation
```

The system should connect these domains through explicit commands/events
rather than uncontrolled direct dependencies.

------------------------------------------------------------------------

# 64. Website Data Strategy

Dynamic website information should preferably come from canonical
academy entities.

For example:

``` text
Website
   ↓
Program data
   ↓
Programs domain
```

``` text
Website
   ↓
Coach profiles
   ↓
People/Coach domain
```

``` text
Website
   ↓
Schedule
   ↓
Training domain
```

This prevents:

``` text
Dashboard Schedule = Monday 19:00
Website Schedule = Monday 18:00
```

------------------------------------------------------------------------

# 65. Analytics Strategy

Analytics should primarily derive from domain events and canonical
records.

Examples:

``` text
StudentCreated
MembershipActivated
PaymentRecorded
AttendanceRecorded
TrialBooked
TrialConverted
```

This creates a consistent foundation for metrics.

------------------------------------------------------------------------

# 66. Search Strategy

Global search should eventually provide a unified experience while
respecting domain ownership.

Search targets:

``` text
Students
Staff
Leads
Memberships
Payments
Classes
Competitions
Events
```

Search should never bypass authorization.

------------------------------------------------------------------------

# 67. Soft Deletion and Archiving

Most business records should not be destructively deleted casually.

Possible approach:

``` text
Active
Archived
```

Some records should be effectively immutable:

-   Payments
-   Invoices
-   Promotions
-   Audit logs
-   Historical attendance

Correction should occur through additional records or controlled
amendments where appropriate.

------------------------------------------------------------------------

# 68. Future Extensions

The model should leave room for:

-   Student mobile accounts
-   Coach mobile accounts
-   Parent portals
-   E-commerce
-   Marketplace
-   Athlete profiles
-   Academy networks
-   Competition ecosystem
-   Access-control hardware
-   AI services

These should extend the domain model rather than distort its core.

------------------------------------------------------------------------

# 69. Domain Model Summary

The platform can be understood through five major business systems:

``` text
1. ACADEMY OPERATIONS
   People
   Disciplines
   Programs
   Classes
   Attendance

2. COMMERCIAL OPERATIONS
   Memberships
   Payments
   Invoices

3. CUSTOMER ACQUISITION
   Leads
   Trials
   Conversion

4. MARTIAL ARTS
   Progression
   Promotions
   Competitions

5. DIGITAL PRESENCE
   Website
   Content
   Domains
   Communication
```

All five are connected through:

``` text
Academy
People
Identity
Permissions
Events
Notifications
Audit
```

------------------------------------------------------------------------

# 70. Next Specification

The next document should be:

``` text
WORKFLOWS.md
```

It should define the actual behavior of the system in chronological user
journeys.

Priority workflows:

1.  Academy onboarding
2.  Website creation
3.  Student registration
4.  Trial booking
5.  Trial conversion
6.  Program enrollment
7.  Membership purchase
8.  Payment recording
9.  Class scheduling
10. Attendance
11. Membership renewal
12. Membership freeze/cancellation
13. Student progression/promotion
14. Competition registration
15. Class cancellation
16. Staff onboarding
17. Website customization/publishing
18. Notification/automation execution
19. Multi-location operations
20. Academy subscription lifecycle

After workflows, the architecture can be designed against a much more
stable behavioral specification.

------------------------------------------------------------------------

# 71. Domain Modeling Rule

> **The database should represent the business, not dictate it.**
>
> **The UI should represent the workflows, not define them.**
>
> **The domains should own their data and rules.**
>
> **Events should connect domains where appropriate.**
>
> **The final architecture should emerge from these boundaries rather
> than being chosen first.**
