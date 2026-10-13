# Checkpoint 1: System, Design, and Collaboration Baseline

**Due:** Sunday of Week 7, 11:59 PM ET | **Format:** Single PDF + repository link | **One submission per team**

---

# Section 0: Team & Project Identity

| Field              | Your entry                                                                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Project title      | RX-Machina                                                                                                                                                                            |
| One-sentence pitch | "Rx Machina helps patients, prescribers, and pharmacists manage prescription refill requests so that every approval, denial, and fulfillment is tracked in an immutable audit trail." |
| Repository URL     | https://github.com/jenaisssante/RX-Machina                                                                                                                                            |

## Team members

| Name           | Role / main responsibility |
| -------------- | -------------------------- |
| Andrew Boban   | TBD                        |
| Omar Assed     | TBD                        |
| Tyler Brinkman | TBD                        |

---

# Section 1: System Request

## 1.1 Problem & motivation

Patients must call to get status updates on prescription refills, interrupting prescribers and pharmacists who are otherwise engaged in patient care. This creates an inefficient, phone-heavy loop where all three parties lose time to coordination overhead that adds no clinical value.

## 1.2 Goals (2–4 outcomes)

1. Give patients a live view of their refill request status, eliminating the need to call for updates.
2. Provide prescribers and pharmacists a central dashboard to view and act on open requests without interruption.
3. Reduce time-to-fill by removing coordination delays between parties.

**Success metrics:** fewer inbound patient status calls, shorter time-to-fill, fewer workflow interruptions for prescribers and pharmacists.

## 1.3 Scope

| In scope (this project will…)        | Out of scope (explicitly NOT doing) |
| ------------------------------------ | ----------------------------------- |
| User registration and authentication | Third-party OAuth / SSO             |

## 1.4 Stakeholders

## 1.5 Constraints & risks

| Constraint                                                                                | Risk & mitigation |
| ----------------------------------------------------------------------------------------- | ----------------- |
| 7-week timeline                                                                           |                   |
| Patients may only view and create refill requests on their own prescriptions              |                   |
| Prescribers may only view and act on requests belonging to their own patients             |                   |
| Pharmacists must receive only the minimum patient information necessary to fill a request |                   |
| Refill requests may only be created against existing, active prescriptions                |                   |
| Prescription details must not be modified during the refill request lifecycle             |                   |
| Every status change must record the actor, the transition, and the timestamp              |                   |
| Audit records must be immutable once written                                              |                   |
| Denial and unable-to-fill transitions require a written reason                            |                   |

---

# Section 2: Related & Competing Applications

## 2.1 Landscape (at least 3 existing apps)

| Application | What it does         | Strength   | Weakness / gap           |
| ----------- | -------------------- | ---------- | ------------------------ |
| Calendly    | Automated scheduling | Easy setup | No team resource pooling |

## 2.2 Feature comparison

| Feature                | Competitor A | Competitor B | Competitor C | YOUR system |
| ---------------------- | ------------ | ------------ | ------------ | ----------- |
| Real-time availability | Yes          | Yes          | No           | Yes         |

## 2.3 Your differentiator

---

# Section 3: Users & Personas (at least 2)

| Persona (name + one line)                                | Primary goals / what they need                           | Technical level (low/med/high) |
| -------------------------------------------------------- | -------------------------------------------------------- | ------------------------------ |
| Patient — user who needs a prescription refilled         | Submit refill requests and track their status            | Low                            |
| Pharmacist — user who fulfills prescriptions             | View active prescription requests and fill approved ones | Medium                         |
| Prescriber — user who approves or denies refill requests | View open prescription requests and approve or deny them | Medium                         |

---

# Section 4: Functional Requirements (at least 10)

| ID    | Shall statement                                                                                                                                  | Priority | Acceptance check (Given/When/Then)                                                                                                                        |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RX-01 | The system shall restrict each patient to viewing only their own prescriptions and refill requests.                                              | Must     | Given a logged-in patient, when they view prescriptions, then only records linked to their account appear.                                                |
| RX-02 | The system shall allow a patient to submit a refill request for an existing prescription and return a confirmation.                              | Must     | Given a patient with an active prescription, when they submit a refill request, then a new request is created and a confirmation is displayed.            |
| RX-03 | The system shall display the current status of each refill request and update it in real time when it changes.                                   | Must     | Given a patient viewing their requests, when a status changes, then the updated status is visible without a page reload.                                  |
| RX-04 | The system shall allow a patient to cancel a pending request before it is approved and remove it from the prescriber's queue.                    | Must     | Given a request in pending status, when the patient cancels it, then the request is removed from both the patient view and prescriber queue.              |
| RX-05 | The system shall allow a patient to view their full refill history with the outcome of each past request.                                        | Should   | Given a patient, when they open their history, then all past requests and their final statuses are listed.                                                |
| RX-06 | The system shall present prescribers a queue of open refill requests sorted by wait time, oldest first.                                          | Must     | Given a logged-in prescriber, when they open the queue, then requests are ordered by submission time ascending.                                           |
| RX-07 | The system shall allow a prescriber to approve or deny a request, requiring a written reason for any denial.                                     | Must     | Given a prescriber reviewing a request, when they select deny, then submission is blocked until a reason is entered.                                      |
| RX-08 | The system shall allow a prescriber to view a patient's past refill requests while reviewing a new one.                                          | Should   | Given a prescriber on the review screen, when they expand patient history, then prior requests and outcomes are displayed.                                |
| RX-09 | The system shall allow a pharmacist to view approved requests assigned to their pharmacy and mark each as filled.                                | Must     | Given a pharmacist, when they view their queue, then only approved requests for their pharmacy appear and a fill action is available.                     |
| RX-10 | The system shall allow a pharmacist to mark an approved request as unable to fill with a required reason, visible to the patient and prescriber. | Must     | Given an approved request, when a pharmacist marks it unable to fill and enters a reason, then the patient and prescriber both see the status and reason. |
| RX-11 | The system shall allow any authorized user to view the full audit history of a request, showing every status change with timestamp and actor.    | Must     | Given any user with access to a request, when they open the audit trail, then all status transitions, actors, and timestamps are displayed in order.      |

**MVP requirements:** RX-01, RX-02, RX-03, RX-04, RX-06, RX-07, RX-09, RX-10, RX-11

---

# Section 5: Domain Model

*(UML class diagram — at least 3 entities, with relationships, multiplicities, and attributes)*

---

# Section 6: Design Model

## 6.1 Responsibility assignment

| Behavior       | Owner class | GRASP pattern      |
| -------------- | ----------- | ------------------ |
| Reserve a slot | Slot        | Information Expert |

## 6.2 Three-layer structure

| Layer        | Classes / interfaces |
| ------------ | -------------------- |
| Presentation |                      |
| Domain       |                      |
| Data         |                      |

## 6.3 Key class signatures (Java)

```java
// Key design-level operations for your MVP
<ReturnType> <Class>.<method>(<params>)
```

## 6.4 Sequence diagram

*(One non-obvious interaction flow)*

---

# Section 7: Design Principles & Decisions

## 7.1 SOLID decisions

| Principle   | Where it applies                     | Likely change that justifies it               |
| ----------- | ------------------------------------ | --------------------------------------------- |
| Open/Closed | `NotificationPolicy` is an interface | A second notification channel (SMS) is coming |

## 7.2 Architecture Decision Records (2–3)

### ADR-001: \<short title\>

**Status:** Accepted | **Date:** yyyy-mm-dd

**Context:**

**Decision:**

**Consequences:**
- \+ benefit
- \- cost

---

# Section 8: Version Control & Collaboration

## 8.1 Branching and workflow

*(Describe branching strategy and how changes reach main — 3–5 sentences)*

## 8.2 Assignment 1 slice reconciliation

| Team member | Slice contributed                  | How it was reconciled / merged                                              |
| ----------- | ---------------------------------- | --------------------------------------------------------------------------- |
| Jane Smith  | Domain model (User, Slot, Booking) | Merged via PR #3; overlapping `User` class renamed to match team convention |

## 8.3 Repository evidence

*(Paste: one merged pull request with a review comment + commit history view)*

---

# Section 9: Interface Sketches

**Screen 1 purpose:** ____________________________________________

| User action | System response |
| ----------- | --------------- |
|             |                 |

**Screen 2 purpose:** ____________________________________________

| User action | System response |
| ----------- | --------------- |
|             |                 |

---

# Section 10: Technology Stack

| Layer                    | Technology chosen | Why this fits your project |
| ------------------------ | ----------------- | -------------------------- |
| Frontend / UI            |                   |                            |
| Backend / logic          |                   |                            |
| Data storage             |                   |                            |
| External services / APIs |                   |                            |
| AI / LLM (if any)        |                   |                            |
| Build & run              |                   |                            |
| Testing framework        |                   |                            |

**Build-from-CLI:** *(how is the system built and launched from a terminal?)*

---

# Section 11: Quality Requirements (at least 3)

| Quality attribute | Target (measurable) | How you will achieve it                    |
| ----------------- | ------------------- | ------------------------------------------ |
| Performance       |                     | e.g., core action < 2s for 95% of requests |
| Security          |                     | e.g., passwords hashed; owner-only access  |
| Reliability       |                     | e.g., no data loss on crash                |
| Modifiability     |                     | e.g., adding a new [X] touches ≤ 2 modules |

---

# Section 12: AI-Use Disclosure

*(Which AI tools, for what parts, and how you verified the output — 2–4 sentences)*
