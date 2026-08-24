# Brazil Market Watch — routine design for Tripla sales & directors

> **Canonical output:** every run of this routine appends to the **"Bits Diários"** tab of the Tripla Market Dossier artifact (`tripla-analysis/tripla-report.html`, published at https://claude.ai/code/artifact/2a2be716-0389-4802-afd6-ba417375cb98). The dossier is now in Portuguese and is the base going forward — see `.claude/skills/br-market-watch/SKILL.md` for the exact HTML block to append per run.

**Goal:** a recurring digest of Brazilian-market news that helps sales and directors spot **market gaps and opportunities tied to what Tripla sells** (the three pillars: Disponibilidade, Proteção, Conformidade, and every named technology/partner in the portfolio). Only trustworthy sources; every item ships with a link, a source/date, and a short preview — never a bare headline.

---

## 1. Source map

### Tier 1 — Brazil-focused cybersecurity media
| Outlet | URL | Focus |
|---|---|---|
| CISO Advisor | cisoadvisor.com.br | Breaches, threat actors, vendor news, BR regulatory angle |
| Minuto da Segurança da Informação | minutodaseguranca.blog.br | Ransomware trends, industry reports, CISO commentary |
| BoletimSec | boletimsec.com | Fast-turnaround breach/attack bulletins |
| Cyber Security Brazil | cybersecbrazil.com.br/blog | Attack tracking, community-sourced incidents |
| Security Report *(verify current URL/status before use)* | securityreport.com.br | Enterprise security market coverage |

### Tier 2 — Enterprise IT / digital-transformation media
| Outlet | URL | Focus |
|---|---|---|
| IT Forum | itforum.com.br | Enterprise IT leadership moves, cybersecurity vertical, market moves |
| TI Inside | tiinside.com.br | IT/telecom, hosts the annual "Cybersecurity Forum" — good for event & trend coverage |
| Convergência Digital | convergenciadigital.com.br | Telecom + enterprise tech, public-sector IT |
| Mobile Time | mobiletime.com.br | Mobile, telecom regulation (Anatel), fintech/identity |
| TELETIME | teletime.com.br | Telecom infrastructure, connectivity, corporate mobility |

### Tier 3 — Legal / regulatory (LGPD, GRC, AI governance)
| Outlet | URL | Focus |
|---|---|---|
| Migalhas — coluna de Proteção de Dados/IA | migalhas.com.br/coluna/migalhas-de-protecao-de-dados | LGPD case law, ANPD decisions, legal analysis |
| JOTA | jota.info | Regulatory/legal news, ANPD, government policy |
| Data Privacy Brasil | dataprivacy.com.br | Privacy research institute, LGPD/AI policy tracking |
| ANPD (official) | gov.br/anpd | Primary source for enforcement actions, guidance, fines |

### Tier 4 — Partner official newsrooms / blogs
Track each manufacturer Tripla resells for two reasons: (a) product/threat-intel content sales can reuse with clients, (b) signals of *where the manufacturer itself is investing in Brazil* (new BR hires, local pricing, competing local partners) — both a threat and a coattail opportunity for Tripla.

Cloudflare Blog · Okta Blog/Sec · Qualys Blog · BeyondTrust Blog · Illumio Blog · TXOne Networks resources · Trend Micro Research · Netwrix Blog · Cohesity Blog · XM Cyber Blog · Check Point Research · Huawei Enterprise Brasil newsroom · Dahua newsroom · OneTrust Blog · PhishX Blog

*(Exact blog paths change over time — confirm the live URL for each vendor at setup and re-validate quarterly rather than hard-coding paths that may 404.)*

### Tier 5 — Vertical press (expand per active sales motion)
Pull in on demand for the verticals Tripla actually sells into: healthcare (Saúde), financial services, energy/utilities, industry/mining, retail, call center/BPO. Examples: Febraban Tech coverage for financial services, ANEEL/ANS for energy/health regulation, Valor Econômico's tecnologia desk for cross-vertical business impact. Don't monitor all of these continuously — activate the relevant vertical when a deal or vertical-specific campaign is live.

---

## 2. What "relevant and trustworthy" means here

**Trustworthy filter (source-side):**
- Tier 1–4 sources count as trustworthy by default.
- Anything outside that list (a random blog, a press-release aggregator, an unlabeled LinkedIn post) needs a named primary source inside the article before it's included — link through to that primary source, not the aggregator.
- Prefer the vendor's own blog for vendor-specific claims (a product launch, a CVE disclosure) and prefer independent media for market/competitive claims.

**Relevance filter (content-side)** — an item qualifies if it hits at least one of:
1. Names a Tripla pillar or technology (SOC/MDR, WAF, ZTNA, EDR, PAM/IAM, vulnerability management, OT security, DLP/insider risk, LGPD/DPO, GRC/TPRM, vCISO, AI governance, VDI, data center/colocation, video surveillance, energy storage) in a Brazilian business context.
2. Names one of Tripla's manufacturers/partners (Cloudflare, Okta, Qualys, BeyondTrust, Trend Micro, Huawei, Check Point, Illumio, TXOne, Cohesity, Netwrix, OneTrust, Dahua, Livoltek, PhishX) doing something in Brazil — a launch, a price move, a partner-program change, a local executive hire.
3. Is a regulatory development (ANPD ruling/fine, new LGPD guidance, AI Act-style rulemaking) that creates compliance urgency for Tripla's target verticals.
4. Is an incident (breach, ransomware, DDoS, data leak) at a company in one of Tripla's target verticals (health, financial, energy, industry, retail, logistics, call center) — these are the clearest "gap" signals: a named prospect just proved they need what Tripla sells.
5. Is a competitor move (a rival integrator/MSSP announcing a new practice, a manufacturer signing a competing partner) — market-positioning intelligence for directors.

**Recency window:** default to items published in the last 7 days for the weekly cadence; last 30 days for the monthly synthesis.

**Dedup:** collapse multiple outlets covering the same underlying event into one item, and cite the most authoritative/original source.

---

## 3. Output format

Each digest item follows this template:

```
### [Headline as published](https://link-to-source)
**Source · Date** — one-line source credibility note if not obviously Tier 1-4
> 2–3 sentence preview of what the article actually says (not just the headline).
**Tag:** Pillar · Tech/Vendor · Vertical
**Why it matters for Tripla:** one sentence connecting it to a specific gap or opportunity —
a prospect, a talking point, a competitive threat, or a new regulatory hook.
```

The digest itself is grouped by pillar (Disponibilidade / Proteção / Conformidade) with a short "Top 3 for this week" summary at the top for directors who won't read the full list.

**Cadence:**
- **Weekly** (Monday): tactical digest — 8–15 items, sales-usable talking points and prospect-relevant incidents.
- **Monthly**: a synthesis pass on top of four weekly digests — named trends, repeated regulatory themes, competitor moves, and an explicit "market gaps we should pursue" section for directors.

---

## 4. How to run it

A ready-to-use skill (`br-market-watch`) has been added to this repo at `.claude/skills/br-market-watch/SKILL.md` — invoke it (e.g. `/br-market-watch`) to produce one digest on demand, following exactly the source map and format above.

For a recurring, unattended cadence, two options:
1. **Preferred — a scheduled job on a persistent Claude Code session/account** (not this ephemeral remote session): use the `loop`/cron scheduling available there to fire the `br-market-watch` skill weekly and deliver the digest by email/Slack/wherever the sales team already looks.
2. **Fallback — session-local `CronCreate`**: works, but jobs here are session-only and auto-expire after 7 days, so it cannot carry the routine indefinitely on its own; use it only to bridge until option 1 is set up.

Whichever mechanism runs it, re-validate the Tier-4 partner blog URLs and the Tier-1/2 outlet list roughly quarterly — media outlets change domains, get acquired, or go quiet, and a stale source list is how a "trustworthy" filter silently degrades.
