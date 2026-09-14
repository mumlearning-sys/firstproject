Absolutely. We now move to **STEP 7 — Build the Actual GitHub Copilot Development System Files**.

This is where we stop discussing the system conceptually and define the **actual files and instructions that will become the reusable development system**.

One important update to our earlier design: GitHub Copilot's current customization model gives us a stronger native foundation than simply using one `AGENTS.md`. We can combine repository-wide instructions, path-specific instructions, prompt files, custom agents, agent skills, and—in supported workflows—subagents. ([GitHub Docs][1])

---

# STEP 7 — BUILD THE ACTUAL COPILOT DEVELOPMENT SYSTEM

## 7.1 Final Development System Architecture

Our system will have six layers:

```text
┌──────────────────────────────────────────────┐
│              PROJECT CONTEXT                 │
│                                              │
│ Requirements / Architecture / ADRs / Domain  │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│          GLOBAL COPILOT INSTRUCTIONS         │
│                                              │
│     .github/copilot-instructions.md          │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│        ORCHESTRATION / AGENT BEHAVIOR        │
│                                              │
│                AGENTS.md                     │
└───────────────────────┬──────────────────────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
┌──────────────┐ ┌─────────────┐ ┌──────────────┐
│ PATH RULES   │ │ PROMPT      │ │ SPECIALIZED  │
│              │ │ WORKFLOWS   │ │ SKILLS       │
└──────────────┘ └─────────────┘ └──────────────┘
          │             │             │
          └─────────────┼─────────────┘
                        ▼
┌──────────────────────────────────────────────┐
│          APPLICATION DEVELOPMENT             │
└──────────────────────────────────────────────┘
```

GitHub officially supports repository-wide instructions, path-specific instructions, `AGENTS.md`, reusable prompt files, and agent skills for different scopes and purposes. ([GitHub Docs][1])

---

# 7.2 Final Repository Structure

Our target repository will look like this:

```text
banking-conversation-intelligence/
│
├── .github/
│   │
│   ├── copilot-instructions.md
│   │
│   ├── instructions/
│   │   │
│   │   ├── python.instructions.md
│   │   ├── testing.instructions.md
│   │   ├── ai.instructions.md
│   │   ├── streamlit.instructions.md
│   │   ├── snowflake.instructions.md
│   │   ├── security.instructions.md
│   │   └── documentation.instructions.md
│   │
│   ├── prompts/
│   │   │
│   │   ├── analyze-requirement.prompt.md
│   │   ├── plan-feature.prompt.md
│   │   ├── implement-feature.prompt.md
│   │   ├── generate-tests.prompt.md
│   │   ├── review-code.prompt.md
│   │   ├── investigate-bug.prompt.md
│   │   └── architecture-change.prompt.md
│   │
│   ├── skills/
│   │   │
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
│       │
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
│
├── src/
│
├── tests/
│
├── config/
│
├── prompts/
│
└── README.md
```

### Important clarification

Earlier we discussed conceptual "agents." Now we should map those roles onto Copilot's actual capabilities rather than pretending that every Markdown file automatically creates an independently running agent.

Current Copilot customization supports **custom agents** as specialist personas and **agent skills** as reusable task-specific instruction/resource packages. ([GitHub Docs][2])

That gives us a more robust system.

---

# 7.3 File 1 — `.github/copilot-instructions.md`

This is the **global project constitution**.

It should contain persistent instructions that apply across the repository.

Its structure should be:

```text
# Project Purpose

# Core Architecture

# Technology Stack

# Development Principles

# AI Development Rules

# Data Rules

# Security Rules

# Testing Rules

# Performance Rules

# Documentation Rules

# Definition of Done

# Prohibited Patterns
```

---

## Proposed Core Content

The beginning of the file should establish the project:

```text
This repository contains an enterprise banking conversation
intelligence application.

The application processes customer and virtual assistant
conversation data and converts conversations into structured,
traceable business insights.

The system must support:

- Conversation ingestion
- Conversation normalization
- AI-powered conversation analysis
- Customer problem identification
- Sentiment analysis
- Resolution assessment
- Assistant accuracy assessment
- Hierarchical taxonomy classification
- Evidence extraction
- Structured insight persistence
- Analytics and dashboards
- Conversation exploration
- AI evaluation and regression testing
```

---

## Architecture Instructions

```text
Follow the repository's layered modular architecture.

Presentation code must not contain core business logic.

Streamlit pages must call application services.

Application services coordinate use cases and infrastructure.

Domain logic must remain independent of UI frameworks,
database implementations, and specific AI providers.

Infrastructure integrations must implement interfaces or
abstractions defined by the application architecture.

Do not introduce direct dependencies between unrelated layers.

Avoid placing new functionality into existing files merely
because they are convenient. Place functionality according
to the architecture.
```

---

## AI Instructions

```text
Treat all LLM output as untrusted until validated.

Use structured outputs for business-critical AI processing.

Validate structured output using defined schemas.

Do not expose provider-specific implementation details
outside the infrastructure layer.

All AI processing must support traceability where applicable,
including:

- provider
- model
- prompt version
- processing timestamp
- processing status
- failure information
```

---

## Data Instructions

```text
Do not load unnecessarily large datasets into application memory.

Push filtering, aggregation, and heavy analytical processing
to Snowflake where appropriate.

Use pagination or limited result sets for user-facing
conversation exploration.

Do not execute raw database queries directly from Streamlit pages.

Use repositories or defined data-access abstractions.
```

---

## Development Behavior

```text
For non-trivial changes:

1. Understand the requirement.
2. Retrieve relevant project context.
3. Identify affected components.
4. Check architectural impact.
5. Create an implementation plan.
6. Implement the smallest correct solution.
7. Add or update tests.
8. Review the change.
9. Update documentation where required.

Do not make broad unrelated changes.

Do not rewrite existing working components without identifying
a specific requirement or defect.

Do not invent requirements. Document assumptions when necessary.
```

---

# 7.4 File 2 — `AGENTS.md`

This file defines the **ORC-style orchestration behavior**.

Our ORC model remains:

```text
O = ORCHESTRATE
R = REASON
C = COORDINATE
```

The root `AGENTS.md` should define how the primary development workflow behaves.

GitHub and VS Code documentation support `AGENTS.md` as agent-oriented repository instructions, though support can vary by Copilot feature and environment. ([GitHub Docs][1])

---

## Proposed `AGENTS.md` Structure

```text
# Development System Mission

# ORC Workflow

# Task Classification

# Context Retrieval

# Planning Rules

# Implementation Rules

# Testing Rules

# Review Rules

# Completion Rules
```

---

## ORC Workflow

The core instruction will be:

```text
For every significant development request:

ORCHESTRATE
- Determine the task type.
- Identify relevant project context.
- Identify required specialist perspectives.

REASON
- Analyze requirements.
- Identify assumptions.
- Identify dependencies.
- Identify risks.
- Check architectural constraints.

COORDINATE
- Apply architecture guidance.
- Apply implementation guidance.
- Apply testing guidance.
- Apply security guidance where relevant.
- Apply review before completion.
```

---

# 7.5 Task Classification

The system should first classify incoming requests.

```text
USER REQUEST
      │
      ▼
TASK CLASSIFICATION
      │
      ├── Feature
      │
      ├── Bug
      │
      ├── Refactor
      │
      ├── AI Feature
      │
      ├── Taxonomy Change
      │
      ├── Architecture Change
      │
      ├── UI Change
      │
      └── Investigation
```

Each task type follows a different workflow.

---

# 7.6 Feature Workflow

```text
FEATURE REQUEST
       │
       ▼
UNDERSTAND
       │
       ▼
LOAD CONTEXT
       │
       ▼
CHECK REQUIREMENTS
       │
       ▼
CHECK ARCHITECTURE
       │
       ▼
PLAN
       │
       ▼
IMPLEMENT
       │
       ▼
TEST
       │
       ▼
REVIEW
       │
       ▼
COMPLETE
```

---

# 7.7 Bug Workflow

```text
BUG
 │
 ▼
REPRODUCE
 │
 ▼
INVESTIGATE
 │
 ▼
ROOT CAUSE
 │
 ▼
FIX PLAN
 │
 ▼
IMPLEMENT FIX
 │
 ▼
REGRESSION TEST
 │
 ▼
REVIEW
```

The system must not immediately start modifying code without attempting to understand the cause.

---

# 7.8 Architecture Change Workflow

```text
ARCHITECTURE REQUEST
        │
        ▼
CURRENT STATE
        │
        ▼
IMPACT ANALYSIS
        │
        ▼
OPTIONS
        │
        ▼
RECOMMENDATION
        │
        ▼
ADR UPDATE
        │
        ▼
IMPLEMENTATION PLAN
```

---

# 7.9 File 3 — Python Instructions

Location:

```text
.github/instructions/python.instructions.md
```

Frontmatter:

```yaml
---
applyTo: "src/**/*.py,tests/**/*.py,scripts/**/*.py"
---
```

Core rules:

```text
Use Python 3.11+.

Use clear type hints for public interfaces.

Prefer small focused functions.

Keep classes focused on a clear responsibility.

Avoid hidden side effects.

Validate external input.

Use meaningful exceptions.

Do not silently suppress exceptions.

Avoid broad except clauses.

Do not introduce unnecessary global state.

Keep domain logic independent of infrastructure.

Design code for testability.
```

---

# 7.10 File 4 — Testing Instructions

Location:

```text
.github/instructions/testing.instructions.md
```

Applies to:

```text
tests/**/*.py
```

Core rules:

```text
Use pytest.

Follow Arrange, Act, Assert where practical.

Unit tests must isolate external dependencies.

Mock AI providers and database integrations in unit tests.

Integration tests must be clearly separated.

Test normal behavior, edge cases, and failure conditions.

Bug fixes require regression tests where practical.

AI functionality requires evaluation datasets or defined
evaluation cases.
```

---

# 7.11 File 5 — AI Instructions

Location:

```text
.github/instructions/ai.instructions.md
```

This file is particularly important for your application.

Core rules:

```text
Do not place provider-specific AI calls in the domain layer.

Use AI provider abstractions.

Use structured output schemas.

Validate every business-critical AI response.

Handle malformed responses.

Handle provider failures.

Handle timeouts and retries according to configuration.

Version prompts.

Capture traceability metadata.

Add evaluation cases for new AI functionality.

Do not assume confidence scores are inherently reliable.

Do not send the entire taxonomy to the LLM unless explicitly
required by a justified design.
```

---

# 7.12 File 6 — Snowflake Instructions

Location:

```text
.github/instructions/snowflake.instructions.md
```

Rules:

```text
Database access must use repository or data-access abstractions.

Do not execute SQL directly from presentation code.

Push filtering and aggregation to Snowflake.

Avoid loading complete large datasets into Python.

Use controlled result limits and pagination where appropriate.

Centralize connection configuration.

Never hard-code credentials.

Consider query efficiency when introducing analytical features.
```

---

# 7.13 File 7 — Streamlit Instructions

Location:

```text
.github/instructions/streamlit.instructions.md
```

Rules:

```text
Keep Streamlit code focused on presentation and user interaction.

Do not place core business logic in Streamlit pages.

Do not place raw database access in Streamlit pages.

Use application services.

Use session state only for appropriate UI state.

Use caching deliberately.

Do not cache sensitive data without considering security.

Do not load unnecessary large datasets.

Create reusable UI components where patterns repeat.
```

---

# 7.14 File 8 — Security Instructions

Location:

```text
.github/instructions/security.instructions.md
```

Rules:

```text
Never hard-code secrets.

Never commit credentials.

Do not expose secrets in logs.

Minimize sensitive conversation information in logs.

Validate external input.

Review authentication and authorization impact for user-facing
features.

Treat external service responses as untrusted.

Do not introduce insecure default configurations.
```

---

# 7.15 File 9 — Documentation Instructions

Location:

```text
.github/instructions/documentation.instructions.md
```

Rules:

```text
Update documentation when behavior, architecture,
configuration, interfaces, or developer workflows change.

Documentation must explain:

- what changed
- why it changed
- how to use it
- important constraints

Do not duplicate information unnecessarily.

Keep architecture decisions in ADRs where appropriate.
```

---

# 7.16 Prompt Files

Prompt files give us reusable workflows for common development tasks. GitHub currently documents them as reusable, task-specific prompt templates, though availability and preview status can vary by environment. ([GitHub Docs][1])

Our prompt directory:

```text
.github/prompts/
│
├── analyze-requirement.prompt.md
├── plan-feature.prompt.md
├── implement-feature.prompt.md
├── generate-tests.prompt.md
├── review-code.prompt.md
├── investigate-bug.prompt.md
└── architecture-change.prompt.md
```

---

# 7.17 Prompt — Analyze Requirement

Purpose:

```text
Understand the request before coding.
```

Expected workflow:

```text
REQUEST
   │
   ▼
PROJECT CONTEXT
   │
   ▼
REQUIREMENTS
   │
   ▼
AFFECTED COMPONENTS
   │
   ▼
ACCEPTANCE CRITERIA
   │
   ▼
RISKS
```

Expected output:

```text
## Understanding

## Relevant Context

## Scope

## Acceptance Criteria

## Dependencies

## Assumptions

## Risks

## Recommended Next Step
```

---

# 7.18 Prompt — Plan Feature

Purpose:

```text
Convert an approved requirement into implementation tasks.
```

Expected output:

```text
## Feature Objective

## Architecture Impact

## Affected Modules

## Implementation Steps

## Data Changes

## AI Changes

## Test Strategy

## Risks

## Definition of Done
```

---

# 7.19 Prompt — Implement Feature

Purpose:

```text
Implement an approved feature plan.
```

Rules:

```text
Read the approved implementation plan.

Inspect existing related code before creating new abstractions.

Reuse existing patterns where appropriate.

Make the smallest correct change.

Do not make unrelated refactors.

Add tests.

Report changed files.

Report assumptions.
```

---

# 7.20 Prompt — Generate Tests

Workflow:

```text
FEATURE
   │
   ▼
TEST ANALYSIS
   │
   ├── Normal Cases
   │
   ├── Edge Cases
   │
   ├── Failure Cases
   │
   └── Regression Risks
```

Output:

```text
## Test Strategy

## Unit Tests

## Edge Cases

## Failure Cases

## Integration Tests

## AI Evaluation Cases

## Missing Test Coverage
```

---

# 7.21 Prompt — Review Code

Review sequence:

```text
REQUIREMENT
     ↓
ARCHITECTURE
     ↓
CODE QUALITY
     ↓
ERROR HANDLING
     ↓
TESTS
     ↓
SECURITY
     ↓
PERFORMANCE
```

Output:

```text
## Summary

## Requirement Compliance

## Architecture Findings

## Code Quality Findings

## Test Coverage Findings

## Security Findings

## Performance Findings

## Required Changes

## Optional Improvements
```

---

# 7.22 Prompt — Investigate Bug

Workflow:

```text
BUG REPORT
    │
    ▼
REPRODUCTION
    │
    ▼
CODE INSPECTION
    │
    ▼
ROOT CAUSE
    │
    ▼
FIX OPTIONS
    │
    ▼
RECOMMENDATION
```

The key rule:

```text
Do not implement a speculative fix before identifying the
most likely root cause.
```

---

# 7.23 Prompt — Architecture Change

Output:

```text
## Current Architecture

## Requested Change

## Why the Current Architecture Is Insufficient

## Options

## Recommended Option

## Impact

## Migration Plan

## ADR Requirement

## Implementation Plan
```

---

# 7.24 Agent Skills

This is where our specialist knowledge becomes reusable.

GitHub describes skills as folders containing instructions, scripts, and resources that Copilot can load when relevant to specialized tasks. GitHub specifically recommends using broad custom instructions for common rules and skills for detailed, task-specific guidance. ([GitHub Docs][3])

Our skills:

```text
.github/skills/
│
├── feature-development/
├── ai-feature/
├── taxonomy-classification/
├── snowflake-development/
├── streamlit-development/
└── code-review/
```

---

# 7.25 Feature Development Skill

```text
.github/skills/feature-development/SKILL.md
```

Workflow:

```text
FEATURE REQUEST
       │
       ▼
READ REQUIREMENTS
       │
       ▼
READ ARCHITECTURE
       │
       ▼
INSPECT EXISTING CODE
       │
       ▼
PLAN
       │
       ▼
IMPLEMENT
       │
       ▼
TEST
       │
       ▼
REVIEW
```

Checklist:

```text
□ Requirement understood

□ Relevant context loaded

□ Architecture checked

□ Dependencies identified

□ Plan created

□ Implementation completed

□ Tests added

□ Tests passed

□ Review completed

□ Documentation updated
```

---

# 7.26 AI Feature Skill

This is specific to your project.

```text
AI FEATURE
     │
     ▼
BUSINESS REQUIREMENT
     │
     ▼
INPUT CONTRACT
     │
     ▼
PROMPT DESIGN
     │
     ▼
STRUCTURED OUTPUT
     │
     ▼
VALIDATION
     │
     ▼
ERROR HANDLING
     │
     ▼
TRACEABILITY
     │
     ▼
EVALUATION
```

Checklist:

```text
□ Provider abstraction used

□ Prompt version defined

□ Input validated

□ Output schema defined

□ Output validated

□ Failure handling included

□ Retry behavior considered

□ Traceability captured

□ Evaluation cases added
```

---

# 7.27 Taxonomy Classification Skill

This skill will protect one of the most important and complicated parts of the project.

The required approach remains:

```text
CONVERSATION
      │
      ▼
CUSTOMER PROBLEM
      │
      ▼
PROBLEM SUMMARY
      │
      ▼
EMBEDDING / RETRIEVAL
      │
      ▼
TOP TAXONOMY CANDIDATES
      │
      ▼
LLM CLASSIFICATION
      │
      ▼
LEVEL 1
      │
      ▼
LEVEL 2
      │
      ▼
LEVEL 3
      │
      ▼
VALIDATED RESULT
```

The skill should enforce:

```text
Do not send the complete taxonomy to the LLM by default.

Retrieve relevant candidates first.

Preserve taxonomy hierarchy.

Support ambiguous cases.

Support unknown or unclassified outcomes.

Capture classification confidence.

Capture reasoning/evidence in a controlled format where required.

Add ambiguous classification cases to evaluation datasets.
```

---

# 7.28 Snowflake Development Skill

Checklist:

```text
□ Repository abstraction used

□ Query pushed to Snowflake

□ No unnecessary full-table load

□ Filtering performed at source

□ Aggregation performed at source

□ Large result handling considered

□ Connection management reused

□ Credentials protected

□ Performance considered
```

---

# 7.29 Streamlit Development Skill

Workflow:

```text
USER REQUIREMENT
       │
       ▼
UI DESIGN
       │
       ▼
DATA REQUIREMENT
       │
       ▼
APPLICATION SERVICE
       │
       ▼
QUERY SERVICE
       │
       ▼
SNOWFLAKE
       │
       ▼
RESULT
       │
       ▼
UI
```

Important rule:

```text
STREAMLIT IS THE PRESENTATION LAYER.

IT MUST NOT BECOME THE ENTIRE APPLICATION.
```

---

# 7.30 Code Review Skill

Every major change should be reviewed against:

```text
REQUIREMENT
      +
ARCHITECTURE
      +
QUALITY
      +
TESTING
      +
SECURITY
      +
PERFORMANCE
```

Review checklist:

```text
□ Does it solve the requirement?

□ Does it follow architecture?

□ Are responsibilities correctly placed?

□ Are external dependencies isolated?

□ Are errors handled?

□ Are tests sufficient?

□ Are secrets protected?

□ Is performance acceptable?

□ Are unnecessary changes included?
```

---

# 7.31 Project Context Documents

The Copilot system cannot work effectively without reliable project context.

We will create:

```text
docs/
│
├── project/
│   └── product-overview.md
│
├── requirements/
│   ├── business-requirements.md
│   ├── epics.md
│   └── features/
│
├── architecture/
│   ├── system-architecture.md
│   ├── application-architecture.md
│   ├── data-architecture.md
│   └── integration-architecture.md
│
├── domain/
│   ├── conversation-model.md
│   ├── insight-model.md
│   └── taxonomy-model.md
│
├── adr/
│   ├── ADR-001-primary-language.md
│   ├── ADR-002-user-interface.md
│   ├── ADR-003-application-architecture.md
│   └── ...
│
└── development/
    ├── coding-standards.md
    ├── testing-strategy.md
    └── development-workflow.md
```

---

# 7.32 Product Overview Document

This is probably the most important context file after the Copilot instructions.

File:

```text
docs/project/product-overview.md
```

It should clearly state:

```text
WHAT WE ARE BUILDING

WHY WE ARE BUILDING IT

WHO WILL USE IT

WHAT DATA IT PROCESSES

WHAT INSIGHTS IT GENERATES

WHAT THE APPLICATION DOES

WHAT THE APPLICATION DOES NOT DO
```

---

## Our Project Summary

The application is:

> An enterprise banking conversation intelligence platform that processes customer and virtual-assistant conversation data and transforms it into structured, searchable, traceable business insights.

Primary capabilities:

```text
CONVERSATION INGESTION

↓

NORMALIZATION

↓

AI ANALYSIS

↓

CUSTOMER PROBLEM IDENTIFICATION

↓

SENTIMENT

↓

RESOLUTION

↓

ASSISTANT ACCURACY

↓

FAILURE ANALYSIS

↓

TAXONOMY CLASSIFICATION

↓

STRUCTURED INSIGHT

↓

SNOWFLAKE

↓

ANALYTICS

↓

STREAMLIT
```

---

# 7.33 Context Retrieval Strategy

This is critical to reducing unnecessary tokens.

The system should not load everything.

Instead:

### Example 1

Request:

> Add a dashboard filter.

Load:

```text
product-overview

dashboard requirements

Streamlit instructions

analytics service

Snowflake instructions
```

---

### Example 2

Request:

> Improve taxonomy classification.

Load:

```text
taxonomy requirements

taxonomy domain model

taxonomy skill

AI instructions

evaluation dataset
```

---

### Example 3

Request:

> Fix Snowflake connection failure.

Load:

```text
connection configuration

Snowflake infrastructure code

Snowflake instructions

relevant tests

error information
```

---

# 7.34 Context Selection Model

```text
USER REQUEST
       │
       ▼
TASK TYPE
       │
       ▼
REQUIRED CONTEXT
       │
       ├── PRODUCT
       ├── REQUIREMENT
       ├── ARCHITECTURE
       ├── DOMAIN
       ├── EXISTING CODE
       └── INSTRUCTIONS
              │
              ▼
       MINIMUM RELEVANT CONTEXT
```

This is a core part of your original requirement:

> Better accuracy without unnecessarily consuming tokens.

---

# 7.35 Custom Agents

We originally discussed conceptual specialist agents.

We can now formalize selected ones as actual Copilot custom agents where your available Copilot environment supports them.

Current Copilot documentation identifies custom agents as specialist personas with their own instructions, tools, and context. ([GitHub Docs][2])

Our recommended initial set is:

```text
ORCHESTRATOR

REQUIREMENTS

ARCHITECTURE

IMPLEMENTATION

TESTING

CODE REVIEW
```

I would **not create too many agents initially**.

Why?

Because:

```text
MORE AGENTS
≠
BETTER DEVELOPMENT
```

Too many agents can create:

```text
OVERLAP

CONFLICTING INSTRUCTIONS

EXTRA COMPLEXITY

MORE CONTEXT USAGE
```

Our system should therefore be:

```text
ONE PRIMARY ORCHESTRATOR

+
A SMALL NUMBER OF SPECIALIST AGENTS

+
SKILLS

+
PROMPT WORKFLOWS

+
PERSISTENT PROJECT CONTEXT
```

That is more practical than trying to create 20 artificial agents.

---

# 7.36 Final Agent Model

```text
                     USER

                      │

                      ▼

                ORCHESTRATOR

                      │

       ┌──────────────┼──────────────┐

       ▼              ▼              ▼

 REQUIREMENTS     ARCHITECTURE   IMPLEMENTATION

       │              │              │

       └──────────────┼──────────────┘

                      ▼

                   TESTING

                      │

                      ▼

                  REVIEW
```

Additional specialized skills activate when needed:

```text
AI FEATURE

TAXONOMY

SNOWFLAKE

STREAMLIT

SECURITY
```

This is the **final recommended ORC-style development system**.

---

# 7.37 What Step 7 Has Now Defined

We have now completed the design of:

### ✅ Repository-wide Copilot instructions

### ✅ ORC orchestration instructions

### ✅ Task classification model

### ✅ Python instructions

### ✅ Testing instructions

### ✅ AI instructions

### ✅ Snowflake instructions

### ✅ Streamlit instructions

### ✅ Security instructions

### ✅ Documentation instructions

### ✅ Requirement analysis workflow

### ✅ Feature planning workflow

### ✅ Implementation workflow

### ✅ Testing workflow

### ✅ Code review workflow

### ✅ Bug investigation workflow

### ✅ Architecture change workflow

### ✅ Feature development skill

### ✅ AI development skill

### ✅ Taxonomy skill

### ✅ Snowflake skill

### ✅ Streamlit skill

### ✅ Code review skill

### ✅ Project context architecture

### ✅ Context minimization strategy

### ✅ Custom-agent model

---

# STEP 8 — FINAL VALIDATION & APPLICATION DEVELOPMENT LAUNCH

The next step is the final transition.

In **Step 8**, we will define exactly:

## 1. How to set up the repository

## 2. How to install/configure the development environment

## 3. How to enable and verify Copilot customization

## 4. How to validate that our instruction system is working

## 5. How to run a test feature through the ORC workflow

## 6. How to create the first real application development request

## 7. How development proceeds from EPIC → Feature → Task → Code → Test

The workflow will look like:

```text
STEP 8
SYSTEM SETUP
     │
     ▼
VALIDATE COPILOT SYSTEM
     │
     ▼
RUN PILOT FEATURE
     │
     ▼
VERIFY ORCHESTRATION
     │
     ▼
VERIFY TESTING
     │
     ▼
VERIFY REVIEW
     │
     ▼
SYSTEM READY
     │
     ▼
START APPLICATION DEVELOPMENT
```

After that, the development system is no longer theoretical—we can begin building the actual banking conversation intelligence application using the framework we designed.

[1]: https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide?utm_source=chatgpt.com "Adding repository custom instructions for GitHub Copilot in your IDE - GitHub Docs"
[2]: https://docs.github.com/en/copilot/reference/customization-cheat-sheet?utm_source=chatgpt.com "Copilot customization cheat sheet - GitHub Docs"
[3]: https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills?utm_source=chatgpt.com "Adding agent skills for GitHub Copilot - GitHub Docs"
