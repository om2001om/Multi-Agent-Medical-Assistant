# Multi-Agent Medical Assistant 🩺🤖

![Status: Active Development](https://img.shields.io/badge/Status-Active%20Development-blue)
![Python Version](https://img.shields.io/badge/Python-3.10%2B-blue)
![Architecture: Multi-Agent](https://img.shields.io/badge/Architecture-Multi--Agent-success)

The **Multi-Agent Medical Assistant** is an advanced, distributed artificial intelligence system designed to augment clinical workflows. By employing a multi-agent system (MAS) architecture, the application decomposes complex clinical tasks—such as patient triage, differential diagnosis, and evidence-based literature retrieval—into specialized, isolated processes. 

This modular approach significantly mitigates LLM hallucinations, improves deterministic outputs, and provides a scalable co-pilot for healthcare professionals and patients.

---

## 📑 Table of Contents
- [System Architecture](#-system-architecture)
- [Data Flow](#-data-flow--interaction-sequence)
- [Technical Stack](#%EF%B8%8F-technical-stack)
- [Security & Ethics](#%EF%B8%8F-security-ethics-and-compliance)
- [Installation & Setup](#-installation--setup)
- [Contributing](#-contributing)
- [Disclaimer](#-disclaimer)

---

## 🏗 System Architecture

The system utilizes a **Supervisor-Worker** agent topology to efficiently route and process complex medical queries.

### 1. The Orchestrator (Supervisor Agent)
* **Role:** Acts as the primary API gateway and semantic router.
* **Function:** Classifies user intent, maintains global conversation state, and decides which specialist agent should handle the current turn.

### 2. Patient Intake & Triage Agent
* **Role:** Simulates the clinical front desk and initial nursing assessment.
* **Function:** Empathetic and conversational. Uses structured output parsing to extract key clinical variables (e.g., Chief Complaint, Duration, Pain Scale) from natural language dialogue.

### 3. Diagnostic Reasoning Agent
* **Role:** The Clinical Decision Support (CDS) core.
* **Function:** Receives strictly structured JSON data from the Triage Agent. Employs constrained reasoning to generate a ranked list of differential diagnoses based on physiological possibilities.

### 4. Medical Research & RAG Agent
* **Role:** Validates findings against up-to-date medical literature.
* **Function:** Equipped with tool-calling capabilities to search external databases (PubMed, ClinicalTrials) and utilizes Retrieval-Augmented Generation (RAG) to fetch relevant clinical guidelines from vector databases.

---

## 🔄 Data Flow & Interaction Sequence

1. **Input:** A patient states their symptoms: *"I've had a throbbing headache behind my left eye for 3 days and I feel nauseous."*
2. **Routing:** The Orchestrator routes the prompt to the **Triage Agent**.
3. **Extraction:** The Triage Agent standardizes the input into a JSON payload containing `symptom`, `type`, `location`, `duration`, and `associated_symptoms`.
4. **Handoff:** The Orchestrator passes this structured data to the **Diagnostic Agent**.
5. **Reasoning:** The Diagnostic Agent formulates a preliminary differential diagnosis (e.g., 1. Migraine, 2. Cluster Headache).
6. **Verification:** The Orchestrator queries the **Research Agent** to validate the differential against recent medical protocols and retrieve citations.
7. **Output:** The system compiles a final, medically grounded, and structurally sound response for user or physician review.

---

## 🛠️ Technical Stack

* **Agent Framework:** `LangGraph`, `CrewAI`, or `AutoGen`
* **LLM Backend:** OpenAI (`GPT-4o`), Anthropic (`Claude 3.5 Sonnet`), or open-weights medical models (via Ollama).
* **Backend API:** `FastAPI` (Python)
* **Vector Database:** `ChromaDB` (Local) / `Pinecone` (Production)
* **State Management:** `Redis`
* **Data Validation:** `Pydantic`

---

## 🛡️ Security, Ethics, and Compliance

Building AI for healthcare requires rigorous guardrails:

* **Data Privacy (HIPAA Alignment):** Implementation of middleware (e.g., Microsoft Presidio) to mask Personally Identifiable Information (PII) before prompts hit external APIs. 
* **Zero Data Retention:** Conversational data is kept in ephemeral memory and wiped post-session unless explicitly opted-in for local logging.
* **Hallucination Mitigation:** The system uses strict similarity thresholds for vector retrieval. If sufficient literature is not found, the agent triggers a "Refusal to Answer" protocol.
* **Human-in-the-Loop (HITL):** Designed as an *assistant*. Final diagnostic and prescriptive decisions must always be approved by a licensed medical professional.

---

## 🚀 Installation & Setup

### Prerequisites
* Python 3.10+
* Docker & Docker Compose (optional, for Redis/Vector DBs)
* API Keys for your chosen LLM provider (e.g., OpenAI, Anthropic)

### Local Environment Setup

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/om2001om/Multi-Agent-Medical-Assistant.git](https://github.com/om2001om/Multi-Agent-Medical-Assistant.git)
   cd Multi-Agent-Medical-Assistant
