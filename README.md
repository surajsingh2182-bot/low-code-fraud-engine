# Enterprise Automated Fraud Detection Engine & RAG Dashboard

An end-to-end cloud-native AI ecosystem built to identify transactional risk via structured machine learning and automate compliance auditing using unstructured Large Language Models (LLMs).

## Core Architecture

### Engine 1: Predictive Binary Classification (BigQuery ML & Scikit-Learn)
- **Problem Space:** Supervised Binary Classification (`isFraud = 1` or `0`).
- **Data Strategy:** Handled severe Class Imbalance (0.2% fraud presence) utilizing `class_weight='balanced'` within a Logistic Regression framework to maximize **Recall** over simple Accuracy.
- **Feature Engineering:** Implemented user behavioral analysis ratios (Transaction Amount vs. Historical Average Spending Profile) to mitigate False Positives and preserve operational budgets.
- **Production Gateway:** Simulated real-time inference handling incoming JSON payloads to trigger dynamic risk routing policies:
  - `Probability < 0.20` -> Direct Auto-Approval
  - `0.20 <= Probability < 0.80` -> Step-up low-code MFA verification workflow (Make.com/Twilio)
  - `Probability >= 0.80` -> Immediate Account Freeze

### Engine 2: Conversational Risk Investigator Dashboard (Flowise RAG Pipeline)
- **Orchestration:** Built a Model-Agnostic Retrieval-Augmented Generation (RAG) visual canvas.
- **Data Pipeline:** Ingested raw chat logs and support transcripts, applied a recursive character splitter (Chunk Size: 500 / Overlap: 50), and transformed text snippets into vector representations using open-source HuggingFace sentence transformers.
- **LLM Safety & Guardrails:** Orchestrated a zero-randomness Google Gemini engine (`Temperature: 0.0`) constrained by a strict System Prompt to eliminate hallucination vectors during multi-vendor context handoffs.
- **Cost & Scale Optimization:** Designed a semantic caching layer architecture to resolve `429 Too Many Requests` bottlenecks and prevent runaway token expenditure on duplicate structural queries.
