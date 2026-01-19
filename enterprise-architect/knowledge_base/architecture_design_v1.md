# Architecture Design Specification v1.0
**Project:** {YOUR_NAME} Digital Estate
**Author:** Enterprise Architect ({YOUR_COMPANY})
**Date:** 2026-01-19
**Status:** DRAFT

## 1. Executive Summary

This document defines the architecture for a scalable, self-governing, and autonomous AI orchestration platform hosted on personal infrastructure (Synology NAS + Mac Mini).

**Core Philosophy:**
-   **Headless & API-First:** All interactions are programmatic. No dependency on GUIs.
-   **GitOps Governance:** "Git is the Source of Truth." All workflows, agent configurations, and infrastructure states are defined in code.
-   **Micro-Agent Architecture:** Small, single-purpose containers communicating via a message bus, orchestrated by a central workflow engine.

---

## 2. System Topology

The system is split into two physical planes to optimize for "Always-On Reliability" (NAS) vs "High-Performance Inference" (Mac).

### 2.1 The Control Plane ({YOUR_NAS})
*Hosted via Docker / Container Manager*

| Component | Technology | Role |
|-----------|------------|------|
| **Orchestrator** | **n8n** (Self-Hosted) | The "Conductor." Executes workflows, manages triggers, handles API glue. |
| **GitOps Controller** | **ArgoCD** (or Git-Sync) | Syncs state from GitHub to local infrastructure. |
| **Message Bus** | **Redis** | Async communication channel for agents (Pub/Sub). |
| **State Store** | **PostgreSQL** | Long-term memory for agents and workflow history. |
| **Vector Store** | **Qdrant** | Semantic memory for AI retrieval (RAG). |

### 2.2 The Inference Plane (Mac Mini)
*Hosted via Colima / Docker*

| Component | Technology | Role |
|-----------|------------|------|
| **LLM Provider** | **Ollama** | Exposes local LLMs (Llama 3, Qwen) via API. |
| **Worker Agents** | **Python/Docker** | Heavy-duty ephemeral agents (e.g., "Deep Research"). |

### 2.3 The Edge Plane (Raspberry Pi 5)
*Hosted via Docker*

| Component | Technology | Role |
|-----------|------------|------|
| **Ingress** | **Nginx PM** | Security Gateway (MFA/Auth). |
| **DNS** | **AdGuard** | Primary DNS (Redundant on NAS). |
| **Watchdog** | **Agent** | Security Monitor (Physical Isolation). |

---

## 3. GitOps Workflow (The Governance Model)

This is the mechanism that ensures **Self-Governance** and **Immutability**.

1.  **Code:** You (or an AI Designer) write a workflow definition (JSON) or Agent Config (YAML).
2.  **Commit:** Changes are pushed to a `feature-branch` on GitHub.
3.  **Validate (CI):** GitHub Actions trigger:
    -   *Linting:* Is the JSON valid?
    -   *Policy Check:* Does this agent violate safety rules? (e.g., "No direct email sending").
    -   *Integration Test:* Dry-run the workflow.
4.  **Merge:** Upon approval, code merges to `main`.
5.  **Deploy (CD):**
    -   **ArgoCD** detects the change in `main`.
    -   It pulls the new config to the Synology.
    -   It restarts the relevant container or updates the n8n workflow.

**Outcome:** The live system *always* matches the Git repo. No manual "tinkering" allowed.

---

## 4. Directory Structure (TOGAF Aligned)

The repository structure mirrors the Architecture domains:

```
personal-ai-orchestration/
├── 1-Business-Layer/          # [Phase B] Rules & Processes
│   ├── Governance/            # OPA Policies & RBAC
│   └── Workflows/             # n8n Workflow Definitions
├── 2-Application-Layer/       # [Phase C] Logical Components
│   ├── Agents/                # Micro-agent Code (Python)
│   └── Orchestrator/          # Custom n8n Nodes
├── 3-Data-Layer/              # [Phase C] Information Architecture
│   ├── Schemas/               # SQL DDLs
│   └── Event-Bus/             # Redis Topic Standards
├── 4-Technology-Layer/        # [Phase D] Physical Infrastructure
│   ├── Control-Plane/         # Synology Docker Config
│   └── Inference-Plane/       # Mac Mini Docker Config
└── .github/                   # [ADM Governance]
    └── workflows/             # CI/CD Pipelines
```

## 5. Technology Stack Selection

| Layer | Selection | Rationale | Cost |
|-------|-----------|-----------|------|
| **Repo** | **GitHub** | Industry standard, free Actions runners. | Free |
| **CD** | **ArgoCD** | Best-in-class GitOps, runs in K8s/Docker. | OSS |
| **Orchestration** | **n8n** | Visual design, JSON export, self-hosted. | OSS |
| **Bus** | **Redis** | Fast, simple, reliable Pub/Sub. | OSS |
| **Inference** | **Ollama** | Zero-latency local LLM, OpenAI-compatible API. | OSS |

---

## 6. Implementation Roadmap

### Phase 1: Foundation (IT Admin Task)
-   [ ] Set up GitHub Repository structure.
-   [ ] Deploy Redis & PostgreSQL on Synology.
-   [ ] Deploy n8n with Git volume mount.
-   [ ] Configure simple "Hello World" GitOps loop.

### Phase 2: The First Agent (Email Triage)
-   [ ] Build "Classifier" container (Python + Ollama).
-   [ ] Define n8n workflow (IMAP Trigger -> Redis -> Agent).
-   [ ] Implement GitOps policy: "Draft Mode Only" (No send).

### Phase 3: Scaling
-   [ ] Deploy ArgoCD for full cluster management.
-   [ ] Add Vector Store (Qdrant) for long-term agent memory.

---

## 7. Governance Rules (Policy-as-Code)

1.  **No Direct Internet Output:** Agents cannot POST to external APIs unless whitelisted in the Orchestrator.
2.  **Human-in-the-Loop:** All "Write" actions (Email, Post, Delete) require a Human Approval step in n8n.
3.  **Resource Limits:** Docker containers must have CPU/RAM limits defined in `docker-compose.yml`.
