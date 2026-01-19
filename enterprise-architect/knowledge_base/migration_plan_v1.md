# Migration Plan: To {YOUR_NAME} Digital Estate
**From:** Ad-Hoc Infrastructure
**To:** {YOUR_NAME} Digital Estate (TOGAF/GitOps)
**Strategy:** "Wrap and Govern"

## Phase 1: Preparation (The Build)
**Goal:** Establish the new Sovereign Cloud foundation.

1.  **Repository Setup:**
    -   Create `{YOUR_NAME}-Digital-Estate` Git repository.
    -   Initialize TOGAF directory structure (`1-Business`, `2-Application`, etc.).
2.  **Control Plane Init:**
    -   Deploy **ArgoCD** (The Governor) on Synology.
    -   Deploy **n8n** (The Orchestrator) on Synology.
    -   Deploy **Redis** (The Bus) on Synology.
3.  **Network Segregation:**
    -   Create `ai-internal` (Isolated) and `ai-bridge` networks.
    -   Configure Synology Ports: Port 1 (Trusted), Port 2 (AI/Docker).

## Phase 2: Media Services Migration (The Import)
**Goal:** Move Media Stack (Plex/*arr) under GitOps control with Zero Data Loss.

1.  **Audit:** IT Admin captures exact `docker-compose.yml` and `.env` from Mac Mini (`~/infra/stacks/media`).
2.  **Refactor:**
    -   Copy config to `4-Technology-Layer/Inference-Plane/Media-Services/docker-compose.yml`.
    -   Ensure **Volume Mappings** match existing named volumes exactly.
    -   Ensure **Port Mappings** match exactly.
3.  **The Switch (Maintenance Window - 5 Mins):**
    -   `docker compose down` (on Mac Mini).
    -   `git push` (to Estate Repo).
    -   ArgoCD (or Script) triggers `docker compose up -d` using the new Repo files.
4.  **Verification:** Check Plex/Sonarr connectivity.

## Phase 3: AI Services Migration (The Upgrade)
**Goal:** Bring Ollama and Paperless under Governance.

1.  **Refactor:** Move `~/infra/stacks/paperless` config to `4-Technology-Layer/Inference-Plane/AI-Services/`.
2.  **Security Hardening:**
    -   Inject **Nginx Proxy** in front of Ollama (Mac Mini).
    -   Update n8n (Synology) to talk to Ollama via the secured Proxy.
3.  **The Switch:** Reboot stack via GitOps.

## Phase 4: {YOUR_COMPANY} Integration (The Bridge)
**Goal:** Connect Business Agents (CEO/CPO) to the Estate.

1.  **Connector Agent:** Deploy `madeai-connector` container to `2-Application-Layer/Agents`.
2.  **API Contract:** Define the Webhook Schema (e.g., `POST /webhook/madeai/trigger`).
3.  **Tool Update:** Update {YOUR_COMPANY} `prompt.md` files to include the new "Remote Tool" capability.

---

## Rollback Plan
If "The Switch" fails:
1.  `docker compose down` (New Stack).
2.  `cd ~/infra/stacks/media && docker compose up -d` (Old Stack).
3.  Services recover immediately using existing data.
