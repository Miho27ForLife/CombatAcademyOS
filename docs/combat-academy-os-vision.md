# Combat Academy OS --- Full Product Vision

**Status:** Vision / Product North Star\
**Version:** 0.1\
**Purpose:** Define the complete long-term vision before deciding what
belongs in V1, V2, V3, or later.

------------------------------------------------------------------------

## 1. Executive Vision

Combat Academy OS is a **vertical SaaS platform for martial arts and
combat-sports academies**.

It combines:

1.  A professional, customizable public website for every academy.
2.  A unified academy management dashboard.
3.  Student, coach, staff, class, membership, attendance, and payment
    management.
4.  Martial-arts-specific progression and competition management.
5.  Lead and trial-class management.
6.  Communication and automation.
7.  Analytics and business intelligence.
8.  Multi-location management.
9.  A SaaS administration layer for the platform operator.

The fundamental proposition is:

> **Give every combat-sports academy its own digital headquarters: a
> branded website for the outside world and a powerful operating system
> for everything happening inside the academy.**

The product should be designed specifically around how martial-arts
academies actually operate rather than adapting generic gym software to
combat sports.

------------------------------------------------------------------------

# 2. Product Philosophy

## 2.1 One Academy, One Digital System

The academy should not need separate systems for:

-   Website
-   Students
-   Schedule
-   Attendance
-   Memberships
-   Payments
-   Coaches
-   Leads
-   Competitions
-   Belt progression
-   Communication

The platform should connect these areas through shared data.

For example:

``` text
Website Visitor
      ↓
Lead
      ↓
Trial Booking
      ↓
Trial Attendance
      ↓
Student
      ↓
Program Enrollment
      ↓
Membership
      ↓
Class Attendance
      ↓
Progression
      ↓
Competition
```

------------------------------------------------------------------------

## 2.2 One Source of Truth

The same academy data should power both the dashboard and public
website.

For example:

If an owner changes a coach's profile in the dashboard, the public
website should automatically reflect the change.

If the owner changes a class schedule, the public timetable should
update.

If a program is disabled, it should disappear from relevant public
website sections.

There should be minimal duplicate data entry.

------------------------------------------------------------------------

## 2.3 Configuration Before Custom Development

Academies should be able to personalize their website extensively
without needing technical knowledge.

However, the platform should not initially attempt to become Wix or
Webflow.

The preferred model is:

> **Structured flexibility rather than unlimited visual freedom.**

Academies choose templates, sections, themes, content, images, layouts,
and settings within a controlled system.

For unusual requirements, the academy can request paid custom work.

------------------------------------------------------------------------

# 3. Target Customers

The platform should support different academy sizes.

## Small Academy

Example:

-   1 location
-   1--3 disciplines
-   50--100 students
-   2--5 coaches
-   Owner manages most operations

Needs simplicity.

## Medium Academy

Example:

-   1--2 locations
-   Multiple disciplines
-   100--500 students
-   Several coaches
-   Reception/management staff

Needs operational automation and role-based access.

## Large Academy

Example:

-   Multiple branches
-   Hundreds or thousands of students
-   Many disciplines
-   Dedicated management
-   Coaches and administrative personnel
-   Complex memberships and reporting

Needs multi-location management, granular permissions, analytics, and
automation.

------------------------------------------------------------------------

# 4. Supported Combat Sports

The platform should not be hard-coded around one discipline.

Potential disciplines include:

-   Brazilian Jiu-Jitsu
-   Judo
-   Wrestling
-   Sambo
-   MMA
-   Boxing
-   Muay Thai
-   Kickboxing
-   Karate
-   Taekwondo
-   Krav Maga
-   Submission Grappling
-   Combat Fitness
-   Self Defense
-   Custom disciplines

The system should allow an academy to create its own discipline.

------------------------------------------------------------------------

# 5. Core Domain Model

The fundamental relationship should look approximately like this:

``` text
Platform
  │
  └── Academy
       │
       ├── Locations
       │
       ├── People
       │    ├── Students
       │    └── Staff
       │
       ├── Disciplines
       │    └── Programs
       │         └── Classes
       │
       ├── Memberships
       ├── Payments
       ├── Attendance
       ├── Progression
       ├── Competitions
       ├── Leads
       ├── Trials
       ├── Communications
       ├── Analytics
       │
       └── Website
            ├── Pages
            ├── Sections
            ├── Theme
            ├── Media
            └── Domain
```

This is the conceptual backbone of the product.

------------------------------------------------------------------------

# 6. Academy Management

Every customer has an Academy entity.

## Academy Profile

-   Name
-   Logo
-   Description
-   Contact details
-   Phone
-   Email
-   WhatsApp
-   Social media
-   Address
-   Opening hours
-   Timezone
-   Currency
-   Languages
-   Legal/business information

## Academy Branding

-   Primary color
-   Secondary color
-   Accent color
-   Typography
-   Logo variants
-   Favicon
-   Brand images
-   Brand videos

------------------------------------------------------------------------

# 7. Multi-Location Management

An academy may have multiple branches.

Example:

``` text
ABC Combat Academy

├── Rabat
├── Salé
├── Casablanca
└── Tangier
```

Each location can have:

-   Address
-   Facilities
-   Rooms
-   Classes
-   Coaches
-   Students
-   Opening hours
-   Capacity
-   Local contact information

Management can be centralized or location-specific.

------------------------------------------------------------------------

# 8. People System

A unified person model should support different roles.

Possible roles:

-   Owner
-   Platform Administrator
-   Academy Administrator
-   Manager
-   Coach
-   Assistant Coach
-   Receptionist
-   Accountant
-   Marketing Staff
-   Other Staff
-   Student
-   Parent/Guardian

A person may have multiple relationships with an academy.

For example:

``` text
Person
 ├── Student
 └── Coach
```

------------------------------------------------------------------------

# 9. Student Management

Every student gets a complete profile.

## Personal Information

-   Name
-   Photo
-   Date of birth
-   Gender where relevant
-   Contact information
-   Address
-   Emergency contact
-   Guardian/parent relationship

## Academy Information

-   Enrollment date
-   Status
-   Locations
-   Disciplines
-   Programs
-   Coaches
-   Memberships

## Activity

-   Attendance
-   Payments
-   Membership history
-   Progression
-   Promotions
-   Competitions
-   Achievements
-   Notes
-   Communication history

## Documents

Potentially:

-   Waivers
-   Registration documents
-   Agreements
-   Other academy documents

Sensitive information must be protected with appropriate permissions and
data-handling practices.

------------------------------------------------------------------------

# 10. Parent / Guardian Management

This is especially important for children's programs.

A guardian may:

-   Manage one or more children
-   View schedules
-   View attendance
-   Manage memberships
-   Make payments
-   Receive notifications
-   Book trials
-   Receive academy announcements

Example:

``` text
Parent
 ├── Child A
 ├── Child B
 └── Child C
```

------------------------------------------------------------------------

# 11. Disciplines

A discipline defines a martial art or combat sport taught by the
academy.

Each discipline can have:

-   Name
-   Description
-   Image
-   Skill/rank system
-   Programs
-   Coaches
-   Classes
-   Age groups
-   Pricing
-   Public website content

The academy can create custom disciplines.

------------------------------------------------------------------------

# 12. Programs

A discipline may contain multiple programs.

Example:

``` text
Brazilian Jiu-Jitsu

├── Kids BJJ
├── Adults Beginners
├── Adults Advanced
└── Competition Team
```

A program may define:

-   Discipline
-   Name
-   Description
-   Age range
-   Skill level
-   Required rank
-   Assigned coaches
-   Classes
-   Capacity
-   Membership eligibility
-   Pricing
-   Website visibility

------------------------------------------------------------------------

# 13. Classes

A class is an actual scheduled training session.

A class can contain:

-   Program
-   Discipline
-   Location
-   Room
-   Primary coach
-   Assistant coaches
-   Date/time
-   Duration
-   Capacity
-   Required level
-   Enrolled students
-   Attendance

The system should support recurring classes.

------------------------------------------------------------------------

# 14. Calendar and Scheduling

The academy needs a central calendar.

It should eventually include:

-   Recurring classes
-   One-off classes
-   Private lessons
-   Seminars
-   Competitions
-   Events
-   Holidays
-   Academy closures
-   Coach availability
-   Room availability

Potential calendar views:

-   Day
-   Week
-   Month
-   Location
-   Coach
-   Discipline

------------------------------------------------------------------------

# 15. Attendance

Attendance should be tightly connected to classes and students.

Possible attendance statuses:

-   Present
-   Absent
-   Late
-   Excused

Possible check-in methods:

-   Coach manually marks attendance
-   Reception marks attendance
-   QR code
-   Student self check-in
-   Future RFID/NFC integrations

Attendance should produce useful statistics:

-   Student attendance rate
-   Class attendance
-   Program attendance
-   Coach/class statistics
-   Attendance trends

------------------------------------------------------------------------

# 16. Membership Management

Memberships are central to academy operations.

## Membership Plans

Possible plans:

-   Monthly
-   Quarterly
-   Six-month
-   Annual
-   Class pack
-   Private training
-   Family plan
-   Student plan
-   Kids plan
-   Competition plan
-   Custom plan

Each plan can define:

-   Price
-   Duration
-   Included programs
-   Class limits
-   Locations
-   Renewal behavior
-   Freeze rules
-   Cancellation rules
-   Discounts
-   Access restrictions

------------------------------------------------------------------------

# 17. Membership Lifecycle

A membership should have a clear lifecycle.

``` text
Pending
   ↓
Active
   ↓
Expiring
   ↓
Expired
```

Additional states may include:

-   Frozen
-   Cancelled
-   Suspended
-   Payment overdue

The system should eventually automate actions around these states.

------------------------------------------------------------------------

# 18. Payments and Finance

The platform should support academy financial operations.

## Payments

-   Cash
-   Bank transfer
-   Card
-   Online payment
-   Other configured methods

## Financial Objects

-   Payments
-   Invoices
-   Refunds
-   Discounts
-   Credits
-   Outstanding balances

## Reports

-   Revenue
-   Revenue by discipline
-   Revenue by program
-   Revenue by location
-   Outstanding payments
-   Membership revenue
-   Refunds
-   Growth
-   Recurring revenue

Online payment integrations can be added progressively.

------------------------------------------------------------------------

# 19. Leads / CRM

The public website should be directly connected to a lightweight CRM.

A visitor may become:

``` text
Visitor
 ↓
Lead
 ↓
Contacted
 ↓
Trial booked
 ↓
Trial attended
 ↓
Converted
 ↓
Student
```

Lead information:

-   Name
-   Contact
-   Discipline of interest
-   Program
-   Age group
-   Preferred schedule
-   Source
-   Notes
-   Assigned staff member
-   Status
-   Communication history

Potential lead sources:

-   Website
-   WhatsApp
-   Social media
-   Referral
-   Walk-in
-   Advertisement
-   Other

------------------------------------------------------------------------

# 20. Trial-Class Management

Trial classes are a major bridge between marketing and membership.

A trial record can contain:

-   Lead
-   Discipline
-   Program
-   Class
-   Coach
-   Date
-   Status
-   Notes
-   Follow-up

Statuses:

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

The platform should eventually measure:

> Trial → Membership conversion rate.

------------------------------------------------------------------------

# 21. Martial-Arts Progression

This is a major differentiator.

The platform should support different progression systems rather than
assuming every sport uses belts.

## BJJ Example

``` text
White Belt
 ├── Stripe 1
 ├── Stripe 2
 ├── Stripe 3
 └── Stripe 4

Blue Belt
 ├── Stripe 1
 ├── Stripe 2
 ...
```

## Judo Example

``` text
White
Yellow
Orange
Green
Blue
Brown
Black
```

## Custom Systems

Academies should eventually be able to define their own:

-   Ranks
-   Belts
-   Stripes
-   Grades
-   Levels
-   Requirements

## Promotion History

Every promotion should be recorded:

-   Previous rank
-   New rank
-   Date
-   Coach
-   Notes
-   Optional certificate/media

------------------------------------------------------------------------

# 22. Competition Management

Combat academies frequently participate in competitions.

The system should eventually support:

## Competition Events

-   Event name
-   Organizer
-   Location
-   Date
-   Website/link
-   Registration deadline

## Athlete Registration

-   Student
-   Discipline
-   Division
-   Age category
-   Weight category
-   Rank
-   Registration status

## Results

-   Opponent
-   Match
-   Result
-   Method
-   Round
-   Medal
-   Notes

## Athlete Record

Potentially:

``` text
BJJ Competition Record

Wins: 18
Losses: 5
Draws: 1

Gold: 5
Silver: 3
Bronze: 2
```

------------------------------------------------------------------------

# 23. Coach Management

Each coach gets a profile.

Information may include:

-   Name
-   Photo
-   Bio
-   Disciplines
-   Ranks
-   Certifications
-   Programs
-   Classes
-   Students
-   Availability

Potential future functionality:

-   Coach schedule
-   Coach attendance
-   Compensation
-   Performance metrics
-   Private lessons

------------------------------------------------------------------------

# 24. Staff Management

The system should support non-coaching personnel.

Examples:

-   Reception
-   Management
-   Accounting
-   Marketing
-   Administration
-   Maintenance
-   Other personnel

Future HR functionality may include:

-   Staff schedules
-   Attendance
-   Leave
-   Contracts
-   Compensation
-   Documents

This should not be assumed to be an early-stage feature.

------------------------------------------------------------------------

# 25. Roles and Permissions

Security and access control should be foundational.

Example:

## Owner

Full access.

## Manager

Operational access but limited ownership/security settings.

## Coach

-   Assigned classes
-   Relevant students
-   Attendance
-   Progression

No financial administration unless explicitly granted.

## Reception

-   Students
-   Memberships
-   Payments
-   Trial bookings

## Marketing

-   Leads
-   Website
-   Communications
-   Analytics

Permissions should eventually become granular.

Example:

``` text
students.read
students.write

attendance.read
attendance.write

payments.read
payments.write

website.read
website.write
```

------------------------------------------------------------------------

# 26. Communication

The platform should eventually centralize academy communication.

Potential channels:

-   In-app notifications
-   Email
-   SMS
-   WhatsApp
-   Push notifications

Use cases:

-   Class cancellation
-   Schedule change
-   Membership expiration
-   Payment reminder
-   Trial reminder
-   Promotion announcement
-   Competition reminder
-   Academy announcement

Communication history should be attached to relevant people/events where
appropriate.

------------------------------------------------------------------------

# 27. Automation Engine

Eventually, the platform should allow rule-based automation.

Examples:

``` text
WHEN membership expires in 3 days
→ send reminder
```

``` text
WHEN trial is booked
→ notify reception
→ send confirmation
```

``` text
WHEN trial is tomorrow
→ send reminder
```

``` text
WHEN student misses several classes
→ create follow-up task
```

``` text
WHEN membership expires
→ notify student
→ notify staff
```

The automation engine should become a reusable platform service rather
than hard-coded logic scattered throughout the application.

------------------------------------------------------------------------

# 28. Public Website

Every academy gets a public website.

Potential pages:

-   Home
-   About
-   Programs
-   Disciplines
-   Coaches
-   Schedule
-   Pricing
-   Membership
-   Gallery
-   Testimonials
-   Events
-   Competitions
-   Blog
-   Contact
-   Trial booking

Not every page needs to be enabled.

------------------------------------------------------------------------

# 29. Website Customization

The owner should control the website from the dashboard.

## Theme

-   Template
-   Colors
-   Typography
-   Buttons
-   Layout style
-   Navigation style

## Content

-   Text
-   Images
-   Videos
-   Coaches
-   Programs
-   Schedule
-   Pricing
-   Testimonials

## Sections

Sections should be enabled/disabled and configured.

Example:

``` text
Homepage

☑ Hero
☑ About
☑ Disciplines
☑ Programs
☑ Coaches
☑ Schedule
☑ Pricing
☑ Testimonials
☑ Gallery
☑ Location
☐ Blog
```

------------------------------------------------------------------------

# 30. Website Templates

The platform should eventually offer specialized templates.

Potential templates:

-   Modern Combat Academy
-   BJJ Academy
-   MMA Gym
-   Traditional Martial Arts
-   Competition Academy
-   Kids Martial Arts
-   Premium Performance Gym
-   Minimal Academy

Templates should share the same underlying content model.

------------------------------------------------------------------------

# 31. Domain Management

The product should support:

## Platform Subdomain

Example:

``` text
academy.platform.com
```

## Custom Domain

Example:

``` text
academyname.com
```

The platform may offer domain registration/management as part of the
service.

Potential domain features:

-   Domain connection
-   DNS configuration
-   SSL
-   Renewal tracking
-   Verification
-   Redirects

------------------------------------------------------------------------

# 32. SEO

Website configuration should eventually include:

-   Page titles
-   Meta descriptions
-   Open Graph metadata
-   Sitemap
-   Robots configuration
-   Structured data
-   Local SEO
-   Academy location information
-   Discipline-specific pages

The goal is to help academies attract local prospects.

------------------------------------------------------------------------

# 33. Media Library

Central media management:

-   Images
-   Videos
-   Logos
-   Documents
-   Certificates
-   Gallery assets

Media should be reusable across:

-   Website
-   Student profiles
-   Coach profiles
-   Competitions
-   Academy content

------------------------------------------------------------------------

# 34. Analytics

The academy dashboard should eventually answer:

> How is my academy performing?

## Student Analytics

-   Total students
-   Active students
-   New students
-   Retention
-   Churn
-   Student growth

## Membership Analytics

-   Active memberships
-   Expiring memberships
-   Renewals
-   Cancellations

## Financial Analytics

-   Revenue
-   Recurring revenue
-   Outstanding payments
-   Revenue by program
-   Revenue by location

## Marketing Analytics

-   Leads
-   Trials
-   Trial attendance
-   Conversion rate
-   Lead sources

## Attendance Analytics

-   Attendance rate
-   Class utilization
-   Student attendance trends
-   Program popularity

------------------------------------------------------------------------

# 35. Academy Dashboard

The dashboard should be role-aware.

A typical owner dashboard:

``` text
ACADEMY OVERVIEW

Students             312
Active Memberships   247
New Leads             42
Trials This Month     31
Revenue          108,400
Attendance             78%

---------------------------------

Today's Classes
Upcoming Trials
Expiring Memberships
Outstanding Payments

---------------------------------

Student Growth
Revenue
Attendance
Trial Conversion
```

The dashboard should emphasize **actionable information**, not just
decorative charts.

------------------------------------------------------------------------

# 36. Student Portal

Eventually students should have their own experience.

Potential features:

-   Profile
-   Schedule
-   Membership
-   Payments
-   Attendance
-   Progression
-   Promotions
-   Competitions
-   Notifications
-   Academy information

------------------------------------------------------------------------

# 37. Coach Portal

Coaches should eventually have a focused interface.

Features:

-   Today's classes
-   Class roster
-   Attendance
-   Student profiles
-   Progression
-   Promotions
-   Schedule
-   Competition athletes

The coach experience should be simpler than the owner's dashboard.

------------------------------------------------------------------------

# 38. Parent Portal

For children:

-   Children's schedules
-   Attendance
-   Memberships
-   Payments
-   Notifications
-   Progression
-   Trial booking
-   Academy communication

------------------------------------------------------------------------

# 39. Mobile Applications

Eventually:

## Student App

``` text
Home
Schedule
Membership
Attendance
Progression
Payments
Competitions
Notifications
```

## Coach App

``` text
Classes
Roster
Attendance
Students
Progression
Schedule
```

The web platform should remain the primary foundation; mobile apps can
follow once usage justifies them.

------------------------------------------------------------------------

# 40. Academy Events

Beyond regular classes:

-   Seminars
-   Open mats
-   Workshops
-   Belt ceremonies
-   Competitions
-   Camps
-   Academy social events

Potentially:

-   Event registration
-   Capacity
-   Payments
-   Attendance
-   Public website display

------------------------------------------------------------------------

# 41. E-commerce / Merchandise

Potential future module:

-   Academy merchandise
-   Gi
-   Rash guards
-   Gloves
-   Shirts
-   Equipment
-   Supplements where legally appropriate
-   Event tickets

This should be treated as a later expansion, not assumed to be core.

------------------------------------------------------------------------

# 42. Documents

Academies may eventually manage:

-   Waivers
-   Registration forms
-   Policies
-   Contracts
-   Certificates
-   Student documents

Potential future capabilities:

-   Digital signatures
-   Document templates
-   Expiration tracking

------------------------------------------------------------------------

# 43. Tasks

Internal academy work can eventually be managed through tasks.

Examples:

``` text
Follow up with trial student
Renew equipment order
Call parent
Prepare competition registration
Contact inactive student
```

Tasks may be assigned to staff and linked to students, leads, events, or
memberships.

------------------------------------------------------------------------

# 44. Audit Log

Important actions should eventually be recorded.

Example:

``` text
Sep 10 19:42
Coach Ahmed changed Mohamed's rank
Blue Belt 2 Stripes → Blue Belt 3 Stripes

Sep 10 18:20
Reception recorded payment
450 MAD

Sep 10 17:05
Manager changed Wednesday BJJ class
19:00 → 19:30
```

This is important for accountability and troubleshooting.

------------------------------------------------------------------------

# 45. Search

The platform should eventually have global search.

Example:

``` text
Search: Mohamed
```

Returns:

-   Student
-   Payment
-   Membership
-   Attendance
-   Competition
-   Tasks
-   Communication

Search should be fast and permission-aware.

------------------------------------------------------------------------

# 46. Notifications Center

Users should have a central notification inbox.

Examples:

-   Payment overdue
-   Membership expiring
-   New trial
-   Class changed
-   New website inquiry
-   Competition registration
-   Promotion

Notifications should respect roles and permissions.

------------------------------------------------------------------------

# 47. Integrations

Potential integrations:

-   WhatsApp
-   Email providers
-   SMS providers
-   Online payment providers
-   Google Maps
-   Google Calendar
-   Social platforms
-   Analytics platforms
-   Domain providers
-   Accounting software
-   Future access-control hardware

Integrations should be modular.

------------------------------------------------------------------------

# 48. SaaS Platform Administration

The academy owner is not the only administrator.

The platform operator needs a separate administration system.

## Platform Admin

-   Academies
-   Users
-   Plans
-   Subscriptions
-   Billing
-   Domains
-   Templates
-   Feature flags
-   Usage
-   Support
-   System health
-   Platform analytics

Example:

``` text
PLATFORM

Academies       427
Active          391
Trial            36

MRR
...

New Academies
...

Churn
...

Support Tickets
...
```

------------------------------------------------------------------------

# 49. SaaS Plans

The platform should eventually support different plans.

Example structure:

``` text
Starter
Professional
Business
Enterprise
```

Features can be controlled by plan.

However, pricing and packaging should be determined after customer
validation.

------------------------------------------------------------------------

# 50. White-Label Potential

A mature version could allow deeper branding.

Potentially:

-   Academy domain
-   Academy branding
-   Academy-branded student portal
-   Academy-branded mobile application

This can become a premium offering.

------------------------------------------------------------------------

# 51. Internationalization

The platform should be designed for multiple markets.

Potential requirements:

-   Multiple currencies
-   Multiple languages
-   Timezones
-   Date formats
-   Local payment providers
-   Local tax/invoice requirements
-   Local communication providers

Initial market can be narrow while architecture remains extensible.

------------------------------------------------------------------------

# 52. Security

Security is foundational.

The platform should eventually include:

-   Authentication
-   Authorization
-   Role-based access control
-   Tenant isolation
-   Secure password handling
-   Session management
-   Audit logs
-   Data encryption where appropriate
-   Backup strategy
-   Rate limiting
-   Secure file access
-   Privacy controls

------------------------------------------------------------------------

# 53. Multi-Tenancy

This is a SaaS product.

Multiple academies must coexist safely.

Conceptually:

``` text
Platform
│
├── Academy A
│    ├── Users
│    ├── Students
│    ├── Classes
│    └── Website
│
├── Academy B
│    ├── Users
│    ├── Students
│    ├── Classes
│    └── Website
│
└── Academy C
     ├── Users
     ├── Students
     ├── Classes
     └── Website
```

Data belonging to one academy must never leak into another academy.

------------------------------------------------------------------------

# 54. Core Architectural Principle

The system should be **domain-driven**, not page-driven.

Do not design the backend as:

``` text
Website
Dashboard
Students Page
Payments Page
```

Instead, design around domain concepts:

``` text
Academy
Person
Student
Staff
Discipline
Program
Class
Membership
Payment
Attendance
Lead
Trial
Promotion
Competition
Event
Website
```

The interfaces are views over those domains.

------------------------------------------------------------------------

# 55. Key Relationships

The most important relationships include:

``` text
Academy
 ├── has many Locations
 ├── has many People
 ├── has many Disciplines
 ├── has many Programs
 ├── has many Classes
 ├── has many Membership Plans
 ├── has many Leads
 └── has one Website
```

``` text
Student
 ├── belongs to Academy
 ├── enrolls in Programs
 ├── attends Classes
 ├── has Memberships
 ├── makes Payments
 ├── has Progression
 └── participates in Competitions
```

``` text
Program
 ├── belongs to Discipline
 ├── has Coaches
 ├── has Classes
 └── is available through Memberships
```

``` text
Website
 ├── belongs to Academy
 ├── uses Template
 ├── has Pages
 ├── has Sections
 ├── uses Academy data
 └── uses Custom Domain
```

------------------------------------------------------------------------

# 56. Core User Journeys

The final product should support these major journeys.

## Journey A --- New Academy

``` text
Create account
 ↓
Create academy
 ↓
Configure branding
 ↓
Add location
 ↓
Add disciplines
 ↓
Add programs
 ↓
Add coaches
 ↓
Create classes
 ↓
Create membership plans
 ↓
Customize website
 ↓
Connect domain
 ↓
Publish
```

## Journey B --- New Prospect

``` text
Visit website
 ↓
Choose discipline
 ↓
Book trial
 ↓
Academy receives lead
 ↓
Staff confirms
 ↓
Trial attended
 ↓
Follow-up
 ↓
Membership purchased
 ↓
Student created
```

## Journey C --- Existing Student

``` text
Student
 ↓
Membership
 ↓
Class
 ↓
Attendance
 ↓
Progression
 ↓
Competition
```

## Journey D --- Membership Renewal

``` text
Membership approaching expiration
 ↓
Automation
 ↓
Reminder
 ↓
Payment
 ↓
Membership renewed
```

------------------------------------------------------------------------

# 57. What Makes This Product Different

The product should not compete only on:

> "We have a dashboard."

Generic gym software can do that.

The differentiation should come from the combination:

### 1. Martial-Arts-Native

Belts, ranks, stripes, disciplines, competition records, coaches,
classes.

### 2. Website + Operations

The public website and internal system are connected.

### 3. Extremely Easy Setup

An academy should be able to get online without technical knowledge.

### 4. Customizable Without Being Complicated

Owners get control without needing a full website-building tool.

### 5. Lead-to-Membership Pipeline

Marketing activity connects directly to academy operations.

### 6. Eventually, Academy-Specific Intelligence

The platform can tell an owner:

> "Your BJJ program grew 18% this quarter, but attendance among members
> approaching renewal has declined."

That is much more valuable than raw tables.

------------------------------------------------------------------------

# 58. Long-Term Product Evolution

The long-term progression can be:

``` text
Website
   ↓
Website + Dashboard
   ↓
Academy Management
   ↓
Academy Operating System
   ↓
Academy Growth Platform
   ↓
Combat Sports Ecosystem
```

The platform could eventually support networks of academies,
competitions, instructors, seminars, athletes, and other combat-sports
organizations.

------------------------------------------------------------------------

# 59. What We Should NOT Decide Yet

The vision deliberately does not decide:

-   Exact pricing
-   Exact technology stack
-   Exact database
-   Exact UI design
-   Payment provider
-   WhatsApp provider
-   Domain registrar
-   Mobile technology
-   V1 scope
-   V2 scope
-   Marketing strategy

Those decisions should come after the product map is stable.

------------------------------------------------------------------------

# 60. Product Planning Method

After this vision is accepted, the next planning stage should score
every feature using:

## Business Value

How strongly does it help acquire, convert, retain, or monetize
customers?

## Customer Frequency

How frequently will academy personnel use it?

## Differentiation

How difficult would it be for generic gym software to reproduce our
advantage?

## Complexity

How expensive is it to implement correctly?

## Dependencies

What must exist before this feature can work?

## Risk

Does it introduce significant technical, legal, security, or operational
risk?

------------------------------------------------------------------------

# 61. V1 / V2 / V3 Principle

We should NOT decide versions simply by feature count.

Instead:

``` text
V1
=
Smallest product that can genuinely be sold
and successfully operate an academy.
```

``` text
V2
=
Features that significantly increase operational value,
retention, and willingness to pay.
```

``` text
V3
=
Advanced differentiation, automation,
analytics, ecosystem, and scale.
```

The full vision remains the north star even when individual versions are
intentionally much smaller.

------------------------------------------------------------------------

# 62. North-Star Statement

> **Combat Academy OS is the digital operating system for martial-arts
> academies.**
>
> It gives every academy a professional online presence and a unified
> platform to manage its people, disciplines, programs, classes,
> memberships, attendance, payments, progression, competitions, leads,
> communication, and growth.
>
> The system is designed around the realities of combat sports, while
> remaining flexible enough to support different disciplines, academy
> structures, business models, and locations.

------------------------------------------------------------------------

# 63. Final Product Map

``` text
                         COMBAT ACADEMY OS
                                │
        ┌───────────────────────┼────────────────────────┐
        │                       │                        │
     PUBLIC                    ACADEMY                  PLATFORM
     EXPERIENCE                OPERATIONS                OPERATIONS
        │                       │                        │
   Website                    People                   Academies
   Domain                     Students                 Plans
   SEO                        Parents                  Billing
   Programs                   Coaches                  Domains
   Schedule                   Staff                    Templates
   Pricing                    Disciplines              Feature Flags
   Leads                      Programs                 Support
   Trial Booking              Classes                  Usage
   Content                    Calendar                 Analytics
                              Attendance
                              Memberships
                              Payments
                              Progression
                              Competitions
                              Events
                              Communication
                              Automation
                              Analytics
                              Tasks
                              Documents

                                │
                                │
                         SHARED FOUNDATION
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
          Identity          Permissions        Multi-tenancy
          Security          Audit Logs         Notifications
          Search            Media              Integrations
          Data Model        API                Events
```

------------------------------------------------------------------------

## 64. Repository Direction

The repository should eventually reflect the product domains rather than
the marketing pages.

A conceptual structure could become:

``` text
combat-academy-os/
│
├── docs/
│   ├── vision.md
│   ├── product-map.md
│   ├── domain-model.md
│   ├── user-roles.md
│   ├── workflows.md
│   ├── architecture.md
│   ├── permissions.md
│   └── roadmap.md
│
├── apps/
│   ├── web/
│   ├── dashboard/
│   └── platform-admin/
│
├── packages/
│   ├── ui/
│   ├── domain/
│   ├── database/
│   ├── auth/
│   └── config/
│
└── README.md
```

This is only a conceptual repository structure. The actual
implementation architecture should be decided after the domain model and
V1 scope are defined.

------------------------------------------------------------------------

# 65. Immediate Next Step

Do **not** start coding yet.

The next document should be:

``` text
PRODUCT-MAP.md
```

It should break every major module in this vision into:

-   Features
-   Sub-features
-   Users
-   User stories
-   Dependencies
-   Data entities
-   Business value
-   Complexity
-   Risks
-   Potential integrations

Only after that should we produce:

``` text
ROADMAP.md

V1
V2
V3
Future
```

The vision is the destination.

The product map defines the territory.

The roadmap decides which roads we build first.
