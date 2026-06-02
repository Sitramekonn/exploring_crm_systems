# Prompt Engineering Log

**Course:** ICS499 – Software Engineering and Capstone Project  
**Purpose:** Document AI prompts used during CRM research, with analysis of effectiveness

---

## Best Prompts That Produced Useful Results

---

### Prompt 1 – CRM Consultant Comparison

**Tool Used:** Claude (Anthropic)

**Prompt:**
```
Act as a senior CRM consultant with 15 years of experience.

A mid-sized retail company with 75 employees is evaluating CRM systems.
They currently use spreadsheets for sales tracking and Gmail for customer communication.
Their annual software budget is approximately $25,000.

Compare Salesforce, HubSpot CRM, and Zoho CRM for this company.
Include:
- Strengths and weaknesses for this specific use case
- Realistic pricing for 75 users
- Implementation complexity
- Which one you would recommend and why
```

**Why It Worked:**  
Providing specific context (company size, budget, current tools) prevented generic answers. The role-framing ("Act as a senior CRM consultant") made Claude adopt an advisory tone with real trade-offs rather than marketing language.

**Result Quality:** Excellent — produced a structured, actionable comparison with a clear recommendation (Zoho CRM for the budget) and honest caveats about Salesforce's cost at that scale.

---

### Prompt 2 – Historical Timeline

**Tool Used:** Claude (Anthropic)

**Prompt:**
```
Create a chronological timeline of the evolution of CRM software,
starting from pre-digital customer tracking (Rolodex era) through
modern AI-powered CRM systems in 2025.

Format as a markdown table with columns: Era, Year, Key Development.
Include at least 10 milestones.
```

**Why It Worked:**  
Specifying the output format (markdown table) and a minimum number of entries produced a directly usable artifact. The time range anchored the response.

**Result Quality:** Very good — clean table, accurate milestones. One entry (Siebel founding year) was verified via TechTarget and confirmed correct.

---

### Prompt 3 – Open-Source CRM Evaluation

**Tool Used:** ChatGPT (OpenAI)

**Prompt:**
```
I am a software engineering student evaluating open-source CRM systems.
I need to pick one to install and demonstrate for a class project.

Compare SuiteCRM, EspoCRM, Odoo CRM, and Vtiger CRM on these criteria:
1. Ease of local installation (Docker preferred)
2. Technology stack (language, database)
3. Quality of documentation
4. Community activity (GitHub stars, last commit)
5. Which is best for a beginner to self-host?

Give a clear recommendation with reasoning.
```

**Why It Worked:**  
Explicitly listing the evaluation criteria and specifying the audience ("software engineering student," "class project," "beginner") focused the response. Asking for a recommendation forced a commitment rather than a non-committal "it depends" answer.

**Result Quality:** Good — ChatGPT recommended EspoCRM for ease and SuiteCRM for features. Verified Docker availability on both GitHub repos.

---

### Prompt 4 – Industry Use Cases

**Tool Used:** Perplexity AI

**Prompt:**
```
Give me specific, concrete examples of how CRM systems solve real business
problems in each of these five industries:
- Retail
- Healthcare
- Education
- Manufacturing
- Nonprofits

For each industry, describe:
- The specific problem without CRM
- How CRM solves it
- One real-world feature that addresses the problem
```

**Why It Worked:**  
Breaking the request into a repeating structure (problem → solution → feature) produced consistent, parallel responses across all five industries, making them easy to compare and document.

**Result Quality:** Excellent — Perplexity cited sources for each industry, making validation easy. The nonprofit use case was the weakest and required supplemental research.

---

### Prompt 5 – CRM Module Architecture

**Tool Used:** Claude (Anthropic)

**Prompt:**
```
List and describe the major functional modules found in a typical
enterprise CRM system.

For each module, provide:
- Module name
- Primary purpose (1–2 sentences)
- 3 key data fields or features within the module
- Which business team primarily uses it (Sales, Marketing, Support, Management)

Present as a structured markdown table.
```

**Why It Worked:**  
The detailed sub-requirements (name, purpose, fields, team) forced Claude to think through each module thoroughly rather than listing names only. The table format request made the output immediately usable in the report.

**Result Quality:** Excellent — produced a complete, accurate module list. Compared against SuiteCRM's module documentation to verify nothing major was omitted.

---

## AI Effectiveness Analysis

### What AI Did Well

- **Rapid summarization of established knowledge** — CRM history, module descriptions, and industry use cases are well-represented in training data; AI produced accurate, well-organized summaries quickly.
- **Structured output on demand** — requesting specific formats (markdown tables, numbered lists, bullet structures) consistently produced usable, copy-ready output.
- **Role-based advisory responses** — prompts framed as "act as a consultant" or "act as an expert" produced more opinionated, actionable answers than neutral questions.
- **Explaining technical concepts accessibly** — AI excelled at translating jargon (SFA, SLA, lead scoring) into plain language without losing accuracy.
- **Generating comparison frameworks** — asking AI to compare products on specific criteria produced better results than asking for general overviews.

---

### What AI Struggled With

- **Current pricing accuracy** — multiple AI tools gave outdated or approximate pricing. Salesforce's actual per-user costs, HubSpot's tier boundaries, and Zoho's plan names had all changed since some training data was collected. Pricing must always be verified on vendor sites.
- **Specific version details** — EspoCRM's current version number and SuiteCRM's latest release date were incorrect in Claude's first response. GitHub was used to verify.
- **Market share statistics** — AI cited different market share percentages for the same vendors depending on which report it was drawing from. IDC vs. Gartner vs. G2 data varies significantly.
- **Distinguishing open-source editions from commercial versions** — Vtiger and Odoo both have community (free) and commercial (paid) versions with significantly different features. AI sometimes described the paid version's features when discussing the free community edition.

---

### What Information Required Validation

| Topic | Validated Against |
|-------|-----------------|
| CRM pricing (all vendors) | Vendor websites (salesforce.com, hubspot.com, zoho.com) |
| Market share figures | IDC 2025 CRM Market Share Report (via Perplexity) |
| EspoCRM installation steps | Official EspoCRM documentation (docs.espocrm.com) |
| SuiteCRM vs. SugarCRM relationship | SuiteCRM.com "About" page |
| Open-source license types | GitHub repositories for each CRM |

---

### What Surprised Me

1. **AI is genuinely useful for learning unfamiliar domains fast.** Understanding CRM well enough to write a professional comparative analysis took a few hours with AI versus what would likely be days of independent research.
2. **Prompt quality matters enormously.** The difference between a vague question ("Tell me about CRM") and a structured prompt with role, context, and output format was dramatic — easily a 5× improvement in output quality.
3. **AI confidently gave outdated pricing.** This was the most dangerous finding. The numbers sounded authoritative but were wrong. AI should never be trusted for current pricing without verification.
4. **Open-source "free" is more nuanced than AI initially described.** AI's first response made open-source CRMs sound entirely free. The reality — hosting costs, developer time, and the feature gap between community and commercial editions — required follow-up prompting to surface.

---

### What I Would Do Differently Next Time

1. **Always include "What might be wrong about this?" as a follow-up prompt** — this caused AI to self-critique and flag areas of uncertainty.
2. **Ask for sources immediately** — "What sources support this?" often caused Perplexity to surface citable references that could be verified.
3. **Use AI for structure, then fill with verified facts** — let AI generate the comparison table skeleton, then manually fill in pricing and market share from primary sources.
4. **Test installation claims yourself** — AI described EspoCRM installation as "straightforward." In reality, there were minor Docker port conflicts that required troubleshooting.

---

### Would I Trust AI for Software Research?

**Yes — as a starting point and thinking partner, not as a final authority.**

AI tools dramatically accelerated the research process. The CRM domain would have taken significantly longer to understand through traditional web research. AI helped identify the right questions to ask, the right products to compare, and the right dimensions of comparison.

However, AI should not be trusted as the sole source for:
- Current pricing
- Current market share statistics
- Specific version details or release dates
- Legal, licensing, or compliance claims

The correct workflow is: **AI for breadth and structure → primary sources for accuracy → personal evaluation for practical claims**.

This is how professional software consultants already work — AI is a powerful accelerant, not a replacement for verification and hands-on evaluation.
