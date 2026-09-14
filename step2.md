Absolutely. We should now formally start **Step 1: Product Requirements and MVP Scope**.

I’ll use everything we have established about your project so far and structure it as the foundation for the development system.

# Enterprise Banking Conversation Intelligence Platform

## Product Requirements Document (PRD) — Draft v1.0

---

# 1. Product Overview

## 1.1 Product Name

**Enterprise Banking Conversation Intelligence Platform**

### Future integrated capability

The platform will eventually include:

**Conversation Intelligence + Governed Natural Language Analytics (NLP-to-SQL)**

---

## 1.2 Product Vision

Build an enterprise-grade AI platform that transforms large volumes of banking customer conversations into **structured, actionable, trustworthy business intelligence**.

The platform should enable business users to understand:

* Why customers contact the bank
* What customers are trying to accomplish
* What problems customers experience
* Whether the virtual assistant understood them
* Whether the customer's problem was resolved
* Where the experience failed
* Why customers were redirected or escalated
* Which customer problems are increasing
* Which virtual assistant experiences need improvement

The platform should eventually allow authorized business users to ask questions directly in natural language and retrieve answers from governed enterprise data.

---

# 2. Business Problem

Banks generate very large volumes of customer interactions through:

* Virtual assistants
* Chatbots
* Digital banking applications
* Customer support journeys
* Digital help experiences

These interactions contain valuable insights, but most conversation data is difficult to analyse at scale.

## Current challenges

### Unstructured conversations

Conversation logs contain:

* Customer messages
* Bot messages
* Multiple turns
* Intent metadata
* Navigation events
* Handoff information
* Contextual metadata

However, raw conversation logs do not directly provide actionable business insights.

---

### Manual analysis does not scale

With potentially **150,000+ conversations per day**, manual review is not feasible.

Business teams cannot realistically read conversations individually to understand:

* Customer problems
* Failure patterns
* Resolution
* Customer sentiment
* Emerging issues

---

### Existing chatbot intent is not enough

The existing bot intent may indicate what the system thought the customer wanted.

It does **not necessarily represent**:

* The customer's actual problem
* Whether the bot understood correctly
* Whether the problem was resolved
* Whether the customer had to repeat themselves
* Whether the customer needed a human
* Whether the customer was frustrated

Therefore, the application must analyse the **entire conversation**, not simply reuse existing intent labels.

---

### Limited ability to identify emerging problems

Traditional dashboards require predefined:

* Metrics
* Categories
* Reports
* Queries

They are less effective when business users want to discover:

> What new problems are customers experiencing?

> Why are customers increasingly contacting us?

> What is causing unresolved conversations?

> Which experiences are generating customer frustration?

The platform should help identify both known and emerging patterns.

---

# 3. Product Objective

The primary objective is to build a system that:

### Processes complete banking conversations

Rather than analysing isolated messages.

### Understands customer intent and problems

Separating:

* What the customer wanted
* What problem the customer experienced
* What the bot recognized
* What actually happened

### Generates structured insights

Using controlled AI extraction and validation.

### Classifies conversations using a scalable taxonomy

Without placing hundreds or thousands of taxonomy categories into every LLM prompt.

### Enables business analysis

Through dashboards, filtering, exploration, and eventually natural-language questions.

### Provides enterprise-grade governance

Including:

* Versioning
* Traceability
* Evaluation
* Security
* Auditability
* Human review

---

# 4. Target Users

## 4.1 Primary Users

### Business and Digital Product Teams

They need to understand:

* Customer needs
* Customer problems
* Virtual assistant performance
* Digital journey friction

---

### Conversation and Virtual Assistant Teams

They need to identify:

* Intent failures
* Misunderstanding
* Unresolved conversations
* Missing capabilities
* Escalation patterns

---

### Customer Experience Teams

They need to understand:

* Customer sentiment
* Experience issues
* Recurring complaints
* Emerging pain points

---

### Analytics Teams

They need:

* Structured conversation data
* Reliable taxonomy
* Queryable insights
* Business metrics

---

### Data Science / AI Teams

They need:

* Evaluation datasets
* Model performance metrics
* Classification quality
* Feedback data

---

### Leadership

They need:

* Executive KPIs
* Trends
* Major customer problems
* Areas requiring investment

---

# 5. Core Product Capabilities

The platform will be developed in major capabilities.

---

# Capability 1 — Conversation Intelligence

This is the **recommended first MVP focus**.

The system receives a complete customer conversation and generates structured intelligence.

---

## 5.1 Conversation Summary

Generate a concise summary of the conversation.

Example:

> The customer attempted to locate a previous account statement but was unable to find the required document through the digital experience.

The summary must represent the actual conversation and should not introduce unsupported information.

---

## 5.2 Customer Goal

Identify:

> What was the customer trying to accomplish?

Examples:

* Check account balance
* Download statement
* Cancel card
* Replace card
* Understand transaction
* Report fraud
* Close account
* Speak with representative

---

## 5.3 Customer Problem

Identify:

> What actual problem, need, or issue did the customer experience?

This is one of the most important outputs.

Example:

**Customer goal:** Download statement

**Customer problem:** Unable to locate the required historical statement.

These should not be treated as the same field.

---

## 5.4 Customer Problem Summary

Generate a concise, business-readable description.

Example:

> Customer was unable to locate and download a historical account statement through the available digital journey.

This summary will later support:

* Taxonomy retrieval
* Business analysis
* Emerging problem detection
* Trend analysis

---

## 5.5 Sentiment Analysis

Determine overall conversation sentiment.

Initial categories:

* Positive
* Neutral
* Negative
* Mixed
* Unknown

The architecture should support sentiment progression in the future.

Example:

```text
Neutral
   ↓
Frustrated
   ↓
Negative
```

---

## 5.6 Resolution Analysis

Determine whether the customer's original need was resolved.

Initial outcomes should be evaluated and finalized during design.

Potential categories:

* Resolved
* Partially resolved
* Unresolved
* Redirected successfully
* Redirected unsuccessfully
* Human assistance required
* Insufficient information

The system must support uncertainty.

It should not force a confident resolution decision when evidence is weak.

---

## 5.7 Virtual Assistant Accuracy

Evaluate whether the assistant correctly understood and handled the customer's request.

Potential categories:

* Accurate
* Partially accurate
* Inaccurate
* Unable to determine

Signals may include:

* Customer correction
* Repeated questions
* Intent mismatch
* Incorrect information
* Incorrect navigation
* Explicit dissatisfaction
* Escalation

---

## 5.8 Failure Analysis

Identify why an interaction failed.

Examples:

* Intent not understood
* Incorrect answer
* Missing information
* Incorrect navigation
* Customer needed human assistance
* System limitation
* Authentication issue
* Journey limitation

The exact failure taxonomy should be designed separately from the customer problem taxonomy.

---

## 5.9 Handoff Analysis

Identify:

* Whether human assistance was requested
* Whether human assistance was required
* Why the handoff occurred
* Whether the handoff appeared successful

---

# 6. Conversation Intelligence Processing Flow

The target processing flow is:

```text
Raw Conversation
        ↓
Data Validation
        ↓
Conversation Normalization
        ↓
Conversation Reconstruction
        ↓
Complete Conversation Context
        ↓
AI Understanding
        ↓
Structured Insight Extraction
        ↓
Taxonomy Classification
        ↓
Confidence Assessment
        ↓
Validation
        ↓
Insight Storage
        ↓
Analytics
```

---

# 7. Input Data Requirements

The system must support flexible conversation inputs.

Potential fields include:

### Conversation Metadata

* Conversation ID
* Session ID
* Customer identifier or anonymized identifier
* Timestamp
* Channel
* Product
* Application page/context

### Conversation Messages

* Message ID
* Timestamp
* Speaker
* Customer message
* Bot response

### Bot Metadata

* Recognized intent
* Intent confidence
* Navigation action
* Handoff event

The data model must allow additional fields without requiring major redesign.

---

# 8. Conversation Normalization

Before AI processing, conversations must be converted into a standard internal structure.

The normalization layer should support:

* Message ordering
* Session reconstruction
* Speaker identification
* Timestamp normalization
* Metadata preservation
* Missing data handling
* Duplicate detection
* Data quality validation

The normalized conversation becomes the standard input to AI processing.

---

# 9. Structured Insight Output

Each processed conversation should generate structured output.

## Core fields

```text
conversation_id
processing_timestamp
processing_status
```

## Conversation understanding

```text
conversation_summary
customer_goal
customer_problem
customer_problem_summary
```

## Experience assessment

```text
sentiment
sentiment_confidence

resolution_status
resolution_confidence

assistant_accuracy
assistant_accuracy_confidence
```

## Taxonomy

```text
level_1_category
level_2_category
level_3_category

taxonomy_confidence
```

## Experience issues

```text
failure_reason
failure_type

handoff_required
handoff_reason

repeated_question_detected
customer_correction_detected
```

## Governance and traceability

```text
model_name
model_version

prompt_version

taxonomy_version

processing_run_id
```

The final schema will be refined during architecture and data-model design.

---

# 10. Taxonomy Requirements

A major requirement is a scalable hierarchical taxonomy.

The platform should support:

```text
Level 1
   ↓
Level 2
   ↓
Level 3
```

Example:

```text
Cards
   ↓
Card Management
   ↓
Cancel Card
```

---

## 10.1 Critical Design Principle

The application must **not** include hundreds or thousands of taxonomy categories directly inside every LLM prompt.

Instead, the system should use retrieval.

Target flow:

```text
Conversation
       ↓
Customer Problem Summary
       ↓
Embedding
       ↓
Semantic Retrieval
       ↓
Relevant Taxonomy Candidates
       ↓
LLM Classification
       ↓
Confidence Validation
       ↓
Final Category
```

---

## 10.2 Hierarchical Classification

Classification should occur progressively.

Example:

```text
Customer Problem
       ↓
Level 1 Candidates
       ↓
Select Level 1
       ↓
Relevant Level 2 Candidates
       ↓
Select Level 2
       ↓
Relevant Level 3 Candidates
       ↓
Select Level 3
```

This reduces:

* Token usage
* Prompt complexity
* Classification ambiguity

---

## 10.3 Unknown Categories

The taxonomy system must support:

```text
Unknown
Needs Review
Unclassified
```

The system should not force a conversation into an incorrect category simply because no suitable category exists.

---

# 11. Taxonomy Management

Future functionality should allow authorized users to manage taxonomy.

Each category may contain:

* Category ID
* Category name
* Level
* Parent category
* Description
* Synonyms
* Examples
* Status
* Version

The system should also support:

* Category updates
* Versioning
* Classification review
* Human correction

---

# 12. Human Review

The system must support human-in-the-loop workflows.

Examples requiring review:

* Low confidence classification
* Ambiguous conversation
* Unknown category
* Model disagreement
* New emerging customer problem

Human corrections should be captured as valuable evaluation data.

However:

> A single human correction should not automatically change production taxonomy or AI behavior.

Changes should remain governed.

---

# 13. Insight Storage

AI-generated insights should be stored in a structured, queryable form.

The system must support:

* Reprocessing
* Versioning
* Historical comparison
* Model changes
* Prompt changes
* Taxonomy changes

Important principle:

> Historical AI outputs should not simply be overwritten.

The system should preserve which version generated each result.

---

# 14. Analytics Dashboard

The first business application should provide a dashboard for exploring conversation insights.

---

## 14.1 Executive KPIs

Potential KPIs:

* Total conversations
* Resolution rate
* Unresolved rate
* Negative sentiment rate
* Human handoff rate
* Assistant accuracy

---

## 14.2 Customer Problems

Business users should be able to see:

* Top customer problems
* Problem trends
* Increasing problems
* Emerging issues

---

## 14.3 Virtual Assistant Performance

The dashboard should support:

* Intent volume
* Resolution by intent
* Accuracy by intent
* Failure patterns
* Escalation patterns

---

## 14.4 Conversation Explorer

Users should be able to filter conversations by:

* Date
* Product
* Intent
* Customer problem
* Sentiment
* Resolution
* Channel
* Customer segment

They should be able to inspect individual conversations and associated AI insights.

---

# 15. Capability 2 — Governed Natural Language Analytics

This capability should be developed after the Conversation Intelligence foundation is established.

The objective is to allow business users to ask questions naturally.

Examples:

> What were the top five customer problems last month?

> Which virtual assistant intents have the lowest resolution rate?

> Why are customers asking to speak with representatives?

> Which customer problems increased compared with last month?

---

# 16. NLP-to-SQL Requirements

The system should **not** directly send the complete database schema to an LLM and ask for SQL.

Instead:

```text
User Question
       ↓
Question Understanding
       ↓
Structured Business Request
       ↓
Semantic Retrieval
       ↓
Relevant Approved Metadata
       ↓
SQL Generation
       ↓
Deterministic Validation
       ↓
Authorization Validation
       ↓
Snowflake
       ↓
Result Validation
       ↓
Business Answer
```

---

# 17. Semantic Layer

The semantic layer will act as the controlled business representation of enterprise data.

It should define:

## Metrics

Examples:

* Conversation volume
* Resolution rate
* Handoff rate
* Negative sentiment rate
* Assistant accuracy

## Dimensions

Examples:

* Customer problem
* Intent
* Product
* Customer segment
* Channel
* Date

## Entities

Examples:

* Conversation
* Customer
* Product
* Account
* Card

Each item should contain:

* Business name
* Description
* Technical mapping
* Synonyms
* Examples
* Authorization requirements

---

# 18. SQL Safety

Generated SQL must be treated as **untrusted until validated**.

The system must validate:

### Statement type

Initially allow controlled analytical operations.

Primarily:

```sql
SELECT
```

Block operations such as:

```sql
INSERT
UPDATE
DELETE
DROP
ALTER
TRUNCATE
```

unless separately designed and authorized.

---

### Database objects

Validate:

* Tables
* Views
* Columns
* Schemas

against approved metadata.

---

### Joins

Only approved relationships should be allowed.

---

### Query controls

Apply:

* Row limits
* Timeouts
* Cost controls
* Complexity limits

---

### Authorization

The LLM must never determine authorization.

Authorization must be handled deterministically by the application and data platform.

---

# 19. Snowflake Integration

The NLP-to-SQL architecture should support Snowflake.

Key requirements:

* Secure authentication
* Role-based access
* Least privilege
* Secure views
* Query tagging
* Query history
* Timeout controls
* Cost monitoring

Credentials must never be exposed to the LLM.

---

# 20. Evaluation Framework

AI components must be formally evaluated.

---

## Conversation Intelligence Evaluation

Evaluate:

### Summary

* Accuracy
* Completeness
* Hallucination

### Customer problem extraction

* Correct problem identified

### Taxonomy

* Level 1 accuracy
* Level 2 accuracy
* Level 3 accuracy

### Resolution

* Correct outcome

### Assistant accuracy

* Correct experience assessment

### Sentiment

* Correct classification

---

## NLP-to-SQL Evaluation

Evaluate independently:

* Question understanding
* Metric selection
* Dimension selection
* Time interpretation
* Metadata retrieval
* SQL validity
* SQL correctness
* Result correctness
* Safety

Do not rely on one overall AI score.

---

# 21. Scale Requirements

The architecture should eventually support approximately:

**150,000+ conversations per day**

The processing architecture must support:

* Batch processing
* Parallel processing
* Controlled concurrency
* Rate limits
* Retry
* Checkpointing
* Idempotency
* Failure recovery

---

# 22. Security Requirements

Security is mandatory.

The platform must address:

* Authentication
* Authorization
* Role-based access
* Data privacy
* PII handling
* Sensitive data masking
* Secrets management
* Prompt injection
* SQL injection
* Generated SQL abuse
* Audit logging

Key principles:

> The LLM is not the security layer.

> Generated SQL is not trusted until deterministically validated.

> Sensitive information should not be unnecessarily exposed to AI models or logs.

---

# 23. Observability

The system should capture:

## Application metrics

* Requests
* Success
* Failure
* Latency

## AI metrics

* Model calls
* Token usage
* Cost
* Processing duration
* Structured output failures
* Confidence distribution

## SQL metrics

* Query success
* Query failure
* Execution duration
* Validation failure
* Cost

---

# 24. MVP Scope

Now the most important decision.

## MUST HAVE — MVP 1

The first working version should include:

### Conversation ingestion

* JSON input
* Conversation validation
* Normalization
* Conversation reconstruction

### AI conversation processing

* Conversation summary
* Customer goal
* Customer problem
* Customer problem summary
* Sentiment
* Resolution
* Assistant accuracy

### Taxonomy

* Three-level taxonomy
* Taxonomy storage
* Embeddings
* Candidate retrieval
* Hierarchical classification
* Confidence score
* Unknown classification

### Insight storage

Store:

* Raw conversation reference
* Generated insights
* Taxonomy
* Model version
* Prompt version

### Basic business application

Initial dashboard:

* Conversation volume
* Resolution
* Sentiment
* Top problems
* Taxonomy distribution

### Conversation explorer

* Search
* Filtering
* Individual conversation view
* AI-generated insight view

### Evaluation

Initial evaluation dataset.

Track:

* Taxonomy accuracy
* Problem extraction accuracy
* Resolution accuracy

---

# SHOULD HAVE — MVP 1.x

* Human review queue
* Taxonomy correction
* Sentiment progression
* Failure reason classification
* Emerging problem detection
* Customer segment analysis
* Improved dashboard
* Processing API
* Better observability

---

# FUTURE — MVP 2

## Governed NLP-to-SQL

* Semantic layer
* Natural language question understanding
* Metadata retrieval
* SQL generation
* SQL validation
* Snowflake execution
* Business answer generation

This should be developed as a **separate major capability**, while sharing common enterprise platform components.

---

# 25. Explicitly Out of Scope for Initial MVP

To keep the project achievable, the first MVP should not initially include:

* Fully autonomous agents
* Self-modifying AI systems
* Automatic model retraining
* Automatic taxonomy changes
* Unrestricted database access
* Arbitrary SQL generation
* Real-time streaming
* Multiple UI applications
* Complex microservice deployment
* Kubernetes-first architecture

---

# 26. MVP Success Criteria

The MVP will be considered successful when it can take a set of banking conversations and reliably:

### Process conversations

✔ Load conversations
✔ Normalize them
✔ Preserve complete context

### Generate insights

✔ Summarize conversation
✔ Identify customer goal
✔ Identify customer problem
✔ Determine sentiment
✔ Assess resolution
✔ Assess assistant accuracy

### Classify conversations

✔ Retrieve relevant taxonomy candidates
✔ Assign Level 1
✔ Assign Level 2
✔ Assign Level 3
✔ Handle unknown cases

### Store results

✔ Persist structured insights
✔ Track model version
✔ Track prompt version
✔ Support reprocessing

### Provide business access

✔ Show key KPIs
✔ Show top customer problems
✔ Allow filtering
✔ Allow conversation exploration

---

# 27. Recommended Development Sequence

Here is the sequence I recommend we now follow:

## Stage A — Requirements

**We are here now.**

Deliverables:

* PRD
* MVP scope
* Use cases
* Acceptance criteria

---

## Stage B — Architecture

Next:

* System architecture
* Component design
* Data flow
* AI pipeline
* Taxonomy architecture
* Storage design
* Security architecture

---

## Stage C — Data Model

Define:

```text
Conversation
Message
Conversation Insight
Taxonomy
Processing Run
Model Version
Prompt Version
Evaluation Result
```

---

## Stage D — Implementation Plan

Break into:

```text
Epic
   ↓
Feature
   ↓
Component
   ↓
Task
```

Define dependencies and parallel work.

---

## Stage E — Repository Setup

Create the actual application repository structure based on the architecture.

---

## Stage F — Foundation Implementation

Build:

* Configuration
* Logging
* Domain models
* Data contracts
* LLM abstraction
* Embedding abstraction
* Testing

---

## Stage G — Conversation Intelligence MVP

Build the actual processing pipeline.

---

# My recommendation for our immediate next step

We have now completed the **first draft of the PRD and MVP Scope**.

## Next, we should create:

### **Step 2 — Detailed System Architecture**

This should include:

1. Overall architecture diagram
2. Application components
3. Data flow
4. Conversation processing pipeline
5. LLM processing architecture
6. Taxonomy retrieval architecture
7. Vector/embedding architecture
8. Data storage architecture
9. Evaluation architecture
10. Security architecture
11. Observability architecture
12. Deployment approach
13. Technology decisions
14. Repository structure

After that, we can create the **implementation roadmap and task DAG**, and then hand the first real development tasks to the development system we designed.

**My recommendation: we proceed next with the detailed System Architecture and Architecture Decision Records.**
