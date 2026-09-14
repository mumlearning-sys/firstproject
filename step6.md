Great. Let's proceed with **Step 5 — Technology Stack, Detailed Technical Decisions, and Architecture Decision Records (ADRs)**.

This is an important step because we now move from **what we are building** to **exactly how we will build it**.

Based on our project requirements—enterprise banking conversation intelligence, high-volume processing, Snowflake, Streamlit, scalable taxonomy classification, traceability, and your GitHub Copilot development system—I recommend the following architecture.

---

# STEP 5 — TECHNOLOGY STACK & TECHNICAL DECISIONS

## 1. Final Technology Direction

```text
                    ENTERPRISE APPLICATION

 ┌──────────────────────────────────────────────────┐
 │                  PRESENTATION                    │
 │                                                  │
 │                  STREAMLIT                       │
 │                                                  │
 │        Dashboard / Analytics / Explorer          │
 └───────────────────────┬──────────────────────────┘
                         │
                         ▼
 ┌──────────────────────────────────────────────────┐
 │                APPLICATION LAYER                 │
 │                                                  │
 │          Python Application Services             │
 │                                                  │
 │ Orchestration / Business Logic / Guardrails      │
 └───────────────────────┬──────────────────────────┘
                         │
           ┌─────────────┼─────────────┐
           │             │             │
           ▼             ▼             ▼
     AI SERVICES     TAXONOMY      DATA SERVICES
           │             │             │
           ▼             ▼             ▼
       LLM API      Embeddings      Snowflake
```

---

# 2. Recommended Core Stack

| Area                  | Recommendation                                              |
| --------------------- | ----------------------------------------------------------- |
| Primary Language      | **Python**                                                  |
| Python Version        | **Python 3.11+**                                            |
| Business UI           | **Streamlit**                                               |
| Database / Analytics  | **Snowflake**                                               |
| Primary LLM           | **Provider abstraction with configurable model**            |
| AI Output             | **Structured JSON / schema validation**                     |
| Data Validation       | **Pydantic**                                                |
| Embeddings            | **Provider abstraction**                                    |
| Vector Search         | **Snowflake-native capability or configurable abstraction** |
| Testing               | **Pytest**                                                  |
| Data Processing       | **Pandas / Snowpark where appropriate**                     |
| Configuration         | **Environment-based configuration + typed settings**        |
| Logging               | **Structured logging**                                      |
| Containerization      | **Docker**                                                  |
| CI/CD                 | **GitHub Actions or enterprise-approved pipeline**          |
| Source Control        | **GitHub**                                                  |
| Development Assistant | **GitHub Copilot development system**                       |

---

# ADR-001 — PRIMARY LANGUAGE

## Decision

Use:

# **Python 3.11**

### Why

Python is the correct foundation for this project because the application requires:

```text
LLM Integration
+
Data Processing
+
Snowflake Integration
+
Streamlit
+
Embedding Models
+
AI Evaluation
+
Testing
```

Python also provides a mature ecosystem around AI and data applications.

### Decision

```text
ADR-001

PRIMARY LANGUAGE = PYTHON 3.11
```

Snowflake's Python connector supports modern Python versions, while Snowflake currently documents its 4.x connector as the production-default line. ([Snowflake Documentation][1])

---

# ADR-002 — USER INTERFACE

## Decision

Use:

# **Streamlit**

for the initial enterprise application.

---

## Why Streamlit

Your application is primarily:

```text
BUSINESS ANALYTICS
       +
DATA EXPLORATION
       +
FILTERING
       +
CONVERSATION ANALYSIS
       +
AI INSIGHTS
```

This makes Streamlit a good fit.

The key advantage is:

```text
Business Application
        +
Python
        +
AI
        +
Snowflake
```

within one development ecosystem.

Streamlit has a client-server architecture and provides built-in concepts for multipage apps, session state, caching, forms, and automated app testing—all relevant to this application. ([Streamlit Docs][2])

---

## Important Architecture Rule

We should **not** build the entire application inside Streamlit scripts.

Bad architecture:

```text
streamlit_app.py

│
├── SQL Queries
├── Business Logic
├── LLM Calls
├── Taxonomy Logic
├── Data Processing
└── UI Code
```

Recommended:

```text
src/
│
├── presentation/
│   └── streamlit/
│
├── application/
│
├── domain/
│
├── infrastructure/
│
└── shared/
```

Then:

```text
STREAMLIT
     ↓
APPLICATION SERVICES
     ↓
DOMAIN LOGIC
     ↓
INFRASTRUCTURE
```

---

# ADR-003 — APPLICATION ARCHITECTURE

## Decision

Use a:

# **Layered Modular Architecture**

with selected clean-architecture principles.

```text
┌─────────────────────────────┐
│ PRESENTATION                │
│ Streamlit                   │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│ APPLICATION                 │
│ Use Cases / Services        │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│ DOMAIN                      │
│ Models / Rules              │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│ INFRASTRUCTURE              │
│ Snowflake / AI / APIs       │
└─────────────────────────────┘
```

---

## Rule

Dependencies should generally move inward.

```text
UI
 ↓
APPLICATION
 ↓
DOMAIN

INFRASTRUCTURE
       ↓
INTERFACES
       ↑
APPLICATION
```

This means:

* Streamlit should not know Snowflake implementation details.
* Domain logic should not directly call an LLM.
* Business logic should not depend on a specific AI provider.
* Taxonomy logic should not depend directly on a vector database implementation.

---

# ADR-004 — AI PROVIDER STRATEGY

## Decision

Use:

# **AI Provider Abstraction**

Do not hard-code the application around one LLM provider.

Architecture:

```text
                 APPLICATION

                     │
                     ▼

             AI SERVICE INTERFACE

                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼

      OpenAI      Azure AI     Future Provider
```

The actual provider implementation can be configured.

---

## Why

Your project will evolve.

Today:

```text
MODEL A
```

Tomorrow:

```text
MODEL B
```

We don't want:

```text
Change Model
      ↓
Rewrite Application
```

Instead:

```text
Change Configuration
      ↓
Change Provider Adapter
      ↓
Application Logic Remains Stable
```

---

# ADR-005 — STRUCTURED AI OUTPUT

## Decision

All important AI outputs should be validated against a schema.

Example:

```text
ConversationInsight
│
├── conversation_summary
│
├── customer_goal
│
├── customer_problem
│
├── sentiment
│
├── resolution_status
│
├── assistant_accuracy
│
├── failure_type
│
├── failure_reason
│
└── evidence
```

---

## Validation Flow

```text
LLM RESPONSE
      │
      ▼
STRUCTURED PARSER
      │
      ▼
PYDANTIC VALIDATION
      │
      ├──── Invalid ────► RETRY / ERROR
      │
      ▼
VALID INSIGHT
```

This prevents application logic from processing uncontrolled free text.

---

# ADR-006 — PROMPT MANAGEMENT

## Decision

Prompts should be:

```text
VERSIONED
TESTABLE
CENTRALIZED
TRACEABLE
```

Recommended structure:

```text
prompts/
│
├── conversation/
│   ├── insight_v1.py
│   ├── insight_v2.py
│
├── taxonomy/
│   ├── classification_v1.py
│
└── evaluation/
    └── evaluation_v1.py
```

Each AI processing result should store:

```text
prompt_version
model_name
model_version
processing_timestamp
```

---

# ADR-007 — TAXONOMY STRATEGY

This is one of the most important decisions.

We previously identified the problem:

```text
500 TAXONOMY ITEMS
        ↓
SEND EVERYTHING TO LLM
        ↓
HIGH TOKEN COST
        +
CONFUSION
        +
SLOW PROCESSING
```

We will **not** do that.

---

## Recommended Flow

```text
CUSTOMER CONVERSATION
          │
          ▼
CUSTOMER PROBLEM SUMMARY
          │
          ▼
EMBEDDING
          │
          ▼
TAXONOMY RETRIEVAL
          │
          ▼
TOP CANDIDATES
          │
          ▼
LLM CLASSIFICATION
          │
          ▼
FINAL TAXONOMY
```

---

## Hierarchical Classification

```text
LEVEL 1
   │
   ▼
RETRIEVE LEVEL 2
   │
   ▼
SELECT LEVEL 2
   │
   ▼
RETRIEVE LEVEL 3
   │
   ▼
SELECT LEVEL 3
```

This significantly reduces the context sent to the model.

---

# ADR-008 — EMBEDDING PROVIDER

## Decision

Use an abstraction.

```text
EmbeddingService
       │
       ├── OpenAIEmbeddingProvider
       │
       ├── AzureEmbeddingProvider
       │
       └── FutureEmbeddingProvider
```

The application should depend on:

```python
EmbeddingProvider
```

not:

```python
SpecificProviderClient
```

---

# ADR-009 — VECTOR STORAGE

## Initial Recommendation

Start with:

# **Snowflake-native data architecture where practical**

because Snowflake is already the primary enterprise data platform for this application.

Architecture:

```text
TAXONOMY
    │
    ▼
EMBEDDING
    │
    ▼
SNOWFLAKE STORAGE
    │
    ▼
VECTOR RETRIEVAL
```

However, we should keep the retrieval layer abstract:

```text
TaxonomyRetrievalService
           │
           ▼
VectorRepository Interface
           │
     ┌─────┴─────┐
     │           │
Snowflake    External Vector DB
```

### Why?

We should not introduce another infrastructure platform unless it gives a clear benefit.

---

# ADR-010 — SNOWFLAKE DATA ACCESS

## Decision

Use:

# **Repository + Data Access Layer**

Architecture:

```text
APPLICATION SERVICE
        │
        ▼
REPOSITORY INTERFACE
        │
        ▼
SNOWFLAKE IMPLEMENTATION
```

Example:

```text
ConversationRepository
       │
       ▼
SnowflakeConversationRepository
```

The Snowflake Python connector supports standard database operations and asynchronous queries, which may be useful for certain longer-running workloads. ([Snowflake Documentation][1])

---

# Important Rule

The UI should never directly execute raw database logic.

Bad:

```text
STREAMLIT
   │
   └── SQL QUERY
```

Good:

```text
STREAMLIT
   │
   ▼
ANALYTICS SERVICE
   │
   ▼
REPOSITORY
   │
   ▼
SNOWFLAKE
```

---

# ADR-011 — SNOWFLAKE CONNECTION MANAGEMENT

## Decision

Use centralized connection management.

```text
SnowflakeConnectionFactory
          │
          ▼
Repository
```

Connection configuration should come from:

```text
Environment Variables
        OR
Enterprise Secret Manager
        OR
Snowflake Connection Configuration
```

Never:

```text
password = "..."
```

inside code.

Snowflake documents environment variables and named connection configuration as supported approaches, along with stronger enterprise authentication options such as SSO and key-pair authentication. ([Snowflake Documentation][3])

---

# ADR-012 — DATA PROCESSING STRATEGY

Your expected scale is important.

You previously mentioned:

```text
150,000+ CONVERSATIONS / DAY
```

and potentially very large datasets.

Therefore:

# Do not load all records into Pandas.

Instead:

```text
SMALL DATASET
      ↓
PANDAS ACCEPTABLE


LARGE DATASET
      ↓
SNOWFLAKE PROCESSING
      +
BATCH PROCESSING
      +
PAGINATION
```

Recommended principle:

```text
MOVE COMPUTATION TO DATA
```

rather than:

```text
MOVE ALL DATA TO APPLICATION
```

---

# ADR-013 — BATCH PROCESSING

## Decision

AI processing should support:

```text
BATCHES
+
PARALLELISM
+
CONTROLLED CONCURRENCY
+
RETRIES
+
CHECKPOINTS
```

Architecture:

```text
PROCESSING RUN
       │
       ▼
BATCH
       │
       ▼
CONVERSATION 1
CONVERSATION 2
CONVERSATION 3
CONVERSATION N
       │
       ▼
RESULT PERSISTENCE
```

---

## Important Rule

Concurrency should be configurable.

```text
MAX_WORKERS = CONFIGURATION
```

Not:

```python
workers = 100
```

hard-coded in the application.

---

# ADR-014 — PROCESSING ORCHESTRATION

## Initial Decision

For MVP, use a controlled Python orchestration layer rather than immediately introducing a complex external workflow platform.

Architecture:

```text
Processing Orchestrator
        │
        ├── Load
        │
        ├── Validate
        │
        ├── Analyze
        │
        ├── Classify
        │
        ├── Persist
        │
        └── Evaluate
```

Each step should produce a traceable result.

---

# ADR-015 — CONFIGURATION MANAGEMENT

## Decision

Use typed configuration.

Recommended:

```text
ApplicationSettings
│
├── Environment
│
├── AISettings
│
├── SnowflakeSettings
│
├── ProcessingSettings
│
├── LoggingSettings
│
└── SecuritySettings
```

Configuration sources:

```text
LOCAL
.env

TEST
Environment Variables

PRODUCTION
Secret Manager / Platform Configuration
```

---

# ADR-016 — SECRET MANAGEMENT

## Decision

Secrets must never exist in:

```text
Git Repository

Source Code

Prompt Files

Logs

Test Output
```

Recommended:

```text
LOCAL DEVELOPMENT
       ↓
Environment Variables

CI/CD
       ↓
GitHub Secrets / Enterprise Secret Store

PRODUCTION
       ↓
Enterprise Secret Management
```

---

# ADR-017 — LOGGING

## Decision

Use structured logging.

Every important operation should contain:

```text
timestamp

environment

correlation_id

processing_run_id

conversation_id

service

operation

status

duration

error
```

Example conceptual flow:

```text
REQUEST
   │
   ▼
CORRELATION ID
   │
   ▼
SERVICE LOGS
   │
   ▼
DATABASE LOGS
   │
   ▼
AI LOGS
```

---

# ADR-018 — AI OBSERVABILITY

AI processing needs additional tracking.

Each execution should capture:

```text
Processing ID

Conversation ID

Model

Prompt Version

Processing Duration

Input Size

Output Size

Retries

Success / Failure

Error Type

Estimated Cost
```

This will later allow us to answer:

```text
Which model performed better?

Which prompt version failed?

Why did processing cost increase?

Which conversation types fail?

How long does processing take?
```

---

# ADR-019 — TESTING STACK

## Decision

Use:

# **Pytest**

with layered tests.

```text
TESTS
│
├── UNIT
│
├── INTEGRATION
│
├── CONTRACT
│
├── AI EVALUATION
│
└── END-TO-END
```

---

## Unit Tests

Test:

```text
Domain Logic

Validation

Classification

Services

Transformation
```

No external dependencies.

---

## Integration Tests

Test:

```text
Snowflake

AI Provider

Embedding Provider

Repository Layer
```

using controlled test environments.

---

## AI Evaluation Tests

Test:

```text
Conversation
      ↓
Expected Insight
      ↓
Actual Insight
      ↓
Comparison
```

---

# ADR-020 — TEST DATA STRATEGY

We should maintain:

```text
tests/
│
├── fixtures/
│
├── conversations/
│
├── taxonomy/
│
└── evaluation/
```

Evaluation data should include:

```text
Normal Conversations

Long Conversations

Incomplete Conversations

Ambiguous Conversations

Failed Conversations

Multiple Problems

Escalation Cases
```

---

# ADR-021 — CI/CD

## Decision

Every Pull Request should trigger:

```text
CODE CHECK
      ↓
FORMAT CHECK
      ↓
LINT
      ↓
UNIT TEST
      ↓
INTEGRATION TEST
      ↓
AI EVALUATION
      ↓
BUILD
```

Conceptually:

```text
DEVELOPER
    │
    ▼
PULL REQUEST
    │
    ▼
AUTOMATED CHECKS
    │
    ├── Code Quality
    │
    ├── Tests
    │
    ├── Architecture Checks
    │
    ├── Security Checks
    │
    └── AI Regression Checks
    │
    ▼
REVIEW
    │
    ▼
MERGE
```

---

# ADR-022 — CONTAINERIZATION

## Decision

Use:

# **Docker**

for reproducible application environments.

Architecture:

```text
SOURCE CODE
     │
     ▼
DOCKER IMAGE
     │
     ▼
DEV
TEST
PRODUCTION
```

This helps ensure:

```text
WORKS ON MY MACHINE
```

does not become:

```text
FAILS IN PRODUCTION
```

Streamlit's deployment documentation specifically supports container-based deployment patterns, including Docker, and notes that deployment environments need their own dependencies and secure secret handling. ([Streamlit Docs][4])

---

# ADR-023 — STREAMLIT STATE MANAGEMENT

## Decision

Use Streamlit session state only for:

```text
UI State

Selected Filters

Navigation Context

Temporary User Interaction
```

Do not use session state as:

```text
DATABASE
PROCESSING ENGINE
BUSINESS LOGIC STORE
```

---

# ADR-024 — DASHBOARD QUERY STRATEGY

This is particularly important because you previously mentioned potentially:

```text
2 MILLION+ RECORDS
```

and filters containing:

```text
500+ VALUES
```

Therefore:

# Never load the full dataset into the UI.

Instead:

```text
USER FILTER
      │
      ▼
FILTER REQUEST
      │
      ▼
QUERY SERVICE
      │
      ▼
SNOWFLAKE
      │
      ▼
AGGREGATED RESULT
      │
      ▼
STREAMLIT
```

For example:

Bad:

```text
SNOWFLAKE
     ↓
2 MILLION ROWS
     ↓
PANDAS
     ↓
FILTER IN STREAMLIT
```

Good:

```text
FILTER
   ↓
SNOWFLAKE QUERY
   ↓
AGGREGATED DATA
   ↓
STREAMLIT
```

---

# ADR-025 — CACHING STRATEGY

Use caching carefully.

Potential layers:

```text
UI CACHE

QUERY CACHE

REFERENCE DATA CACHE

TAXONOMY CACHE
```

Good candidates:

```text
Taxonomy

Reference Tables

Static Metadata

Common Aggregations
```

Avoid blindly caching:

```text
User-Specific Sensitive Data
```

without proper controls.

Streamlit's execution model includes caching mechanisms specifically intended to avoid unnecessary recomputation, but caching needs to be designed around the application's session and data-access behavior. ([Streamlit Docs][5])

---

# ADR-026 — API / FASTAPI DECISION

This is an important one.

## Should we use FastAPI immediately?

### My recommendation:

# **Not for the first implementation layer.**

Start with:

```text
STREAMLIT
     │
     ▼
APPLICATION SERVICES
     │
     ▼
DOMAIN
     │
     ▼
INFRASTRUCTURE
```

Later:

```text
                 ┌──────────────┐
                 │ STREAMLIT    │
                 └──────┬───────┘
                        │
                        ▼
                 APPLICATION CORE
                        ▲
                        │
                 ┌──────┴───────┐
                 │ FASTAPI      │
                 └──────────────┘
```

The important thing is:

# Build the core application independently of Streamlit.

Then exposing it through an API later becomes possible without rewriting business logic.

---

# ADR-027 — DEVELOPMENT REPOSITORY STRUCTURE

I recommend:

```text
banking-conversation-intelligence/
│
├── .github/
│   │
│   ├── workflows/
│   │
│   ├── instructions/
│   │
│   └── prompts/
│
├── docs/
│   │
│   ├── architecture/
│   │
│   ├── adr/
│   │
│   ├── requirements/
│   │
│   └── development/
│
├── src/
│   │
│   ├── presentation/
│   │   └── streamlit/
│   │
│   ├── application/
│   │
│   ├── domain/
│   │
│   ├── infrastructure/
│   │
│   └── shared/
│
├── tests/
│   │
│   ├── unit/
│   ├── integration/
│   ├── evaluation/
│   └── fixtures/
│
├── prompts/
│
├── scripts/
│
├── config/
│
├── pyproject.toml
│
├── Dockerfile
│
├── README.md
│
└── .env.example
```

---

# 3. Detailed Application Module Architecture

## Presentation

```text
presentation/
│
└── streamlit/
    │
    ├── app.py
    │
    ├── pages/
    │   ├── dashboard.py
    │   ├── conversations.py
    │   ├── taxonomy.py
    │   └── evaluation.py
    │
    ├── components/
    │
    └── state/
```

---

## Application Layer

```text
application/
│
├── services/
│   │
│   ├── conversation_service.py
│   ├── insight_service.py
│   ├── taxonomy_service.py
│   ├── analytics_service.py
│   └── processing_service.py
│
├── use_cases/
│
└── dto/
```

---

## Domain Layer

```text
domain/
│
├── models/
│
├── services/
│
├── rules/
│
├── repositories/
│
└── exceptions/
```

---

## Infrastructure

```text
infrastructure/
│
├── snowflake/
│
├── ai/
│
├── embeddings/
│
├── repositories/
│
└── configuration/
```

---

# 4. Final End-to-End Processing Architecture

```text
                    SOURCE DATA

                         │

                         ▼

                CONVERSATION INGESTION

                         │

                         ▼

                    VALIDATION

                         │

                         ▼

                  NORMALIZATION

                         │

                         ▼

               CANONICAL CONVERSATION

                         │

                         ▼

                CONVERSATION ANALYSIS

                         │

                         ▼

               CUSTOMER PROBLEM SUMMARY

                         │

                         ▼

                TAXONOMY RETRIEVAL

                         │

                         ▼

                  TOP CANDIDATES

                         │

                         ▼

               TAXONOMY CLASSIFICATION

                         │

                         ▼

                 STRUCTURED INSIGHT

                         │

                         ▼

                    VALIDATION

                         │

                         ▼

                   PERSISTENCE

                         │

                         ▼

                     SNOWFLAKE

                         │

                         ▼

                ANALYTICS SERVICES

                         │

                         ▼

                    STREAMLIT

                         │

                         ▼

                  BUSINESS USER
```

---

# 5. Recommended Initial LLM Processing Contract

Each conversation processing request should conceptually return:

```text
ConversationInsight
│
├── summary
│
├── customer_goal
│
├── customer_problem
│
├── customer_problem_summary
│
├── sentiment
│
├── sentiment_reason
│
├── resolution_status
│
├── resolution_reason
│
├── assistant_accuracy
│
├── accuracy_reason
│
├── failure_type
│
├── failure_reason
│
├── handoff_required
│
├── taxonomy
│
│   ├── level_1
│   ├── level_2
│   └── level_3
│
├── confidence
│
└── evidence
```

This contract can evolve, but the core principle is:

```text
LLM OUTPUT
     =
STRUCTURED BUSINESS DATA
```

not just:

```text
LLM OUTPUT
     =
LONG TEXT RESPONSE
```

---

# 6. Technology Decisions Summary

## Final Proposed Stack

```text
LANGUAGE
Python 3.11

UI
Streamlit

DATABASE
Snowflake

ARCHITECTURE
Layered Modular Architecture

VALIDATION
Pydantic

AI
Provider Abstraction

EMBEDDINGS
Provider Abstraction

VECTOR SEARCH
Snowflake-first / Abstracted

TESTING
Pytest

AI EVALUATION
Custom Evaluation Framework

LOGGING
Structured Logging

CONFIGURATION
Typed Environment-Based Settings

CONTAINER
Docker

CI/CD
GitHub Actions / Enterprise Pipeline

DEVELOPMENT
GitHub Copilot Agentic Development System
```

---

# 7. What We Have Completed So Far

At the planning and architecture level, we now have:

### ✅ Step 1 — Business Problem & Requirements

### ✅ Step 2 — Enterprise System Architecture

### ✅ Step 3 — Domain Model & Data Architecture

### ✅ Step 4 — Epics, Features, Tasks & Development Roadmap

### ✅ Step 5 — Technology Stack

### ✅ Application Architecture

### ✅ Technology Decisions

### ✅ Architecture Decision Direction

### ✅ Repository Structure

### ✅ AI Provider Strategy

### ✅ Taxonomy Strategy

### ✅ Snowflake Strategy

### ✅ Testing Strategy

### ✅ Deployment Direction

---

# Next Step — Step 6

Now we should move to the **most important operational step**:

# **STEP 6 — GitHub Copilot Development System Execution Model**

This is where we define exactly how your reusable development system will operate when you say something like:

> Implement EPIC 2 Feature 2.1.

The system should automatically understand:

```text
REQUEST
   ↓
ORCHESTRATOR
   ↓
CONTEXT COLLECTION
   ↓
REQUIREMENT ANALYSIS
   ↓
ARCHITECTURE REVIEW
   ↓
TASK DECOMPOSITION
   ↓
IMPLEMENTATION PLAN
   ↓
CODE
   ↓
TESTS
   ↓
REVIEW
   ↓
QUALITY GATES
   ↓
DOCUMENTATION
```

In **Step 6**, we should define the actual operating instructions for each specialist agent, the orchestrator workflow, context files, task handoffs, approval gates, and the exact way you will interact with GitHub Copilot while developing this application.

That will finally connect the **development system we wanted to build first** with the actual application development process.

[1]: https://docs.snowflake.com/en/developer-guide/python-connector/python-connector?utm_source=chatgpt.com "Snowflake Connector for Python | Snowflake Documentation"
[2]: https://docs.streamlit.io/develop/concepts/architecture/architecture?utm_source=chatgpt.com "Understanding Streamlit's client-server architecture - Streamlit Docs"
[3]: https://docs.snowflake.com/en/developer-guide/python-connector/python-connector-connect?utm_source=chatgpt.com "Connecting to Snowflake with the Python Connector | Snowflake Documentation"
[4]: https://docs.streamlit.io/deploy/concepts?utm_source=chatgpt.com "Deployment concepts - Streamlit Docs"
[5]: https://docs.streamlit.io/develop/concepts/architecture?utm_source=chatgpt.com "Working with Streamlit's execution model - Streamlit Docs"
