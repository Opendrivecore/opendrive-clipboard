# OpenDrive Clipboard

> **An AI agent that drafts post-drive paperwork for licensed driving instructors to review and approve.**

OpenDrive Clipboard is the **Devpost submission product** for the [Google for Startups AI Agents Challenge](https://googleforstartups.devpost.com/) (event #3197, due **June 5, 2026 5:00 PM PT**). It is the post-drive paperwork-drafting layer of the broader Street Law OpenDrive / OpenDriveEDU platform.

**Tagline:** *Beacon records. Clipboard reviews. The licensed instructor decides.*

---

## What it does

Clipboard receives a fake / demo post-drive scenario (late braking near a crosswalk, residential speed creep, yellow-light indecision, etc.) and turns it into an **instructor-ready draft debrief note** with:

- Safety summary
- Observed safety concern
- Suggested lesson focus
- Reflection prompt for the student
- Practice assignment for next session
- Language-access preview
- Saved demo log entry

**Every output is marked `DRAFT — INSTRUCTOR REVIEW REQUIRED` and cannot reach a student or family until the licensed human instructor approves it.** Nothing reaches a delivered state without the instructor's sign-off. This is the regulatory boundary, the product differentiator, and the load-bearing safety claim.

---

## ICP — who this is built for

**Licensed Washington State Behind-the-Wheel (BTW) driving instructors at small driving schools who currently write post-drive notes by hand.** Mr. Law (founder, licensed BTW instructor) is the persona the product is designed for.

The agent is sold to driving schools, not to teen drivers, families, or car manufacturers. The student in the passenger seat is a downstream beneficiary, not a user.

---

## Two products, one umbrella

| Product | Role | Repository |
|---|---|---|
| **OpenDrive Beacon** | In-vehicle sensor stack (CAN, IMU, GPS, forward camera) — passive recorder, no microphone | private mono-repo |
| **OpenDrive Clipboard** *(this repo)* | Post-drive paperwork-drafting agent (Gemini ADK + 3 MCP servers + Vue/Laravel review-gate UI) | this repo |

OpenDrive is the company / umbrella brand sitting above both products.

---

## Status — Phase A bootstrap (2026-05-01)

This is a fresh public repository. **Code is not in this commit.** It will land across Phases B → E:

| Phase | Window | Deliverable |
|---|---|---|
| **A — Setup** *(now)* | now → May 7 (kickoff) | repo seeded with LICENSE, README, BOUNDARY, planning docs |
| **B — Agent skeleton** | May 7 → May 14 | Gemini ADK agent calls 1 MCP server (`beacon-telemetry-tools`) and emits a draft string |
| **C — Full agent loop** | May 14 → May 21 | All 3 MCP servers up; agent does `classify → retrieve → draft → queue` end-to-end |
| **D — Cloud deploy** | May 21 → May 28 | Cloud Run deploy of agent + MCP + Laravel review-gate dashboard |
| **E — Polish & ship** | May 28 → June 5 | Demo video, README polish, screenshots, Devpost form submitted by **June 4 EOD** (24-hour buffer) |

See `docs/SUBMISSION_PLAN.md` for the full plan, `docs/ENGINEERING_PLAN.md` for the agent architecture, and `docs/PRODUCT_BRIEF.md` for the product spec.

---

## Architecture (planned)

```
┌──────────────────────────────────────────────────────────┐
│  Vue + Laravel demo dashboard  (Cloud Run)               │
│  - Scenario Intake                                        │
│  - Debrief Note Review (HUMAN-REVIEW GATE — hard stop)   │
│  - Language Access Preview                                │
│  - Demo Log                                               │
│  - Boundary page (links to BOUNDARY.md)                  │
└──────────────────────────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────┐
│  Gemini ADK Agent  (Python, Cloud Run, Gemini 2.5 Pro)   │
│  Loop: plan → MCP tool call → reflect → ... → stop       │
└──────────────────────────────────────────────────────────┘
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
       ┌──────────┐ ┌─────────┐ ┌────────────────┐
       │ beacon-  │ │curric-  │ │ instructor-    │
       │ telemetry│ │ ulum-   │ │ review-tools   │
       │ -tools   │ │ tools   │ │ (MCP)          │
       │ (MCP)    │ │ (MCP)   │ │                │
       └──────────┘ └─────────┘ └────────────────┘
```

---

## Hard boundaries

These are non-negotiable and will not change:

1. **The licensed human instructor is the sole authority.** Clipboard never instructs the student.
2. **No microphone.** The vehicle cabin is never recorded.
3. **`DRAFT — INSTRUCTOR REVIEW REQUIRED`** appears on every agent output. Approval is required before any delivery.
4. **Demo data only in this repo.** No live student records, no PII, no production OpenDrive Beacon secrets.
5. **The agent assists the instructor, never the student.** Clipboard is not an "AI coach", "AI driving instructor", or "personal AI tutor". Those phrasings are banned.

See `docs/BOUNDARY.md` for the full statement.

---

## License

Apache License 2.0 — see `LICENSE`.

---

## Contact

**Mr. Law (Jason Law)** — Founder + Lead Instructor, Street Law OpenDrive / OpenDriveEDU
mrlaw@streetlawopendrive.com
https://streetlawopendrive.com

---

*OpenDrive™, OpenDrive Beacon™, and OpenDrive Clipboard™ are common-law working names of Street Law OpenDrive. All marks are subject to counsel-led trademark clearance review.*
