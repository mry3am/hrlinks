# Our Final Product: AI-Powered HR Automation System

In simple words, **we are building a software that works like a digital HR department.**
A company will use our system to manage employees, attendance, salaries, hiring, leave, and other HR activities. Instead of HR staff manually performing every step, **our system will automatically do most of the work and involve humans only when necessary.**

## 1. What will our system actually do?

Imagine a textile factory with 200 employees.

The owner registers the company on our platform, imports employee records, sets company policies, and assigns managers.

After that:

Employees mark attendance, and our system automatically tracks it.

Employees request leave, and the system checks eligibility and sends it for approval.

Managers approve requests, and the system updates the relevant records.

The system calculates salaries using attendance, leave, overtime, and payroll policies.

Finance reviews and authorizes payroll.

New employees are onboarded automatically through guided workflows.

Employees leaving the company go through an automated clearance and settlement process.

The owner sees reports and pending actions without asking HR to prepare them.

**Our system becomes the central place where HR work happens.**

## 2. How will end users use it?

### One platform, different experiences

### Company Owner / Admin

Sets up the company, policies, employees, and monitors everything.

### Employee

Marks attendance, requests leave, views payslips, and submits requests.

### Manager / Supervisor

Approves requests, verifies attendance, and completes assigned tasks.

### Finance

Reviews salaries, approves payroll, and authorizes payments.

Workers without smartphones can use biometric attendance, and supervisors can submit permitted requests on their behalf.

## 3. In what way are we providing automation?

This is our biggest selling point.

### Example: An employee requests leave.

### Employee submits leave request

### System automatically checks

Leave balance, company policy, eligibility, conflicting requests

### Manager approves when required

### System finishes everything

Updates leave balance, attendance, payroll inputs, and notifies the employee
No HR employee needs to manually transfer information between departments.

We will use four types of automation:
| Type | What it means |
| --- | --- |
| Rule-based | Automatically check company policies and calculate results |
| Workflow-based | Move requests through approvals, tasks, and completion |
| Event-based | Automatically react when something happens, such as a new employee joining |
| AI-assisted | Suggest steps, draft documents, answer policy questions, and explain results |

AI will assist with understanding and recommendations. **Sensitive decisions and salary calculations will use controlled approvals and deterministic rules, not AI guesses.**

## 4. How many workflows will we have?

Our planned system has **14 main modules and approximately 60–80 individual workflows**.

The exact number will be finalized during detailed workflow specification.
| Module | Estimated workflows |
| --- | --- |
| Recruitment & Hiring | 6 |
| Onboarding & Probation | 6 |
| Employee Records | 5 |
| Attendance | 6 |
| Shifts & Overtime | 5 |
| Leave Management | 5 |
| Payroll & Compensation | 8 |
| Employee Services | 5 |
| Performance & Career | 5 |
| Employee Relations | 4 |
| Training & Recognition | 4 |
| Offboarding | 6 |
| Compliance & Reporting | 5 |
| Administration & Automation | 6 |
| Total planned | 76 |

These are estimated workflow counts, not 76 already-designed or implemented automations.

## 5. What technologies will we use?

| Layer | Recommended technology | Purpose |
| --- | --- | --- |
| Frontend | Next.js + React + TypeScript | Website, employee portal, admin dashboard |
| Backend | NestJS + TypeScript | Main business logic and APIs |
| Database | PostgreSQL + Prisma | Employee, attendance, payroll and workflow records |
| Automation | Temporal | Reliable long-running workflows, retries and human approval waits |
| Queue / Cache | Redis + BullMQ | Notifications, imports and smaller background jobs |
| AI | Python FastAPI + LLM API | AI suggestions, explanations and document assistance |
| File Storage | S3-compatible storage | Contracts, documents, payslips |
| Authentication | Secure session/OIDC authentication + RBAC | Login, permissions and access control |
| Integrations | REST APIs, webhooks, CSV/Excel adapters | Biometric devices and existing company tools |
| Deployment | Docker + managed cloud services | Hosting and scaling |
| Monitoring | OpenTelemetry + error monitoring | Track failures and performance |

We should start with a **modular monolith**, not complicated microservices. Temporal is a recommended workflow-engine choice, subject to operational cost and implementation evaluation.

## 6. How will the company start using our system?

**Register company** — Owner creates a company workspace.

**Configure policies** — System suggests attendance, leave, payroll and approval settings.

**Import employees** — Upload Excel records or add employees manually.

**Assign roles** — Set managers, supervisors, finance approvers and employee access.

**Connect existing tools** — Optionally connect biometric devices, email and accounting software.

**Start operations** — Employees and managers begin using the platform.

**System runs workflows** — Routine processes execute automatically; exceptions go to authorized people.

## 7. What makes our product different from a normal HRMS?

| Traditional HR software | Our HR automation product |
| --- | --- |
| HR enters and updates records | System collects and updates permitted records automatically |
| HR manually follows up | System sends reminders and escalations |
| HR prepares payroll inputs | System reconciles inputs and calculates payroll |
| HR coordinates onboarding | System creates and manages onboarding tasks |
| HR tracks pending approvals | System routes and monitors approvals |
| HR prepares routine documents | System generates documents from approved templates |
| HR creates reports | System generates reports automatically |
| HR coordinates processes across tools | System executes connected workflows and assigns remaining tasks |

## 8. What is our final goal?

PRODUCT VISION
**A company should be able to run most routine HR administration without needing a large HR team.**
Our platform will **manage employee information, execute HR workflows, calculate payroll, coordinate approvals, handle routine requests, and report exceptions automatically**.

Humans remain responsible for hiring decisions, sensitive employee matters, required verification, and financial authorization.

**In one sentence:** We're building an **automation-first HR operating system** where employees submit requests, managers make necessary decisions, finance authorizes payments, and the software handles the administrative work between them.
