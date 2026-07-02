# Feedby

**Feedback that ships** — a drop-in widget that ends in a pull request.

A 30 KB widget on your app. One LLM call to triage. pgvector to cluster. An agent in your infra to ship the fix. No keys leave your repo.

**Private beta** · [feedby.borg.tools](https://feedby.borg.tools) · [Get beta access](https://feedby.borg.tools)

---

## What is it?

Feedby is an AI-first, embeddable feedback system. A user hits a button on your app, writes or screenshots a problem — and within minutes an AI agent has triaged it, linked it to code, and opened a draft PR in your repo. You review and merge.

The source code is **proprietary** (private beta). This repo is a public product overview.

---

## The loop

```
User hits widget → captures text / screenshot / console logs
       ↓
Feedby API → LLM triage (severity, category, duplicate check via pgvector)
       ↓
Admin inbox → clusters, hot-score ranking, real-time updates
       ↓
"Ship Fix" → Agent Runner (E2B Firecracker microVM, your GitHub App key)
       ↓
Pull Request opened in your repo — you review and merge
```

---

## Key features

- **Widget** — Preact + Shadow DOM, zero deps on the host page, CSP-safe, under 30 KB gzipped
- - **Screenshot + annotation** — capture, draw rectangles/arrows, attach console and network logs
  - - **AI triage** — severity, category, deduplication via pgvector HNSW + Voyage 3 embeddings
    - - **Clustered inbox** — hot-score ranking, real-time Supabase broadcast, keyboard nav
      - - **Agent Runner** — isolated E2B Firecracker microVM per dispatch; your GitHub App key never transits Feedby SaaS; local-in-VM RAG on your repo
        - - **MCP server** — Streamable HTTP, OAuth 2.1 PKCE, 6 tools; connects any MCP-compatible AI client
          - - **Multi-tenant from day one** — RLS-enforced on all tables, CI pg_policies gate
            - - **Gamification** — points ledger, badges, leaderboard; increases feedback quality and volume
              - - **Self-hostable Agent Runner** — Docker Compose, Cosign-signed image, auto-update
               
                - ---

                ## Tech stack

                | Layer | Tech |
                |---|---|
                | Frontend | Next.js 16, React, Tailwind |
                | Widget | Preact, Shadow DOM, goober, IIFE + ESM dual output |
                | Backend | Supabase (Postgres + Auth + Storage + Realtime + pgvector) |
                | Queue | Inngest |
                | AI | AI SDK 6, Voyage 3 embeddings, Claude Haiku (via MCP gateway) |
                | Agent sandbox | E2B Firecracker microVMs |
                | Auth | WorkOS SSO, OAuth 2.1 PKCE |
                | Observability | Langfuse, Sentry, Resend (EU) |
                | Infra | Cloudflare CDN, Upstash Redis, Docker Compose self-host |

                ---

                ## Status

                | Milestone | Date | Highlights |
                |---|---|---|
                | v1.0 | 2026-04-16 | MVP — feedback-to-PR loop end-to-end, 9 phases, 8 days |
                | v1.1 | 2026-04-30 | Production hardening, MCP bearer fix, gamification live |
                | v1.2 | 2026-05-13 | Widget CDN deploy, admin shell, token UI, LLM gateway |
                | v1.3 | active | Adaptive UX, design system, cost dashboard, AI triage stabilization |

                ---

                ## Security model

                - Customer hosts the Agent Runner — **your GitHub App key never transits Feedby SaaS**
                - - Agent runs in E2B Firecracker microVM — ephemeral, isolated per dispatch
                  - - Lethal-trifecta prevention: private repo access + arbitrary network egress are **never** combined
                    - - No auto-merge, ever — humans always review the PR
                      - - RLS on all 8 tenant tables, CI gate blocks regressions
                       
                        - ---

                        ## Contact

                        Built by [Wojciech Wiesner](https://theones.io) · [wojciech@theones.io](mailto:wojciech@theones.io)
                        Beta access: [feedby.borg.tools](https://feedby.borg.tools)

                        _The source code for this project is proprietary. This repository is a public product overview only._
