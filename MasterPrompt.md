# MASTER APPLICATION DEVELOPMENT PROMPT

## 1. PROJECT INITIATION

We have completed the setup of the Enterprise AI Development System.

The development environment includes:

- An Orchestrator agent
- Requirements and planning agents
- Architecture agent
- Developer and debugger agents
- AI/LLM engineering agent
- Data engineering agent
- SQL engineering agent
- Snowflake engineering agent
- Semantic layer engineering agent
- AI evaluation agent
- Security agent
- QA agent
- Independent reviewer
- Documentation agent
- Reusable skills
- Planning artifacts
- Architecture decision records
- Validation and review workflows
- Stage 8 synthetic validation framework

We are now starting development of the actual application.

This prompt defines the **business problem, application vision, architecture direction, functional scope, development approach, quality requirements, and expected deliverables**.

The development system must follow its mandatory lifecycle:

**Request → Requirements → Acceptance Criteria → Architecture → Implementation Plan → Task DAG → Implementation → Testing → Security → Review → Documentation → Completion**

Do not jump directly into coding.

---

# 2. APPLICATION NAME

The working application name is:

# Enterprise Banking Conversation Intelligence and NLP-to-SQL Platform

The final application may contain two connected but independently deployable capabilities:

### Capability A
**Conversation Intelligence Platform**

Analyse banking customer conversations with a Virtual Assistant or chatbot and transform large volumes of conversations into structured, actionable business intelligence.

### Capability B
**Natural Language to Business Data Platform**

Allow authorized business users to ask questions in natural language and safely retrieve answers from governed enterprise data using a semantic layer and controlled SQL generation.

The architecture should allow both capabilities to share common platform services where appropriate, while keeping their business logic independently maintainable.

---

# 3. PRIMARY BUSINESS PROBLEM

Enterprise banking organizations generate large volumes of customer interactions through:

- Virtual assistants
- Chatbots
- Digital banking applications
- Customer support conversations
- Help and support journeys

These conversations contain valuable information about:

- Why customers contacted the bank
- What problems customers experienced
- What they were trying to accomplish
- Whether the virtual assistant understood them
- Whether the assistant resolved the issue
- Whether the customer was redirected
- Whether the customer needed a human representative
- Where experiences failed
- What intents generate repeated friction
- Which customer problems are increasing
- Which product or service journeys need improvement

Today, much of this information is difficult to analyse at scale.

Raw conversation logs are usually:

- Large
- Unstructured
- Inconsistent
- Spread across multiple systems
- Difficult for business users to explore
- Difficult to convert into actionable insights

Traditional dashboards require predefined metrics and cannot easily answer new questions.

Manual analysis does not scale.

The application we are building should solve this problem.

---

# 4. APPLICATION VISION

Build an enterprise-grade AI-powered analytics platform that can:

1. Process large volumes of banking customer conversations.

2. Understand complete conversation threads rather than isolated messages.

3. Generate structured AI insights from each conversation.

4. Categorize conversations using a scalable business taxonomy.

5. Identify customer problems and emerging themes.

6. Measure experience quality.

7. Measure virtual assistant performance.

8. Identify resolution and failure patterns.

9. Detect situations where customers need human assistance.

10. Store AI-generated structured insights for analytics.

11. Provide an intuitive business-user application for exploring insights.

12. Allow business users to ask questions using natural language.

13. Convert approved business questions into safe analytical queries.

14. Use a semantic layer to prevent uncontrolled access to raw database structures.

15. Generate SQL only through governed and validated processes.

16. Execute queries against enterprise data sources such as Snowflake.

17. Return business-readable answers with supporting evidence.

The system must be designed for enterprise scale, governance, security, explainability, maintainability, and continuous improvement.

---

# 5. HIGH-LEVEL APPLICATION ARCHITECTURE

The platform should be designed as a modular architecture.

The initial architecture should contain the following major layers.

## Layer 1 — Data Sources

Potential data sources include:

- Banking chatbot conversation logs
- Virtual assistant interaction logs
- Intent recognition logs
- Customer interaction metadata
- Intent metadata
- Product metadata
- Existing business taxonomy
- Enterprise analytical databases
- Snowflake
- Other governed enterprise data sources

The architecture must support multiple source systems over time.

---

# 6. CAPABILITY A — CONVERSATION INTELLIGENCE

## 6.1 Conversation Ingestion

The system must ingest conversation data.

Initial input sources may include:

- JSON files
- CSV files
- Database tables
- Data lake files
- APIs
- Streaming sources in the future

Each conversation may contain:

- Conversation ID
- Customer or anonymized customer identifier
- Session ID
- Timestamp
- Customer message
- Bot response
- Recognized intent
- Confidence score
- Channel
- Product
- Page or application context
- Navigation action
- Handoff information
- Other available metadata

The ingestion layer must be designed so additional fields can be added without redesigning the entire application.

The system should maintain clear data contracts.

---

## 6.2 Conversation Normalization

Raw conversations should be transformed into a consistent internal format.

The system must support:

- Message ordering
- Conversation reconstruction
- Session grouping
- Customer and bot message identification
- Metadata preservation
- Timestamp normalization
- Missing field handling
- Duplicate detection where applicable
- Data quality validation

The system should not lose useful context while transforming raw data.

The normalized representation should become the standard input to downstream AI processing.

---

## 6.3 Conversation-Level Understanding

The AI system must analyse the **complete conversation thread**.

It should not make conclusions solely from individual messages unless the conversation contains only one message.

The system should understand:

### Customer goal

What was the customer trying to do?

Examples:

- Check account balance
- Download a statement
- Understand a charge
- Cancel a card
- Replace a card
- Report fraud
- Close an account
- Speak with a representative

---

### Customer problem

What actual issue or need did the customer have?

Examples:

- Could not access a statement
- Card was lost
- Customer did not understand a transaction
- Virtual assistant misunderstood the request
- Customer could not complete an action
- Customer wanted a human representative

The system should generate a concise, business-readable problem summary.

Example:

> Customer was attempting to obtain a previous account statement but could not locate the correct document through the digital experience.

---

### Conversation outcome

Determine what happened.

Possible outcomes may include:

- Fully resolved
- Partially resolved
- Redirected successfully
- Redirected unsuccessfully
- Customer abandoned
- Human handoff required
- Human handoff completed
- Unresolved
- Insufficient information

The final taxonomy should be designed carefully rather than assuming these exact labels are final.

---

### Resolution assessment

The system should determine whether the customer's original need was successfully addressed.

Potential structured fields:

- Resolution status
- Resolution confidence
- Resolution reason
- Evidence from conversation

The architecture must allow uncertain cases.

The system must not force every conversation into a confident resolution decision.

---

### Assistant accuracy

The system should evaluate whether the virtual assistant correctly understood and responded to the customer.

Potential categories:

- Accurate
- Partially accurate
- Inaccurate
- Unable to determine

Possible signals include:

- Intent mismatch
- Customer correction
- Repeated question
- Explicit dissatisfaction
- Incorrect navigation
- Incorrect information
- Repeated failure
- Conversation abandonment

---

### Customer sentiment

Determine sentiment at a conversation level.

Potential categories:

- Positive
- Neutral
- Negative
- Mixed
- Unknown

Where appropriate, the system should also identify sentiment progression.

Example:

> Neutral → Frustrated → Negative

Do not oversimplify sentiment into a single label when the conversation demonstrates a clear change.

---

## 6.4 Structured Insight Extraction

For each conversation, the system should generate structured insight fields.

The initial design should consider the following.

### Core fields

```text
conversation_id
processing_timestamp
model_version
prompt_version
```

### Conversation understanding

```text
conversation_summary
customer_goal
customer_problem
customer_problem_summary
```

### Experience analysis

```text
sentiment
sentiment_confidence
resolution_status
resolution_confidence
assistant_accuracy
assistant_accuracy_confidence
```

### Taxonomy

```text
level_1_category
level_2_category
level_3_category
taxonomy_confidence
```

### Journey information

```text
identified_intent
recognized_bot_intent
product
customer_journey
application_context
```

### Experience issues

```text
failure_reason
failure_type
handoff_required
handoff_reason
repeated_question_detected
customer_correction_detected
```

### Evidence

```text
supporting_evidence
key_customer_quotes
key_bot_responses
```

### AI governance

```text
model_name
model_version
prompt_version
taxonomy_version
processing_status
processing_error
```

The exact schema should be finalized during the architecture stage.

All AI-generated structured outputs should be validated against explicit schemas.

---

# 7. SCALABLE TAXONOMY ARCHITECTURE

A major requirement is scalable classification.

The system should support a hierarchy such as:

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

Another example:

```text
Accounts
    ↓
Account Services
    ↓
Close Account
```

However, the taxonomy must not require placing hundreds or thousands of possible categories into every LLM prompt.

The system should use a scalable retrieval-based approach.

Recommended conceptual flow:

```text
Conversation
        ↓
Customer Problem Summary
        ↓
Embedding Generation
        ↓
Semantic Similarity Search
        ↓
Retrieve Relevant Level 1 Candidates
        ↓
Retrieve Relevant Level 2 Candidates
        ↓
Retrieve Relevant Level 3 Candidates
        ↓
LLM Classification Among Candidates
        ↓
Validation
        ↓
Final Taxonomy Assignment
```

The architecture should support:

- Category descriptions
- Synonyms
- Examples
- Parent-child relationships
- Embeddings
- Similarity scores
- Candidate retrieval
- Confidence scores
- Unknown or uncategorized outcomes
- Human review queues
- Taxonomy versioning

---

# 8. TAXONOMY MANAGEMENT

The application should eventually provide taxonomy management capabilities.

Potential features:

### Taxonomy explorer

Allow users to browse:

```text
Level 1
    └── Level 2
          └── Level 3
```

---

### Category definitions

Each category should support:

- Category name
- Description
- Examples
- Synonyms
- Parent category
- Version
- Status
- Created date
- Updated date

---

### Classification review

Allow analysts to review:

- Conversation
- AI-generated summary
- Assigned category
- Confidence
- Candidate categories
- Alternative classifications

---

### Human correction

Authorized users should be able to correct classification.

These corrections should be captured for:

- Taxonomy improvement
- Evaluation datasets
- Future model improvement
- Retrieval improvement

Do not automatically retrain or change taxonomy behavior from a single correction.

Changes should be governed.

---

# 9. CAPABILITY B — NATURAL LANGUAGE TO BUSINESS DATA

The second major capability is a governed NLP-to-SQL system.

The objective is to allow authorized business users to ask questions such as:

> How many customers contacted us about card cancellation last month?

> Which customer problems increased the most compared with the previous month?

> What percentage of conversations were unresolved?

> Show the top five reasons customers asked to speak with a representative.

> Which virtual assistant intents have the lowest resolution rate?

The application should understand the question and retrieve the correct business information.

The system must not simply send the entire database schema to an LLM and ask it to write SQL.

The architecture must be controlled.

---

# 10. NLP-TO-SQL ARCHITECTURE

The expected architecture should follow this logical flow.

```text
Business User Question
        ↓
Authentication and Authorization
        ↓
Question Understanding
        ↓
Business Intent Identification
        ↓
Semantic Retrieval
        ↓
Relevant Metadata Selection
        ↓
Business Metric Resolution
        ↓
Entity Resolution
        ↓
Time Period Resolution
        ↓
SQL Generation
        ↓
Deterministic SQL Validation
        ↓
Security and Policy Validation
        ↓
Query Cost and Safety Controls
        ↓
Snowflake Execution
        ↓
Result Validation
        ↓
Business Interpretation
        ↓
Natural Language Response
```

Every stage should be independently observable.

---

# 11. QUESTION UNDERSTANDING

The system should understand questions in terms of structured business meaning.

For example:

Question:

> What were the top five customer problems last month?

The system should extract:

```text
Business Objective:
Rank customer problems

Metric:
Conversation volume

Dimension:
Customer problem

Time Period:
Last month

Ranking:
Top 5

Sorting:
Descending
```

The output should be represented using a structured schema.

The application should not immediately generate SQL.

---

# 12. SEMANTIC LAYER

The semantic layer is a critical component.

The application should maintain business-friendly metadata rather than relying on raw database names.

Example:

```text
Business Metric:
Resolution Rate

Definition:
Percentage of conversations where the customer's primary problem was successfully resolved.

Calculation:
Resolved Conversations / Total Eligible Conversations
```

The semantic layer should contain metadata for:

### Metrics

Examples:

- Conversation volume
- Resolution rate
- Unresolved rate
- Handoff rate
- Negative sentiment rate
- Assistant accuracy
- Repeat question rate

---

### Dimensions

Examples:

- Customer problem
- Product
- Intent
- Customer segment
- Channel
- Date
- Month
- Geography
- Journey

---

### Entities

Examples:

- Conversation
- Customer
- Account
- Product
- Card
- Transaction

---

### Business definitions

Each metric or dimension should include:

- Business name
- Description
- Technical representation
- Source table
- Source column
- Grain
- Synonyms
- Example questions
- Authorization requirements

---

# 13. SEMANTIC RETRIEVAL

The system should retrieve relevant metadata dynamically.

It should not place every:

- Table
- Column
- Metric
- Taxonomy value
- Business definition

into every prompt.

The system should use:

- Embeddings
- Vector search
- Keyword search where appropriate
- Hybrid retrieval
- Metadata filtering
- Business domain filtering

Example:

```text
Question
    ↓
Embedding
    ↓
Retrieve Candidate Metrics
    ↓
Retrieve Candidate Dimensions
    ↓
Retrieve Candidate Tables
    ↓
Retrieve Candidate Relationships
    ↓
LLM Selects Approved Candidates
```

The final SQL generation step should only receive the minimum relevant metadata.

---

# 14. SQL GENERATION

SQL generation must be constrained.

The model should receive:

- The structured user question
- Approved business metrics
- Approved dimensions
- Approved entities
- Approved tables
- Approved columns
- Approved joins
- Approved filters
- Database dialect
- SQL policies

The model should not be allowed to invent arbitrary:

- Tables
- Columns
- Joins
- Databases
- Schemas

Generated SQL should be structured and traceable.

The system should capture:

```text
generated_sql
model_version
prompt_version
semantic_metadata_used
candidate_metadata
generation_timestamp
```

---

# 15. SQL SAFETY AND VALIDATION

Generated SQL must never be treated as automatically safe.

Before execution, deterministic validation should check:

### Statement type

Initially allow only approved analytical operations.

Prefer:

```sql
SELECT
```

Restrict or block:

```sql
INSERT
UPDATE
DELETE
DROP
ALTER
TRUNCATE
CREATE
GRANT
REVOKE
```

unless explicitly required and separately governed.

---

### Object validation

Verify:

- Database
- Schema
- Table
- View
- Column

against approved metadata.

---

### Join validation

Verify joins against approved relationships.

Prevent arbitrary joins.

---

### Data access validation

Verify:

- User authorization
- Role
- Allowed data domain
- Restricted columns
- Sensitive information

---

### Query complexity

Check:

- Excessive joins
- Cartesian joins
- Missing filters
- Full scans
- Excessive cost risk

---

### Query limits

Apply:

- Maximum execution time
- Maximum returned rows
- Query timeout
- Warehouse controls
- Cost controls

---

# 16. SNOWFLAKE INTEGRATION

The initial enterprise analytical database target is Snowflake.

The architecture should support:

- Secure authentication
- Role-based access
- Least privilege
- Warehouse selection
- Query tagging
- Query history
- Cost monitoring
- Timeout controls
- Secure views
- Row-level controls where applicable
- Column-level protection where applicable

The application should not expose credentials to the LLM.

The application should not allow the LLM to decide authorization.

Authorization must be deterministic and controlled by the application and data platform.

---

# 17. BUSINESS RESPONSE GENERATION

After a query executes successfully, the system should transform results into a business-friendly response.

Example:

User asks:

> Which customer problem increased the most last month?

The response should provide:

### Direct answer

> Card cancellation requests increased the most, rising by 18% compared with the previous month.

### Supporting information

```text
Current Month: 12,500 conversations
Previous Month: 10,600 conversations
Increase: 1,900
Percentage Increase: 18%
```

### Optional explanation

> The increase was primarily associated with customers attempting to cancel cards after reporting loss or replacement issues.

The response must distinguish:

- Facts directly returned by data
- Calculated results
- AI-generated interpretation

The system should not invent business explanations that are not supported by data.

---

# 18. RESULT VALIDATION

After SQL execution, validate:

- Result schema
- Expected metric types
- Null values
- Empty result sets
- Unexpected row counts
- Calculation consistency
- Data freshness where available

The AI response should be generated only after results are validated.

If the query returns no data, the system should clearly explain that.

It should not hallucinate an answer.

---

# 19. EXPLAINABILITY AND TRACEABILITY

Every important answer should be traceable.

The system should maintain an execution record.

Example:

```text
User Question
    ↓
Question Interpretation
    ↓
Metrics Identified
    ↓
Dimensions Identified
    ↓
Semantic Metadata Retrieved
    ↓
SQL Generated
    ↓
Validation Result
    ↓
Query Executed
    ↓
Result Summary
```

Depending on user permissions, users should eventually be able to view:

- How the question was interpreted
- Metrics used
- Filters used
- Time period interpreted
- SQL generated
- Data source
- Query execution status

The system should support enterprise auditability.

---

# 20. USER INTERFACE

The initial user interface can be built using Streamlit for rapid development.

However, the application architecture must avoid tightly coupling business logic to Streamlit.

The UI should be separated from:

- AI services
- Data services
- Taxonomy services
- SQL services
- Semantic retrieval services

The interface should eventually support:

## Conversation Intelligence Dashboard

Potential sections:

### Executive Overview

KPIs such as:

- Total conversations
- Resolution rate
- Unresolved rate
- Negative sentiment
- Human handoff rate
- Assistant accuracy

---

### Customer Problems

Show:

- Top problems
- Problem trends
- Emerging problems
- Problem growth

---

### Virtual Assistant Performance

Show:

- Intent volume
- Intent accuracy
- Resolution
- Failure patterns
- Escalations
- Navigation issues

---

### Conversation Explorer

Allow filtering by:

- Date
- Intent
- Product
- Customer problem
- Sentiment
- Resolution
- Customer segment
- Channel

Allow users to inspect individual conversations.

Sensitive data must be protected or masked.

---

### AI Insight Explorer

Allow users to search:

- What are customers complaining about?
- Why are customers escalating?
- Which problems are unresolved?
- What issues are increasing?

---

# 21. NATURAL LANGUAGE ANALYTICS INTERFACE

The application should also provide a natural language query interface.

Example:

```text
Ask a question about customer conversations or business data.
```

The interface should display:

### User question

### Interpreted question

### Applied filters

### Data source

### Result

### Optional SQL

SQL visibility should be permission-controlled.

---

# 22. DATA MODEL

The architecture should design a scalable data model.

Potential logical entities:

```text
Conversation
Conversation_Message
Conversation_Insight
Taxonomy_Category
Taxonomy_Version
Intent_Metadata
Semantic_Metric
Semantic_Dimension
Semantic_Entity
Semantic_Relationship
Prompt_Version
Model_Version
Processing_Run
Query_Request
Query_Execution
Evaluation_Case
Evaluation_Result
Human_Review
Feedback
```

The data model should support:

- Versioning
- Auditability
- Reprocessing
- Model changes
- Prompt changes
- Taxonomy changes

Avoid overwriting historical AI decisions without preserving version history.

---

# 23. MODEL AND PROMPT MANAGEMENT

AI behavior must be versioned.

Track:

```text
model_name
model_version
prompt_name
prompt_version
taxonomy_version
semantic_layer_version
processing_version
```

The application must support reproducibility.

We should be able to answer:

> Which model and prompt produced this insight?

> Why did this classification change?

> Which semantic metadata was used to generate this SQL?

---

# 24. AI EVALUATION FRAMEWORK

AI components must not be evaluated only through manual testing.

Create a formal evaluation framework.

---

## Conversation Intelligence Evaluation

Evaluate:

### Customer problem extraction

Did the system correctly identify the customer's actual problem?

### Conversation summary

Is the summary accurate and complete?

### Taxonomy classification

Did the system assign the correct:

- Level 1
- Level 2
- Level 3

### Resolution

Did the system correctly determine the outcome?

### Assistant accuracy

Did the system correctly identify whether the assistant handled the request properly?

### Sentiment

Is sentiment correctly identified?

---

## NLP-to-SQL Evaluation

Evaluate separately:

### Question understanding

### Metric identification

### Dimension identification

### Time period interpretation

### Semantic retrieval

### Metadata selection

### SQL validity

### SQL correctness

### Execution success

### Answer correctness

### Safety

### Authorization

### Hallucination

Do not rely on one overall score.

Each stage should have measurable metrics.

---

# 25. HUMAN-IN-THE-LOOP CAPABILITY

The architecture should support human review.

Examples:

### Low confidence taxonomy

Route to:

```text
Needs Review
```

### Ambiguous question

Ask the user for clarification.

Example:

> Do you mean card cancellation requests or completed card cancellations?

### Low confidence AI insight

Flag the result.

### Failed SQL

Do not automatically invent a different answer.

Provide:

- Error handling
- Retry logic where appropriate
- Clarification
- Controlled fallback

Human feedback should be captured as evaluation data.

---

# 26. PERFORMANCE AND SCALE

The application must be designed for enterprise scale.

Conversation processing may eventually involve:

```text
150,000+ conversations per day
```

The architecture should support:

- Batch processing
- Parallel processing
- Controlled concurrency
- Rate limits
- Retry mechanisms
- Checkpointing
- Failure recovery
- Idempotency
- Incremental processing
- Queue-based processing if required

Avoid designs that assume small datasets.

---

# 27. COST MANAGEMENT

AI processing costs must be measurable.

Track:

- Input tokens
- Output tokens
- Model
- Cost estimate
- Processing duration
- Conversations processed
- Cost per conversation
- Cost per successful insight

For NLP-to-SQL track:

- Model calls
- Tokens
- Query execution cost where available
- Warehouse usage
- Failed query cost

The architecture should support future cost optimization.

---

# 28. OBSERVABILITY

Implement structured observability.

Track:

### Application metrics

- Request volume
- Processing volume
- Success rate
- Failure rate
- Latency

### AI metrics

- Model latency
- Token usage
- Structured output failures
- Retry rate
- Confidence distribution

### SQL metrics

- Query success
- Query failure
- Validation failures
- Execution duration
- Query cost

### Business metrics

- Resolution rate
- Assistant accuracy
- Top customer problems
- Taxonomy distribution

Logging must avoid unnecessary sensitive customer information.

---

# 29. SECURITY REQUIREMENTS

Security is mandatory.

The system must address:

- Authentication
- Authorization
- Role-based access
- Tenant or customer isolation where applicable
- Data privacy
- Sensitive data masking
- Secrets management
- Prompt injection
- SQL injection
- Generated SQL abuse
- Unauthorized data access
- Logging controls
- Audit trails

Important principle:

# The LLM is never the authorization layer.

Important principle:

# LLM instructions are not security controls.

Important principle:

# Generated SQL is untrusted until deterministic validation.

---

# 30. DEVELOPMENT ARCHITECTURE

The application should use clear separation of responsibilities.

A recommended high-level code structure should be evaluated during architecture planning.

Conceptually:

```text
application/
│
├── api/
│
├── ui/
│
├── services/
│
│   ├── conversation_service/
│   ├── insight_service/
│   ├── taxonomy_service/
│   ├── semantic_service/
│   ├── question_understanding/
│   ├── sql_generation/
│   ├── sql_validation/
│   └── result_interpretation/
│
├── domain/
│
├── repositories/
│
├── integrations/
│
│   ├── llm/
│   ├── embeddings/
│   ├── vector_store/
│   └── snowflake/
│
├── evaluation/
│
├── security/
│
├── observability/
│
├── configuration/
│
└── tests/
```

The architecture agent should refine this structure based on actual technology choices.

Do not blindly implement this structure if a better repository design exists.

---

# 31. INITIAL TECHNOLOGY DIRECTION

The development system should evaluate and recommend the final technology choices.

Initial direction:

### Programming language

Python

### Application framework

Streamlit for the initial business application interface.

Potential future API layer:

- FastAPI or equivalent

### LLM integration

Provider abstraction.

The application should avoid being permanently coupled to one model provider.

Possible providers may include:

- OpenAI
- Azure OpenAI
- Google Gemini
- Enterprise-approved models

### Embeddings

Use a provider abstraction.

### Vector retrieval

Choose an appropriate enterprise-compatible vector storage/retrieval mechanism.

### Database

Enterprise analytical data source:

- Snowflake

Application metadata storage should be selected based on requirements.

---

# 32. DEVELOPMENT PHASES

Do not attempt to build the entire platform at once.

The application should be developed in controlled phases.

---

# PHASE 1 — FOUNDATION

Build:

- Repository structure
- Configuration
- Environment management
- Logging
- Error handling
- Data contracts
- Core domain models
- AI provider abstraction
- Embedding abstraction
- Basic persistence
- Testing framework
- Security baseline
- Observability baseline

Deliverable:

A clean application foundation.

---

# PHASE 2 — CONVERSATION INGESTION

Build:

- Input adapters
- Conversation loading
- Validation
- Normalization
- Conversation reconstruction
- Data quality checks

Start with:

```text
JSON
```

Then design adapters for additional sources.

Deliverable:

Normalized conversations available for processing.

---

# PHASE 3 — CONVERSATION AI PROCESSING

Build:

- Conversation summarization
- Customer goal extraction
- Customer problem extraction
- Sentiment
- Resolution
- Assistant accuracy
- Structured output validation

Deliverable:

Structured AI insights for conversations.

---

# PHASE 4 — TAXONOMY AND SEMANTIC CLASSIFICATION

Build:

- Taxonomy storage
- Taxonomy hierarchy
- Category definitions
- Embeddings
- Candidate retrieval
- Hierarchical classification
- Confidence handling
- Unknown classification
- Human review capability

Deliverable:

Scalable taxonomy classification.

---

# PHASE 5 — INSIGHT STORAGE AND ANALYTICS

Build:

- Insight persistence
- Version tracking
- Processing runs
- Queryable analytical model
- Aggregations

Deliverable:

Processed conversation insights available for analysis.

---

# PHASE 6 — BUSINESS DASHBOARD

Build:

- KPI dashboard
- Filters
- Customer problem analysis
- Trend analysis
- Virtual assistant performance
- Conversation explorer

Deliverable:

Business users can explore insights without SQL.

---

# PHASE 7 — SEMANTIC LAYER

Build:

- Business metrics
- Dimensions
- Entities
- Definitions
- Synonyms
- Table mappings
- Column mappings
- Relationships
- Semantic retrieval

Deliverable:

A governed business semantic layer.

---

# PHASE 8 — NLP QUESTION UNDERSTANDING

Build:

- Question parsing
- Intent detection
- Metric identification
- Dimension identification
- Time interpretation
- Ambiguity detection
- Structured question representation

Deliverable:

Natural language questions converted into structured business requests.

---

# PHASE 9 — GOVERNED NLP-TO-SQL

Build:

- Metadata retrieval
- SQL generation
- SQL schema validation
- Object validation
- Join validation
- Security validation
- Query cost controls
- Snowflake execution

Deliverable:

Safe end-to-end analytical question execution.

---

# PHASE 10 — BUSINESS ANSWER GENERATION

Build:

- Result interpretation
- Business response generation
- Evidence display
- Explanation
- Empty result handling
- Error handling
- Optional SQL transparency

Deliverable:

Business-friendly answers grounded in query results.

---

# PHASE 11 — EVALUATION AND GOVERNANCE

Build:

- Evaluation datasets
- Regression tests
- Prompt version tracking
- Model version tracking
- AI metrics
- SQL evaluation
- Human review workflow

Deliverable:

Production-ready AI evaluation and governance.

---

# 33. DEVELOPMENT RULES

The development system must follow these rules.

## Do not over-engineer the first release.

Build the architecture for scalability but implement the MVP incrementally.

## Do not create unnecessary agents.

The runtime application should use orchestration only where it provides measurable value.

## Prefer deterministic controls.

Use:

- Schema validation
- Database metadata validation
- Policy engines
- Explicit authorization
- SQL parsing
- Structured outputs

Do not rely solely on LLM reasoning.

## Preserve modularity.

AI providers, embeddings, vector stores, databases, and UI layers should be replaceable where practical.

## Test every major component.

## Maintain traceability.

## Version AI behavior.

## Protect sensitive data.

## Document decisions.

---

# 34. WHAT NOT TO BUILD INITIALLY

Do not include the following unless justified by a real requirement:

- Fully autonomous self-modifying agents
- Automatic production database schema discovery by unrestricted LLM access
- Automatic model retraining
- Automatic taxonomy modification
- Unrestricted SQL execution
- Production customer data in prompts
- Complex microservice architecture without need
- Excessive framework dependencies
- Premature Kubernetes deployment architecture
- Multiple vector databases
- Multiple orchestration frameworks

Start with the simplest architecture that satisfies enterprise requirements.

---

# 35. REQUIRED DEVELOPMENT ARTIFACTS

Before implementation begins, create a work item containing:

```text
request.md
requirements.md
acceptance-criteria.md
architecture-impact.md
implementation-plan.md
task-dag.yaml
risk-register.md
```

Before completion:

```text
validation-report.md
security-report.md
review-report.md
completion-summary.md
```

Material architectural decisions should create ADRs.

---

# 36. FIRST DEVELOPMENT TASK

The development system should now begin with the following process.

## Step 1

Analyse this entire application definition.

Identify:

- Explicit requirements
- Implicit requirements
- Assumptions
- Ambiguities
- Constraints
- Risks
- Dependencies
- Out-of-scope items

Do not start coding.

---

## Step 2

Produce a complete:

```text
Product Requirements Document
```

for the MVP.

---

## Step 3

Define the MVP boundary.

Separate:

```text
Must Have
Should Have
Future
```

The first version should be achievable and should establish the correct architecture.

---

## Step 4

Design the complete system architecture.

Include:

- Component architecture
- Data flow
- AI processing flow
- Taxonomy flow
- Semantic layer flow
- NLP-to-SQL flow
- Security boundaries
- Storage architecture
- Integration points
- Failure handling
- Observability

---

## Step 5

Create an architecture decision record for major decisions.

---

## Step 6

Create the complete implementation roadmap.

Break development into:

- Epics
- Features
- Components
- Tasks
- Dependencies

Identify which tasks can run in parallel.

---

## Step 7

Create the initial repository architecture.

Do not build unnecessary features.

---

## Step 8

Begin implementation only after:

- Requirements are approved
- MVP scope is defined
- Architecture is documented
- Acceptance criteria exist
- Task dependencies are understood
- Security baseline is defined
- Evaluation strategy exists

---

# 37. SUCCESS CRITERIA

The application will be successful when it can demonstrate the following.

## Conversation Intelligence

Given a complete customer conversation, the system can:

- Understand the conversation
- Generate an accurate summary
- Identify the customer goal
- Identify the customer problem
- Classify the problem
- Determine sentiment
- Determine resolution
- Evaluate assistant accuracy
- Store structured results
- Provide traceability

---

## Taxonomy

The system can:

- Scale beyond hundreds of prompt categories
- Retrieve relevant taxonomy candidates
- Classify conversations hierarchically
- Handle ambiguity
- Handle unknown categories
- Track taxonomy versions

---

## NLP-to-SQL

Given an authorized business question, the system can:

- Understand the question
- Identify metrics
- Identify dimensions
- Identify filters
- Interpret time periods
- Retrieve relevant semantic metadata
- Generate constrained SQL
- Validate SQL deterministically
- Apply authorization
- Execute safely
- Validate results
- Generate a business answer

---

## Engineering Quality

The system must be:

- Modular
- Testable
- Secure
- Observable
- Scalable
- Traceable
- Versioned
- Maintainable

---

# 38. FINAL INSTRUCTION TO THE ORCHESTRATOR

You are responsible for coordinating development of this application.

Do not treat this prompt as an instruction to immediately generate all application code.

Your responsibility is to:

1. Understand the product vision.
2. Convert it into detailed requirements.
3. Identify gaps and assumptions.
4. Define the MVP.
5. Design the architecture.
6. Create a phased implementation roadmap.
7. Create the task dependency graph.
8. Delegate work to appropriate specialists.
9. Enforce security and quality gates.
10. Ensure every major feature has acceptance criteria.
11. Ensure AI components have evaluation strategies.
12. Ensure generated SQL is governed and deterministically validated.
13. Ensure business definitions are controlled through a semantic layer.
14. Preserve durable artifacts and architectural decisions.
15. Begin implementation incrementally.

The immediate objective is:

# Create the complete MVP requirements, architecture, roadmap, task DAG, and implementation plan for the Enterprise Banking Conversation Intelligence and NLP-to-SQL Platform.

Do not skip planning.

Do not start implementation until the planning artifacts are complete and internally reviewed.

When implementation begins, build the platform incrementally, beginning with the foundation and the highest-value MVP capability.

Every completed development stage must include:

- Implementation evidence
- Test evidence
- Security review
- Independent review
- Documentation
- Completion summary

The system should continuously optimize for:

**Business value + Accuracy + Safety + Scalability + Maintainability + Traceability.**