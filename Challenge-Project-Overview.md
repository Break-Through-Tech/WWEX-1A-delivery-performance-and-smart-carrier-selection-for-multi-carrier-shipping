---

> ## Challenge Advisor: Update & Finalize Your Project Overview
>
> > 💡 **These grey text instructions are just for you, the team's Challenge Advisor; please delete them once you have completed the steps below.**
>
> We've pre-populated this Challenge Project Overview page — which is what will be shared with your Break Through Tech student team in August — using the details from your submission form. You should have received an email inviting you to join this repo as a Collaborator, enabling you to add files and make edits.
> 
> In order for your project to be finalized and assigned to a team, please:
> 1. **Review all sections below** and update or expand any content as needed, making sure to address the SME Feedback in the section immediately below. Look for square brackets to find the places below that require additional inputs from you (e.g., "About [Company / Org Name]").
> 2. **Add your dataset** to the [data folder](data) in this repo.
> 3. **Close the Issue assigned to you in this repo** to let us know that you have made your edits and the overview page is ready for final review. You can do this by going to the _Issues_ tab in the top left section of the menu above, add a comment that says "CA review complete", and click the button to Close the Issue. 
>
> If you're unfamiliar with how to edit a page like this in GitHub, check out [this tutorial](https://ubc-lib-geo.github.io/gis-workshop-waml-template/content/handson/edit-readme.html) for a quick overview (start with step 2 and only edit this page), and [this guide](https://ubc-lib-geo.github.io/gis-workshop-waml-template/content/markdown.html) on how to use Markdown to compose text.
>
>
> ❌ Remember that this is a public repo. Do NOT include: Proprietary data, PII, API keys, credentials, or anything confidential.

---

### 🔍 SME Feedback from the Break Through Tech Evaluation Team

## 📋 BTT Internal Evaluation Notes
*(This section is for BTT staff and CAs only — remove before sharing with students)*

### Technical Vetting
| Check | Status | Notes |
| :--- | :--- | :--- |
| Python Compatibility | 🟢 | 100% Python-native stack (`pandas`, `pydantic`, `langchain`, `faiss-cpu` / `chromadb`, `langgraph`). Runs standard open-source workflows. |
| Data Readiness | 🟢 | Standard NPR 7150.2 is structured, numbered, and publicly available. Requirements benchmark set is generated/curated directly in Python. |
| Resource Check | 🟢 | Fully compliant with Google Colab free tier. Lightweight text corpora (< 1 GB) avoid memory crashes; rate limits managed via caching and batching. |

### Internal Scores
- **Student Fit Score:** 9/10 (Difficulty rating 6/10 — ideal "sweet spot" for undergraduate fellows post-ML Foundations).
- **Technical Depth Score:** 9/10 (Spans full end-to-end ML lifecycle: baseline RAG, structured JSON outputs, tool-calling agents, precision/recall benchmarking, and error analysis).
- **Overall Recommendation:** APPROVE AS IS

### Advisor Feedback Draft
This project is exceptionally well-scoped for Break Through Tech fellows. It offers an ideal pedagogical progression starting from a deterministic Non-Agentic RAG baseline up to a tool-calling LLM agent, while anchoring all work on a day-one ground-truth benchmark set. 

**Key Advisor Guardrails:**
1. Ensure students enforce strict Pydantic JSON schemas so every compliance verdict explicitly cites standard clause numbers.
2. Implement request caching and batching to avoid hitting API rate limits during iteration.
3. Keep multi-agent orchestration (LangGraph) strictly as a December stretch goal after the single-agent pipeline is validated.

---

# Compliance Copilot: Auditing Software Requirements Against Engineering Standards

**Company / Org:** Break Through Tech AI Studio (Industry Application: Aerospace, MedTech, FinTech GRC)  
**Challenge Advisor:** BTT AI Studio Technical Lead  
**Program:** Break Through Tech AI Studio - Fall 2026  

---

## 🏢 About NASA & Regulated Engineering Standards
In safety-critical software engineering—such as NASA space missions, medical device development, and financial infrastructure—every single software requirement must undergo a rigorous, clause-by-clause manual audit against strict compliance standards (e.g., NASA NPR 7150.2) before code can be written. 

Currently, this compliance review relies on a small group of senior domain experts, creating a massive operational bottleneck and gatekeeping step. Automating this process transforms weeks of tedious manual review into rapid, structured first-pass reports that any engineer on the team can inspect and verify.

---

## 🎯 The Challenge

### Project Summary
The team will build an AI-powered **Compliance Copilot** that reads software requirements, retrieves relevant clauses from an engineering compliance standard, evaluates whether each requirement satisfies the standard, and generates a structured audit report. The project follows a clear two-stage architectural evolution:
1. **Non-Agentic Baseline:** A deterministic Retrieval-Augmented Generation (RAG) pipeline that queries relevant standard clauses and judges requirement compliance.
2. **Tool-Calling LLM Agent:** A dynamic agent equipped with tools (`retrieve_clause`, `check_requirement`, `log_gap`) that orchestrates the audit workflow and compiles final JSON and Markdown gap reports.

### Success Criteria
- **Day-One Ground-Truth Benchmark:** A human-labeled benchmark of requirement/clause pairs with explicit verdicts (`Meets`, `Partial`, `Gap`).
- **Grounded Verification:** Zero ungrounded or hallucinated verdicts; 100% of generated verdicts must cite the specific standard clause ID.
- **Quantitative Performance:** High Precision and Recall in identifying seeded non-compliance gaps compared against the ground-truth benchmark.

### Project Milestones
| Month | Milestone | Key Activities |
| :--- | :--- | :--- |
| **September** | Data Parsing & Day-One Benchmark Setup | Parse NASA NPR 7150.2 into structured clause chunks. Create a labeled ground-truth benchmark (30–50 requirement/clause pairs) with seeded compliance gaps and draft an accompanying Data Card. |
| **October** | Non-Agentic RAG Baseline & Schema Enforcement | Build an in-memory vector index (FAISS/Chroma). Implement a deterministic RAG retrieval pipeline using Pydantic JSON schemas to force structured citations and verdicts. Evaluate initial precision and recall. |
| **November** | Tool-Calling Agent & Automated Reporting | Wrap the pipeline into an LLM agent with dedicated tools (`retrieve_clause`, `check_requirement`, `log_gap`). Add API rate-limit caching and batching. Generate full JSON and Markdown gap reports. |
| **December** | Evaluation, Error Analysis & Stretch Horizons | Conduct quantitative benchmark evaluation comparing Baseline vs. Agent performance. Perform qualitative error analysis. Executing stretch goals (LangGraph multi-agent refactoring or cross-standard generalization). |

---

## 📊 Dataset

**Name and Source:** NASA Software Engineering Requirements (NPR 7150.2) + Synthetic Software Requirements Specification Dataset  
**Format:** Plain Text, JSON, CSV  
**Size:** < 1 GB (100% Google Colab Free Tier Compliant)  
**Location:** Public NASA NODIS Library & Student-Generated Evaluation Repository  

### Key Details
- **Compliance Standard (Answer Key):** NASA NPR 7150.2 document containing numbered, structured software engineering requirements.
- **Requirements to Audit:** Student-created software requirements set, intentionally containing compliant items and seeded non-compliance gaps (e.g., missing security specifications, vague testing criteria).
- **Data Card:** A documentation artifact explaining dataset curation, gap distribution, and labeling criteria.

---

## 🛠️ Suggested Approach

**ML Problem Type:** Natural Language Processing (NLP), Retrieval-Augmented Generation (RAG), LLM Tool-Calling Agents, Structured Output Enforcement.  
**Recommended Libraries:**
- `python` (Core language)
- `pandas` (Data manipulation and evaluation reporting)
- `pydantic` (JSON schema and output type validation)
- `langchain` / `faiss-cpu` / `chromadb` (Vector store, chunking, and retrieval)
- `langgraph` (Optional December stretch goal for multi-agent state graph)

**Evaluation Metrics:**
- **Precision:** Percentage of flagged compliance gaps that are true gaps.
- **Recall:** Percentage of actual compliance gaps correctly flagged by the system.
- **Citation Groundedness:** Proportion of model verdicts that correctly cite valid clause numbers.

---

## 📚 Resources to Get Started

**Background Reading:**
- NASA Procedural Requirements: *NPR 7150.2 Software Engineering Requirements*.
- Industry overview of AI applications in Governance, Risk, and Compliance (GRC).

**Technical Tutorials:**
- LangChain Documentation: *Retrieval-Augmented Generation (RAG) & Vector Stores*.
- LangChain / OpenAI / Gemini Guides: *Tool Calling and Function Output Structuring with Pydantic*.
- Grounding & Prompt Engineering: *Forcing Citations and Reducing Hallucination in Audit Reports*.

---

## 🤝 How We'll Work Together

**Check-ins:** Weekly technical lab sections and biweekly Challenge Advisor check-ins.  
**Communication:** BTT Project Slack Channel & GitHub Issue Tracker.  
**Response Time:** Within 24–48 hours for non-urgent technical questions. 

**Recommended Environment:**
- **Development Environment:** Google Colab (Free Tier CPU/T4 GPU).
- **Code Repository:** GitHub (Public project repository with modular Python scripts).
- **Deliverables:** Colab Prototype Notebook, Ground-truth Benchmark CSV, Data Card, JSON/Markdown Audit Report, and Reproducibility README.

---

## 🤝 How We'll Work Together (v2)

**Official check-ins:** During our biweekly 45-minute AI Studio Lab Section meeting block (2nd and 4th week of every month)

 **Other ways to reach out to me with questions:** 
* [e.g., Your team's channel within Break Through Tech’s Discord space]
* [e.g., Email; please copy your teammates and AI Studio Coach]
* [e.g., Request a team check-in on Zoom]
* [Note: I will aim to respond within 48 hours. Please reach out to your AI Studio Coach with urgent questions.]

> 💡 **Challenge Advisor: Please update the above based on your availability and preference. If you are not able to answer questions or meet with fellows outside of the biweekly Lab Section check-ins, simply write in "N/A (only available during the official check-in times)"**

**Recommended free coding / collaboration tools**
* […]
* […]

---

## 🚀 Getting Started

1. **Review this overview document** and note any questions for our first meeting
2. **Begin reviewing the dataset** using the link above
3. **Read the GitHub Projects documentation** [here](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)

I’m excited to work with you!

---

## ❓ Questions?

Please bring any questions to our first meeting during the week of August 24th (Break Through Tech’s Bridge to Studio - Session C). 
