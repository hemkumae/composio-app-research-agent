# API & MCP App Research Agent

A Python/Jupyter research pipeline that investigates 100 SaaS and developer applications across 10 categories. It collects evidence about API availability, authentication, developer credential access, native or third-party MCP support, and integration feasibility, then produces analysis files and an interactive GitHub Pages dashboard.

**Live dashboard:** https://hemkumae.github.io/composio-app-research-agent/  
**Repository:** https://github.com/hemkumae/composio-app-research-agent

## Project overview

The goal is to help evaluate how readily an application can be connected to an AI agent or automation workflow. The pipeline searches for developer documentation, extracts structured information, stores research records, flags uncertain claims, and supports human verification.

### Categories covered

The 100-app list is organized into 10 categories:

- CRM
- Customer Support
- Communication & Telephony
- Marketing & Advertising
- E-commerce
- Web Data & Sales Intelligence
- Developer Tools & Infrastructure
- Project Management & Productivity
- Payments & Finance
- AI, Research & Meeting Tools

## Current research snapshot

The current local research dataset contains 100 unique application records. The published dashboard visualizes the collected data and the manual-review sample.

| Measure | Current result |
|---|---:|
| Applications researched | 100 |
| Categories | 10 |
| API availability marked Yes | 98 |
| API availability Unknown | 1 |
| API availability marked No | 1 |
| MCP marked Native | 30 |
| MCP marked Third-party | 8 |
| MCP marked Unknown | 61 |
| MCP marked Unsupported | 1 |
| Manually reviewed sample | 20 |
| Manual review outcomes | 13 verified, 6 partially verified, 1 inconclusive |

These are **research-dataset classifications, not a claim that all 100 records are independently verified**. In particular, API availability and MCP classifications should be checked against the cited vendor documentation before being used for production integration decisions.

### Manual verification results

A 20-application sample (two apps per category) was checked against official documentation where available:

- 13 records were marked Verified.
- 6 records were marked Partially verified.
- 1 record was marked Inconclusive because the exact product identity or official documentation could not be confirmed.
- API availability agreed for 18 comparable records in the initial comparison (18/18). This is a sample result, not a 100-app accuracy score.
- MCP disagreements were identified during comparison and targeted corrections were applied to eight records. The initial MCP comparison had only 11 comparable records; its pre-correction agreement was 3/11. This should not be presented as a final post-correction accuracy score.

Unknown or inconclusive values are retained rather than being treated as proof that a capability does not exist.

## Pipeline and architecture

1. **Input:** Read the application names and categories from `apps.csv`.
2. **Discovery:** Use Tavily search to find relevant vendor websites, developer portals, API references, authentication guides, and MCP documentation.
3. **Extraction:** Collect page content and ask a local LLM to produce structured research records.
4. **Storage:** Save research checkpoints and structured results as JSON and CSV.
5. **Quality checks:** Normalize fields, flag missing or suspicious values, and create a human-review queue.
6. **Manual verification:** Compare a sample of records with official documentation and record disagreements.
7. **Analysis:** Summarize API, MCP, authentication, category, credential-access, and buildability fields.
8. **Dashboard:** Generate an interactive HTML report with charts, methodology notes, and a searchable application table.

### Technologies

- Python
- Jupyter Notebook
- Tavily Search and Extract
- Ollama with the local `llama3.2:3b` model for structured extraction
- pandas
- Pydantic
- Requests
- Plotly (loaded by the dashboard through its CDN)

The current workflow uses a local Ollama model for extraction, avoiding dependence on paid OpenAI API credits. Tavily search requires a Tavily API key.

## Run locally

### 1. Prerequisites

- Python 3.10 or newer
- Jupyter Notebook
- Ollama installed and running
- A Tavily API key

### 2. Install Python dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate        # macOS/Linux
# .venv\\Scripts\\activate     # Windows PowerShell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Set up the local model

Install Ollama, start the Ollama service, and download the model:

```bash
ollama pull llama3.2:3b
```

The notebook expects the Ollama chat endpoint at `http://localhost:11434/api/chat` and the model name `llama3.2:3b`.

### 4. Configure Tavily

Provide your Tavily key through the notebook's secure input / environment-variable setup. Do not hard-code a real key into a notebook that will be committed to GitHub.

### 5. Run the notebook

```bash
jupyter notebook
```

Open `app_research_ai_agent.ipynb` and follow the notebook cells in order. The full research and analysis workflow may take time because it performs web searches and page extraction.

## Main files and generated outputs

Repository inputs and code:

- `app_research_ai_agent.ipynb` — research pipeline notebook.
- `apps.csv` — 100-application input list and categories.
- `requirements.txt` — Python dependencies.
- `README.md` — project documentation.
- `index.html` — published interactive dashboard.

Generated locally by the workflow:

- `research_results.json` and `research_results.csv` — structured research records.
- `research_audit_report.csv` and `research_review_queue.csv` — automated quality flags and records requiring attention.
- `manual_verification_sample.csv` — 20-app manual verification sample.
- `manual_verification_comparison.csv` and `manual_verification_disagreements.csv` — field-level comparison outputs.
- `research_results_analyzed.csv` and `analysis_*_summary.csv` — normalized dataset and analysis summaries.

Generated research files are local workflow outputs unless explicitly committed to the repository.

## Methodology and limitations

- Official vendor developer documentation is preferred over blogs, directories, or search snippets.
- Search results are discovery leads; they are not sufficient evidence on their own.
- A native MCP server is distinguished from a third-party adapter or integration.
- An Unknown value means the research did not establish the answer; it does not mean the feature is unavailable.
- Product identity matters. Similar product names or domains must not be treated as the same application without confirmation.
- Authentication and credential-access requirements can vary by plan, tenant, region, app type, permissions, and approval status.
- Credential-access and buildability summaries include heuristic classifications and should be treated as indicative, not independently verified measurements.
- The 20-app manual sample is limited and is not a statistically representative estimate of accuracy across all 100 apps.
- API and MCP capabilities can change; re-check official documentation before implementation.

## Security

Never commit API keys, access tokens, passwords, `.env` files, or private MCP headers. If a secret is accidentally exposed, revoke it and create a replacement.

## License and use

For educational and evaluation purposes. Review each vendor's API terms, rate limits, pricing, and data-handling requirements before building or deploying an integration.
