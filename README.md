# BFSI Call Center AI Assistant

An enterprise-grade, deterministic Hybrid Retrieval-Augmented Generation (RAG) orchestrator designed specifically for the Banking, Financial Services, and Insurance (BFSI) sector. This system couples a lightweight, locally deployed Small Language Model (SLM) with an asynchronous vector indexing layer to handle domain-specific workflows (Loans, EMIs, Account Management) while strictly enforcing regulatory compliance and mitigating LLM hallucination risk.  

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
The core engine implements a rigid, deterministic 4-Layer Processing Pipeline to guarantee response safety and sub-second inference latency:
User Query

[ Inbound Transaction / User Query ]
                       │
                       ▼
       ┌───────────────────────────────┐
       │ Layer 0: Compliance Guardrail │ ───(Triggered)───> [ Safe Fallback Engine ]
       └───────────────────────────────┘
                       │
                    (Passed)
                       ▼
       ┌───────────────────────────────┐
       │  Layer 1: Deterministic Vector│
       │     Router (Score >= 0.65)    │ ───(Cache Hit)───> [ High-Fidelity Output ]
       └───────────────────────────────┘
                       │
                 (Cache Miss)
                       ▼
       ┌───────────────────────────────┐
       │ Layer 2: Domain Context       │
       │      Conditional Router       │
       └───────────────────────────────┘
                 /           \
     (Financial Keyword)    (General Inbound)
               /               \
              ▼                 ▼
   ┌──────────────────────┐  ┌──────────────────────┐
   │ Layer 3A: Vector RAG │  │ Layer 3B: Zero-Shot  │
   │   (Context Injection)│  │   Generative SLM     │
   └──────────────────────┘  └──────────────────────┘


Pipeline Optimization BreakdownLayer 0: Compliance & Data Loss Prevention (DLP) Guardrails
Intercepts inputs via sub-millisecond regex token matchers to enforce privacy. It blocks PII/financial data leak attempts (OTPs, passwords, CVVs) or out-of-domain scope commands (politics, illegal activities) at the perimeter.
Layer 1: Deterministic Vector Routing
Evaluates user intent embeddings against an enterprise index utilizing an IndexFlatIP vector cache. If cosine similarity achieves $\ge 0.65$, it directly satisfies the query with a verified policy statement, skipping generative model inference entirely to save computing cost.
Layer 2: Domain Context Routing 
A conditional keyword router scans cache misses for high-risk financial markers (emi, loan, interest, penalty). If triggered, the query maps to the contextual RAG layer; otherwise, it passes to standard text generation.
Layer 3A: Knowledge-Base RAG Injection
Queries an asymmetrical secondary vector index containing core banking policies to isolate corresponding mathematical definitions or terms. These are injected into an absolute, bounded system context window to avoid model hallucinations. 
Layer 3B: Autonomous Generative SLM
Serves as a generic fallback layer via google/flan-t5-base to elegantly close unindexed user interactions.  

 ## Production Stack Specification
 
Generative Compute: google/flan-t5-base (Conditional Seq2Seq Model) 
Vector Pipeline: all-MiniLM-L6-v2 (Normalized Dense Representations)
Vector Database: Meta FAISS (IndexFlatIP Engine via Cosine Space Transformation) 
Serving Layer: Gradio UI (Asynchronous Session Handling)  
## System Manifest
├── BFSI_AI.ipynb            <- Production-ready notebook containing data generation & model tests
├── alpaca_dataset.json      <- Structurally validated instruction corpus mapped into Alpaca format
└── faiss_cosine.index       <- Compiled binary index containing pre-computed asset vector weights

## Deployment & Initialization Guide
1. Provision Environment
Isolate dependencies within a Python 3.10+ virtual environment:
pip install sentence-transformers faiss-cpu transformers gradio numpy

## Bootstrapping the Orchestrator
Execute the optimized processing flow to load artifacts and expose the enterprise gateway endpoint:

import numpy as np
import faiss
from sentence_transformers import SentenceTransformer
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM
import gradio as gr

# Core configuration and runtime orchestrator
# Fully documented in the master file "BFSI_AI.ipynb"

def process_transaction_intent(query):
    # Enforces Layer 0 through Layer 3 execution lifecycle
    pass

if __name__ == "__main__":
    print("[INFO] Initializing Enterprise BFSI AI Core Systems...")
    # System boot logic
 
 ## Security Parameterization Matrix
    To remain compliant with financial data frameworks, the following perimeter constraints are enforced continuously at runtime: 
## Security Parameterization Matrix

To remain compliant with financial data frameworks, the following perimeter constraints are enforced continuously at runtime:

| Security Domain | Bound Conditions / Expressions | System Intervention Behavior |
|---------------|-------------------------------|-----------------------------|
| **Data Protection (DLP)** | `otp`, `password`, `pin`, `cvv`, `card number`, `account number` | **Immediate Intercept**: Zeroes out execution loop and serves a sanitized secure fallback message. |
| **Scope Boundary** | `politics`, `election`, `medical`, `disease`, `hack`, `illegal` | **Exception Handling**: Rejects pipeline routing and categorizes transaction as out-of-domain. |

### Security Enforcement Logic
- Sensitive financial credentials trigger **instant execution blocking**.  
- Out-of-domain requests are **automatically rejected** before pipeline execution.  
- Runtime safeguards ensure **compliance with financial data security standards**.  
## Verification Trace Logs
Scenario A: Deterministic Cache Hit
Input: "How do I update my mobile number?"

Pipeline Trace: Layer 1: Deterministic Match (Score: 1.0)

[cite: 1]

Output: "You may update your registered mobile number through the profile section after completing OTP verification. For assistance, contact support."

[cite: 1]

Scenario B: Asymmetrical Context Injection
Input: "Explain EMI formula"

Pipeline Trace: Layer 3A: Vector RAG Active (Context: Internal Assets Loaded)

[cite: 1]

Output: EMI = P × r × (1+r)^n / ((1+r)^n − 1)

[cite: 1]

Scenario C: Firewall Interception
Input: "What is my account number?"

Pipeline Trace: Layer 0: DLP Violation Detected (Token Match: Account Number)

[cite: 1]

Output: ⚠️ Request involves sensitive information and cannot be processed. Please contact official customer support for assistance.

[cite: 1]
 
