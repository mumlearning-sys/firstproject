Great. We will now move to **Step 2: Detailed System Architecture**.

I am going to keep this aligned with the most important principle we established:

> **We are not just building a chatbot or a collection of AI agents. We are building a governed enterprise application where AI is one component of a controlled, observable, testable system.**

This architecture also follows the direction we discussed for the development system: **clear orchestration, specialized components, deterministic validation, independent review, and strong traceability**. A layered architecture with a thin client, intelligence/orchestration layer, model/inferencing services, knowledge/retrieval layer, and secured tools is consistent with current enterprise AI architecture guidance. ([Microsoft Learn][1])

---

# Step 2 — Detailed System Architecture

# 1. Architecture Overview

Our platform has **two major business capabilities**.

### Capability A — Conversation Intelligence

```text
Banking Conversation Data
        │
        ▼
Conversation Processing Pipeline
        │
        ▼
AI Understanding
        │
        ▼
Structured Insights
        │
        ▼
Taxonomy Classification
        │
        ▼
Analytics & Dashboard
```

### Capability B — Governed Natural Language Analytics

```text
Business Question
        │
        ▼
Question Understanding
        │
        ▼
Semantic Layer
        │
        ▼
Metadata Retrieval
        │
        ▼
Governed Query Generation
        │
        ▼
SQL Validation
        │
        ▼
Snowflake
        │
        ▼
Business Answer
```

These two capabilities should share selected common services but remain independently maintainable.

---

# 2. Overall Target Architecture

The complete architecture should look conceptually like this:

```text
┌──────────────────────────────────────────────────────────┐
│                    USER EXPERIENCE LAYER                 │
│                                                          │
│   Streamlit Application                                 │
│                                                          │
│   • Dashboard                                           │
│   • Conversation Explorer                               │
│   • Insight Explorer                                    │
│   • Natural Language Analytics                          │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│                   APPLICATION / API LAYER                │
│                                                          │
│   • Request Handling                                     │
│   • Authentication                                       │
│   • Authorization                                        │
│   • Input Validation                                     │
│   • Application APIs                                     │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│                 BUSINESS SERVICES LAYER                  │
│                                                          │
│   Conversation Service                                   │
│   Insight Service                                        │
│   Taxonomy Service                                       │
│   Analytics Service                                      │
│   Question Understanding Service                         │
│   Semantic Service                                       │
│   Query Service                                          │
└───────────────┬───────────────────────────────┬──────────┘
                │                               │
                ▼                               ▼
┌─────────────────────────────┐    ┌─────────────────────────────┐
│ CONVERSATION INTELLIGENCE   │    │ NATURAL LANGUAGE ANALYTICS  │
│                             │    │                             │
│ • Normalization             │    │ • Question Understanding    │
│ • AI Extraction             │    │ • Semantic Retrieval        │
│ • Problem Identification    │    │ • Metric Resolution         │
│ • Resolution                │    │ • Dimension Resolution      │
│ • Sentiment                 │    │ • Query Generation          │
│ • Accuracy                  │    │ • Query Validation          │
│ • Taxonomy                  │    │ • Result Interpretation     │
└──────────────┬──────────────┘    └──────────────┬──────────────┘
               │                                  │
               ▼                                  ▼
┌──────────────────────────────────────────────────────────┐
│                 AI / INTELLIGENCE SERVICES               │
│                                                          │
│  LLM Provider Abstraction                                │
│  Embedding Provider                                      │
│  Structured Output Validation                            │
│  Prompt Management                                       │
│  Model Management                                        │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│                  KNOWLEDGE & SEMANTIC LAYER              │
│                                                          │
│  Taxonomy                                                 │
│  Business Metrics                                         │
│  Dimensions                                               │
│  Entities                                                 │
│  Relationships                                            │
│  Metadata Retrieval                                       │
│  Embeddings                                               │
│  Vector Search                                            │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│                      DATA LAYER                          │
│                                                          │
│  Conversation Store                                      │
│  Insight Store                                           │
│  Taxonomy Store                                          │
│  Evaluation Store                                        │
│  Application Metadata                                    │
│                                                          │
│  Snowflake                                               │
│  Enterprise Data Sources                                 │
└──────────────────────────────────────────────────────────┘
```

---

# 3. Core Architectural Principle

The architecture should be based on this separation:

## User Interface

Responsible for:

* Displaying information
* Collecting user input
* Visualization
* Filtering
* Interaction

The UI should **not contain core AI or business logic**.

---

## Application Services

Responsible for:

* Coordinating business workflows
* Calling domain services
* Handling application requests

---

## Domain / Business Logic

Responsible for:

* Conversation processing
* Taxonomy classification
* Business rules
* Resolution logic
* Semantic interpretation

---

## AI Services

Responsible for:

* LLM calls
* Embeddings
* Prompt execution
* Structured output

AI services should be isolated behind abstractions so the application is not permanently tied to one provider.

---

## Knowledge and Semantic Layer

Responsible for:

* Business definitions
* Taxonomy
* Metrics
* Dimensions
* Metadata
* Retrieval

This is particularly important for the NLP-to-SQL capability. A governed semantic layer reduces the need for an LLM to reason directly over large physical schemas and helps provide consistent business definitions. ([Semantic.io][2])

---

## Data Layer

Responsible for:

* Storage
* Retrieval
* Persistence
* Analytical queries

---

# 4. Conversation Intelligence Architecture

This will be our **first major application capability**.

## Complete Processing Flow

```text
RAW CONVERSATION DATA
        │
        ▼
┌─────────────────────┐
│ INGESTION           │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ DATA VALIDATION     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ NORMALIZATION       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ CONVERSATION        │
│ RECONSTRUCTION      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ AI INSIGHT          │
│ EXTRACTION          │
└──────────┬──────────┘
           │
           ├────────────────────┐
           │                    │
           ▼                    ▼
┌──────────────────┐    ┌──────────────────┐
│ CUSTOMER         │    │ EXPERIENCE       │
│ UNDERSTANDING    │    │ ANALYSIS         │
└──────────────────┘    └──────────────────┘
           │                    │
           ▼                    ▼
Customer Goal           Sentiment
Customer Problem        Resolution
Problem Summary         Assistant Accuracy
                        Failure Analysis
                        Handoff Analysis
           │                    │
           └──────────┬─────────┘
                      │
                      ▼
              ┌───────────────┐
              │ TAXONOMY      │
              │ CLASSIFICATION│
              └───────┬───────┘
                      │
                      ▼
              ┌───────────────┐
              │ VALIDATION    │
              └───────┬───────┘
                      │
                      ▼
              ┌───────────────┐
              │ INSIGHT STORE │
              └───────────────┘
```

---

# 5. Conversation Ingestion Architecture

We should design ingestion using an adapter pattern.

```text
                    ┌──────────────┐
                    │ JSON Adapter │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │ CSV Adapter  │
                    └──────┬───────┘
                           │
SOURCE DATA ───────────────┼──────────────►
                           │
                    ┌──────▼───────┐
                    │ DB Adapter   │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │ API Adapter  │
                    └──────┬───────┘
                           │
                           ▼
                ┌───────────────────┐
                │ STANDARDIZED      │
                │ INGESTION MODEL   │
                └───────────────────┘
```

For the MVP:

### Start with

```text
JSON
```

But design the architecture so we can later add:

* CSV
* Database
* API
* Data lake
* Streaming

without changing the core processing pipeline.

---

# 6. Canonical Conversation Model

Every source should eventually become one standard internal representation.

Conceptually:

```text
Conversation
│
├── conversation_id
├── session_id
├── customer_id
├── channel
├── metadata
│
└── messages[]
       │
       ├── message_id
       ├── timestamp
       ├── speaker
       ├── content
       └── metadata
```

Additional metadata may include:

```text
Bot Intent
Intent Confidence
Navigation Action
Application Context
Product
Handoff Event
```

The important principle is:

> **The downstream AI pipeline should not care whether the original data came from JSON, CSV, API, or database.**

---

# 7. AI Insight Extraction Architecture

I recommend a **structured extraction pipeline**, not a completely free-form agent.

## Why?

Because we need:

* Consistency
* Repeatability
* Evaluation
* Traceability
* Schema validation

The processing flow should be:

```text
NORMALIZED CONVERSATION
        │
        ▼
┌──────────────────────────┐
│ CONVERSATION CONTEXT     │
│ BUILDER                  │
└─────────────┬────────────┘
              │
              ▼
┌──────────────────────────┐
│ AI INSIGHT EXTRACTION    │
└─────────────┬────────────┘
              │
              ▼
┌──────────────────────────┐
│ STRUCTURED OUTPUT        │
│ VALIDATION               │
└─────────────┬────────────┘
              │
              ├──────────────┐
              │              │
              ▼              ▼
        VALID OUTPUT     INVALID OUTPUT
              │              │
              ▼              ▼
        NEXT STEP       RETRY / FAILURE
```

The LLM should return a defined schema.

Conceptually:

```json
{
  "conversation_summary": "",
  "customer_goal": "",
  "customer_problem": "",
  "customer_problem_summary": "",
  "sentiment": "",
  "sentiment_confidence": 0.0,
  "resolution_status": "",
  "resolution_confidence": 0.0,
  "assistant_accuracy": "",
  "assistant_accuracy_confidence": 0.0,
  "failure_reason": "",
  "handoff_required": false
}
```

The actual implementation should use strongly typed application models.

---

# 8. Recommended AI Processing Pattern

I do **not recommend immediately creating 10 independent LLM agents**.

Instead:

### Initial MVP

Use a controlled pipeline.

```text
Conversation
     │
     ▼
AI Insight Extraction
     │
     ▼
Structured Output
     │
     ▼
Taxonomy Retrieval
     │
     ▼
Taxonomy Classification
     │
     ▼
Validation
```

This is easier to:

* Test
* Debug
* Evaluate
* Scale
* Improve

Later, if evaluation demonstrates a need, we can split specific tasks.

For example:

```text
Conversation Understanding
       │
       ├── Problem Extraction
       ├── Resolution Assessment
       ├── Assistant Evaluation
       └── Sentiment Analysis
```

But we should not introduce agent complexity before it is needed.

---

# 9. Taxonomy Architecture

This is one of the most important architectural components.

## Taxonomy Structure

```text
Taxonomy
│
├── Level 1
│
├── Level 2
│      │
│      └── Parent Level 1
│
└── Level 3
       │
       └── Parent Level 2
```

Example:

```text
Cards
    │
    └── Card Management
            │
            └── Cancel Card
```

---

# 10. Taxonomy Retrieval Flow

The system should **not send the full taxonomy to the LLM**.

Instead:

```text
CUSTOMER PROBLEM SUMMARY
        │
        ▼
EMBEDDING GENERATION
        │
        ▼
VECTOR / SEMANTIC SEARCH
        │
        ▼
TOP LEVEL 1 CANDIDATES
        │
        ▼
SELECT LEVEL 1
        │
        ▼
TOP RELEVANT LEVEL 2
        │
        ▼
SELECT LEVEL 2
        │
        ▼
TOP RELEVANT LEVEL 3
        │
        ▼
SELECT LEVEL 3
        │
        ▼
CONFIDENCE VALIDATION
```

---

# 11. Taxonomy Classification Decision

The classifier should return something like:

```text
Selected Category:
Cancel Card

Level:
3

Confidence:
0.91

Alternative Candidate:
Replace Card

Alternative Confidence:
0.42

Decision:
Accepted
```

If confidence is low:

```text
Decision:
Needs Review
```

If no suitable category exists:

```text
Decision:
Unknown / Unclassified
```

This prevents the model from being forced into incorrect categories.

---

# 12. Taxonomy Storage

The taxonomy needs its own controlled repository.

Conceptually:

```text
Taxonomy_Category
│
├── category_id
├── category_name
├── level
├── parent_category_id
├── description
├── synonyms
├── examples
├── status
├── version
├── created_at
└── updated_at
```

Embeddings should be generated for:

* Category name
* Description
* Synonyms
* Examples

---

# 13. Vector / Retrieval Architecture

We should introduce a provider abstraction.

```text
APPLICATION
      │
      ▼
┌───────────────────────────┐
│ Retrieval Service         │
└─────────────┬─────────────┘
              │
      ┌───────▼────────┐
      │ Vector Provider│
      │ Abstraction    │
      └───────┬────────┘
              │
       ┌──────┴─────────────┐
       │                    │
       ▼                    ▼
Vector Store A       Vector Store B
```

This gives us flexibility.

The actual vector technology should be selected during the technology decision stage based on:

* Existing enterprise infrastructure
* Scale
* Cost
* Security
* Integration requirements

We should **not prematurely select a vector database**.

---

# 14. Insight Storage Architecture

AI-generated insights should be stored separately from raw messages.

Conceptually:

```text
Conversation
      │
      │ 1
      │
      ▼
Conversation_Message
      │
      │
      ▼
Conversation_Insight
      │
      ├── AI Outputs
      ├── Taxonomy
      ├── Confidence
      ├── Model Version
      ├── Prompt Version
      └── Processing Run
```

This is important because:

> One conversation may be processed multiple times using different models or prompts.

We must preserve history.

Example:

```text
Conversation ABC
│
├── Processing Run 1
│      ├── Model A
│      ├── Prompt v1
│      └── Insight Result A
│
└── Processing Run 2
       ├── Model B
       ├── Prompt v2
       └── Insight Result B
```

---

# 15. Model and Prompt Management

Every AI output should be reproducible.

Track:

```text
Model Name
Model Version
Prompt Name
Prompt Version
Taxonomy Version
Embedding Model
Processing Version
```

The application should eventually allow us to answer:

> Why did this classification change?

For example:

```text
Old Result
Model: Gemini X
Prompt: v2
Taxonomy: v3

New Result
Model: OpenAI X
Prompt: v4
Taxonomy: v4
```

---

# 16. Evaluation Architecture

Evaluation must be a permanent component.

```text
┌─────────────────────┐
│ Evaluation Dataset  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Application Output  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Comparison Engine   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Evaluation Metrics  │
└─────────────────────┘
```

Evaluate separately:

### Conversation Understanding

* Summary
* Customer goal
* Customer problem

### Experience Analysis

* Sentiment
* Resolution
* Assistant accuracy

### Taxonomy

* Level 1
* Level 2
* Level 3

---

# 17. Dashboard Architecture

For the MVP:

```text
STREAMLIT
    │
    ▼
APPLICATION SERVICE
    │
    ▼
ANALYTICS SERVICE
    │
    ▼
INSIGHT REPOSITORY
    │
    ▼
DATABASE
```

The Streamlit UI should not directly contain database queries everywhere.

Instead:

```text
UI
 ↓
Service
 ↓
Repository
 ↓
Database
```

This will allow us to later replace Streamlit without rewriting the business logic.

---

# 18. Natural Language Analytics Architecture

This will be the second major capability.

The architecture should be:

```text
USER QUESTION
        │
        ▼
┌────────────────────────┐
│ AUTHORIZATION CHECK    │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ QUESTION UNDERSTANDING │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ STRUCTURED QUESTION    │
│ REPRESENTATION         │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ SEMANTIC RETRIEVAL     │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ APPROVED METADATA      │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ QUERY GENERATION       │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ DETERMINISTIC          │
│ VALIDATION             │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ QUERY EXECUTION        │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ RESULT VALIDATION      │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│ BUSINESS RESPONSE      │
└────────────────────────┘
```

This semantic-first approach is particularly suitable for a governed enterprise environment because business meaning, approved relationships, and access policies can be controlled before query execution. ([Semantic.io][2])

---

# 19. Question Understanding Architecture

The first step is **not SQL generation**.

Example question:

> Which customer problems increased the most last month?

The application should convert it into something like:

```text
Intent:
Trend Analysis

Metric:
Conversation Volume

Dimension:
Customer Problem

Time Period:
Last Month

Comparison:
Previous Month

Ranking:
Top

Sort:
Descending

Limit:
5
```

This structured representation should be validated before moving forward.

---

# 20. Semantic Layer Architecture

The semantic layer becomes the bridge between:

```text
BUSINESS LANGUAGE
        │
        ▼
SEMANTIC DEFINITIONS
        │
        ▼
PHYSICAL DATA
```

The semantic layer should contain:

### Metrics

```text
Metric Name
Definition
Calculation
Source
Version
```

### Dimensions

```text
Dimension Name
Definition
Source Table
Source Column
Synonyms
```

### Entities

```text
Customer
Conversation
Product
Account
Card
```

### Relationships

```text
Conversation
    │
    ├── Customer
    │
    ├── Intent
    │
    └── Product
```

The semantic layer should be version-controlled and governed as a core application asset, not treated as an informal prompt attachment. ([Strategy][3])

---

# 21. Metadata Retrieval

We should use:

```text
Natural Language Question
        │
        ▼
Embedding
        │
        ▼
Hybrid Retrieval
        │
        ├── Vector Search
        │
        ├── Keyword Search
        │
        └── Metadata Filtering
        │
        ▼
Candidate Metadata
```

Example:

Question:

> What is the resolution rate for card cancellation?

Retrieve:

```text
Metric:
Resolution Rate

Dimension:
Customer Problem

Value:
Card Cancellation

Relevant Tables:
Conversation Insights

Approved Join:
Conversation → Insight
```

The LLM should only receive the relevant, approved context.

---

# 22. Governed Query Generation

The LLM should receive:

```text
Structured Question
+
Approved Metric
+
Approved Dimensions
+
Approved Tables
+
Approved Columns
+
Approved Relationships
+
Approved Filters
+
Database Dialect
```

It should not receive unrestricted database access.

---

# 23. SQL Validation Architecture

Generated SQL should pass multiple gates.

```text
GENERATED SQL
       │
       ▼
┌─────────────────────┐
│ SQL PARSER          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ STATEMENT VALIDATOR │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ OBJECT VALIDATOR    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ JOIN VALIDATOR      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ SECURITY VALIDATOR  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ COST VALIDATOR      │
└──────────┬──────────┘
           │
           ▼
      EXECUTION
```

Important:

> The LLM cannot bypass validation.

---

# 24. Snowflake Integration Architecture

Conceptually:

```text
APPLICATION
      │
      ▼
QUERY SERVICE
      │
      ▼
AUTHORIZATION
      │
      ▼
QUERY VALIDATION
      │
      ▼
SNOWFLAKE CONNECTOR
      │
      ▼
SNOWFLAKE
```

The connector should handle:

* Secure authentication
* Roles
* Query tagging
* Timeout
* Result handling

The LLM should never see:

* Credentials
* Secrets
* Connection strings

---

# 25. Result Interpretation Architecture

After execution:

```text
DATABASE RESULT
       │
       ▼
RESULT VALIDATION
       │
       ▼
BUSINESS INTERPRETATION
       │
       ▼
FINAL RESPONSE
```

The final answer should separate:

### Direct facts

```text
12,500 conversations
```

### Calculated facts

```text
18% increase
```

### AI interpretation

```text
The increase appears concentrated in...
```

AI interpretations should be grounded in available results.

---

# 26. Security Architecture

Security should operate across all layers.

```text
┌──────────────────────────────────────────┐
│ USER AUTHENTICATION                      │
├──────────────────────────────────────────┤
│ APPLICATION AUTHORIZATION                │
├──────────────────────────────────────────┤
│ DATA ACCESS CONTROL                      │
├──────────────────────────────────────────┤
│ AI INPUT PROTECTION                      │
├──────────────────────────────────────────┤
│ SQL VALIDATION                           │
├──────────────────────────────────────────┤
│ DATA MASKING                             │
├──────────────────────────────────────────┤
│ AUDIT LOGGING                            │
└──────────────────────────────────────────┘
```

Key rule:

> **Security controls must be deterministic and enforced outside the LLM.**

Enterprise AI guidance similarly emphasizes security, observability, and governance as cross-layer concerns rather than something delegated to individual models or agents. ([AWS Documentation][4])

---

# 27. Observability Architecture

We need observability across:

```text
USER REQUEST
      │
      ▼
APPLICATION
      │
      ▼
AI PROCESSING
      │
      ▼
DATABASE
```

Capture:

### Application

* Request ID
* Processing duration
* Success
* Failure

### AI

* Model
* Prompt version
* Tokens
* Latency
* Cost
* Retry

### Retrieval

* Candidates retrieved
* Similarity scores
* Final selection

### SQL

* Generated SQL
* Validation status
* Execution time
* Query cost where available

All of this should be connected through a correlation ID.

Example:

```text
Request ID: XYZ123
```

Every component logs against that ID.

---

# 28. Recommended Initial Repository Architecture

The application repository should conceptually look like:

```text
banking-conversation-platform/
│
├── app/
│   │
│   ├── api/
│   │
│   ├── ui/
│   │
│   ├── domain/
│   │
│   ├── services/
│   │   │
│   │   ├── conversation/
│   │   ├── insights/
│   │   ├── taxonomy/
│   │   ├── analytics/
│   │   ├── semantic/
│   │   ├── question_understanding/
│   │   ├── query_generation/
│   │   └── result_interpretation/
│   │
│   ├── repositories/
│   │
│   ├── integrations/
│   │   │
│   │   ├── llm/
│   │   ├── embeddings/
│   │   ├── vector_store/
│   │   └── snowflake/
│   │
│   ├── evaluation/
│   │
│   ├── security/
│   │
│   ├── observability/
│   │
│   └── configuration/
│
├── tests/
│
├── data/
│
├── scripts/
│
├── docs/
│
├── requirements/
│
└── development-system/
```

The final structure should be refined before implementation, but the key idea is to separate:

* UI
* Domain logic
* Services
* Integrations
* Data access
* AI
* Evaluation
* Security

---

# 29. Architecture Decision Records

We should formally create ADRs for major decisions.

## ADR-001

### Decision

Start with a **modular monolith**.

### Reason

We do not currently need microservices.

Benefits:

* Faster development
* Easier debugging
* Simpler deployment
* Lower operational complexity

Future services can be extracted if scale requires it.

---

## ADR-002

### Decision

Start Conversation Intelligence before NLP-to-SQL.

### Reason

Conversation Intelligence is the immediate core business problem and creates structured data that will later be useful for analytics.

---

## ADR-003

### Decision

Use provider abstractions for AI.

### Reason

Avoid permanent dependence on:

* One LLM provider
* One embedding provider

---

## ADR-004

### Decision

Use retrieval-based taxonomy classification.

### Reason

Avoid putting hundreds or thousands of taxonomy categories in prompts.

---

## ADR-005

### Decision

Use structured LLM outputs.

### Reason

Improve:

* Validation
* Reliability
* Testing
* Downstream processing

---

## ADR-006

### Decision

Use deterministic validation for NLP-to-SQL.

### Reason

Generated SQL cannot be trusted automatically.

---

## ADR-007

### Decision

Use a governed semantic layer.

### Reason

The application should operate using business definitions rather than exposing the entire raw physical schema to the LLM. This approach is especially important for consistent, governed enterprise analytics. ([Semantic.io][2])

---

# 30. Recommended Technology Direction

At this stage, I recommend the following **initial direction**, subject to final architecture decisions.

| Area                     | Recommended Direction          |
| ------------------------ | ------------------------------ |
| Language                 | Python                         |
| Initial UI               | Streamlit                      |
| Future API               | FastAPI                        |
| Application Architecture | Modular monolith               |
| AI                       | Provider abstraction           |
| Embeddings               | Provider abstraction           |
| Taxonomy Retrieval       | Vector / hybrid retrieval      |
| Analytics Database       | Snowflake                      |
| Structured Models        | Pydantic or equivalent         |
| Testing                  | Pytest                         |
| SQL Validation           | SQL parser + policy validation |
| Configuration            | Environment-based              |
| Logging                  | Structured logging             |

We should avoid finalizing the exact LLM, vector database, or orchestration framework until we evaluate your existing enterprise environment and constraints.

---

# 31. Architecture by Development Phase

## Phase 1

### Foundation

```text
Configuration
Logging
Error Handling
Domain Models
Testing
AI Abstractions
```

---

## Phase 2

### Conversation Data

```text
JSON
   ↓
Validation
   ↓
Normalization
   ↓
Canonical Model
```

---

## Phase 3

### Conversation Intelligence

```text
Conversation
    ↓
AI Insight Extraction
    ↓
Structured Output
```

---

## Phase 4

### Taxonomy

```text
Problem Summary
    ↓
Embedding
    ↓
Candidate Retrieval
    ↓
Classification
```

---

## Phase 5

### Insight Storage

```text
Conversation Insights
      ↓
Persistence
      ↓
Analytics
```

---

## Phase 6

### Dashboard

```text
KPIs
Trends
Problems
Conversation Explorer
```

---

## Phase 7

### Semantic Layer

```text
Metrics
Dimensions
Entities
Relationships
```

---

## Phase 8

### NLP-to-SQL

```text
Question
   ↓
Understanding
   ↓
Semantic Retrieval
   ↓
Governed Query
```

---

# 32. Important Architecture Recommendation

I want to emphasize one thing.

Your original idea involved creating a sophisticated **development system with multiple specialized agents**.

That is separate from the runtime architecture of the application.

### Development system

Used for:

```text
Requirements
Architecture
Planning
Coding
Testing
Security Review
Documentation
Independent Review
```

### Application runtime

Used for:

```text
Conversation Processing
AI Insight Extraction
Taxonomy Classification
Analytics
NLP-to-SQL
```

We should **not automatically put all development agents into the production application**.

The application should remain as simple as possible.

---

# 33. Current Project Status

We have now completed at the planning level:

### ✅ Business problem definition

### ✅ Product vision

### ✅ Product Requirements Draft

### ✅ MVP scope

### ✅ High-level architecture

### ✅ Detailed component architecture

### ✅ Conversation Intelligence architecture

### ✅ Taxonomy architecture

### ✅ NLP-to-SQL target architecture

### ✅ Security principles

### ✅ Initial Architecture Decisions

---

# 34. Next Step

The next step should be:

# **Step 3 — Detailed Data Model + Domain Model**

We should now define the actual entities and relationships for the application.

Specifically:

```text
Conversation
      │
      ├── Conversation Message
      │
      ├── Conversation Metadata
      │
      └── Conversation Insight
                 │
                 ├── AI Insight
                 │
                 ├── Taxonomy Classification
                 │
                 ├── Confidence
                 │
                 └── Processing Version
```

We also need to define:

### Taxonomy Model

```text
Taxonomy Category
Taxonomy Version
Category Relationship
Category Embedding
```

### AI Processing Model

```text
Processing Run
Model Version
Prompt Version
Processing Result
Processing Error
```

### Evaluation Model

```text
Evaluation Dataset
Evaluation Case
Expected Result
Actual Result
Evaluation Score
```

### Future Semantic Layer

```text
Metric
Dimension
Entity
Relationship
Business Definition
```

---

## My recommendation

👉 **We proceed next with Step 3: Complete Data Model and Domain Architecture.**

That will give us the actual blueprint for the database schema, application models, relationships, and versioning strategy before we create the implementation roadmap and start coding.

[1]: https://learn.microsoft.com/en-us/azure/well-architected/ai/application-design?utm_source=chatgpt.com "Application Design for AI Workloads on Azure - Microsoft Azure Well-Architected Framework | Microsoft Learn"
[2]: https://semantic.io/insights/text-to-sql-vs-semantic-layer?utm_source=chatgpt.com "Text-to-SQL vs. Semantic Layer: When to Use Each in 2026 | Semantic.io"
[3]: https://www.strategy.com/software/blog/semantic-layer-architecture-for-enterprise-ai-the-five-core-components?utm_source=chatgpt.com "Semantic Layer Architecture for Enterprise AI: 5 Components"
[4]: https://docs.aws.amazon.com/prescriptive-guidance/latest/govern-architect-agentic-ai/enterprise-architecture.html?utm_source=chatgpt.com "Agentic AI architecture in the enterprise - AWS Prescriptive Guidance"
