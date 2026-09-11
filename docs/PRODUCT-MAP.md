# Combat Academy OS --- Product Map

**Status:** Working Product Specification\
**Version:** 0.1\
**Relationship:** Detailed decomposition of `docs/vision.md`\
**Purpose:** Map the complete product before deciding V1, V2, V3, or
later.

------------------------------------------------------------------------

# 1. How to Read This Document

The Product Map describes the complete product independently of release
planning.

It answers:

-   What capabilities should the final platform have?
-   Who uses each capability?
-   What data does each capability depend on?
-   What are the major workflows?
-   What is core infrastructure versus optional functionality?
-   Which areas are likely to be complex or risky?

It does **not** yet decide what gets built first.

The later roadmap should be derived from this map.

------------------------------------------------------------------------

# 2. Product Architecture at a Glance

``` text
                              COMBAT ACADEMY OS
                                      │
          ┌───────────────────────────┼───────────────────────────┐
          │                           │                           │
      PUBLIC SIDE                ACADEMY SIDE                PLATFORM SIDE
          │                           │                           │
      Website                     Dashboard                  Platform Admin
      Landing Pages               Students                   Academies
      Programs                    Staff                      Subscriptions
      Schedule                    Disciplines                Billing
      Coaches                     Programs                   Domains
      Pricing                     Classes                    Templates
      Trial Booking               Attendance                  Support
      Contact                     Memberships                 Usage
      SEO                         Payments                    Feature Flags
                                  Leads                       System Health
                                  Progression
                                  Competitions
                                  Events
                                  Communication
                                  Analytics
                                  Tasks
                                  Documents

                              SHARED FOUNDATION
                                      │
          ┌──────────────┬────────────┼────────────┬──────────────┐
          │              │            │            │              │
       Identity      Authorization  Tenancy      Media         Notifications
       Security      Permissions    Isolation    Search        Audit
       API           Events         Files        Settings       Integrations
```

------------------------------------------------------------------------

# 3. Actors and Roles

## 3.1 Platform-Level Actors

### Platform Owner

Owns the SaaS business.

Potential capabilities:

-   Manage academies
-   Manage subscriptions
-   Manage plans
-   Manage platform settings
-   Manage templates
-   View platform analytics
-   Manage support
-   Manage domains
-   Manage feature availability

### Platform Administrator

Operational staff for the SaaS provider.

Access should be controlled by granular permissions.

------------------------------------------------------------------------

# 4. Academy-Level Roles

## Academy Owner

Highest authority inside an academy.

Typical access:

-   Academy settings
-   Locations
-   Staff
-   Students
-   Programs
-   Classes
-   Memberships
-   Payments
-   Website
-   Leads
-   Analytics
-   Competitions
-   Progression
-   Integrations
-   Billing/subscription

------------------------------------------------------------------------

## Academy Administrator

Operational administrator.

Typical access:

-   Students
-   Staff
-   Programs
-   Classes
-   Memberships
-   Attendance
-   Leads
-   Website

Financial and security settings can be restricted.

------------------------------------------------------------------------

## Manager

Runs day-to-day operations.

Potential access:

-   Students
-   Classes
-   Coaches
-   Attendance
-   Memberships
-   Leads
-   Events
-   Communication
-   Operational analytics

------------------------------------------------------------------------

## Coach

Focused operational access.

Typical access:

-   Assigned classes
-   Class rosters
-   Attendance
-   Relevant student profiles
-   Progression
-   Promotions
-   Competition athletes

Should not automatically have access to financial data.

------------------------------------------------------------------------

## Assistant Coach

Similar to coach but potentially more restricted.

------------------------------------------------------------------------

## Receptionist

Front-desk workflow.

Typical access:

-   Students
-   New registrations
-   Leads
-   Trial bookings
-   Membership status
-   Payments
-   Check-in
-   Schedule

------------------------------------------------------------------------

## Accountant / Finance Staff

Financial access.

Typical access:

-   Payments
-   Invoices
-   Refunds
-   Revenue reports
-   Outstanding balances

No unnecessary access to coaching/progression information.

------------------------------------------------------------------------

## Marketing Staff

Typical access:

-   Leads
-   Website
-   Content
-   Campaign sources
-   Communication
-   Marketing analytics

------------------------------------------------------------------------

## Student

Self-service access.

Potential capabilities:

-   View profile
-   Schedule
-   Membership
-   Attendance
-   Payments
-   Progression
-   Competitions
-   Notifications

------------------------------------------------------------------------

## Parent / Guardian

For minors.

Potential capabilities:

-   View linked children
-   Schedule
-   Attendance
-   Memberships
-   Payments
-   Progression
-   Communication
-   Trial booking

------------------------------------------------------------------------

# 5. Permission Model

Permissions should be independent from UI roles.

Example:

``` text
students.read
students.create
students.update
students.delete

attendance.read
attendance.write

payments.read
payments.write
payments.refund

website.read
website.write
website.publish

progression.read
progression.write

competitions.read
competitions.write
```

Roles are collections of permissions.

This makes custom academy roles possible later.

------------------------------------------------------------------------

# 6. Tenant Model

The platform is multi-tenant.

``` text
Platform
│
├── Academy A
│   ├── Users
│   ├── Students
│   ├── Staff
│   ├── Locations
│   ├── Programs
│   └── Website
│
├── Academy B
│   ├── Users
│   ├── Students
│   ├── Staff
│   ├── Locations
│   ├── Programs
│   └── Website
│
└── Academy C
```

Every academy-owned record must be associated with the correct tenant.

Tenant isolation is a foundational security requirement.

------------------------------------------------------------------------

# 7. Academy Module

## Academy

Core fields:

-   ID
-   Name
-   Legal name
-   Description
-   Logo
-   Contact information
-   Email
-   Phone
-   WhatsApp
-   Website
-   Timezone
-   Currency
-   Default language
-   Status
-   Created date

## Academy Settings

-   Registration settings
-   Membership settings
-   Attendance settings
-   Progression settings
-   Communication settings
-   Website settings
-   Notification settings
-   Privacy settings

------------------------------------------------------------------------

# 8. Locations

An academy can have one or many locations.

## Location

-   Name
-   Address
-   Phone
-   Email
-   Coordinates
-   Opening hours
-   Timezone
-   Facilities
-   Status

## Rooms

A location may have rooms/training areas.

Example:

``` text
Rabat Academy
├── Main Mat
├── Boxing Room
├── MMA Cage
└── Private Training Room
```

Rooms can later be used for scheduling conflict detection.

------------------------------------------------------------------------

# 9. People Domain

A shared `Person` concept should prevent duplicate identity records.

``` text
Person
├── Student relationship
├── Staff relationship
└── Guardian relationship
```

A person can have more than one relationship with the academy.

Example:

``` text
Person: Ahmed

Student
+
Coach
```

------------------------------------------------------------------------

# 10. Student Module

## Student Record

### Identity

-   Person
-   Photo
-   Date of birth
-   Contact details
-   Address

### Academy Profile

-   Enrollment date
-   Status
-   Home location
-   Assigned coach
-   Notes

### Relationships

-   Parent/guardian
-   Family members
-   Emergency contact

### Training

-   Disciplines
-   Programs
-   Classes
-   Attendance
-   Progression
-   Competitions

### Commercial

-   Memberships
-   Payments
-   Invoices
-   Discounts

------------------------------------------------------------------------

# 11. Student Status

Possible states:

``` text
Lead
Trial
Active
Inactive
Suspended
Frozen
Expired
Archived
```

The exact state model should be refined during domain modeling.

------------------------------------------------------------------------

# 12. Parent / Guardian Domain

A guardian can manage multiple students.

``` text
Guardian
├── Child A
├── Child B
└── Child C
```

Capabilities:

-   View children
-   Manage contact information
-   Book trials
-   View memberships
-   Make payments
-   Receive notifications
-   Review attendance

------------------------------------------------------------------------

# 13. Discipline Module

A discipline represents a combat sport or martial art.

Examples:

-   BJJ
-   Judo
-   Wrestling
-   Boxing
-   Muay Thai
-   MMA
-   Sambo
-   Karate
-   Taekwondo
-   Kickboxing
-   Submission Grappling
-   Custom

Fields:

-   Name
-   Description
-   Image
-   Skill system
-   Active/inactive
-   Public visibility

------------------------------------------------------------------------

# 14. Program Module

Programs are structured offerings within a discipline.

Example:

``` text
BJJ
├── Kids
├── Adults Beginners
├── Adults Advanced
└── Competition Team
```

Program properties:

-   Name
-   Discipline
-   Description
-   Age range
-   Skill range
-   Capacity
-   Coaches
-   Locations
-   Classes
-   Membership eligibility
-   Price
-   Website visibility

------------------------------------------------------------------------

# 15. Class Module

A class represents a training session.

## Class Definition

-   Program
-   Location
-   Room
-   Coach
-   Assistant coaches
-   Capacity
-   Skill requirement
-   Duration
-   Schedule
-   Status

## Recurrence

Examples:

``` text
Every Monday 19:00
Every Wednesday 19:00
```

The system should support exceptions.

Example:

``` text
Normal:
Wednesday 19:00

Exception:
September 23 → Cancelled
```

------------------------------------------------------------------------

# 16. Scheduling Engine

The scheduling system should eventually understand:

-   Coach conflicts
-   Room conflicts
-   Location conflicts
-   Capacity
-   Student eligibility
-   Holidays
-   Cancellations
-   Rescheduled classes
-   Private sessions
-   Events

------------------------------------------------------------------------

# 17. Calendar

Calendar views:

-   Academy
-   Location
-   Coach
-   Program
-   Discipline

Event types:

-   Class
-   Private lesson
-   Seminar
-   Competition
-   Event
-   Closure
-   Meeting

------------------------------------------------------------------------

# 18. Enrollment

A student can enroll in a program.

Example:

``` text
Mohamed
   ↓
BJJ Adults
   ↓
Monday + Wednesday classes
```

Enrollment should track:

-   Start date
-   End date
-   Status
-   Program
-   Location
-   Eligibility
-   Notes

------------------------------------------------------------------------

# 19. Attendance

Attendance is linked to:

``` text
Student
   +
Class
   +
Date
```

Statuses:

-   Present
-   Absent
-   Late
-   Excused

Potential future methods:

-   Coach marking
-   Reception check-in
-   QR code
-   Student self check-in
-   Access-control integrations

------------------------------------------------------------------------

# 20. Membership Plans

A plan defines a commercial offering.

Examples:

-   BJJ Monthly
-   All Access Monthly
-   Kids Annual
-   10-Class Pack
-   Family Plan
-   Private Training

Plan properties:

-   Name
-   Price
-   Currency
-   Duration
-   Included programs
-   Included locations
-   Class limits
-   Freeze rules
-   Cancellation rules
-   Renewal behavior
-   Eligibility

------------------------------------------------------------------------

# 21. Memberships

A membership is a student's actual subscription/enrollment in a
commercial plan.

``` text
Plan
  ↓
Membership
  ↓
Student
```

Membership states:

``` text
Pending
Active
Expiring
Expired
Frozen
Cancelled
Suspended
Overdue
```

------------------------------------------------------------------------

# 22. Membership Lifecycle

Example:

``` text
Create
 ↓
Activate
 ↓
Approaching Expiration
 ↓
Renew
 ↓
Activate Again
```

Alternative:

``` text
Active
 ↓
Freeze
 ↓
Active
```

Or:

``` text
Active
 ↓
Cancel
 ↓
Expired
```

------------------------------------------------------------------------

# 23. Payment Module

Payment records should support:

-   Student
-   Membership
-   Invoice
-   Amount
-   Currency
-   Date
-   Payment method
-   Status
-   Reference
-   Notes

Methods:

-   Cash
-   Bank transfer
-   Card
-   Online payment
-   Other

------------------------------------------------------------------------

# 24. Invoice Module

Potential features:

-   Invoice creation
-   Invoice numbering
-   Payment status
-   PDF generation
-   Refund association
-   Customer details
-   Tax configuration where applicable

------------------------------------------------------------------------

# 25. Financial Reporting

Potential reports:

-   Daily revenue
-   Monthly revenue
-   Revenue by discipline
-   Revenue by program
-   Revenue by location
-   Outstanding payments
-   Refunds
-   Membership revenue
-   Recurring revenue

------------------------------------------------------------------------

# 26. Lead / CRM Module

Lead represents a prospective customer.

Fields:

-   Name
-   Contact
-   Source
-   Discipline
-   Program
-   Age group
-   Preferred time
-   Assigned staff
-   Notes
-   Status

Pipeline:

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

Alternative ending:

``` text
Lost
```

------------------------------------------------------------------------

# 27. Trial Module

Trial booking connects the website and CRM to academy operations.

Trial:

-   Lead
-   Program
-   Discipline
-   Class
-   Coach
-   Date
-   Status
-   Notes
-   Follow-up

Statuses:

``` text
Requested
Confirmed
Attended
No-show
Follow-up
Converted
Lost
```

------------------------------------------------------------------------

# 28. Conversion

The system should connect:

``` text
Lead
 ↓
Trial
 ↓
Student
 ↓
Membership
```

This allows measurement of:

-   Leads
-   Trial bookings
-   Trial attendance
-   Conversions
-   Conversion rate
-   Source performance

------------------------------------------------------------------------

# 29. Progression Domain

Progression must be configurable per discipline.

Possible systems:

-   Belts
-   Stripes
-   Ranks
-   Levels
-   Grades
-   Custom progression

------------------------------------------------------------------------

# 30. Rank System

Example:

``` text
BJJ

White
 ├── 1 stripe
 ├── 2 stripes
 ├── 3 stripes
 └── 4 stripes

Blue
 ├── 1 stripe
 └── ...
```

The system should not assume that every rank has stripes.

------------------------------------------------------------------------

# 31. Promotion

Promotion record:

-   Student
-   Discipline
-   Previous rank
-   New rank
-   Date
-   Coach
-   Notes
-   Optional certificate
-   Optional media

Promotion history must be immutable/auditable.

------------------------------------------------------------------------

# 32. Competition Module

## Competition

-   Name
-   Organizer
-   Date
-   Location
-   Registration deadline
-   Website
-   Notes

## Division

-   Discipline
-   Age category
-   Gender/category where applicable
-   Weight
-   Skill/rank
-   Ruleset

## Athlete Registration

-   Student
-   Competition
-   Division
-   Status
-   Registration fee
-   Notes

------------------------------------------------------------------------

# 33. Competition Results

Match/result data can include:

-   Opponent
-   Round
-   Result
-   Method
-   Time
-   Points
-   Decision
-   Medal

The system should allow discipline-specific result structures later.

------------------------------------------------------------------------

# 34. Coach Module

Coach profile:

-   Person
-   Bio
-   Photo
-   Disciplines
-   Ranks
-   Certifications
-   Programs
-   Classes
-   Locations
-   Availability

Potential later features:

-   Coach compensation
-   Coach attendance
-   Private lessons
-   Performance analytics

------------------------------------------------------------------------

# 35. Staff Module

Staff may include:

-   Reception
-   Manager
-   Accountant
-   Marketing
-   Administrator
-   Operations staff

Potential later features:

-   Staff schedules
-   Leave
-   Attendance
-   Contracts
-   Compensation
-   Documents

------------------------------------------------------------------------

# 36. Website Module

Every academy receives a website.

Core concepts:

``` text
Website
├── Domain
├── Theme
├── Pages
├── Sections
├── Navigation
├── Media
└── SEO
```

------------------------------------------------------------------------

# 37. Website Pages

Potential pages:

-   Home
-   About
-   Disciplines
-   Programs
-   Coaches
-   Schedule
-   Pricing
-   Gallery
-   Testimonials
-   Events
-   Competitions
-   Blog
-   Contact
-   Trial booking

Pages should be optional/configurable.

------------------------------------------------------------------------

# 38. Website Templates

Templates should be purpose-built for academies.

Potential families:

-   BJJ Academy
-   MMA Academy
-   Boxing Gym
-   Traditional Martial Arts
-   Kids Academy
-   Competition Academy
-   Premium Combat Club
-   Minimal Academy

Templates share the same underlying data model.

------------------------------------------------------------------------

# 39. Website Customization

Customization areas:

### Brand

-   Logo
-   Colors
-   Fonts
-   Favicon

### Layout

-   Section order
-   Section visibility
-   Navigation
-   CTA placement

### Content

-   Text
-   Images
-   Videos
-   Testimonials
-   Announcements

### Dynamic Content

-   Coaches
-   Programs
-   Schedule
-   Pricing
-   Locations

------------------------------------------------------------------------

# 40. Website Section System

A section should have:

``` text
Section
├── Type
├── Position
├── Enabled
├── Configuration
├── Content
└── Data source
```

Examples:

``` text
Hero
Program Grid
Coach Grid
Schedule
Pricing
Testimonials
Gallery
Map
CTA
FAQ
```

------------------------------------------------------------------------

# 41. Customization Requests

The platform can support:

``` text
Owner
 ↓
Customization Request
 ↓
Description
 ↓
Quote
 ↓
Approval
 ↓
Implementation
```

This creates an optional professional-services revenue stream.

------------------------------------------------------------------------

# 42. Domain Management

Support:

-   Platform subdomain
-   Custom domain
-   Domain verification
-   DNS guidance
-   SSL
-   Renewal status
-   Redirects

Domain ownership and registrar strategy should be decided later.

------------------------------------------------------------------------

# 43. SEO

Potential capabilities:

-   Meta title
-   Meta description
-   Social preview
-   Sitemap
-   Structured data
-   Local business information
-   Page indexing settings
-   Canonical URLs

------------------------------------------------------------------------

# 44. Media Library

Central asset management:

-   Images
-   Videos
-   Documents
-   Logos
-   Certificates

Metadata:

-   Filename
-   Type
-   Size
-   Dimensions
-   Upload date
-   Owner/tenant
-   Usage references

------------------------------------------------------------------------

# 45. Communication Module

Potential channels:

-   In-app
-   Email
-   SMS
-   WhatsApp
-   Push

Message types:

-   Announcement
-   Reminder
-   Transactional message
-   Marketing message
-   Operational notification

------------------------------------------------------------------------

# 46. Notification Engine

Events can trigger notifications.

Examples:

``` text
Membership expires
 → send reminder
```

``` text
Trial booked
 → notify staff
 → send confirmation
```

``` text
Class cancelled
 → notify enrolled students
```

Notifications should be permission- and preference-aware.

------------------------------------------------------------------------

# 47. Automation Engine

Long-term automation should use a general model:

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
Create staff task
```

This should be designed as reusable infrastructure.

------------------------------------------------------------------------

# 48. Events Module

Events include:

-   Seminars
-   Open mats
-   Workshops
-   Camps
-   Belt ceremonies
-   Academy events
-   Competitions

Potential capabilities:

-   Registration
-   Capacity
-   Payment
-   Attendance
-   Website publishing
-   Notifications

------------------------------------------------------------------------

# 49. Task Module

Internal tasks:

-   Follow up with lead
-   Contact inactive student
-   Renew membership
-   Register competitor
-   Prepare seminar
-   Contact parent

Task fields:

-   Assignee
-   Due date
-   Priority
-   Status
-   Related entity
-   Notes

------------------------------------------------------------------------

# 50. Document Module

Potential documents:

-   Waivers
-   Registration forms
-   Policies
-   Contracts
-   Certificates
-   Student documents

Later:

-   Digital signatures
-   Document templates
-   Expiration tracking

------------------------------------------------------------------------

# 51. Analytics Module

Analytics should be built around decisions.

## Academy Health

-   Active students
-   Student growth
-   Retention
-   Churn

## Attendance

-   Attendance rate
-   Class utilization
-   Student attendance
-   Program popularity

## Revenue

-   Revenue
-   Recurring revenue
-   Outstanding balances
-   Revenue by program
-   Revenue by location

## Marketing

-   Leads
-   Trials
-   Conversion
-   Source performance

------------------------------------------------------------------------

# 52. Dashboard

The dashboard should be role-specific.

## Owner Dashboard

``` text
Students
Memberships
Revenue
Leads
Trials
Attendance

Today's Classes
Upcoming Trials
Expiring Memberships
Outstanding Payments
```

## Coach Dashboard

``` text
Today's Classes
Class Rosters
Attendance
Student Progression
Upcoming Events
```

## Reception Dashboard

``` text
Check-ins
New Leads
Trials
Memberships Expiring
Payments Due
```

------------------------------------------------------------------------

# 53. Search

Global search should eventually cover:

-   Students
-   Staff
-   Leads
-   Memberships
-   Payments
-   Classes
-   Competitions
-   Events

Search must respect permissions and tenant boundaries.

------------------------------------------------------------------------

# 54. Audit Log

Important actions should be recorded.

Examples:

``` text
User changed membership
User recorded payment
Coach promoted student
Manager changed class schedule
Admin modified website
```

Audit data:

-   Actor
-   Action
-   Entity
-   Timestamp
-   Previous value where appropriate
-   New value where appropriate

------------------------------------------------------------------------

# 55. Integrations

Potential integrations:

## Communication

-   WhatsApp
-   Email
-   SMS

## Payments

-   Payment gateways
-   Bank/payment providers

## Maps

-   Google Maps or equivalent

## Calendar

-   Google Calendar

## Analytics

-   Web analytics

## Domains

-   Domain providers

## Access Control

Future:

-   QR
-   RFID
-   NFC
-   Door/access systems

------------------------------------------------------------------------

# 56. Student Portal

Potential interface:

``` text
Home
Schedule
Membership
Attendance
Payments
Progression
Competitions
Notifications
Profile
```

------------------------------------------------------------------------

# 57. Coach Portal

Potential interface:

``` text
Today
My Classes
Roster
Attendance
Students
Progression
Schedule
Competitions
```

------------------------------------------------------------------------

# 58. Parent Portal

Potential interface:

``` text
Children
Schedule
Attendance
Memberships
Payments
Progression
Notifications
```

------------------------------------------------------------------------

# 59. Mobile Applications

Long-term:

## Student App

-   Schedule
-   Membership
-   Attendance
-   Progression
-   Payments
-   Competitions
-   Notifications

## Coach App

-   Classes
-   Attendance
-   Students
-   Progression
-   Schedule

The web platform should establish the core domain/API first.

------------------------------------------------------------------------

# 60. Platform Administration

Separate from academy dashboards.

## Academy Management

-   Create academy
-   Suspend academy
-   View status
-   Manage subscription
-   Support access

## Subscription Management

-   Plans
-   Trials
-   Billing
-   Renewals
-   Cancellation
-   Usage limits

## Template Management

-   Website templates
-   Section templates
-   Theme presets

## Feature Flags

Control features by:

-   Plan
-   Academy
-   Region
-   Release

------------------------------------------------------------------------

# 61. SaaS Billing

Potential concepts:

``` text
Plan
 ↓
Subscription
 ↓
Invoice
 ↓
Payment
```

Potential metrics:

-   MRR
-   ARR
-   Churn
-   Trial conversion
-   Average revenue per academy

------------------------------------------------------------------------

# 62. Support

Long-term support system:

-   Help center
-   Support requests
-   Ticket status
-   Academy context
-   Internal notes
-   Support history

------------------------------------------------------------------------

# 63. Platform Usage

Potential usage metrics:

-   Students
-   Staff
-   Locations
-   Website traffic
-   Storage
-   Messages
-   API usage
-   Active users

These can support pricing and infrastructure management.

------------------------------------------------------------------------

# 64. Core Cross-Module Workflows

## Workflow A --- Academy Setup

``` text
Create Academy
 ↓
Configure Profile
 ↓
Add Location
 ↓
Add Disciplines
 ↓
Add Programs
 ↓
Add Coaches
 ↓
Create Classes
 ↓
Create Membership Plans
 ↓
Customize Website
 ↓
Connect Domain
 ↓
Publish
```

------------------------------------------------------------------------

## Workflow B --- Website Visitor to Student

``` text
Website Visitor
 ↓
View Program
 ↓
Book Trial
 ↓
Lead Created
 ↓
Trial Scheduled
 ↓
Confirmation
 ↓
Trial Attendance
 ↓
Follow-up
 ↓
Student Created
 ↓
Membership Activated
```

------------------------------------------------------------------------

## Workflow C --- Existing Student

``` text
Student
 ↓
Membership
 ↓
Program Enrollment
 ↓
Class
 ↓
Attendance
 ↓
Progression
 ↓
Competition
```

------------------------------------------------------------------------

## Workflow D --- Membership Renewal

``` text
Membership Near Expiration
 ↓
Automation Trigger
 ↓
Reminder
 ↓
Payment
 ↓
Renewal
 ↓
New Membership Period
```

------------------------------------------------------------------------

## Workflow E --- Class Cancellation

``` text
Manager Cancels Class
 ↓
Calendar Updated
 ↓
Affected Students Identified
 ↓
Notification Triggered
 ↓
Students Notified
```

------------------------------------------------------------------------

## Workflow F --- Promotion

``` text
Coach Reviews Student
 ↓
Promotion Decision
 ↓
Promotion Recorded
 ↓
Student Progression Updated
 ↓
Optional Certificate
 ↓
Student Notification
```

------------------------------------------------------------------------

# 65. Data Relationship Overview

``` text
ACADEMY
 │
 ├── LOCATIONS
 │    └── ROOMS
 │
 ├── PEOPLE
 │    ├── STUDENTS
 │    ├── STAFF
 │    └── GUARDIANS
 │
 ├── DISCIPLINES
 │    └── PROGRAMS
 │         └── CLASSES
 │
 ├── MEMBERSHIP PLANS
 │    └── MEMBERSHIPS
 │
 ├── PAYMENTS
 │
 ├── LEADS
 │    └── TRIALS
 │
 ├── PROGRESSION SYSTEMS
 │    └── PROMOTIONS
 │
 ├── COMPETITIONS
 │    └── REGISTRATIONS
 │
 ├── EVENTS
 │
 └── WEBSITE
      ├── PAGES
      ├── SECTIONS
      ├── THEME
      ├── MEDIA
      └── DOMAIN
```

------------------------------------------------------------------------

# 66. Important Domain Dependencies

Some features should not be treated as isolated.

``` text
Academy
 ↓
People
 ↓
Students / Staff
```

``` text
Discipline
 ↓
Program
 ↓
Class
 ↓
Attendance
```

``` text
Membership Plan
 ↓
Membership
 ↓
Payment
```

``` text
Lead
 ↓
Trial
 ↓
Student
 ↓
Membership
```

``` text
Discipline
 ↓
Progression System
 ↓
Rank
 ↓
Promotion
```

``` text
Academy Data
 ↓
Website CMS
 ↓
Public Website
```

These dependencies will be critical when constructing the roadmap.

------------------------------------------------------------------------

# 67. Foundational Services

Some capabilities are infrastructure used by almost every module.

## Identity

-   Authentication
-   Accounts
-   Sessions
-   Password/security mechanisms

## Authorization

-   Roles
-   Permissions
-   Access checks

## Tenancy

-   Academy isolation
-   Tenant context

## Files

-   Uploads
-   Storage
-   Media access

## Search

-   Global search
-   Filtering
-   Sorting

## Notifications

-   Notification records
-   Delivery infrastructure

## Events

-   Domain events
-   Event processing

## Audit

-   Action logging

## API

-   Internal API
-   Future external API

------------------------------------------------------------------------

# 68. Non-Functional Requirements

These are not optional features.

## Security

-   Tenant isolation
-   Permission enforcement
-   Secure authentication
-   Secure file access
-   Auditability

## Reliability

-   Backups
-   Error handling
-   Monitoring
-   Recovery strategy

## Performance

-   Fast dashboard navigation
-   Fast search
-   Efficient public websites
-   Caching where appropriate

## Scalability

The architecture should support:

``` text
1 academy
 ↓
100 academies
 ↓
1,000 academies
 ↓
10,000+ academies
```

without fundamentally redesigning the product.

------------------------------------------------------------------------

# 69. Internationalization

The platform should eventually support:

-   Multiple languages
-   Multiple currencies
-   Timezones
-   Date formats
-   Local payment providers
-   Local messaging providers

Initial deployment can remain focused on one market.

------------------------------------------------------------------------

# 70. Privacy and Sensitive Data

Potentially sensitive information should be separated and
permission-controlled.

Examples:

-   Emergency contacts
-   Guardian information
-   Student documents
-   Health/fitness-related information if collected

The platform should follow applicable privacy and data-protection
requirements in each market.

------------------------------------------------------------------------

# 71. Future Expansion Areas

These belong to the long-term vision but should not automatically become
roadmap commitments.

## Marketplace

-   Equipment
-   Academy merchandise
-   Seminars
-   Training services

## Athlete Profiles

Public athlete pages.

## Academy Network

Academy-to-academy discovery.

## Competition Ecosystem

Competition registration and management across organizations.

## Instructor Network

Coach profiles, seminars, recruitment.

## White Label

Academy-branded portals/apps.

## AI

Potential uses:

-   Website content generation
-   Lead follow-up assistance
-   Analytics explanations
-   Schedule optimization
-   Retention-risk signals
-   Administrative assistance

AI should solve real workflows rather than exist as a decorative
chatbot.

------------------------------------------------------------------------

# 72. Feature Classification

Every feature should eventually be classified as one of:

### Core

Necessary for the product's primary value proposition.

### Supporting

Strongly improves the core product.

### Differentiating

Creates an advantage over generic gym software.

### Expansion

Adds a new business area.

### Experimental

Worth testing but not part of the committed product.

### Infrastructure

Not directly visible but required by the system.

------------------------------------------------------------------------

# 73. Candidate Differentiators

The strongest potential differentiators are:

1.  Martial-arts-native progression.
2.  Discipline/program/class architecture.
3.  Website + academy management in one system.
4.  Lead → trial → membership pipeline.
5.  Competition management.
6.  Academy-specific website templates.
7.  Automated communication.
8.  Multi-location combat academy support.
9.  Student/coach/parent experiences.
10. Academy analytics.

------------------------------------------------------------------------

# 74. Features Requiring Careful Scope Control

These can become major projects:

-   Full accounting
-   Payroll
-   HR
-   Unlimited visual website builder
-   Online payments across many countries
-   WhatsApp automation
-   Mobile apps
-   Access-control hardware
-   Advanced competition brackets
-   AI analytics
-   Marketplace
-   Full e-commerce

They remain in the final vision but should not automatically be included
early.

------------------------------------------------------------------------

# 75. Product Boundary

The core product is:

``` text
ACADEMY DIGITAL OPERATING SYSTEM

= Public Website
+ Academy Management
+ Customer Acquisition
+ Martial Arts Progression
+ Business Operations
```

The platform should avoid becoming an unfocused ERP unless customer
demand justifies expansion.

------------------------------------------------------------------------

# 76. Final Capability Map

``` text
COMBAT ACADEMY OS
│
├── ACADEMY
│   ├── Profile
│   ├── Locations
│   ├── Rooms
│   └── Settings
│
├── PEOPLE
│   ├── Students
│   ├── Guardians
│   ├── Coaches
│   └── Staff
│
├── TRAINING
│   ├── Disciplines
│   ├── Programs
│   ├── Classes
│   ├── Calendar
│   └── Attendance
│
├── MEMBERSHIPS
│   ├── Plans
│   ├── Memberships
│   ├── Renewals
│   ├── Freezes
│   └── Access Rules
│
├── FINANCE
│   ├── Payments
│   ├── Invoices
│   ├── Refunds
│   └── Reports
│
├── CRM
│   ├── Leads
│   ├── Trials
│   ├── Follow-ups
│   └── Conversion
│
├── PROGRESSION
│   ├── Rank Systems
│   ├── Belts
│   ├── Stripes
│   ├── Promotions
│   └── History
│
├── COMPETITIONS
│   ├── Events
│   ├── Divisions
│   ├── Athletes
│   ├── Matches
│   └── Results
│
├── WEBSITE
│   ├── Templates
│   ├── Pages
│   ├── Sections
│   ├── Theme
│   ├── Media
│   ├── SEO
│   └── Domains
│
├── COMMUNICATION
│   ├── Notifications
│   ├── Email
│   ├── SMS
│   ├── WhatsApp
│   └── Automation
│
├── EVENTS
│   ├── Seminars
│   ├── Camps
│   ├── Workshops
│   └── Academy Events
│
├── ANALYTICS
│   ├── Students
│   ├── Attendance
│   ├── Memberships
│   ├── Revenue
│   ├── Leads
│   └── Website
│
├── OPERATIONS
│   ├── Tasks
│   ├── Documents
│   ├── Audit Logs
│   └── Search
│
├── PORTALS
│   ├── Student
│   ├── Coach
│   └── Parent
│
├── INTEGRATIONS
│   ├── Payments
│   ├── Messaging
│   ├── Calendar
│   ├── Maps
│   ├── Domains
│   └── Access Control
│
└── PLATFORM
    ├── Academies
    ├── Plans
    ├── Subscriptions
    ├── Billing
    ├── Templates
    ├── Feature Flags
    ├── Usage
    └── Support
```

------------------------------------------------------------------------

# 77. What Comes Next

The next document should be derived from this map rather than invented
separately.

## `DOMAIN-MODEL.md`

It should define the actual entities and relationships:

``` text
Academy
Location
Room
Person
User
Role
Permission
Student
Guardian
Staff
Coach
Discipline
Program
Class
Enrollment
MembershipPlan
Membership
Payment
Invoice
Lead
Trial
ProgressionSystem
Rank
Promotion
Competition
Division
AthleteRegistration
Event
Website
Page
Section
Theme
Domain
Media
Notification
Automation
Task
Document
AuditLog
Subscription
Plan
```

For each entity we should define:

-   Purpose
-   Fields
-   Relationships
-   Ownership
-   Lifecycle
-   Permissions
-   Dependencies
-   Important invariants

After the domain model, we can create:

``` text
WORKFLOWS.md
ARCHITECTURE.md
FEATURE-MATRIX.md
ROADMAP.md
```

The roadmap should be the **last step**, not the first.

------------------------------------------------------------------------

# 78. Product Design Rule

> **The vision defines where the product can go.**
>
> **The product map defines what the product can do.**
>
> **The domain model defines what the product is.**
>
> **The workflows define how it behaves.**
>
> **The roadmap defines what we build first.**
