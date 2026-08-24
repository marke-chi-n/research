---
name: br-market-watch
description: Gera um digest de notícias do mercado brasileiro de TI/cibersegurança/GRC relevantes para os times comercial e de diretoria da Tripla, focado em identificar lacunas e oportunidades de mercado ligadas ao portfólio Tripla (SOC/MDR, Cloudflare, Okta, Qualys, BeyondTrust, Illumio, TXOne, Huawei, LGPD/GRC, etc.). Use quando o usuário pedir "market watch", "digest de notícias", "monitoramento de mercado", "notícias para o time comercial", ou invocar /br-market-watch.
---

# Brazil Market Watch (Tripla)

Produce a news digest for Tripla's sales team and directors. Every item must have a working link, a source + date, and a short preview — never a bare headline or an unsourced claim. Full methodology lives in `tripla-analysis/rotina-market-watch.md` in this repo; this file is the operational checklist.

## Step 1 — Search the source map

Search across these tiers (use web search / fetch tools; confirm URLs are still live before citing):

- **Tier 1 (cybersecurity BR media):** CISO Advisor (cisoadvisor.com.br), Minuto da Segurança da Informação (minutodaseguranca.blog.br), BoletimSec (boletimsec.com), Cyber Security Brazil (cybersecbrazil.com.br/blog), Security Report (securityreport.com.br — verify current status)
- **Tier 2 (enterprise IT / digital transformation):** IT Forum (itforum.com.br), TI Inside (tiinside.com.br), Convergência Digital (convergenciadigital.com.br), Mobile Time (mobiletime.com.br), TELETIME (teletime.com.br)
- **Tier 3 (legal/regulatory — LGPD, GRC, AI governance):** Migalhas coluna de Proteção de Dados/IA (migalhas.com.br), JOTA (jota.info), Data Privacy Brasil (dataprivacy.com.br), ANPD official (gov.br/anpd)
- **Tier 4 (partner newsrooms/blogs — product signals + BR investment signals):** Cloudflare Blog, Okta Blog, Qualys Blog, BeyondTrust Blog, Illumio Blog, TXOne Networks, Trend Micro Research, Netwrix Blog, Cohesity Blog, XM Cyber Blog, Check Point Research, Huawei Enterprise Brasil, Dahua newsroom, OneTrust Blog, PhishX Blog
- **Tier 5 (vertical press, on demand only):** activate for the specific vertical of an active deal — healthcare, financial services, energy, industry/mining, retail, call center/BPO — don't run this tier by default.

Default recency window: last 7 days (weekly digest) or last 30 days (monthly synthesis) — ask the user which if unclear.

## Step 2 — Filter for relevance and trust

**Trust:** Tier 1–4 sources count by default. Anything else needs a named primary source inside the article, and you link through to that primary source.

**Relevance** — include an item if it hits at least one:
1. Names a Tripla pillar/technology (SOC/MDR, WAF, ZTNA, EDR, PAM/IAM, vulnerability management, OT security, DLP/insider risk, LGPD/DPO, GRC/TPRM, vCISO, AI governance, VDI, data center/colocation, video surveillance, energy storage) in a Brazilian business context.
2. Names a Tripla manufacturer/partner (Cloudflare, Okta, Qualys, BeyondTrust, Trend Micro, Huawei, Check Point, Illumio, TXOne, Cohesity, Netwrix, OneTrust, Dahua, Livoltek, PhishX) doing something in Brazil.
3. Is a regulatory development (ANPD ruling/fine, new LGPD guidance, AI-governance rulemaking).
4. Is a security incident (breach, ransomware, DDoS, leak) at a company in a Tripla target vertical — the clearest "gap" signal.
5. Is a competitor move (rival integrator/MSSP, a manufacturer signing a competing BR partner).

Dedup: if multiple outlets cover the same event, keep the most authoritative/original source only.

## Step 3 — Write the digest

Group by pillar (Disponibilidade / Proteção / Conformidade). Open with a 2-3 bullet "Top picks this week" summary for directors who won't read the full list. Each item:

```
### [Headline as published](link)
**Source · Date**
> 2-3 sentence preview of what the article actually says.
**Tag:** Pillar · Tech/Vendor · Vertical
**Why it matters for Tripla:** one sentence tying it to a specific gap, prospect, competitive threat, or regulatory hook.
```

## Step 4 — Deliver: append to the "Bits Diários" tab of the canonical artifact

This routine's canonical output is the **"Bits Diários"** tab inside the Tripla Market Dossier artifact at `tripla-analysis/tripla-report.html` (published artifact: https://claude.ai/code/artifact/2a2be716-0389-4802-afd6-ba417375cb98). Do not create a separate document by default — that artifact is the base and every run adds to it.

1. Read `tripla-analysis/tripla-report.html`.
2. Add a new `<button class="daypill" data-day="YYYY-MM-DD">DD.MM</button>` to `#daypicker`, mark the previous day's button `aria-selected="false"`, and mark the new one `aria-selected="true"`.
3. Add a new `<div class="daylog" data-day="YYYY-MM-DD">…</div>` block (copy the structure of the existing block: a `.top-picks` summary of 2–3 director-level highlights, then `.feed` with one `.feed-item` per news item). Give it the matching JS-toggled `style="display:none"` — the tab-switcher script shows/hides by `data-day`, and only the most recent day should be visible by default.
4. Each `.feed-item` follows this exact structure (all in Portuguese, matching the existing entries):
   ```html
   <div class="feed-item">
     <div class="feed-eyebrow">DD mês · Pilar</div>
     <div class="feed-title">Manchete <span class="read">(leitura de N min)</span></div>
     <div class="feed-source">Fonte · data</div>
     <p class="feed-preview">2-3 frases explicando o que a matéria realmente diz.</p>
     <div class="feed-tags">Tag: <b>Pilar</b> · Tecnologia/Fornecedor · Vertical</div>
     <p class="feed-why"><b>Por que importa para a Tripla:</b> uma frase ligando a notícia a uma lacuna, prospect, ameaça competitiva ou gancho regulatório.</p>
     <a class="feed-link" href="URL real da fonte">Ler na fonte ↗</a>
   </div>
   ```
   Never fabricate a URL — every `feed-link` must be a real, verified link found during Step 1–2. If a strong candidate item has no clean primary-source URL, drop it rather than guess one.
5. Republish: call the Artifact tool with `file_path: tripla-analysis/tripla-report.html` and `url: https://claude.ai/code/artifact/2a2be716-0389-4802-afd6-ba417375cb98` (same URL, so it updates in place rather than creating a new artifact).
6. Commit the updated HTML to the repo with a message like "Bits Diários — DD.MM".

Default to a concise chat summary of what was added (headline count, top picks) rather than pasting the full HTML back to the user — point them at the artifact.

Re-validate the Tier 1/2 outlet list and Tier 4 partner blog URLs roughly quarterly — outlets change domains or go quiet.
