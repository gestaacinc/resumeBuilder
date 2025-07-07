 
# Resume Builder & Referral Program — Product Guide

## 1  Introduction

This document captures:

* A **role-centred swim-lane map** (tasks → events → downstream triggers)
* A **two-week sprint roadmap** to deliver the MVP
* The recommended **technology stack**

Use it as a living backlog skeleton: add acceptance criteria, estimates, and dependencies during refinement.

---

## 2  Role-Centred Map (swim-lane storyboard)

### How to read this table

* **Down a column** = one persona’s journey (e.g., R-02 → R-04 → R-05).
* **Across a row** = the hand-off to the next actor (e.g., C-04 triggers RS-01).

<details>
<summary><strong>Click to expand full swim-lane map</strong></summary>

### 2.1  Referrer (“I bring people in”)

| #        | Referrer user-story (As a Referrer …)                                   | System event it causes →          | Who / What is triggered                          |
| -------- | ----------------------------------------------------------------------- | --------------------------------- | ------------------------------------------------ |
| **R-01** | …I **register** (or convert) so I can earn fees.                        | Referrer profile created          | Enables **C-01** (Candidate can credit referrer) |
| **R-02** | …I **generate a share link / QR** for a vacancy or for the portal home. | Unique token saved                | Powers **C-02** (Candidate arriving via link)    |
| **R-03** | …I **e-mail an invite** so my contact gets a tracked URL.               | Tracked invite e-mail sent        | Also powers **C-02**                             |
| **R-04** | …I **monitor every referral’s status** on my dashboard.                 | Reads application-status stream ← | Fed by **RS-05**                                 |
| **R-05** | …I **see my earnings ledger** so I know when I’ll be paid.              | Reads fee-ledger updates ←        | Fed by **SYS-03** & **FIN-02**                   |

---

### 2.2  Candidate (“I apply for a job”)

| #        | Candidate user-story (As a Candidate …)                           | Trigger ←      | System event it causes →                 | Who / What is triggered |
| -------- | ----------------------------------------------------------------- | -------------- | ---------------------------------------- | ----------------------- |
| **C-01** | …I **sign up** (direct or via referral) so I can build a profile. | Visitor action | Candidate record (+ `referrer_id`) saved | Enables **R-04**        |
| **C-02** | …I **land via ref link / QR** so credit goes to my referrer.      | R-02 / R-03    | Same as C-01                             | —                       |
| **C-03** | …I **complete the CV wizard** for an auto-formatted résumé.       | —              | CV stored & PDF rendered                 | Ready for **C-04**      |
| **C-04** | …I **apply to an open vacancy** with one click.                   | C-03           | Application row created                  | Triggers **RS-01**      |
| **C-05** | …I **track my application status** in a dashboard.                | Reads stream ← | Updated by **RS-05**                     | —                       |
| **C-06** | …I **receive e-mails on status changes** so I stay informed.      | E-mail job ←   | Sent by **SYS-02**                       | —                       |

---

### 2.3  Resource Specialist (RS) (“I screen & endorse”)

| #         | RS user-story (As an RS …)                                       | Trigger ← | System event it causes →      | Who / What is triggered              |
| --------- | ---------------------------------------------------------------- | --------- | ----------------------------- | ------------------------------------ |
| **RS-01** | …I **get a notification** when a new application arrives.        | C-04      | Alert displayed / e-mail sent | Enables RS to act                    |
| **RS-02** | …I **open and review the CV** in the portal.                     | RS-01     | —                             | —                                    |
| **RS-03** | …I **record interview notes & provisional status**.              | RS-02     | Status = “Interviewed”        | Visible to **C-05**, **R-04**        |
| **RS-04** | …I **endorse the candidate** to the DO Team Leader.              | RS-03     | Endorsement object created    | Triggers **TL-01**                   |
| **RS-05** | …I **update final status** (Hired / Rejected) after TL feedback. | TL-02     | Status change saved           | Feeds **C-05**, **R-04**, **SYS-01** |

---

### 2.4  DO Team Leader (TL) (“I give business sign-off”)

| #         | TL user-story (As a TL …)                                                | Trigger ← | System event it causes → | Who / What is triggered |
| --------- | ------------------------------------------------------------------------ | --------- | ------------------------ | ----------------------- |
| **TL-01** | …I **receive an e-mail link** to approve / reject an endorsed candidate. | RS-04     | —                        | —                       |
| **TL-02** | …I **approve / reject** with one click so the RS can proceed.            | TL-01     | TL decision saved        | Drives **RS-05**        |

*If your org skips the TL step, fold TL-01/02 into RS-05.*

---

### 2.5  System (timed & automatic jobs)

| #          | System job                                                        | Trigger | Effect →                                           | Who / What is triggered         |
| ---------- | ----------------------------------------------------------------- | ------- | -------------------------------------------------- | ------------------------------- |
| **SYS-01** | **Start retention clock** when status = “Hired”.                  | RS-05   | Timer row set (T + 30 d, 90 d)                     | Used by **SYS-02**              |
| **SYS-02** | **Check retention milestones** nightly (30 & 90 days).            | Cron    | Emits status-change events (“3 months met” / exit) | Feeds **SYS-03**, e-mails C & R |
| **SYS-03** | **Write fee-ledger entry** (full or prorated) when milestone met. | SYS-02  | Ledger row “Payable”                               | Appears in **R-05**, **FIN-01** |

---

### 2.6  Finance (“I pay the money”)

| #          | Finance user-story (As Finance …)                           | Trigger ← | System event it causes → | Who / What is triggered                |
| ---------- | ----------------------------------------------------------- | --------- | ------------------------ | -------------------------------------- |
| **FIN-01** | …I **export the payable-fees report** each payroll cut-off. | SYS-03    | CSV / API payload ready  | Used in payroll system                 |
| **FIN-02** | …I **mark fees as Paid** after payroll has disbursed funds. | FIN-01    | Ledger row updated       | Notifies **R-05**, sends payout e-mail |

---

### 2.7  Admin / HR Ops (“I configure & monitor”)

| #          | Admin user-story (As an Admin …)                                              |
| ---------- | ----------------------------------------------------------------------------- |
| **ADM-01** | Set or change **fee tiers & proration rules** to keep incentives competitive. |
| **ADM-02** | Manage **roles & permissions** for all users.                                 |
| **ADM-03** | View **programme analytics** (apps, hires, referral share rate, cost / hire). |
| **ADM-04** | Override statuses or fees in edge-case disputes.                              |

</details>

---

## 3  Sprint Roadmap (2-Week Sprints)

| Sprint       | Focus                      | Key outputs / milestones                                 |
| ------------ | -------------------------- | -------------------------------------------------------- |
| **0** (1 wk) | Kick-off & foundations     | Repo, CI pipeline, Docker baseline, Figma wireframes     |
| **1**        | Auth & roles               | JWT sign-up/login, RBAC matrix                           |
| **2**        | HRIS sync & job board      | Vacancy API bridge, search & filter UI                   |
| **3**        | CV builder                 | Multi-step wizard, PDF export, S3 uploads                |
| **4**        | Application workflow       | 1-click apply, RS queue dashboard                        |
| **5**        | Referral engine v1         | Share links / QR, invite e-mails, attribution middleware |
| **6**        | Fee ledger & referrer dash | Tier/prorate engine, earnings dashboard                  |
| **7**        | Notifications & comms      | E-mail templates, digest & retention cron jobs           |
| **8**        | Hardening & QA             | Cross-browser tests, load & security remediation         |
| **9** (1 wk) | UAT & deployment           | Client UAT support, production launch, run-books         |

*Total calendar time ≈ 17 weeks (1 + 8 + 1).*

---

## 4  Recommended Tech Stack

* **Front-end** React + TypeScript (Vite) | Tailwind CSS | shadcn/ui
* **State management** TanStack Query | React Router v6
* **Back-end** Flask (Python 3.12) REST API | SQLAlchemy ORM | Marshmallow
* **Database** MySQL 8 (Amazon RDS or equivalent)
* **Auth** JWT (PyJWT) with refresh-token rotation; OAuth2 ready
* **Files & assets** Amazon S3 + CloudFront CDN
* **E-mail** SendGrid transactional API (HTML templates)
* **QR service** Flask blueprint + python-qrcode
* **Containerisation** Docker & Docker-Compose | CI/CD via GitHub Actions
* **IaC** Terraform modules (VPC, RDS, ECS/Fargate, S3, CloudFront)
* **Observability** CloudWatch logs & metrics | Sentry | AWS X-Ray

---

### Next Steps

1. **Import each user-story** into your tracker.
2. Add **Given / When / Then** acceptance criteria and **story points**.
3. Confirm the **tech-stack** assumptions with DevOps & security teams.
4. Kick off **Sprint 0** (environments, design system, CI baseline).

---

With this markdown guide you can trace *who does what*, *what the system does next*, and *who takes over afterward* — plus you have a clear tech blueprint and delivery cadence.
