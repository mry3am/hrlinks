# HR Automation Operating System

*Complete product and technical system design | Pakistan-focused SaaS | Version 1.0*

## Page 1

HR AUTOMATION OPERATING SYSTEM
Complete product and technical system design | Pakistan-focused SaaS | Version 1.0 Product objective: enable companies to operate routine HR administration with minimal dedicated HR staff. The platform owns essential employee records, performs approved administrative actions, routes necessary decisions to people, and connects existing company tools where supported.
1. Product scope and selected decisions
Area
Decision
Product
Self-contained, multi-company HR automation SaaS
Target customers
Pakistani SMEs, factories, software houses and growing businesses; initial focus approximately
30-500 employees (hypothesis to validate)
Operating model
Request-based automation plus scheduled recurring obligations Access Employee self-service and supervisor-assisted requests; role-specific workspaces Onboarding Minimal input; suggested employee-type steps with company customization Execution Automatic actions where safe; human approvals, physical tasks and sensitive decisions where necessary Attendance Built-in check-in, biometric imports, supervisor entries and spreadsheet uploads Pay Monthly, daily, hourly, piece-rate and mixed earnings Payroll Reusable payroll groups; complete calculations and payslips; finance authorizes disbursement Policies Company-configurable rules within verified legal constraints Integrations Optional; product works without an existing HRMS AI Suggestions, explanations and document drafting; never final authority for payroll or sensitive decisions
2. System architecture and operating model
Users: Employee Portal | Supervisor Workspace | Manager Workspace | Finance Workspace | Company Administration | Platform Operations.
Flow: User request or scheduled trigger -> information retrieval -> policy and permission validation -> automatic actions -> approval/physical task where required -> resume processing -> verify completion -> notify and archive.
Core layers: (1) role-based frontend; (2) application API and authorization; (3) HR domain services; (4) durable automation and approval coordination; (5) deterministic payroll and policy rules; (6) company data, documents and audit logs; (7) optional external integrations; (8) optional AI assistance.

## Page 2

Request lifecycle
Draft -> Submitted -> Validating -> Processing -> Waiting for Approval/Action -> Processing -> Completed.
Alternative states: Needs Information, Blocked, Rejected, Cancelled and Failed. An external request is not complete until its outcome is confirmed.
Human decision boundaries
Hiring and termination judgments, sensitive investigations, disciplinary decisions, contested attendance, exceptional leave, salary changes, financial release, legal interpretation and physical asset handling remain with authorized people. The system prepares, routes, tracks and records those actions.
3. Complete functional modules
1. Company setup and administration
• Company profile, branches, work locations and departments
• Managers, reporting lines, roles, grades and employee categories
• Policy configuration, holidays, working calendars and approval authority
• Excel imports, record validation and configuration history
2. Employee records
• Unique employee profile, contacts, identity and emergency details
• Contracts, documents, employment status and reporting history
• Effective-dated salary terms, benefits and employment changes
• Restricted sensitive records and document expiry tracking
3. Recruitment and hiring
• Hiring requisition, staffing/budget approval and vacancy publication
• Candidate intake, CV organization and interview scheduling
• Evaluation collection, selection/offer approval and offer letters
• Accepted candidate converted into employee record
4. Onboarding and probation
• Suggested onboarding templates by employee type and company rules
• Reuse candidate data; request only missing joining details
• Document requests, account/equipment tasks, induction and joining confirmation
• Probation review, extension and confirmation coordination
5. Attendance and time office
• App/web check-in, biometric integration, supervisor attendance and file imports
• Source attribution, duplicate and missing-punch checks
• Late arrival, absence, correction and dispute handling
• Daily summaries and verified payroll-ready attendance
6. Shifts, leave and overtime
• Fixed, flexible, rotating and overnight shifts; breaks and rest days
• Shift changes, holidays and scheduled work calendars
• Leave eligibility, balances, requests, approvals and cancellation
• Overtime request, authorization, calculation and reconciliation

## Page 3

7. Payroll and compensation
• Monthly, daily, hourly, piece-rate and mixed pay models
• Payroll groups, individual rates, effective-dated changes and allowances
• Tax, statutory contributions, advances, recoveries and authorized deductions
• Calculation, exceptions, finance approval, payment preparation and payslips
8. Employee services
• Employment and salary certificates; approved template generation
• Employee detail updates, salary advances and loan requests
• Equipment/facility requests, policy questions and request tracking
• Notifications, acknowledgments and completion history
9. Performance and career
• Probation and periodic review cycles; goals and evaluations
• Promotion, increment, transfer and job-change requests
• Performance improvement coordination and training needs
• Approved updates to employment history
10. Employee relations
• Confidential grievance and complaint intake
• Restricted case assignment and evidence collection
• Investigation, hearing, response and appeal tracking
• Authorized decisions, notices and resolution history
11. Training and recognition
• Training scheduling, assignments and attendance
• Completion records and certification expiry
• Surveys, nominations and recognition approvals
• Approved rewards coordinated with payroll
12. Offboarding and settlement
• Resignation/separation intake, notice and last-working-day tracking
• Department clearance, handover, asset return and IT access removal
• Final attendance, leave and payroll reconciliation
• Settlement review, authorization, letters and archival
13. Compliance and reporting
• Location- and employment-category-aware statutory rules
• Minimum-wage, hours, overtime, deductions and contribution checks
• Required records, audit exports, deadlines and retention controls
• Headcount, turnover, absenteeism, payroll and exception reporting
14. Automation and integrations
• Request routing, task assignments, approvals and delegated authority
• Notifications, reminders, escalations and failure handling
• Biometric, email, accounting, payroll and recruitment connectors where supported
• Retries, manual fallback, reconciliation and full action audit

## Page 4

4. Employee lifecycle and onboarding
Recruitment -> accepted offer -> onboarding -> active employment -> probation and career changes ->
separation -> clearance -> final settlement -> archival. The employee has one continuous history; verified information carries forward.
Onboarding: manager confirms accepted candidate; system loads available details, proposes company-approved activities for the employee type, prepares documents, routes necessary signatures, provisions supported accounts, assigns physical equipment tasks, schedules orientation, confirms completion and activates employment according to policy. Joining-date changes update outstanding tasks. Companies can start onboarding without using recruitment first.
5. Attendance, shifts, leave and overtime
Attendance sources and reconciliation
Supported capture: web/app attendance, compatible biometric devices, supervisor submissions and Excel/CSV imports. Every event retains source, timestamp, employee match and any correction history. Imports are validated for duplicates, missing data and conflicts; no silent overwrites.
Shift and leave handling
Shift definitions support fixed, flexible, rotating and overnight schedules, breaks, holidays, rest days and authorized changes. Leave records support eligibility, balances, paid/unpaid treatment, approvals, cancellations and corrections. Paid leave must not be treated as unpaid absence.
Overtime and exceptions
The system identifies extra time but does not automatically treat every late checkout as payable overtime. It applies approved contractual and legal rules, tracks authorization and distinguishes disputes from confirmed work.
Missing punches, device failures, roster conflicts and contested attendance route to authorized reviewers; missing records must not silently cause wage deductions.
6. Payroll: complete functional design
Pay arrangements and payroll groups
Support monthly salary, daily wages, hourly wages, piece-rate output and mixed earnings. Companies configure reusable groups (e.g., office staff, factory operators, supervisors, production workers) with individual rates and controlled exceptions. Group assignment and pay changes are effective-dated; previous payroll rules remain reproducible.
Payroll inputs
• Employee contract, applicable pay model, salary rates and effective dates
• Verified attendance, shifts, paid/unpaid leave and authorized overtime
• Accepted production quantities and rates for piece-rate work
• Approved bonuses, allowances, advances, loan recoveries and salary changes
• Applicable statutory tax, employee deductions and employer contributions
• Payment details, payroll period, cutoff, reviewer and finance authorizer Calculation principles Net payable = eligible base earnings + additional approved earnings - lawful employee deductions. Each component uses the correct employee pay arrangement and verified rules; employer contributions are recorded separately. Apply applicable wage floors, overtime entitlements, contribution rules, tax-year rules and contractual protections. A generic formula must not override jurisdiction-specific law.

## Page 5

Payroll run
Stage
System action
Control
Open period
Snapshot eligible employees, policies and rates
Confirm period
Collect
Load attendance, leave, output and changes
Flag missing inputs
Reconcile
Detect conflicts and duplicates
Resolve material exceptions
Calculate
Apply deterministic earning/deduction rules
Check legal rule versions
Validate
Run wage floor, variance and anomaly checks
Block critical errors
Review
Create register, explanations and payslips
Payroll reviewer
Authorize
Prepare payment instructions
Finance approval
Pay
Export/transmit where supported
Finance releases funds
Confirm
Match payment results and failures
Investigate exceptions
Close
Archive run and release payslips
Lock; corrections are auditable
Required payroll controls
• Every salary component has an explainable calculation and source.
• Missing rates, unapproved overtime, duplicate group membership and invalid deductions are flagged.
• Sensitive salary edits and final payment authorization require separate authorized controls.
• After authorization, changes require an auditable adjustment or formally reopened period.
• Run payroll in parallel with verified historical examples before live deployment.
7. Company payroll policy setup
Five-step guided setup: (1) company, locations, industry and employee categories; (2) salary models and payroll groups; (3) shifts, overtime and working time; (4) allowances, advances, deductions and statutory settings; (5)
payroll period, cutoff, review, finance authorization and payslip delivery.
Provide suggested starting configurations, not unverified legal defaults. Companies can configure pay periods, salary structures, allowances, schedules, approval chains and permitted adjustments. The system blocks or flags settings that conflict with verified applicable requirements. Import existing salary data from spreadsheets and require review of inconsistencies.

## Page 6

8. Access, permissions and data model
Role
Primary permissions
Employee
Own attendance, leave, payslips, documents and requests
Supervisor
Assigned team attendance, shifts, overtime and delegated tasks Manager Hiring requests, team decisions, approvals and reviews Finance Payroll review, financial reports and payment authorization Company administrator Company setup, policies, employee records and exceptional administration Platform operator Subscriptions, provisioning and technical operations; no routine private HR access Core records: organizations, branches, departments, positions, employees, contracts, documents, user accounts, permissions, policy versions, payroll groups, candidates, offers, onboarding tasks, requests, approvals, attendance events, shifts, leave, overtime, salary histories, payroll runs, payslips, payments, assets, complaints, training, offboarding, integration logs and audit events.
Security: company isolation, least-privilege access, MFA for sensitive roles, encryption, secure document storage, audit logs, backups, recovery testing, retention controls, exports and independent approval for high-risk changes.
9. Technical implementation blueprint
Layer
Recommended design
Frontend
Next.js + React, role-specific responsive workspaces
Backend
NestJS modular monolith with explicit authorization
Database
PostgreSQL with tenant-aware access and effective-dated records Automation Durable background jobs, idempotent actions, retries, escalation and reconciliation Rules Deterministic versioned payroll and policy calculations Files Protected object storage for documents and exports Background processing Queue/Redis where needed for reliable processing AI assistance Optional policy explanation, drafting and suggestions; no autonomous payroll decisions Integrations Isolated adapters for compatible devices and external services Operations Monitoring, backups, error alerts, audit and recovery procedures
10. Product experience
Action-first interface, not a traditional menu-heavy HRMS. Employee: attendance, leave, payslips, documents and requests. Supervisor: team exceptions and assigned actions. Manager: pending approvals and workforce actions.
Finance: payroll register, calculation explanations, variances and authorization. Company admin: setup, policies, exceptions, employees and oversight.

## Page 7

Home experience: pending actions, blocked exceptions, completed work, search for people/requests and a few primary actions such as Hire Employee, Request Leave, Run Payroll and Create Document. Every request displays status, owner, next action and history.
11. Compliance, validation and safeguards
Before live payroll, verify applicable rules for the employee work location, classification, industry and effective date: wage floors, working hours, overtime, paid leave, lawful deductions, income-tax withholding, EOBI, provincial social security, required records and payment timing. Keep statutory rules versioned and reviewed by qualified local professionals. Missing or expired rules must be flagged, not guessed.
Sensitive cases, investigations, hiring/termination decisions and financial releases remain human-accountable.
Test real biometric devices, piece-rate production examples, exceptions, payroll reconciliation, tenant isolation, permissions, data retention and integration failures.
12. Delivery roadmap
Phase
Scope
Outcome
1 - Core
Company setup, records, policies, requests, approvals, notifications, documents, imports and audit Routine employee administration
2 - Time
Attendance, shifts, leave, corrections, supervisor approvals and initial biometric imports Verified work and absence records
3 - Payroll
Pay groups, pay models, payroll rules, finance approval, payslips and payment exports Complete internal payroll preparation and administration
4 - Lifecycle
Recruitment, onboarding, probation, career changes, offboarding and settlement Connected employee lifecycle
5 - Expansion
Advanced integrations, training, recognition, AI support and reporting Broader automation coverage Recommended validation: pilot with one company using real employee data and one complete attendance/payroll period; reconcile results against independently verified existing payroll. Validate demand and willingness to pay through customer interviews and paid pilots.
13. Product definition and success criteria
A self-contained HR automation operating system for Pakistani businesses that performs routine administration, coordinates human decisions, maintains verified employee records, calculates payroll and documents every outcome. Its purpose is to reduce manual HR workload, errors and administrative dependence while retaining human accountability.
Primary demonstration: import employees -> configure company policies -> record attendance -> process leave and overtime -> reconcile exceptions -> calculate payroll -> obtain finance authorization -> issue payslips. This end-to-end journey proves the core product value.
