# AGENT DIRECTIVE: Server Setup Handbook

> **SYSTEM PROMPT:** This file is the authoritative source of truth for the project structure, technical constraints, and operating procedures. Any AI agent working on this repository MUST consume this context first.

## 1. 🎯 MISSION CONTEXT

**Goal:** Maintain a "Senior SRE Level" documentation and automation suite for Linux Server (Ubuntu/Debian) provisioning.
**Philosophy:** "Plug-and-Play", "Secure by Default", "No Fluff (Direct & Actionable)".
**Target User:** DevOps Engineers, System Admins, and Developers deployment production servers.
**Primary Output:** A high-quality MkDocs static site + A library of Shell Scripts.

---

## 2. 🏗️ ARCHITECTURE & TECH STACK

| Component                | Technology              | Role / Constraint                                                 |
| :----------------------- | :---------------------- | :---------------------------------------------------------------- |
| **Documentation Engine** | MkDocs (Material Theme) | Python-based. Configured in `mkdocs.yml`.                         |
| **Automation Engine**    | Bash Scripts            | Native, Zero-Dependency. Location: `docs/scripts/library`.        |
| **CI/CD Pipeline**       | GitHub Actions          | Defined in `docs/scripts/automation.md` (template).               |
| **Containerization**     | Docker + Compose        | Standard for recipes (`docs/recipes`).                            |
| **SaaS/Product Spec**    | Commercial Plan         | Internal only. Located in `engine-readme.md` (Excluded from git). |

---

## 3. �️ KNOWLEDGE GRAPH (File Map)

### Core Configuration

- `mkdocs.yml`: **[CRITICAL]** The central navigation map. Every new markdown file MUST be linked here.
- `.gitignore`: Controls build artifacts and internal product specs.

### Functional Directories (`docs/`)

- `checklists/`: **[PROTOCOL]** Human-verified procedures. Not just "Todo" lists, but "Verification" matrices (Go/No-Go).
  - `verify_step`: Every item must have a cli command to verify status.
- `scripts/library/`: **[EXECUTABLE]** Production-ready code.
  - `bootstrap.md`: The "Universal Init" script.
  - `maintenance.md`: Self-cleaning logic.
- `security/`: **[HARDENING]** Defensive guides.
  - `docker-gateway.md`: **[CRITICAL]** Networking model to prevent UFW bypass.
- `runbooks/`: **[INCIDENT]** "Break Glass" guides for emergencies.
- `recipes/`: **[TEMPLATES]** Golden images for Docker (React, .NET, PG).

---

## 4. ⚙️ STANDARD OPERATING PROCEDURES (SOP)

### SOP-001: Adding a New Guide

1.  Create the file in the appropriate subdirectory (e.g., `docs/how-to/new-topic.md`).
2.  **IMMEDIATELY** add the link to `mkdocs.yml` navigation.
3.  Ensure the guide includes "Verification" steps (how to check if it worked).

### SOP-002: Updating Scripts

1.  Edit the markdown file (e.g., `docs/scripts/library/bootstrap.md`).
2.  Ensure the code block is valid Bash.
3.  Check for **Idempotency** (script should run multiple times without error).
4.  Add a `log` entry (What changed?) in the file header.

### SOP-003: Handling Internal Specs

1.  Spec file `engine-readme.md` contains the SaaS business logic.
2.  This file is **INTERNAL**. Do not expose its contents in the public `walkthrough.md` or other user-facing docs unless explicitly requested.

---

## 5. 🧠 MEMORY & CONTEXT (Current State)

- **Active Sprint:** "Automation & Operations Overhaul" complete.
- **Key Feature:** "Licensed CLI" distribution model accepted for SaaS.
- **Latest Upgrade:** GitHub Actions guide added with "Pre-Flight Checks".
