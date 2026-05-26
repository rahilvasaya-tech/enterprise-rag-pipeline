# Enterprise RAG Pipeline
### Production-Grade Retrieval-Augmented Generation Architecture

A full-stack enterprise RAG architecture designed for large-scale knowledge access across regulated environments. Built with governance, observability, and production reliability as core requirements.

---

## Architecture Diagram



![Enterprise RAG Architecture](architecture.png)



---

## What This Architecture Solves

Most RAG demos answer questions from documents. This architecture answers questions safely, accurately, and accountably in an enterprise environment where:

- Users have different roles and access levels
- Some questions should not be answered (PII, disallowed intent)
- Every answer must be traceable to a source
- Response quality must be measurable and improvable over time

---

## Architecture Layers

### 1. API Control Layer
Handles all inbound requests before any AI processing begins.

- Authentication: Azure AD, JWT, RBAC enforcement
- Rate Limiting: prevents abuse and controls costs
- Request Validation: rejects malformed or unauthorized requests
- Conversation Controller: manages session IDs, user role enforcement, and conversation state machine

### 2. Query Intelligence Layer
Transforms raw user input into a well-defined, safe retrieval query.

- Query Normalization: cleans and standardizes input, detects vague questions
- Clarification Engine: asks follow-up questions when intent is unclear
- Intent Classification: routes queries by domain (HR / ICT / Finance / Policy)
- Safety and Governance Guardrails: PII detection, disallowed intent detection, do-not-answer enforcement

### 3. Retrieval Pipeline
Finds the most relevant knowledge chunks for the query.

- Query Condenser: compresses multi-turn conversation into a single retrieval query
- Embed Generator: converts query to vector embedding via Azure OpenAI
- Vector Database: semantic similarity search across indexed document chunks
- Keyword Index: exact match retrieval for policy references and specific terms
- Hybrid Retrieval: combines vector and keyword results for higher accuracy

### 4. Citation Layer
Ensures every response is traceable to a source document.

- Citation Extractor: identifies which retrieved chunks contributed to the answer
- Citation Renderer: formats citations into the final response output

### 5. Context Assembly and Generation
Builds the prompt and generates the response.

- Context Assembly: assembles top-k retrieved chunks into structured prompt context
- PII Detection: second pass to ensure no sensitive data enters the LLM prompt
- LLM: Azure OpenAI with strict prompt template, no guesses, no calculations
- Answer Validator: checks citation presence and confidence before returning response
- Policy Boundary Enforcer: ensures response stays within organizational guidelines

### 6. Observability and Governance
Monitors and improves the system over time.

- Audit Logs: full trace of every query, retrieval, and response
- Feedback Loop: user feedback captured and fed into retrieval optimization
- Evaluation Engine: tracks retrieval accuracy and answer correctness at scale

### 7. Admin Controls
Operational management without engineering intervention.

- Admin Ingestion Panel: document upload, chunking, and re-indexing through a UI
- Retrieval Accuracy Controls: tuning semantic ranking and retrieval parameters
- Answer Correctness Monitoring: flagging and reviewing low-confidence responses

---

## Tech Stack

| Layer | Technology |
|---|---|
| Authentication | Azure AD / JWT / RBAC |
| Backend API | FastAPI / Python |
| Embeddings | Azure OpenAI (text-embedding-ada-002) |
| Vector Search | Azure AI Search / Qdrant |
| LLM | Azure OpenAI (GPT-4) |
| Analytics | PostgreSQL |
| Infrastructure | Docker / Azure App Services |

---

## Key Design Decisions

**Why a Query Intelligence layer before retrieval?**
Raw user questions are often vague, multi-part, or unsafe. Processing the query before retrieval improves result quality and prevents policy violations from reaching the LLM.

**Why both vector and keyword retrieval?**
Vector search finds semantically similar content. Keyword search finds exact policy references and specific terms. Neither alone is sufficient for enterprise documents.

**Why an Answer Validator before returning the response?**
In regulated environments, a confident wrong answer is worse than no answer. The validator checks citation presence and confidence score before the response reaches the user.

**Why a Conversation State Machine?**
Multi-turn conversations require tracking what has been asked, what clarifications are pending, and what context carries forward. A state machine makes this explicit and auditable.

**Why an Admin Ingestion Panel?**
Knowledge bases need to stay current. Giving administrators a UI to upload and re-index documents without engineering involvement is critical for production sustainability.

---

## Related Repositories

- [ai-architecture-playbook](https://github.com/rahilvasaya-tech/ai-architecture-playbook)
- [blob-rag-ingestion-pipeline](https://github.com/rahilvasaya-tech/blob-rag-ingestion-pipeline)
- [nl-to-sql-analytics](https://github.com/rahilvasaya-tech/nl-to-sql-analytics)

---

## Author

**Rahil Vasaya**
Enterprise AI Solutions Architect | GenAI & RAG Systems
[linkedin.com/in/rahilvasaya](https://linkedin.com/in/rahilvasaya)
