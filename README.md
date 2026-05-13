# bfsi-ai-call-center-assistant
# BFSI Call Center AI Assistant

A retrieval-first AI assistant for Banking, Financial Services, and Insurance (BFSI) support workflows. The system prioritizes approved dataset responses before using local language models or RAG-based retrieval.

## Features

* FAISS-based semantic search
* Retrieval-first architecture
* Local LLM fallback using Ollama + Mistral
* RAG pipeline for complex policy queries
* Compliance-aware guardrails
* Explainable responses with similarity score and latency
* Gradio-based UI
* Runtime metrics and audit logging

## Architecture

User Query
→ Guardrails
→ SentenceTransformer Embedding
→ FAISS Similarity Search

### Routing Logic

* **Tier 1:** Dataset response (high similarity)
* **Tier 2:** Local SLM fallback
* **Tier 3:** RAG for complex BFSI queries

## Tech Stack

* Python
* SentenceTransformers
* FAISS
* Ollama (Mistral)
* LangChain
* Gradio
* pytest

## Project Structure

plaintext
bfsai-assistant/
│app.py
│pipeline.py
│retrieval.py
│rag.py
│slm.py
│guardrails.py
│metrics.py
│dataset.json
│knowledge_base/
│requirements.txt
└README.md


## Setup

bash
python -m venv .venv
.venv\Scripts\activate

pip install -r requirements.txt

ollama pull mistral
ollama serve

python app.py

## Sample Queries

### Dataset Tier

* “How can I check personal loan eligibility?”

### RAG Tier

* “Explain EMI penalties and interest charges.”

### Guardrail

* “Can you book a movie ticket?”

## Metrics

The system tracks:

* Response latency
* Tier usage distribution
* Evaluation accuracy
* Audit logs


## Compliance Features

* Prevents hallucinated financial values
* Rejects unsafe or out-of-domain queries
* Protects sensitive information (OTP, PIN, CVV, etc.)


## Future Improvements

* Voice-based support
* Real banking API integration
* Multi-language support
* Cloud deployment

## Run Evaluation
bash
python evaluate.py
pytest
