# API & MCP App Research Agent

A Python/Jupyter prototype for researching SaaS application APIs, authentication, developer credential access, native MCP support, and integration feasibility.

## Current implementation status

This repository is an **in-progress prototype**, not a completed 100-app research report.

| Component | Status |
|---|---|
| Sample app input (10 examples) | Included |
| Tavily web search | Tested successfully by the project author |
| Static webpage text extraction | Tested by the project author |
| LLM structured extraction | Implemented in notebook; execution blocked because the author's OpenAI API account returned a 429 `no credits remaining` error |
| Independent verification pass | Implemented as a notebook stage; not yet run |
| Research across 100 apps | Not completed |
| Manual ground-truth accuracy evaluation | Not completed |
| Final findings/dashboard based on verified data | Not completed |

No research statistics or accuracy percentages are claimed here. The ten app names in the notebook are starter examples only.

## Workflow

1. Load app names and categories.
2. Search the web with Tavily for developer/API/authentication/MCP documentation.
3. Read a small number of result pages.
4. Ask an LLM to extract a structured record with evidence URLs.
5. Save checkpoints to JSON and CSV.
6. Re-search a sample for a second-pass verification.
7. Summarize the collected records and flag uncertain fields for human review.

Search snippets are leads, not proof. Prefer official vendor developer documentation and manually inspect important evidence. Distinguish a vendor's native MCP server from a third-party/Composio integration.

## Run locally

Requires Python 3.10+ (Python 3.11/3.12 recommended).

```bash
python3 -m venv .venv
source .venv/bin/activate        # macOS/Linux
# .venv\Scripts\activate       # Windows PowerShell
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter notebook
```

Open `app_research_ai_agent.ipynb` and run cells from top to bottom.

## API keys and billing

The notebook prompts for:
- `OPENAI_API_KEY` — used for structured extraction and verification.
- `TAVILY_API_KEY` — used for web search.

Create keys through the providers' official dashboards. OpenAI API billing is separate from a ChatGPT subscription. The author encountered an OpenAI API 429 response stating that no credits remained; extraction therefore has not been demonstrated end-to-end in the current run.

Never commit API keys, `.env` files, access tokens, or MCP headers. Do not paste secrets into notebook cells before publishing.

## Outputs

When the relevant notebook stages run successfully, it writes:
- `research_results.json`
- `research_results.csv`
- `verification_results.json`
- `manual_review_queue.csv`

These files are not included as completed findings because the extraction and verification stages have not yet completed.

## Verification and accuracy

The second-pass LLM review is a useful screening step, not ground truth. For a defensible accuracy number:
1. Manually verify a sample against official vendor documentation.
2. Record reference values in a separate ground-truth file.
3. Compare fields individually.
4. Report the denominator, sample size, mismatches, and unresolved/unknown fields.

Do not report a hypothetical accuracy value as measured accuracy.

## Composio / MCP note

The notebook includes an optional Composio MCP session example. Composio is an integration and tool-execution platform; its connectors do not prove that an application offers its own native MCP server. Configure a Composio API key only if you choose to run that optional section.

## Project files

- `app_research_ai_agent.ipynb` — research workflow and code.
- `apps.csv` — editable starter app list.
- `requirements.txt` — Python dependencies.
- `index.html` — project overview page for GitHub Pages.

## License

For evaluation and educational use. Confirm vendor documentation terms and API provider pricing/limits before large-scale runs.
