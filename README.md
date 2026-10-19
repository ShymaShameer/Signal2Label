# AI-Powered Pharmacovigilance Signal Detection & Early-Warning Platform

Detecting and prioritizing emerging drug safety signals in FDA FAERS data for antidiabetic drugs (SGLT2 inhibitors, DPP-4 inhibitors and comparators), with a natural-language assistant over the validated results.

## Objectives
1. Identify the most frequently reported drugs and adverse reactions
2. Detect disproportionate drug × reaction reporting (PRR, ROR, IC)
3. Track emerging signals over time
4. Compare seriousness of outcomes across drugs and reactions
5. Cluster drugs by adverse-event reporting profile
6. Prioritize unusual signals with ML (Isolation Forest)
7. Compare reporting by demographics and report source
8. Quantify the effect of data quality and cleaning

## Data
- FDA Adverse Event Reporting System (FAERS) quarterly ASCII files, [2015Q1-2019Q4]
- Not stored in this repo. See *Setup* for download instructions.
- Drugs studied: [list your drugs]

## Pipeline
FAERS files → DuckDB (cleaning, dedup) → Statistics (PRR/ROR/IC) → Time series → ML (anomaly detection, clustering) → Streamlit app → LLM assistant (tool calling + small RAG over FDA labels)

## Repository structure
```
data/raw/      # FAERS files (not tracked)
sql/           # SQL scripts
notebooks/     # analysis notebooks
src/           # reusable Python code
app/           # Streamlit app
rag/           # label/DSC documents and indexing
docs/          # methods, validation, limitations
```

## Setup
```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # add your API key
```
Download the quarterly files from the FDA FAERS page into `data/raw/<quarter>/` (or run `scripts/download_data.py` once added). Python [3.11].

## Usage
```bash
python src/load_data.py          # load files into DuckDB
streamlit run app/app.py         # launch the dashboard and assistant
```

## Key results
*(Fill in at the end)*
- Validation: [x of y known signals detected, median lead time of z quarters]
- Top prioritized signals: [link or table]

## Assistant guardrails
- Numbers come only from computed results (tool calls)
- Label and regulatory statements come only from retrieved FDA documents, with sources
- No causal claims, no personal medical advice

## Limitations
FAERS is a spontaneous reporting system: there is no denominator or exposure data, reports are subject to duplicates, under-reporting and stimulated reporting, and a disproportionality signal is a hypothesis, not proof of causation. Details in `docs/limitations.md`.

## License and disclaimer
For educational purposes. Not for clinical decision-making.