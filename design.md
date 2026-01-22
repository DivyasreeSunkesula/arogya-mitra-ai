# Arogya Mitra AI Chatbot – System Design

## High-Level Architecture

Arogya Mitra follows a **modular, AI-assisted client–server architecture** designed for scale, accessibility, and last-mile reliability.

At a high level:
- Users interact via a **web app frontend** (text and optional voice).
- Requests are routed through a **secure backend API layer**.
- The backend orchestrates **AI services**, **knowledge retrieval**, and **business logic**.
- Responses are generated using **retrieval-augmented AI** grounded in official Ayushman Bharat information.
- The system is built to operate with **mock/synthetic data** during the hackathon, while remaining production-ready for integration with official systems.

---

## Major Components

### 1. Frontend (User Interface Layer)
**Role:** User interaction, accessibility, and input/output handling.

- Text-based conversational UI (primary)
- Optional voice input/output for accessibility (blind, low-literacy users)
- Language selection (multilingual support)
- User role selection (Beneficiary / ASHA / Hospital staff)
- Displays structured responses (steps, summaries, next actions)

**Key Focus:** Simplicity, low cognitive load, accessibility-first design.

---

### 2. Backend
**Role:** Central control plane for logic, security, and AI coordination.

- API gateway for frontend requests
- Session and conversation state management
- Role-aware request routing
- Validation and sanitization of inputs
- Integration with AI services and data sources
- Mock data handling for eligibility and claim workflows

**Key Focus:** Reliability, scalability, and separation of concerns.

---

### 3. AI Services
**Role:** Understanding intent and generating grounded responses.

- Natural language understanding for user queries
- Retrieval-Augmented Generation (RAG) to avoid hallucinations
- Context-aware response generation based on:
  - User role
  - Query type
  - Conversation history
- Confidence scoring to decide when human escalation is required

**Key Focus:** Accuracy, explainability, and trust.

---

### 4. Knowledge & Data Layer
**Role:** Trusted source of truth.

- Official Ayushman Bharat guidelines (public documents)
- FAQs, scheme rules, eligibility criteria
- Synthetic / mock claim and application data (hackathon phase)

**Key Focus:** Data freshness, consistency, and traceability.

---

## System & User Flows

### Flow 1: General Query (Eligibility / Benefits)
1. User enters query (text or voice).
2. Frontend sends request to backend API.
3. Backend identifies user role and query intent.
4. Relevant documents are retrieved from the knowledge base.
5. AI generates a grounded response using retrieved content.
6. Response is returned to frontend and presented to the user.

---

### Flow 2: Hospital Discovery
1. User asks for nearby empanelled hospitals.
2. Backend processes location input (manual or inferred).
3. Mock hospital dataset is queried.
4. AI formats results into a simple, understandable response.
5. Frontend displays hospital list and next steps.

---

### Flow 3: Claim / Application Status (Mocked)
1. User asks about claim or application status.
2. Backend validates request and uses mock identifiers.
3. Mock status is retrieved.
4. AI explains status, timelines, and common next actions.
5. If uncertainty is detected, escalation guidance is provided.

---

### Flow 4: Human Escalation
1. AI detects low confidence or unresolved intent.
2. Backend flags the interaction.
3. User is guided to:
   - Helpline
   - Hospital desk
   - ASHA support
4. Context summary is provided to avoid repetition.

---

## AWS Integration

AWS is used strategically to ensure **scalability, reliability, and security**.

- **Amazon API Gateway** – Entry point for frontend requests.
- **AWS Lambda** – Serverless backend logic for routing, orchestration, and validation.
- **Amazon Bedrock** – Managed access to foundation models for conversational AI.
- **Amazon OpenSearch / Vector Store** – Hybrid search for RAG-based retrieval.
- **Amazon S3** – Storage for documents, guidelines, and mock datasets.
- **Amazon Polly (Optional)** – Text-to-speech for accessibility.
- **Amazon Transcribe (Optional)** – Speech-to-text for voice input.

**Why AWS:** Government-aligned, scalable, secure, and production-ready.

---

## Technical Logic (How the Pieces Connect)

- User input is first **validated and classified** (intent + role).
- Queries are **never answered directly by the model** without retrieval.
- Relevant documents are fetched using **Hybrid search**.
- AI responses are generated **only from retrieved content**, reducing hallucinations.
- A confidence threshold determines whether:
  - The response is delivered directly, or
  - Escalation guidance is triggered.
- Mock data follows the same schema as real systems, ensuring:
  - Easy future integration
  - Minimal architectural changes

This design ensures the system is:
- **Scalable** (millions of users)
- **Trustworthy** (grounded responses)
- **Inclusive** (language and accessibility support)
- **Realistic** (hackathon-feasible, production-aligned)

---

**Arogya Mitra is designed not just as a chatbot, but as a dependable digital health companion that bridges India’s last-mile healthcare information gap.**
