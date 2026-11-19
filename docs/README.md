> **Architecture refresh:** Recent Sleex changes include keeping the eight-tab Quick Ball menu within the viewport and publishing this interactive deck directly through GitHub Pages. The deck describes multi-cloud workers, Azure Container Apps, Cloudflare Workers edge runtime, and zero-leak credential isolation as architecture principles.

> **Team:** The Order of the Phoenix (v5) — Coordinator Harry; Coder Ron; Designer Luna; Maintainer McGonagall; Professor Dumbledore; Reviewer Hermione; Tester Ginny.

# Sleex: Project Documentation & Technical Architecture Deck

> **Role & Perspective:** Technical Architecture & Systems Engineering  
> **Focus & Phase:** Live Multi-Cloud Testing Phase, Dogfooding Benchmarks & System Verification  
> **Primary Asset:** [sleex-presentation.pdf](sleex-presentation.pdf) (18-Slide High-Resolution 16:9 PDF Deck)  
> **Source Deck:** [sleex-presentation.html](sleex-presentation.html) (HTML5/CSS Print Template)  
> **Slide Previews:** [slides/](slides/) (High-resolution PNG renders of each slide)

---

## Executive Summary & Strategic Positioning

**Sleex** and its agent runtime **DeepSeek Harness** ("The Burrow") represent an architectural leap in autonomous AI-assisted software engineering. 

While legacy dev tools (Cursor, Windsurf) remain tethered to developer laptops, and autonomous platforms (Devin, Codex Cloud, Claude Coworker) force customers into expensive, single-model proprietary walled gardens costing hundreds of dollars per seat, **Sleex** delivers:

1. **Ubiquitous 24/7 Mobile & Desktop PWA:** A zero-install, offline-capable Progressive Web App running on Cloudflare Workers and global edge static assets ([sleex.org](https://sleex.org)).
2. **Seven-Agent Engineering Team ("The Order of the Phoenix"):** Harry (Coordinator), Ron (Coder), Hermione (Reviewer), McGonagall (Maintainer), Dumbledore (Professor), plus Dumbledore's Army, Luna (Designer) and Ginny (Tester), running in DeepSeek Harness at sleex.org/burrow/. They turn tasks into tested, reviewed pull requests; merges wait for the owner's go.
3. **90%+ Lower Operating Unit Economics:** Powered by lossless **Headroom 0.41 token compression** (saving 40–70% on LLM context tokens), Cloudflare SQLite Durable Objects, and dynamic workload placement leasing ephemeral serverless micro-workers (AWS Lambda, GCP Cloud Run, Azure Container Apps) and Oracle Cloud Always-Free ARM VPS.
4. **Zero Vendor Lock-In & Model Sovereignty:** Intelligent multi-provider proxy with dynamic price-band parsing (`goLimits`), automatic fallback chains, free-tier prioritization (Gemini Flash, Groq, Cloudflare Workers AI), and Bring-Your-Own-Key (BYOK) support.

---

## Presentation Slide Deck Overview (18 Slides)

The complete presentation slide deck is compiled as an engineering-ready, 16:9 widescreen PDF at [`sleex-presentation.pdf`](sleex-presentation.pdf).

### Slide Index & Core Themes

| # | Slide Title | Category | Core Content & Value Delivered |
|---|---|---|---|
| **01** | **Sleex: Cover Slide** | Title & Meta | Brand identity, technical architecture overview, live testing focus badge |
| **02** | **Live Testing Phase: Objectives, Scope & Readiness** | Quality & Verification | Dogfooding targets (-92% cost, 24/7 autonomous loop, multi-cloud workers) |
| **03** | **The Problem: AI Coding is Broken, Expensive & Trapped** | Architecture Context | Token bloat, desktop laptop lock-in, proprietary silos, PR review bottleneck |
| **04** | **The Solution: The Sleex Autonomous Studio** | Breakthrough | PWA Edge UI, DeepSeek Harness Team, Hybrid Ephemeral Sandboxes |
| **05** | **Mission & Vision: Sovereign AI Engineering** | North Star | Guiding pillars: Ubiquity, Frugality, Autonomy, Sovereignty |
| **06** | **Tech Stack (Part 1): Edge & Frontend** | Architecture | React 19, TS 7, Cloudflare Workers, CodeAgent/MemberDirectory Durable Objects |
| **07** | **Tech Stack (Part 2): VPS & Workspace v3** | Architecture | OCI Ampere ARM64, sleex-edge sshd, sleex-headroom, sleex-manager, sleex-harness, sleex-hindsight |
| **08** | **System Architecture & Traffic Routing Flow** | Flowchart | Complete vector diagram: User PWA → Edge DO → VPS → Workers → GitHub |
| **09** | **The Order of the Phoenix (v5): Seven-Agent Team** | Multi-Agent Swarm | Harry (Coordinator), Ron (Coder), Hermione (Reviewer), McGonagall (Maintainer), Dumbledore (Professor), Luna (Designer), Ginny (Tester) and the task lifecycle |
| **10** | **Model Proxy & Headroom Token Optimizer** | Unit Economics | Headroom compression (-70% tokens), `goLimits` price bands, $0.18/PR |
| **11** | **Hybrid Ephemeral Compute: AWS, GCP & Azure** | Virtualization | Dynamic placement (Lambda, Cloud Run, Azure ACA), Zero-trust reverse SSH |
| **12** | **Competitive Landscape: Why Sleex Wins** | Competitive Matrix| Head-to-head vs. Codex, Claude Coworker, Devin, Cursor/Windsurf |
| **13** | **SWOT Analysis: Strategic Evaluation** | SWOT Matrix | 2x2 matrix: Internal Strengths & Weaknesses vs. External Opportunities & Threats |
| **14** | **Identified Gaps & High-Value Opportunities** | Gap Analysis | Multi-tenant SaaS provisioning, PWA live canvas, spend guardrails |
| **15** | **Actionable Product Roadmap (2027)** | Execution Plan | 4-Quarter milestone timeline: Q1 Hardening, Q2 Canvas, Q3 Fleet, Q4 Production |
| **16** | **Live Testing Methodology & Dogfooding Benchmarks** | Testing Phase | Phase 1 Dogfooding, Phase 2 Multi-Cloud, Phase 3 Headroom Verification |
| **17** | **Test Harness, Automated Suites & Verification** | Quality Assurance | Automated test suites (`tests/*.test.mjs`, `azure-worker.test.mjs`, `cloud-sandbox.test.mjs`) |
| **18** | **Live Testing Phase: Conclusions & Next Steps** | Next Steps | Verification roadmap, internal issue dogfooding, and immediate technical milestones |

---

## Detailed Architectural Breakdown

### 1. Edge & Frontend Layer (Sleex Web & Core Services)
- **React 19 & TypeScript 7 PWA:** Served from Cloudflare Workers Static Assets with strict Cache-Control headers (`public, max-age=0, must-revalidate` for `index.html`, immutable hashed assets). 
- **Owner-Scoped SQLite Durable Objects:**
  - `CodeAgent`: GitHub App token, MCP and Cloudflare connections, cloud box lease state.
  - `MemberDirectory`: Members, access tiers, and Web Push event dispatcher.
- **Model Relay & MCP Server:** DSH reaches every model through `/api/assistant/model/<vendor>` (via Headroom) and every tool through `/api/mcp` (GitHub, Cloudflare, VPS, Portainer). No provider keys live on the VPS.
- **Dynamic Model Cost Router:** Dynamic price tiering (`goLimits`), BYOK token encryption, and model fallbacks from Gemini 2.5 Flash / Groq to Claude 3.7 Sonnet and GPT-4.1.

### 2. VPS & Workspace v3 Layer (sleex-vps, sleex-dsh)
- **Oracle Cloud Always-Free ARM Architecture:** 4 Ampere A1 vCPUs, 24 GB RAM, 200 GB NVMe storage, running Ubuntu 24.04 LTS.
- **Hardened Multi-Container Topology (Zero Exposed Listening Ports):**
  - `sleex-edge`: Hardened OpenSSH daemon dropping all Linux capabilities (`cap_drop: [ALL]`, `no-new-privileges: true`). Reached via Cloudflare Access (`edge.sleex.org`). Dynamically queries Sleex API (`box-key.mjs`) to authorize inbound reverse SSH tunnels from leased ephemeral workers.
  - `sleex-headroom`: Dedicated Headroom 0.41 token saver proxy. Eliminates redundant prompt boilerplate and compresses verbose tool call results, reducing token expenditure by 40–70%.
  - `sleex-manager`: Admin API daemon. Handles host metrics, Docker monitoring, container lifecycle, and one-click blue/green stack updates (`updater.mjs`) with 3-minute self-healing rollback.
  - `sleex-harness`: DeepSeek Harness ("The Burrow", own Portainer stack, 8 GB / 2 vCPU, loopback only behind Sleex login). Runs the seven-agent team with per-role souls and skill loadouts, Playwright Chromium, and the cloud box launcher (`cloud-sandbox.mjs`). Restart = upgrade from pixca-net/sleex-dsh.
  - `sleex-hindsight`: Hindsight long-term agent memory (recall and reflect per repository bank).

---

## Interactive Architecture & Workflow Flowcharts

### Flowchart 1: High-Level System Architecture & Traffic Routing

```mermaid
flowchart TD
    subgraph ClientLayer ["Client Layer (Any Device)"]
        User["User on Mobile / Desktop PWA<br/>(sleex.org)"]
    end

    subgraph EdgeLayer ["Cloudflare Global Edge (sleex Worker)"]
        WorkerRouter["Worker Gateway Router<br/>(Google OAuth 2.0 PKCE / CORS)"]
        MCP["Sleex MCP Server (/api/mcp)<br/>GitHub, Cloudflare, VPS, Portainer"]
        CodeDO["CodeAgent (Durable Object)<br/>GitHub Token, MCP, Cloud Leases"]
        ModelProxy["Model Proxy & Cost Router<br/>(goLimits Bands & Free Tier Priority)"]
    end

    subgraph VPSLayer ["Oracle ARM VPS (Workspace v3 / WireGuard)"]
        Tunnel["Cloudflare Access Tunnel<br/>(sandbox.sleex.org / edge.sleex.org)"]
        EdgeSSHD["sleex-edge (SSH Gateway)<br/>sshd + box-key.mjs Auth"]
        Headroom["sleex-headroom (Token Proxy)<br/>Token Compressor (-70%)"]
        Manager["sleex-manager (Admin API)<br/>Admin API & Auto-Updater"]
        Harness["sleex-harness: DeepSeek Harness<br/>Seven-Agent Team (The Burrow)"]
        Hindsight["sleex-hindsight<br/>Long-Term Agent Memory"]
    end

    subgraph ComputeLayer ["Ephemeral Cloud Workers"]
        Lambda["AWS Lambda Micro-Workers<br/>(&lt;12 min / ~170 free runs/mo)"]
        CloudRun["GCP Cloud Run Containers<br/>(&lt;30 min heavy builds)"]
        AzureACA["Azure Container Apps (ACA)<br/>(ARM Microsoft.App/jobs on-demand)"]
    end

    subgraph UpstreamProviders ["LLM Providers & GitHub"]
        LLMs["OpenRouter / Anthropic / OpenAI / Gemini / Groq / NVIDIA"]
        GitHub["GitHub API / Repositories / Actions"]
    end

    User -->|HTTPS / WSS| WorkerRouter
    WorkerRouter --> MCP
    WorkerRouter --> CodeDO
    WorkerRouter --> ModelProxy
    ModelProxy --> LLMs
    CodeDO -->|Tunnel| Tunnel
    Tunnel --> Manager
    Tunnel --> EdgeSSHD
    Tunnel --> Harness
    Harness --> Headroom
    Headroom -->|Sleex model relay| ModelProxy
    Harness --> Hindsight
    Harness -->|Tools| MCP
    Harness -->|Reverse SSH Sockets| EdgeSSHD
    Lambda -.->|Outbound Reverse SSH| EdgeSSHD
    CloudRun -.->|Outbound Reverse SSH| EdgeSSHD
    AzureACA -.->|Outbound Reverse SSH| EdgeSSHD
    MCP --> GitHub
```

### Flowchart 2: Order of the Phoenix PR Pipeline

```mermaid
flowchart LR
    Task["Owner's task /<br/>GitHub issue"] --> Harry["1. Harry (Coordinator)<br/>Plans with the Elder Wand"]
    Harry -->|Delegates| Ron["2. Ron (Coder)<br/>Harness workspace +<br/>cloud box for heavy runs"]
    Ron -->|Pushes branch| PR["Pull Request"]
    PR --> Hermione["3. Hermione (Reviewer)<br/>Re-runs checks, fixes findings"]
    Hermione --> Ginny["4. Ginny (Tester)<br/>Regression tests"]
    Ginny --> Go{"Owner's go?"}
    Go -->|Not yet| PR
    Go -->|Yes| McGonagall["5. McGonagall (Maintainer)<br/>Summary + one batch merge"]
    McGonagall --> MainBranch["main (Cloudflare build)"]
    Harry -.-> Luna["Luna (Designer)<br/>UI and renders"]
    Harry -.-> Dumbledore["Dumbledore (Professor)<br/>Architecture & memory"]
```

### Flowchart 3: Zero-Trust Reverse SSH Worker Tunneling

```mermaid
sequenceDiagram
    autonumber
    actor C as cloud-sandbox.mjs (Harness)
    participant S as Sleex Edge API
    participant E as sleex-edge (VPS sshd)
    participant W as Ephemeral Worker (Lambda / Cloud Run / Azure ACA)

    C->>S: POST /api/cloud-workers acquire (auto / lambda / cloudrun / azure)
    S-->>C: Returns lease credentials + ephemeral box key
    C->>W: Spawns worker with target lease ID & edge.sleex.org route
    W->>E: Outbound SSH reverse connect to edge.sleex.org via Cloudflare Access
    E->>S: box-key.mjs validates worker public key against active lease
    S-->>E: Verification successful (restrict, port-forwarding)
    E->>E: Mounts reverse socket at /run/sleex-boxes/<lease-id>.sock
    C->>E: Headless DSH connects to the local Unix socket via ProxyCommand
    Note over C,W: Secure bidirectional SSH session established with ZERO open inbound firewall ports!
```

---

## Competitor Comparison & Market Positioning

| Feature / Metric | **SLEEX** | **OpenAI Codex** | **Claude Coworker** | **Devin (Cognition)** | **Cursor / Windsurf** |
|---|---|---|---|---|---|
| **Form Factor** | **Mobile & Desktop PWA** | Mac App / Mobile App | Desktop / Web Preview | Web Console | Desktop IDE Extension |
| **Pricing Model** | **Open Source / BYOK / Self-Hosted** | $20–$200/mo ChatGPT | $20–$100+/mo Pro/Max | $500+/user/month | $20/mo + token markup |
| **Autonomous Loop** | **7-Agent Team, Owner-Gated** | Single task runner | Proactive brief checks | Autonomous agent | Autocomplete / Chat |
| **Model Freedom** | **8+ Providers (Fallback)** | OpenAI GPT Only | Anthropic Claude Only | Proprietary Black-Box | User Configurable |
| **Infrastructure** | **Edge + Hybrid Sandboxes** | OpenAI Cloud Env | Claude Computer | Hosted Cloud VMs | Developer's Laptop |
| **Token Optimization**| **Headroom In-line (-70%)**| Proprietary / None | None (Raw Context) | Opaque | Context Pruning |
| **Self-Hostable** | **Yes (VPS / Private Cloud)**| No | No | No | No (Backend is SaaS) |

---

## SWOT Matrix

```
+---------------------------------------------------+---------------------------------------------------+
|                   STRENGTHS (S)                   |                  WEAKNESSES (W)                   |
+---------------------------------------------------+---------------------------------------------------+
| • 90%+ lower operating costs vs. competitors      | • Hybrid multi-cloud setup requires initial setup |
| • Zero-install mobile PWA with full task steering | • Single-tenant deployment model today            |
| • 7-agent team covering the full SDLC            | • Serverless free-tier quotas cap concurrent runs |
| • Headroom token compression slashing model bills | • Nascent brand recognition vs. venture incumbents|
| • Model-agnostic auto-fallback resilience         | • Dependent on DeepSeek Harness prerelease cadence|
+---------------------------------------------------+---------------------------------------------------+
|                 OPPORTUNITIES (O)                 |                    THREATS (T)                    |
+---------------------------------------------------+---------------------------------------------------+
| • Exploding $30B+ AI developer tools market       | • Hyperscalers (Google/OpenAI) price commoditization |
| • "AI Junior Developer as a Service" subscription | • Microsoft GitHub Copilot expanding agent loops  |
| • Enterprise private/sovereign appliance licenses | • Cloud provider free-tier terms & quota changes  |
| • Standalone Headroom token compression licensing | • Breaking changes in LLM vendor API protocols   |
+---------------------------------------------------+---------------------------------------------------+
```

---

## Strategic Actionable Roadmap (2027)

```
2027 QUARTERLY EXECUTION MILESTONES
--------------------------------------------------------------------------------------------------------
[Q1 2027: Enterprise Hardening] ──► [Q2 2027: Mobile Canvas 2.0] ──► [Q3 2027: Multi-Cloud Fleet] ──► [Q4 2027: Production Scale]
 • Automated R2 state backups        • Interactive collaborative canvas  • Auto-scaling worker cluster       • Public self-service multi-tenant portal
 • Hard spend & dollar budgets       • Low-latency WebSockets terminal   • Multi-repo monorepo graphs        • Enterprise private appliance
 • Turnkey multi-tenant orchestrator • Voice task briefing enhancements  • Sentry/Datadog auto-triage        • SOC2 Type 2 certification
 • SOC2 Type 1 audit compliance      • Automated visual Playwright QA    • One-click Claude/Codex skills     • Multi-Cloud Production Scale Achieved
```

---

## Live Testing Phase & Quality Assurance Verification

### 1. Multi-Tier Automated Test Harness
Sleex maintains comprehensive automated test coverage across edge runtimes, durable objects, and cloud worker dispatchers:
- **Core Runtime & Auth Suites (`tests/*.test.mjs`):** Validates Google OAuth 2.0 PKCE login flows, guest access controls, `MemberDirectory` RBAC, `the model relay, MCP tools, and cloud lease queues.
- **Azure Worker Job Controller (`tests/azure-worker.test.mjs`):** Validates ARM API `Microsoft.App/jobs` execution dispatch, OAuth2 client credential renewal via Azure Active Directory, polling transitions, and timeout handling.
- **Cloud Box Launcher & Harness (sleex-dsh `tests/cloud-sandbox.test.mjs`, `tests/workspace/harness.test.mjs`):** Lease validation, two-box Session rollover, SSH provider patch, and that every agent's skill loadout installs on a fresh box.
- **Blue/Green Auto-Updater Recovery (`updater.mjs`):** Verifies 3-minute post-deployment health probes with automated rollback to the previous container generation on service failure.

### 2. Security & Zero-Trust Validation
- **Zero Exposed Internal Ports:** Services bind strictly to Unix sockets or the internal Docker bridge. Inbound worker connectivity is mediated through Cloudflare Access reverse SSH authentication (`edge.sleex.org`).
- **Hindsight Memory:** Agents recall past decisions per repository bank, and still verify against the real code before acting.
- **Linux Capability Dropping:** Docker runtimes enforce `cap_drop: [ALL]` with non-root execution (`node:1000:1000`), preventing host filesystem or kernel exploitation.

---

## File Manifest & GitHub Pages Publishing

All presentation and architecture documentation files are published via GitHub Pages:
- **Live Presentation Deck:** [https://](https://)
- **PDF Download:** [https://sleex-presentation.pdf](https://sleex-presentation.pdf)
- **Repository Documentation Source:** `sleex-docs/docs`

1. **`index.html` / `sleex-presentation.html`**: Complete HTML5 & CSS presentation source with interactive slide deck, vector SVG diagrams, and responsive layout. Served directly via GitHub Pages.
2. **`sleex-presentation.pdf`**: Master 18-slide presentation deck (16:9, vector text, high-resolution graphics).
3. **`sleex-presentation.md`**: Technical briefing and slide deck transcription.
4. **`slides/`**: High-resolution PNG slide exports (`slide-01.png` through `slide-18.png`).

### Zero-Compile Architecture Updates
Updates to documentation and slides in `docs/` are automatically served live via GitHub Pages without requiring local Chromium PDF/image compilation runs on each update.

---
*Document prepared for Sleex Technical Architecture & Live Testing.*  
*Production URL: [https://sleex.org](https://sleex.org)*
