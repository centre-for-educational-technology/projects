<div class="lab-detail-header" markdown>

<div class="lab-detail-badges" markdown>
<span class="lab-badge lab-badge--stack">Laravel</span>
<span class="lab-badge lab-badge--stack">React</span>
<span class="lab-badge lab-badge--stack">PostgreSQL / pgvector</span>
<span class="lab-badge lab-badge--stack">OpenAI / Anthropic</span>
</div>

# RAGistrat

<div class="lab-detail-desc">
A RAG-powered assessment tool for legal education that provides evidence-based evaluations of student work using the IRAC method. By grounding analysis in authoritative legal sources, it bridges AI reasoning with the demands of legal scholarship.
</div>


</div>

<span class="lab-section-num">// 01</span>

## Overview

RAGistrat employs Retrieval-Augmented Generation to analyze law students' written submissions against a curated knowledge base of legal documents. The system evaluates work using the IRAC method (Issue, Rule, Application, Conclusion) and a configurable set of 17 assessment rules, producing structured feedback with norm citations and rule violation flags for instructor review.

### Workflow

1. **Knowledge base construction** — legal documents (PDF/TXT) are chunked, embedded, and stored as vectors for semantic search
2. **Case definition** — instructors create legal scenarios and exercises
3. **Submission collection** — students submit written legal analyses
4. **AI analysis** — submissions are evaluated against the knowledge base and assessment rules, producing IRAC-structured feedback with norm citations and violation flags
5. **Flag review** — critical issues are flagged for instructor review with accept/dismiss workflow
6. **Instructor grading** — review AI analysis, adjust scores (0–18 German legal grading scale), provide feedback
7. **PDF export** — download structured analysis reports

<span class="lab-section-num">// 02</span>

## Technical Architecture

<div class="lab-specs" markdown>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Backend</div>
<div class="lab-spec-val">Laravel, PHP</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Frontend</div>
<div class="lab-spec-val">React via Inertia.js (JSX), Tailwind CSS</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Database</div>
<div class="lab-spec-val">PostgreSQL with pgvector extension (cosine similarity search)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Queue</div>
<div class="lab-spec-val">Redis, monitored via Laravel Horizon</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">AI / Embeddings</div>
<div class="lab-spec-val">OpenAI embeddings (text-embedding-3-small), switchable LLM (OpenAI GPT / Anthropic Claude)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Deployment</div>
<div class="lab-spec-val">Docker Compose (multi-stage build) targeting VPS</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">CI</div>
<div class="lab-spec-val">GitHub Actions (tests + lint)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">License</div>
<div class="lab-spec-val">All rights reserved</div>
</div>
</div>

### Core Services

| Service | Responsibility |
|---------|----------------|
| `DocumentChunker` | Splits legal documents into overlapping chunks for embedding |
| `VectorSearch` | Cosine similarity search over pgvector embeddings |
| `LlmManager` | Switchable LLM provider abstraction (OpenAI / Anthropic) |
| `PromptBuilder` | Assembles analysis prompts from submission, retrieved context, and assessment rules |
| `RuleEvaluator` | Evaluates submissions against 17 configurable assessment rules |
| `EvaluationRunner` | Fixture-based quality measurement for analysis output |

<span class="lab-section-num">// 03</span>

## RAG Pipeline

The retrieval-augmented generation pipeline operates in two phases:

**Ingestion** — legal source documents are uploaded, text is extracted (PDF/TXT), split into overlapping chunks by the `DocumentChunker`, embedded via OpenAI's text-embedding-3-small model, and stored as vectors in PostgreSQL with the pgvector extension.

**Analysis** — when a student submission is processed, the system performs semantic search over the vector store to retrieve the most relevant legal context, assembles a structured prompt combining the submission text, retrieved passages, and the 17 assessment rules, then sends it to the configured LLM (OpenAI GPT or Anthropic Claude) for IRAC-structured evaluation.

Analysis jobs run asynchronously via Redis queues monitored through Laravel Horizon. Instructors receive notifications when analyses complete.

<span class="lab-section-num">// 04</span>

## IRAC Assessment Framework

The system evaluates legal writing using the IRAC method — a standard framework in legal education:

- **Issue** — has the student correctly identified the legal issue(s)?
- **Rule** — are the relevant legal rules and norms accurately stated?
- **Application** — is the law properly applied to the facts of the case?
- **Conclusion** — does the analysis reach a logically sound conclusion?

Each dimension is evaluated against configurable assessment rules. The AI produces structured feedback with specific norm citations from the knowledge base, flags rule violations for instructor review, and suggests scores on the 0–18 German legal grading scale.

### Assessment Rules

The system ships with 17 assessment rules that define what constitutes quality legal analysis. Rules are configurable by instructors and cover aspects such as norm citation accuracy, logical argumentation structure, completeness of issue identification, and proper legal methodology.

<span class="lab-section-num">// 05</span>

## Quality Assurance

RAGistrat includes a built-in evaluation harness for measuring analysis quality:

- **Fixture-based testing** — predefined submissions with expected outcomes allow systematic quality measurement
- **Mock providers** — all 191 tests (436 assertions) run without API keys using mock embedding and LLM providers
- **Evaluation CLI** — `evaluate:run` command measures analysis accuracy against fixtures, runnable with mock or real providers
- **Policy-based authorization** — 6 authorization policies with 39 dedicated tests ensure proper access control across all resources

<span class="lab-section-num">// 06</span>

## Instructor & Student Interfaces

**Instructor dashboard** — overview statistics, recent activity, case management, source/knowledge base administration, submission review with IRAC breakdown and violation flags, grading with score adjustment, flag accept/dismiss workflow, PDF report export, and notification center.

**Student interface** — case browsing, submission upload, and feedback review after instructor grading.

**Access control** — role-based (admin, instructor, student) with policy-based authorization on all resources. Horizon dashboard restricted to admin/instructor roles.

<span class="lab-section-num">// 07</span>

## Related Publications

<div class="lab-pub" markdown>
<div class="lab-pub-year">—</div>
<div class="lab-pub-info" markdown>
<span class="lab-pub-title">Publications related to this tool will be listed here.</span>
<span class="lab-pub-authors"></span>
<span class="lab-pub-venue"></span>
</div>
</div>

<span class="lab-section-num">// 08</span>

## Source Code & Links

- **Repository**: [github.com/centre-for-educational-technology/ragistrat](#)
