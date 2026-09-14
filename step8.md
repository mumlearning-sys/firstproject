# STEP 8 — FINAL VALIDATION & APPLICATION DEVELOPMENT LAUNCH

This is the final stage of the **GitHub Copilot Development System** before we begin building the actual application.

The objective of Step 8 is simple:

> **Make sure the development system works in practice before we depend on it to build the banking conversation intelligence application.**

---

# 8.1 Where We Are Now

We have completed the design of the overall development system.

## Completed

### Development System Foundation

* ✅ Defined the ORC-style orchestration model
* ✅ Defined the Orchestrator role
* ✅ Defined specialist responsibilities
* ✅ Defined context management principles
* ✅ Defined minimum relevant context strategy

### Copilot Customization Design

* ✅ Repository-wide instructions
* ✅ Path-specific instructions
* ✅ `AGENTS.md`
* ✅ Reusable prompt workflows
* ✅ Specialist skills
* ✅ Custom-agent model

### Development Workflows

* ✅ Feature development workflow
* ✅ Bug investigation workflow
* ✅ Architecture-change workflow
* ✅ AI feature workflow
* ✅ Testing workflow
* ✅ Code review workflow

### Project Context

* ✅ Product context structure
* ✅ Requirements structure
* ✅ Architecture documentation structure
* ✅ Domain documentation structure
* ✅ ADR structure
* ✅ Development standards structure

---

# 8.2 What Step 8 Will Complete

Step 8 turns this:

```text
CONCEPTUAL DEVELOPMENT SYSTEM
```

into:

```text
WORKING DEVELOPMENT SYSTEM
```

The sequence is:

```text
CREATE REPOSITORY
        ↓
CREATE SYSTEM FILES
        ↓
ADD PROJECT CONTEXT
        ↓
CONFIGURE COPILOT
        ↓
VALIDATE INSTRUCTIONS
        ↓
RUN PILOT TASK
        ↓
VERIFY OUTPUT
        ↓
START APPLICATION DEVELOPMENT
```

---

# 8.3 Final Repository Structure

The recommended repository structure will be:

```text
banking-conversation-intelligence/
│
├── .github/
│   │
│   ├── copilot-instructions.md
│   │
│   ├── instructions/
│   │   ├── python.instructions.md
│   │   ├── testing.instructions.md
│   │   ├── ai.instructions.md
│   │   ├── streamlit.instructions.md
│   │   ├── snowflake.instructions.md
│   │   ├── security.instructions.md
│   │   └── documentation.instructions.md
│   │
│   ├── prompts/
│   │   ├── analyze-requirement.prompt.md
│   │   ├── plan-feature.prompt.md
│   │   ├── implement-feature.prompt.md
│   │   ├── generate-tests.prompt.md
│   │   ├── review-code.prompt.md
│   │   ├── investigate-bug.prompt.md
│   │   └── architecture-change.prompt.md
│   │
│   ├── skills/
│   │   ├── feature-development/
│   │   │   └── SKILL.md
│   │   │
│   │   ├── ai-feature/
│   │   │   └── SKILL.md
│   │   │
│   │   ├── taxonomy-classification/
│   │   │   └── SKILL.md
│   │   │
│   │   ├── snowflake-development/
│   │   │   └── SKILL.md
│   │   │
│   │   ├── streamlit-development/
│   │   │   └── SKILL.md
│   │   │
│   │   └── code-review/
│   │       └── SKILL.md
│   │
│   └── agents/
│       ├── orchestrator.agent.md
│       ├── requirements.agent.md
│       ├── architecture.agent.md
│       ├── implementation.agent.md
│       ├── testing.agent.md
│       └── review.agent.md
│
├── AGENTS.md
│
├── docs/
│   │
│   ├── project/
│   │   └── product-overview.md
│   │
│   ├── requirements/
│   │   ├── business-requirements.md
│   │   ├── epics.md
│   │   └── features/
│   │
│   ├── architecture/
│   │   ├── system-architecture.md
│   │   ├── application-architecture.md
│   │   ├── data-architecture.md
│   │   └── integration-architecture.md
│   │
│   ├── domain/
│   │   ├── conversation-model.md
│   │   ├── insight-model.md
│   │   └── taxonomy-model.md
│   │
│   ├── adr/
│   │
│   └── development/
│       ├── coding-standards.md
│       ├── testing-strategy.md
│       └── development-workflow.md
│
├── config/
│
├── src/
│
├── tests/
│
├── scripts/
│
├── README.md
│
├── pyproject.toml
│
└── .gitignore
```

---

# 8.4 Phase A — Create the Repository

The first practical step is to create the GitHub repository.

Recommended name:

```text
banking-conversation-intelligence
```

The initial repository should contain:

```text
README.md
.gitignore
pyproject.toml
.github/
docs/
src/
tests/
config/
scripts/
```

At this point, **do not start writing the actual business application randomly**.

First establish the development system.

---

# 8.5 Phase B — Build the Copilot Instruction Layer

The implementation order should be:

```text
1. copilot-instructions.md

2. AGENTS.md

3. Path-specific instructions

4. Prompt workflows

5. Skills

6. Custom agents
```

Why this order?

Because:

```text
GLOBAL RULES
       ↓
ORCHESTRATION RULES
       ↓
SPECIALIST RULES
       ↓
TASK WORKFLOWS
       ↓
SPECIALIZED KNOWLEDGE
       ↓
AGENT EXECUTION
```

Each layer builds on the previous one.

---

# 8.6 Phase C — Add the Project Context

Before Copilot can intelligently develop the application, it needs to understand the project.

The first context documents should be created in this order:

## Document 1 — Product Overview

```text
docs/project/product-overview.md
```

This answers:

```text
What are we building?

Why are we building it?

Who uses it?

What data does it process?

What business problem does it solve?
```

---

## Document 2 — Business Requirements

```text
docs/requirements/business-requirements.md
```

This defines:

```text
Core capabilities

Business requirements

Functional requirements

Non-functional requirements

Constraints
```

---

## Document 3 — Epics

```text
docs/requirements/epics.md
```

This breaks the application into development areas.

Example:

```text
EPIC 1
PROJECT FOUNDATION

EPIC 2
DATA INGESTION

EPIC 3
CONVERSATION NORMALIZATION

EPIC 4
AI CONVERSATION ANALYSIS

EPIC 5
TAXONOMY CLASSIFICATION

EPIC 6
INSIGHT PERSISTENCE

EPIC 7
ANALYTICS

EPIC 8
STREAMLIT APPLICATION

EPIC 9
AI EVALUATION

EPIC 10
ENTERPRISE QUALITY AND OPERATIONS
```

---

# 8.7 Recommended Application Development Roadmap

Now we can clearly separate the **development system roadmap** from the **actual application roadmap**.

The Copilot system is now:

```text
STEP 1 → STEP 8
```

The application development begins after that.

Recommended application sequence:

```text
PHASE 1
PROJECT FOUNDATION
        ↓
PHASE 2
DATA INGESTION
        ↓
PHASE 3
CONVERSATION NORMALIZATION
        ↓
PHASE 4
DOMAIN MODELS
        ↓
PHASE 5
AI ANALYSIS ENGINE
        ↓
PHASE 6
TAXONOMY CLASSIFICATION
        ↓
PHASE 7
SNOWFLAKE PERSISTENCE
        ↓
PHASE 8
ANALYTICS SERVICES
        ↓
PHASE 9
STREAMLIT APPLICATION
        ↓
PHASE 10
AI EVALUATION
        ↓
PHASE 11
ENTERPRISE HARDENING
```

---

# 8.8 Application Architecture We Will Build

The target architecture should remain modular.

```text
                    ┌──────────────────┐
                    │    STREAMLIT     │
                    │ PRESENTATION UI  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ APPLICATION      │
                    │ SERVICES         │
                    │ / USE CASES      │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ DOMAIN           │
                    │                  │
                    │ Conversations    │
                    │ Insights         │
                    │ Taxonomy         │
                    │ Evaluation       │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ INFRASTRUCTURE   │
                    │                  │
                    │ Snowflake        │
                    │ OpenAI / LLM     │
                    │ Embeddings       │
                    │ External APIs    │
                    └──────────────────┘
```

This prevents the application from becoming:

```text
app.py

├── SQL
├── LLM prompts
├── business logic
├── taxonomy
├── UI
└── everything else
```

That is specifically what our development system is designed to avoid.

---

# 8.9 Phase D — Configure the Python Project

The project should use a modern Python structure.

Recommended stack:

```text
Python 3.11+

Streamlit

Pydantic

pytest

Snowflake connector / approved Snowflake integration

LLM provider abstraction

Structured logging

Configuration management
```

The initial source structure:

```text
src/
│
└── banking_conversation_intelligence/
    │
    ├── domain/
    │
    ├── application/
    │
    ├── infrastructure/
    │
    ├── presentation/
    │
    └── shared/
```

---

# 8.10 Recommended Source Structure

## Domain

```text
domain/
```

Contains:

```text
Conversation

Message

ConversationInsight

TaxonomyClassification

SentimentResult

ResolutionAssessment

AccuracyAssessment
```

Domain should understand the business.

It should not know:

```text
Streamlit

Snowflake

OpenAI

Specific APIs
```

---

## Application

```text
application/
```

Contains:

```text
use_cases/

services/

ports/
```

Examples:

```text
AnalyzeConversation

ClassifyConversation

IngestConversation

GetAnalytics

SearchConversations
```

---

## Infrastructure

```text
infrastructure/
```

Contains:

```text
snowflake/

ai/

embeddings/

repositories/

config/
```

Examples:

```text
SnowflakeConversationRepository

OpenAIProvider

EmbeddingProvider

TaxonomyRepository
```

---

## Presentation

```text
presentation/
```

Contains:

```text
streamlit/

components/

pages/
```

The presentation layer calls application services.

---

# 8.11 Phase E — Validate the Development System

Before starting the actual application, we should test whether Copilot correctly understands the repository.

We should use a small pilot task.

Recommended pilot:

> Create the core Conversation domain model and supporting validation.

Why this task?

Because it tests:

```text
PROJECT CONTEXT
        +
PYTHON INSTRUCTIONS
        +
ARCHITECTURE
        +
TESTING
        +
DOMAIN DESIGN
```

without immediately involving:

```text
Snowflake

LLM providers

Streamlit

Complex taxonomy
```

---

# 8.12 Pilot Task Workflow

You would ask Copilot:

> Analyze and plan the implementation of the Conversation domain model according to the repository architecture. Do not implement yet.

The expected behavior:

```text
REQUEST
   ↓
READ PRODUCT OVERVIEW
   ↓
READ REQUIREMENTS
   ↓
READ ARCHITECTURE
   ↓
READ DOMAIN CONTEXT
   ↓
CREATE PLAN
```

The expected output should include:

```text
Understanding

Affected modules

Proposed domain model

Validation requirements

Dependencies

Implementation plan

Test plan

Assumptions
```

---

# 8.13 Pilot Validation Checklist

We verify:

### Context

```text
Did Copilot understand the project?
```

### Architecture

```text
Did it place the model in the domain layer?
```

### Dependencies

```text
Did it avoid unnecessary Streamlit/Snowflake dependencies?
```

### Testing

```text
Did it propose appropriate tests?
```

### Planning

```text
Did it plan before coding?
```

If yes:

```text
DEVELOPMENT SYSTEM VALIDATED
```

---

# 8.14 Pilot Implementation

After reviewing the plan:

> Implement the approved plan. Follow the repository instructions. Add unit tests. Do not introduce unrelated changes.

Expected process:

```text
IMPLEMENT
    ↓
TYPE VALIDATION
    ↓
UNIT TESTS
    ↓
TEST EXECUTION
    ↓
REVIEW
```

Then review:

```text
Changed files

Architecture compliance

Test coverage

Code quality

Errors
```

---

# 8.15 Second Pilot — AI Feature

The second pilot should test our AI-specific development system.

Example:

> Design a conversation insight extraction interface with structured output validation. Do not integrate a real LLM provider yet.

This validates:

```text
AI INSTRUCTIONS

AI SKILL

DOMAIN MODEL

PROVIDER ABSTRACTION

STRUCTURED OUTPUT

TESTING
```

The result should be an architecture such as:

```text
APPLICATION
      │
      ▼
InsightExtractionPort
      │
      ▼
DOMAIN RESULT MODEL
      │
      ▼
STRUCTURED VALIDATION
      │
      ▼
INFRASTRUCTURE PROVIDER
```

Not:

```text
app.py
   │
   ▼
OpenAI API
```

---

# 8.16 Third Pilot — Snowflake Feature

The third pilot:

> Design the conversation repository interface and Snowflake implementation boundary.

This validates:

```text
SNOWFLAKE INSTRUCTIONS

ARCHITECTURE

REPOSITORY PATTERN

PERFORMANCE

SECURITY
```

Expected:

```text
APPLICATION PORT
       │
       ▼
REPOSITORY INTERFACE
       │
       ▼
SNOWFLAKE IMPLEMENTATION
```

---

# 8.17 The Three-Pilot Validation Strategy

Our final validation should therefore be:

```text
PILOT 1
DOMAIN MODEL
      ↓
Architecture Validation
      ↓
PILOT 2
AI ABSTRACTION
      ↓
AI Development Validation
      ↓
PILOT 3
SNOWFLAKE REPOSITORY
      ↓
Infrastructure Validation
```

If all three work correctly:

```text
┌─────────────────────────────┐
│ DEVELOPMENT SYSTEM VERIFIED │
└─────────────────────────────┘
```

---

# 8.18 How You Will Actually Work With Copilot

Once the system is set up, your normal workflow becomes:

## Small Task

You say:

> Add validation for missing conversation IDs.

Copilot should:

```text
Understand
↓
Inspect relevant model
↓
Implement
↓
Test
```

---

## Medium Feature

You say:

> Add conversation search capability.

Copilot should:

```text
Understand requirement
↓
Load relevant context
↓
Check architecture
↓
Plan
↓
Implement
↓
Test
↓
Review
```

---

## Large Feature

You say:

> Implement taxonomy classification.

Copilot should:

```text
Requirements Analysis
↓
Architecture Review
↓
Taxonomy Context
↓
AI Design
↓
Retrieval Design
↓
Evaluation Strategy
↓
Implementation Plan
↓
Implementation
↓
Testing
↓
AI Evaluation
↓
Review
```

This is exactly why we built the system first.

---

# 8.19 Your Role vs Copilot's Role

This is important.

## You

You provide:

```text
Business objective

Requirements

Priorities

Approval

Feedback

Domain decisions
```

---

## Copilot Development System

It provides:

```text
Context awareness

Planning

Architecture guidance

Implementation assistance

Testing

Review

Consistency
```

---

## The Development Workflow

```text
YOU
 │
 │ Business Request
 ▼
ORCHESTRATOR
 │
 ▼
CONTEXT
 │
 ▼
PLAN
 │
 ▼
SPECIALIST GUIDANCE
 │
 ▼
IMPLEMENTATION
 │
 ▼
TESTING
 │
 ▼
REVIEW
 │
 ▼
YOU APPROVE
```

You remain responsible for business decisions.

The system reduces development friction and improves consistency.

---

# 8.20 Final Definition of Done

Before we consider the **Copilot Development System fully complete**, we should have physically created and validated:

### Core System

* ⬜ `.github/copilot-instructions.md`
* ⬜ `AGENTS.md`
* ⬜ Path-specific instructions
* ⬜ Prompt workflows
* ⬜ Skills
* ⬜ Custom agents where supported

### Project Context

* ⬜ Product overview
* ⬜ Business requirements
* ⬜ Epics
* ⬜ Architecture documents
* ⬜ Domain documents
* ⬜ Development standards
* ⬜ ADR structure

### Validation

* ⬜ Domain pilot
* ⬜ AI pilot
* ⬜ Snowflake pilot
* ⬜ Testing validation
* ⬜ Review validation

Only after these are physically present and validated should we say:

> **The development system setup is complete.**

---

# 8.21 The Most Important Clarification

At this point, there are **two separate things**:

## 1. Development System

```text
GitHub Repository Setup

+
Copilot Instructions

+
ORC Orchestration

+
Agents

+
Skills

+
Prompts

+
Context Documents
```

### Status:

**Designed and ready to build physically.**

---

## 2. Banking Conversation Intelligence Application

```text
Ingestion

Normalization

AI Analysis

Taxonomy

Snowflake

Analytics

Streamlit
```

### Status:

**Ready to begin after the development system files are created and validated.**

---

# 8.22 What We Should Do Next

The next logical step is no longer more architecture discussion.

We should now **physically build the development system repository package**.

That means creating the actual:

```text
.github/copilot-instructions.md

.github/instructions/*.md

.github/prompts/*.md

.github/skills/*/SKILL.md

.github/agents/*.agent.md

AGENTS.md

docs/project/*.md

docs/requirements/*.md

docs/architecture/*.md

docs/domain/*.md
```

Then package it as:

```text
banking-conversation-intelligence-copilot-system.zip
```

After that, you can extract it into your GitHub repository and start using the system as the foundation for the application.

## Current overall status

| Area                              | Status                  |
| --------------------------------- | ----------------------- |
| Business problem                  | ✅ Defined               |
| Application vision                | ✅ Defined               |
| Enterprise architecture direction | ✅ Defined               |
| ORC development approach          | ✅ Defined               |
| Copilot development system design | ✅ Complete conceptually |
| Step 1–8 planning                 | ✅ Complete              |
| Actual system files               | ⏳ Next                  |
| Copilot validation pilots         | ⏳ Pending               |
| Application development           | ⏳ Pending               |

### **So the true next step is: create the actual repository files and package the complete development system.**

That is where we should proceed next.
