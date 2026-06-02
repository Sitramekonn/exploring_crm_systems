# Exploring CRM Systems Using Generative AI

## Student Information

| Field | Details |
|-------|---------|
| Course | ICS499 – Software Engineering and Capstone Project |
| Assignment | Assignment 2 + FP1 – AI-Assisted CRM Research |
| Due Date | Saturday, June 6, 2026 |

---

## Executive Summary

This project explores Customer Relationship Management (CRM) systems using Generative AI as a primary research tool. Acting as a software consultant, I investigated CRM definitions, history, and use cases; compared major commercial and open-source CRM products; evaluated an open-source CRM through hands-on exploration; analyzed how a custom CRM could be architected using a PHP/MySQL stack; and reflected on the strengths and limitations of AI-assisted technical research.

Key findings:
- **Salesforce** dominates the commercial CRM market with ~20% global share, but is cost-prohibitive for small businesses.
- **HubSpot** offers the best balance of usability and pricing for SMBs, with a generous free tier.
- **SuiteCRM** is the most mature open-source option, matching Salesforce features without licensing costs.
- **EspoCRM** is the easiest open-source CRM to install and explore for evaluation purposes.
- AI tools (Claude, ChatGPT) were highly effective for summarizing, comparing, and explaining — but required validation for specific pricing and version details.

---

## AI Tools Used

| Tool | Primary Use |
|------|-------------|
| Claude (Anthropic) | CRM research, architecture analysis, writing assistance |
| ChatGPT (OpenAI) | Product comparisons, prompt experimentation |
| Perplexity AI | Fact-checking and cited search results |

---

## CRM Research Findings

See [`crm_research.md`](./crm_research.md) for full details.

**Summary:**
- CRM stands for Customer Relationship Management — software that centralizes customer data, tracks interactions, and automates sales, marketing, and support workflows.
- Modern CRM evolved from physical Rolodexes (1956) → contact software like ACT! (1987) → Siebel Systems enterprise CRM (1993) → Salesforce cloud CRM (1999) → AI-powered CRM (2020s).
- Key modules: Contacts, Accounts, Leads, Opportunities, Activities, Tasks, Marketing Campaigns, Support Tickets, Reports, Dashboards.

---

## CRM Product Comparisons

See [`crm_research.md`](./crm_research.md) — Part 2.

**Commercial CRM Summary:**

| Product | Best For | Starting Price |
|---------|----------|---------------|
| Salesforce | Large enterprises | $25/user/month |
| HubSpot | SMBs & mid-market | Free tier available |
| Zoho CRM | Cost-conscious SMBs | $14/user/month |
| Microsoft Dynamics 365 | Microsoft-ecosystem orgs | $65/user/month |
| Oracle CRM | Large enterprise/ERP | Custom pricing |

**Open-Source CRM Summary:**

| Product | Best For | Self-Hosted Cost |
|---------|----------|-----------------|
| SuiteCRM | Salesforce replacement | Free |
| EspoCRM | Small teams, ease of use | Free |
| Odoo CRM | CRM + ERP combined | Free (1 app) |
| Vtiger CRM | Sales + support combined | Free |

---

## Open Source CRM Evaluation

See [`crm_research.md`](./crm_research.md) — Part 3, and the [`screenshots/`](./screenshots/) folder.

**Selected CRM:** EspoCRM  
**Reason:** Lightweight, modern UI, easiest installation, strong PHP/MySQL stack matching the course architecture.

Screenshots captured:
- Login screen
- Dashboard
- Contacts module
- Leads module
- Reports module

---

## CRM Architecture Proposal

See [`crm_research.md`](./crm_research.md) — Part 4, and [`architecture/crm_architecture.png`](./architecture/crm_architecture.png).

**Stack:** HTML + CSS + Bootstrap + JavaScript + jQuery → PHP → MySQL  
**MVP Modules:** Authentication, Contacts, Leads, Activities, Dashboard, Basic Reports

---

## Prompt Engineering Examples

See [`prompts_used.md`](./prompts_used.md) for the five best prompts and full AI effectiveness analysis.

---

## Lessons Learned

1. **AI accelerates domain learning dramatically** — understanding CRM in hours instead of days.
2. **Pricing data degrades fast** — always verify current pricing directly on vendor sites.
3. **Prompt specificity matters** — role-framing prompts ("Act as a CRM consultant...") produced far better outputs than vague questions.
4. **AI doesn't replace hands-on evaluation** — install and click through the product yourself.
5. **Open-source ≠ free** — hosting, maintenance, and customization all have real costs.

---

## References

See [`references.md`](./references.md)
