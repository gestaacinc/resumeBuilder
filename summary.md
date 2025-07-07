# Resume Builder — Solo-Dev Roadmap & Feature Matrix

---

## 1  Feature matrix (core release)

| Role | Must–have features for first release |
|------|--------------------------------------|
| **Candidate** | • Register / log-in<br>• **CV wizard** (edit + PDF)<br>• **Job board** (real-time HRIS feed)<br>• **1-click apply**<br>• Status dashboard<br>• E-mail alerts<br>• **Toggle “Become a Referrer”** |
| **Referrer** | • Convert or sign-up<br>• **Generate share link / QR**<br>• **Send invite e-mails**<br>• Referral dashboard (list + accrued ₱)<br>• **Self-apply** to jobs (R-06) |
| **Resource Specialist** | • Notification queue<br>• **CV viewer**<br>• Interview notes + provisional status<br>• **Endorse → TL**<br>• **Set final status** (Hired / Rejected)<br>• **Search CV pool + invite** (RS-06) |
| **DO Team Leader** | • E-mail deep-link with **Approve / Reject** (2-click screen) |
| **System jobs** | • Start retention timer (SYS-01)<br>• Nightly milestone check (SYS-02)<br>• **Accrual-only fee ledger** (SYS-03) |
| **Admin / HR** | • CRUD fee tiers (ADM-01)<br>• Minimal role management (ADM-02)<br>• Toggle “Become a Referrer” default |

> *Finance module is out of scope.*

---

## 2  Solo-developer plan (10 × 1-week iterations)

| Week | Sprint theme & granular tasks | Effort (ideal-dev-days) |
|------|------------------------------|-------------------------|
| **0 Setup** | Repo scaffold · Dockerfile & compose · GitHub CI · staging env | **5 d** |
| **1 Auth & models** | Flask-User/JWT endpoints · role table · RBAC decorator | **5 d** |
| **2 Candidate MVP** | Sign-up form · multi-step CV wizard skeleton | **5 d** |
| **3 CV PDF + HRIS stub** | WeasyPrint → PDF → S3 · mock vacancy API · job-board list/search | **5 d** |
| **4 Apply flow & RS queue** | Apply mutation · RS inbox · status dash · self-apply toggle | **5 d** |
| **5 Referral engine v1** | Token + QR generator · invite e-mail · Referrer dash skeleton | **5 d** |
| **6 RS full actions** | CV viewer · interview log · endorse flow · TL approve screen · **RS-06 search+invite** | **5 d** |
| **7 Fee accrual** | Retention cron · tiered calc · fee ledger API · Referrer accrued ₱ panel | **5 d** |
| **8 Notif + Admin** | HTML mails · digest job · fee-tier CRUD · tiny audit log | **5 d** |
| **9 Polish & QA** | Mobile tweaks · Cypress smoke tests · load test | **5 d** |
| **10 UAT & Launch** | Stakeholder demo · bug-fixes · Terraform → prod · hand-off docs | **5 d** |

**Total = 50 ideal-dev-days ≈ 10 calendar weeks (≈ 2.5 months)**

---

## 3  Solo-dev risk & buffer checklist

| Risk | Mitigation |
|------|------------|
| HRIS API credentials delayed | Mock JSON first; swap in real keys later |
| PDF styling rabbit-hole | Use a ready-made resume template; polish post-MVP |
| QR UI fiddliness | Reuse an Alpine/React component; focus on backend accuracy |
| Burn-out | Reserve Fridays for refactor + QA; push new scope only on Mondays |

 

---
