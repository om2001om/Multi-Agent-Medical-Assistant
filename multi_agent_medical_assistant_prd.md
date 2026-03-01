# Multi-Agent Medical Assistant (MAMA) --- Product Requirements Document (PRD)

------------------------------------------------------------------------

## 1. Product Overview

### Product Name

**Multi-Agent Medical Assistant (MAMA)**

### Product Type

AI-powered multi-agent medical decision-support system.

### Vision

Build a **trustworthy, explainable, and grounded medical AI assistant**
integrating: - Large Language Models (LLMs) - Medical Computer Vision -
Agentic Retrieval-Augmented Generation (RAG) - Real-time research
retrieval - Human-in-the-loop verification

------------------------------------------------------------------------

## 2. Problem Statement

Current medical AI systems suffer from: - Hallucinated medical advice -
Lack of explainability - Stateless interactions - Outdated knowledge -
Low clinician trust

The system must provide: - Grounded responses - Explainable reasoning -
Confidence-aware routing - Longitudinal memory - Expert verification

------------------------------------------------------------------------

## 3. Goals

### Primary Goals

1.  Provide medically grounded AI responses.
2.  Reduce hallucinations using Self‑RAG + CRAG.
3.  Enable explainable medical reasoning.
4.  Support multimodal diagnosis (text + imaging).

### Secondary Goals

-   Research-grade experimentation platform
-   Production-ready modular AI system
-   Human oversight integration

------------------------------------------------------------------------

## 4. Target Users

  User                      Needs
  ------------------------- --------------------------------
  Doctors                   Clinical assistance & research
  Medical students          Explanations
  Researchers               Literature synthesis
  Patients (limited mode)   Informational guidance

------------------------------------------------------------------------

## 5. Success Metrics (KPIs)

  Metric                       Target
  ---------------------------- ----------
  Grounding Score              ≥ 0.80
  Hallucination Rate           \< 10%
  Response Latency             \< 6 sec
  Agent Routing Accuracy       ≥ 90%
  Evidence Citation Coverage   ≥ 85%

------------------------------------------------------------------------

## 6. Product Scope

### Included (MVP)

-   Multi-agent orchestration
-   Advanced RAG
-   Medical imaging analysis
-   Agent memory
-   Reasoning trace
-   Evidence grounding score
-   Voice interaction
-   Human validation

### Excluded (v1)

-   Autonomous diagnosis
-   Prescription generation
-   EMR integration

------------------------------------------------------------------------

## 7. System Architecture

    User Input
       ↓
    Memory Loader
       ↓
    Supervisor Agent
       ↓
    (RAG | CV | Web Agents)
       ↓
    Medical Reasoner
       ↓
    Reasoning Trace Generator
       ↓
    Evidence Grounding Evaluator
       ↓
    Confidence Router
       ↓
    Human Verification
       ↓
    Final Response

------------------------------------------------------------------------

## 8. Functional Requirements

### FR‑1 Multi-Agent Orchestration

-   Graph-based execution using LangGraph
-   State-driven routing
-   Agent handoff support

Agents: - Supervisor Agent - RAG Agent - CV Diagnosis Agent - Web Search
Agent - Reasoning Agent - Trace Agent - Grounding Agent

------------------------------------------------------------------------

### FR‑2 Agent Memory

Memory Types: - Short-term (Redis) - Episodic (Qdrant) - Semantic
(Vector DB)

Memory Schema:

``` json
{
  "patient_id": "",
  "summary": "",
  "entities": [],
  "embedding": [],
  "confidence": 0.0,
  "timestamp": ""
}
```

------------------------------------------------------------------------

### FR‑3 Advanced Agentic RAG

Pipeline:

    PDF → Docling → Semantic Chunking → Embedding → Qdrant

Requirements: - Hybrid retrieval (BM25 + Dense) - Query expansion -
Cross-encoder reranking - Citation linking

------------------------------------------------------------------------

### FR‑4 Medical Imaging Analysis

Supported Models: - Brain Tumor Detection (Object Detection) - Chest
X-ray Classification - Skin Lesion Segmentation - Kidney Stone Detection

Output Schema:

``` json
{
 "prediction": "",
 "probability": 0.0,
 "heatmap": "gradcam.png"
}
```

Must include Grad-CAM visualization and uncertainty estimation.

------------------------------------------------------------------------

### FR‑5 Medical Reasoning Trace

Example Output:

``` json
{
 "reasoning_steps": [
   "Imaging indicates abnormal region",
   "Literature supports glioma correlation"
 ],
 "uncertainties": []
}
```

Requirements: - Structured explanation - Evidence references -
Clinician-readable output

------------------------------------------------------------------------

### FR‑6 Evidence Grounding Score

Algorithm: 1. Split answer into claims. 2. Match claims with retrieved
evidence. 3. Compute support score.

Formula:

    Grounding Score = supported_claims / total_claims

Output:

``` json
{
 "grounding_score": 0.87,
 "unsupported_claims": 2
}
```

Routing Rule:

    if grounding_score < 0.7:
        trigger_web_verification()

------------------------------------------------------------------------

### FR‑7 Confidence-Based Routing

    C =
    0.30 model_prob +
    0.25 grounding_score +
    0.20 retrieval_score +
    0.15 memory_consistency +
    0.10 agent_agreement

  Confidence   Action
  ------------ --------------
  High         Respond
  Medium       Verify
  Low          Human Review

------------------------------------------------------------------------

### FR‑8 Human-in-the-Loop Validation

Doctor dashboard must allow: - Approve/reject diagnosis - View reasoning
trace - View evidence - Feedback logging

------------------------------------------------------------------------

### FR‑9 Guardrails

Input Guardrails: - Unsafe queries - Emergency detection

Output Guardrails: - Medical disclaimer - Unsafe recommendation blocking

------------------------------------------------------------------------

### FR‑10 Voice Interaction

    Speech → STT → Agents → TTS

------------------------------------------------------------------------

## 9. Non‑Functional Requirements

  Category        Requirement
  --------------- -------------------------
  Scalability     Dockerized services
  Reliability     Retry + fallback agents
  Observability   OpenTelemetry tracing
  Security        Encrypted storage
  Latency         Async execution
  Logging         Full agent replay

------------------------------------------------------------------------

## 10. Tech Stack

  Component       Technology
  --------------- ----------------
  Backend         FastAPI
  Orchestration   LangGraph
  Vector DB       Qdrant
  Parsing         Docling
  CV Models       PyTorch
  Guardrails      LangChain
  Voice           ElevenLabs
  Frontend        HTML/CSS/JS
  Deployment      Docker + CI/CD

------------------------------------------------------------------------

## 11. Evaluation & Testing

Metrics: - RAGAS faithfulness - Hallucination rate - Grounding score
distribution - Clinician approval rate

------------------------------------------------------------------------

## 12. Risks & Mitigation

  Risk              Mitigation
  ----------------- -----------------
  Hallucination     Grounding score
  Wrong diagnosis   HITL review
  Outdated info     Web agent
  Model bias        Guardrails

------------------------------------------------------------------------

## 13. Milestones

  Phase             Duration
  ----------------- ----------
  Infrastructure    2 weeks
  Agents + RAG      3 weeks
  CV Integration    2 weeks
  Trust Modules     2 weeks
  UI + Deployment   2 weeks

------------------------------------------------------------------------

## 14. Definition of Done

System is complete when: - Agents coordinate automatically - Responses
include reasoning trace - Grounding score visible - Memory influences
responses - Human review workflow operational

------------------------------------------------------------------------
