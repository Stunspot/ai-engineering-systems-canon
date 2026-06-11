# AI-ENG-D — Corpus Engineering - Data Provenance, Knowledge Hygiene & Source Authority

## **The Foundations of Knowledge Supply Chain Control**

The mature artificial intelligence enterprise does not treat its data as an unmanaged document landfill, a collection of flat PDFs, or a basic vector database filled with disconnected text chunks. Instead, the corpus is modeled as an engineered, governed knowledge substrate.1 This report defines the architectural discipline of **corpus engineering** as the end-to-end knowledge supply-chain management that transforms raw, unstructured organizational information into clean, versioned, deduplicated, permission-aware, canonical, conflict-aware, and lifecycle-governed knowledge assets.1 These assets are designed to support retrieval-augmented generation (RAG), multi-agent memory structures, verifiably accurate citations, and model-facing context compilation.4

```
Raw Organizational Data (PDFs, Wikis, SQL, APIs)  
                      │  
                      ▼  
         <─── Enforces: Provenance, Hygiene, ACLs  
                      │  
                      ▼  
         Governed Corpus Object Store  
                      │  
                      ▼  
 Pre-retrieval Permission Filtering  
                      │  
                      ▼  
        Approximate Nearest Neighbor (ANN)  
                      │  
                      ▼  
           Context Object Assembly  
                      │  
                      ▼  
         LLM Synthesizer (Context Window)
```

Corpus engineering represents a distinct architectural shift from standard data ingestion, document management, RAG preprocessing, retrieval engineering, and memory architectures. The following comparative matrix outlines these structural boundaries:

| Functional Layer | Principal Focus | Operational Scope | Typical Failure Modes |
| :---- | :---- | :---- | :---- |
| **Data Ingestion** | Bulk transport of raw bytes from point A to point B. | ETL pipelines, cloud sync, replication. | Ignores document layout, parses non-printable characters, lacks semantic indexing.2 |
| **Document Management** | Storage, indexing, and human-facing access control of files. | Folder structures, metadata cards, search indexes. | Fails to segment text, lacks vector representations, ignores model compatibility. |
| **RAG Preprocessing** | Ad-hoc text splitting and bulk embedding generation. | Blind character-limit chunking, basic vector loading. | Creates fragmented text, severs logical relations, leaks confidential data.7 |
| **Corpus Engineering** | **Upstream governance, provenance validation, and lifecycle control.** 3 | Ingestion-time security, deduplication, canonical ID assignment, source authority.8 | Vector DB bloat, citation laundering, stale index retrieval.6 |
| **Retrieval Engineering** | Dynamic search execution, similarity scoring, and reranking. | Hybrid search, query expansion, vector distance checks. | Evaluates unauthorized or outdated document chunks.7 |
| **Memory Architecture** | Read/write management of state during agent execution. | Short-term prompt caching, long-term state databases. | Writes hallucinated data back into permanent storage. |

Before an external retrieval system, semantic memory, or context compiler can safely expose source data to a large language model (LLM), specific criteria must be strictly verified. The retrieval engine must not operate under the assumption that semantic similarity translates to factual legitimacy. A text passage can match a user’s query with near-perfect cosine similarity while remaining factually obsolete, unapproved, duplicated, poisoned, misparsed, or entirely restricted under organizational access control policies.7  
To build a secure and accurate retrieval system, the corpus substrate must enforce the following rule:  
**Provenance before relevance: no knowledge object should enter retrieval, memory, citation, or model-facing context unless the system knows where it came from, who owns it, what authority it carries, what scope it applies to, what transformations it has undergone, and how its lifecycle is governed.**  
Corpus engineering operates as the upstream control system that prevents retrieval frameworks from becoming very fast machinery for finding the wrong information with absolute confidence.10

## **The Epistemic Hierarchy of Corpus Artifacts**

Raw organizational information is transformed through several discrete states before it is loaded into an LLM's active context window. This architectural progression is governed by the **Epistemic Hierarchy**, which maps raw source files to structured, model-ready context objects.

```
 (e.g., policy_v2.pdf)  
       │  
       ▼ (Parsing & Structure Normalization)  
 (Markdown representation)  
       │  
       ▼ (Identity & Metadata Ingestion)  
[Corpus Object] (Canonical, metadata-anchored asset)  
       │  
       ├─► [Canonical Chunk] (Semantic segment with security tags)  
       ├─► [Extracted Claim] (Atomic proposition, e.g., "Company matches 4%")  
       ├─► (Contextual overview)  
       ├─► (Graph node mapped to Canonical ID)  
       │  
       ▼ (Retrieval, Filtering & Verification)  
[Context Object] ──►
```

The table below defines the structural boundaries, ingestion mechanics, and operational lifespans of each artifact within the epistemic hierarchy:

| Artifact State | Structural Definition | Ingestion/Transformation Mechanic | Lifespan and Operational Scope |
| :---- | :---- | :---- | :---- |
| **Raw Source** | Unprocessed file stored in its native format (e.g., PDF, DOCX, HTML, SQL record).4 | File discovery, system connection polling, web scraping.4 | Permanent storage inside the enterprise system of record (e.g., SharePoint, Confluence).13 |
| **Normalized Document** | Structured Markdown text representing the structural elements of the raw source file.4 | Structural layout analysis, OCR processing, and table cell formatting.4 | Ephemeral, middle-tier object. Deleted or archived after downstream parsing completes.1 |
| **Corpus Object** | The primary, metadata-anchored unit of governed source knowledge.3 | Identity assignment, security metadata mapping, and source authority evaluation.8 | Permanent. Serves as the primary parent asset in the knowledge database.1 |
| **Canonical Chunk** | A semantically cohesive segment of text derived from a parent Corpus Object.1 | Recursive or structure-aware text splitting with parent-child metadata pointers.2 | Stored permanently in the vector database alongside its embeddings.1 |
| **Extracted Claim** | An atomic, model-verifiable assertion derived from a document (e.g., a single fact).5 | Multi-agent schema extraction, logic isolation, and confidence scoring.4 | Stored in relational tables or graph databases for factual contradiction checks.12 |
| **Embedding Record** | High-dimensional vector representation of a Canonical Chunk.1 | Vectorization via an embedding model (e.g., text-embedding-3-large).1 | Kept in sync with its corresponding chunk. Deleted immediately if the chunk is deprecated. |
| **Generated Summary** | An LLM-synthesized overview of a Corpus Object or cluster.5 | Summarization routines run during document ingestion.5 | Stored in the metadata index to accelerate broad, top-down discovery queries.1 |
| **Entity Record** | A unique node in the enterprise knowledge graph mapping to a real-world concept.9 | Named Entity Recognition (NER), entity clustering, and alias table mapping.9 | Permanent. Updated dynamically as new documents reference the target entity.17 |
| **Citation** | A structural pointer mapping generated text back to its exact document coordinates.11 | Real-time context alignment and source-evidence verification during model generation.4 | Session-bound. Stored in system telemetry logs for audit trail compliance.13 |
| **Context Object** | A managed, validated, and permission-filtered context block loaded into active memory. | Retrieval execution, security pre-filtering, and token-budget context compilation.8 | Transient. Deleted immediately upon completion of the generation session.7 |

## **Conceptual Glossary**

To establish a standard terminology across the engineering canon, the following table defines the critical concepts of corpus design and knowledge supply chain control:

| Term | Technical Definition | Operational Implication |
| :---- | :---- | :---- |
| **Corpus Engineering** | The systematic discipline of designing, constructing, and maintaining the end-to-end organizational knowledge supply chain.1 | Shifts focus from ad-hoc chunking to structured, metadata-rich, lineage-tracked asset management. |
| **Corpus Object** | The canonical, uniquely identified unit of governed source knowledge containing both structural content and administrative metadata.1 | Serves as the primary parent asset from which chunks, embeddings, and summaries are derived. |
| **Knowledge Supply Chain** | The multi-stage pipeline that ingests raw data, enforces hygiene, extracts metadata, evaluates authority, and indexes objects safely.4 | Ensures every step of document transformation is deterministic, validated, and auditable. |
| **Data Provenance** | Detailed documentation tracking the origin, authorship, ownership, and chain of custody of a knowledge asset.3 | Enables downstream models to cite sources with verifiable structural authority.6 |
| **Data Lineage** | The complete history of structural transformations, parsers, and processing steps applied to an asset.3 | Allows tracing of downstream artifacts (such as chunks or embeddings) back to exact source versions. |
| **Source Authority** | A quantitative or categorical metric representing the structural weight, trust level, and compliance status of a source system.10 | Prevents low-authority draft documents from overriding high-authority systems of record during retrieval. |
| **Knowledge Hygiene** | The operational practice of extracting noise, resolving OCR/parsing errors, and deleting duplicate information.1 | Eliminates context-window bloating, redundant embedding costs, and model distraction.2 |
| **Canonical ID** | A persistent, globally unique identifier assigned to an entity, document, or concept within the enterprise knowledge graph.9 | Prevents duplicate records by establishing a single source of truth across disparate source files. |
| **Alias Table** | A mapping schema that associates surface-name variations, historical acronyms, and abbreviations with a single Canonical ID.9 | Enables the retrieval system to resolve lexical variants to the same underlying entity profile.16 |
| **Deduplication** | The removal of exact, near-exact, or semantic duplicates from the active index using lexical hashes or semantic clustering.1 | Lowers overall storage footprint, reduces inference latency, and prevents bias in generation.22 |
| **Normalization** | The standardization of document formats, metadata fields, timestamps, schemas, and locales across all ingested documents.1 | Ensures uniform query filtering and reliable metadata-based partitioning in the database. |
| **Sensitivity Inheritance** | The rule forcing derived elements (chunks, embeddings, summaries) to carry the classification of their parent source.8 | Prevents the accidental laundering of classified or sensitive information into open context windows.7 |
| **Permission-Aware Indexing** | The injection of access control lists (ACLs) directly into vector indexes and metadata schemas at ingestion time.8 | Evaluates user and group authorization *prior* to vector scoring, preventing side-channel leakage.7 |
| **Supersession** | A directed relationship linking an outdated version of a document to its valid, current counterpart. | Directs active retrievals to the current standard while preserving old versions for audit trails. |
| **Retention Class** | A metadata attribute defining the legally mandated lifespan of a document based on regulatory compliance.14 | Automates the safe, defensible deletion of expired records from physical storage and vector indexes.14 |
| **Archival State** | The lifecycle stage indicating if an object is active, cold-stored, under a legal hold, or marked for permanent deletion.14 | Segregates active business context from historical, compliance-only records.14 |
| **Redaction Propagation** | The programmatic removal of sensitive terms or PII across all downstream chunks, summaries, and embeddings. | Guarantees that deleting or masking a term in a source document instantly sanitizes all derivative objects. |
| **Conflict Status** | A metadata flag indicating that an object contains claims that contradict another active knowledge source.10 | Triggers specialized multi-step reasoning models to isolate and resolve epistemic differences.12 |
| **Citation Fidelity** | The mathematical and structural match between the generated answer text and the referenced source coordinates.11 | Detects and blocks "citation laundering," where irrelevant files are cited to support hallucinated claims.11 |

## **Corpus Object Schema**

A robust, enterprise-grade metadata payload must be bound to every Corpus Object at ingestion. This metadata allows downstream retrieval engines, security checkers, and context compilers to filter out out-of-scope, invalid, or unauthorized materials before running search algorithms.7

```
┌────────────────────────────────────────────────────────────────────────┐  
│                          CORPUS OBJECT SCHEMA                          │  
├────────────────────────────────────────────────────────────────────────┤  
│ IDENTITY      : object_id (UUID), canonical_source_id (UUID)           │  
│ ORIGIN        : source_system (Str), source_uri (Str), jurisdiction    │  
│ PROVENANCE    : creator (Str), owner (Str), steward (Str)              │  
│ LIFECYCLE     : version_id (Str), valid_from, valid_until (Timestamps) │  
│ STATE         : version_state (Enum), archival_state (Enum)            │  
│ RELATIONSHIPS : supersedes (UUID), superseded_by (UUID), lineage_parent│  
│ SECURITY      : classification (Enum), permission_scope (JSON/ACL)     │  
│ COMPLIANCE    : retention_class (Enum), legal_hold (Bool)              │  
│ EPISTEMIC     : source_authority (Float), conflict_status (Enum)       │  
└────────────────────────────────────────────────────────────────────────┘
```

The fields below define the technical metadata schema of the standard Corpus Object:

JSON  
{  
  "$schema": "http://json-schema.org/draft-07/schema#",  
  "title": "CorpusObjectSchema",  
  "type": "object",  
  "properties": {  
    "identity": {  
      "type": "object",  
      "properties": {  
        "object_id": { "type": "string", "format": "uuid" },  
        "object_type": { "type": "string", "enum": ["document", "chunk", "claim", "table_row", "entity_profile", "summary"] },  
        "canonical_source_id": { "type": "string", "format": "uuid" }  
      },  
      "required": ["object_id", "object_type", "canonical_source_id"]  
    },  
    "origin": {  
      "type": "object",  
      "properties": {  
        "source_system": { "type": "string" },  
        "source_uri": { "type": "string", "format": "uri" },  
        "jurisdiction": { "type": "string" },  
        "product_scope": { "type": "array", "items": { "type": "string" } }  
      },  
      "required": ["source_system", "source_uri"]  
    },  
    "provenance": {  
      "type": "object",  
      "properties": {  
        "creator": { "type": "string" },  
        "owner": { "type": "string" },  
        "steward": { "type": "string" },  
        "creation_date": { "type": "string", "format": "date-time" },  
        "ingestion_date": { "type": "string", "format": "date-time" },  
        "observed_date": { "type": "string", "format": "date-time" }  
      },  
      "required": ["creator", "owner", "steward", "creation_date", "ingestion_date"]  
    },  
    "lifecycle": {  
      "type": "object",  
      "properties": {  
        "valid_from": { "type": "string", "format": "date-time" },  
        "valid_until": { "type": "string", "format": "date-time" },  
        "version_id": { "type": "string" },  
        "version_state": { "type": "string", "enum": ["draft", "approved", "active", "deprecated", "superseded", "archived", "deleted"] },  
        "supersedes": { "type": "string", "format": "uuid" },  
        "superseded_by": { "type": "string", "format": "uuid" },  
        "lineage_parent": { "type": "string", "format": "uuid" },  
        "lineage_children": { "type": "array", "items": { "type": "string", "format": "uuid" } },  
        "transformation_history": {  
          "type": "array",  
          "items": {  
            "type": "object",  
            "properties": {  
              "activity_id": { "type": "string", "format": "uuid" },  
              "parser_method": { "type": "string" },  
              "normalization_method": { "type": "string" },  
              "timestamp": { "type": "string", "format": "date-time" }  
            }  
          }  
        }  
      },  
      "required": ["valid_from", "version_id", "version_state"]  
    },  
    "security": {  
      "type": "object",  
      "properties": {  
        "sensitivity_class": { "type": "string", "enum": ["public", "internal", "confidential", "secret"] },  
        "redaction_status": { "type": "string", "enum": ["unredacted", "pii_masked", "privilege_cleared"] },  
        "permission_scope": {  
          "type": "object",  
          "properties": {  
            "allowed_users": { "type": "array", "items": { "type": "string" } },  
            "allowed_roles": { "type": "array", "items": { "type": "string" } },  
            "allowed_groups": { "type": "array", "items": { "type": "string" } },  
            "row_level_security_tags": { "type": "array", "items": { "type": "string" } }  
          }  
        },  
        "tenant_scope": { "type": "string" }  
      },  
      "required": ["sensitivity_class", "redaction_status", "permission_scope", "tenant_scope"]  
    },  
    "compliance": {  
      "type": "object",  
      "properties": {  
        "retention_class": { "type": "string" },  
        "archival_state": { "type": "string", "enum": ["active", "cold_archive", "legal_hold", "disposed"] },  
        "legal_hold_status": { "type": "boolean" },  
        "deletion_timestamp": { "type": "string", "format": "date-time" },  
        "review_trigger": {  
          "type": "object",  
          "properties": {  
            "trigger_type": { "type": "string", "enum": ["temporal", "event_driven"] },  
            "trigger_rule": { "type": "string" },  
            "next_review_date": { "type": "string", "format": "date-time" }  
          }  
        }  
      },  
      "required": ["retention_class", "archival_state", "legal_hold_status"]  
    },  
    "epistemic": {  
      "type": "object",  
      "properties": {  
        "canonical_entity_ids": { "type": "array", "items": { "type": "string", "format": "uuid" } },  
        "aliases": { "type": "array", "items": { "type": "string" } },  
        "extracted_claims": { "type": "array", "items": { "type": "object" } },  
        "source_authority": { "type": "number", "minimum": 0.0, "maximum": 1.0 },  
        "confidence_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 },  
        "citation_label": { "type": "string" },  
        "conflict_status": { "type": "string", "enum": ["resolved", "disputed", "uncontested"] }  
      },  
      "required": ["source_authority", "citation_label", "conflict_status"]  
    }  
  },  
  "required": ["identity", "origin", "provenance", "lifecycle", "security", "compliance", "epistemic"]  
}

## **Knowledge Supply Chain Pipeline**

The transformation of raw source systems into governed, context-ready Corpus Objects requires a strict, multi-stage processing pipeline.1 The pipeline is continuous, executing micro-batching and event-driven updates rather than episodic ingestion, to keep the system synchronized with enterprise system changes.6

```
                     RAW SOURCE SYSTEMS  
                             │  
     ┌───────────────────────┼───────────────────────┐  
     ▼                       ▼                       ▼  
SharePoint              Confluence             SQL Databases   
     │                       │                       │  
     └───────────────────────┬───────────────────────┘  
                             │  
                     STAGE 1: Discovery & Registration  
                             │  
                     STAGE 2: Permission Preflight   
                             │  
                     STAGE 3: Ingestion & Lock  
                             │  
                     STAGE 4: Parsing & OCR   
                             │  
                     STAGE 5: Format Normalization   
                             │  
                     STAGE 6: Boilerplate Removal   
                             │  
                     STAGE 7: Redaction & PII Masking   
                             │  
                     STAGE 8: Deduplication   
                             │  
                     STAGE 9: Entity Resolution   
                             │  
                     STAGE 10: Metadata Tagging   
                             │  
                     STAGE 11: Authority Scoring   
                             │  
                     STAGE 12: Lineage Recording   
                             │  
                     STAGE 13: Index Loading   
                             │  
                             ▼  
                  GOVERNED CORPUS OBJECTS
```

### **13-Stage Pipeline Execution Map**

Each stage in the knowledge supply chain operates with a distinct architectural goal, processing raw data deterministically and documenting the execution metrics at every transition point:

| Stage | Process Name | Operational Execution Mechanics | Failure Impact on Retrieval Quality |
| :---- | :---- | :---- | :---- |
| **1** | **Discovery & Registration** | Continuous scanning of enterprise data system connectors; generates a unique, persistent asset ID for discovered files.4 | Unregistered files create blind spots in the enterprise search space. |
| **2** | **Permission Preflight** | Inherits and caches access control lists (ACLs) from the source system *prior* to parsing the file content.23 | Prevents the ingestion of unauthorized files, eliminating permission drift.6 |
| **3** | **Ingestion & Lock** | Pulls raw binary bytes; creates a write-locked, immutable replica inside a temporary landing bucket. | Modifications to the source file during processing can corrupt downstream parser outputs. |
| **4** | **Parsing & OCR** | Runs layout analysis, separates text from figures, and processes scanned documents using OCR.4 | OCR errors misinterpret numeric data, corrupting downstream generation.15 |
| **5** | **Format Normalization** | Translates parsed elements into a structured, strict UTF-8 Markdown representation.4 | Unnormalized syntax structure breaks consistent chunking algorithms.2 |
| **6** | **Boilerplate Removal** | Strips headers, footers, licensing text, and web navigation markers.2 | Boilerplate noise pollutes vector search spaces, inflating context costs.1 |
| **7** | **Redaction & PII Masking** | Runs regex and NER classifiers to redact or replace credentials and customer PII.2 | Missing redactions expose confidential records to external processing endpoints.7 |
| **8** | **Deduplication** | Runs exact byte hashes and MinHash LSH queries to identify and drop duplicate files.1 | Redundant files inflate index sizes and cause bias in retrieved content.22 |
| **9** | **Entity Resolution** | Maps extracted text entities to unique Graph IDs using alias tables.9 | Ambiguous entities lead to retrieval errors across similar projects or users.17 |
| **10** | **Metadata Tagging** | Evaluates document structural headers, mapping attributes to the target JSON schema.1 | Missing metadata tags prevent effective query-time pre-filtering.1 |
| **11** | **Authority Scoring** | Computes a quantitative authority score based on origin system and review status.10 | Treats unapproved drafts as equal to systems of record during retrieval. |
| **12** | **Lineage Recording** | Logs the activities, agents, and processing steps in a PROV-O-compliant ledger.18 | Inability to audit the transformation history limits forensic debugging. |
| **13** | **Index Loading** | Loads the finalized, partitioned vectors, keywords, and graph records into active storage.1 | Out-of-sync indexes serve stale or deleted knowledge assets.6 |

### **Parsing Performance Analysis**

The parser selected for Stage 4 dictates the structural quality ceiling of the entire corpus.2 If the parser misaligns tables, merges distinct columns, or ignores layout hierarchies, downstream semantic retrieval is fundamentally limited.15  
The following evaluation table compares three industry-standard document parsing engines based on empirical evaluation data 15:

| Metric / Capability | Docling (IBM Research) | Unstructured (Unstructured.io) | LlamaParse (LlamaIndex) |
| :---- | :---- | :---- | :---- |
| **Core Structural Engine** | Layout analysis via DocLayNet; table cell parsing via TableFormer.15 | Transformer-based layout analysis and OCR pipeline.15 | Agentic OCR with expert model orchestration.4 |
| **Table Cell Extraction Accuracy** | **97.9% accuracy** on multi-level, nested, and complex table structures.15 | **100% on simple tables**, but drops to **75% on complex, multi-row structures**.15 | **100% on simple tables**, but fails on complex layouts with column shifts.15 |
| **Text Extraction Fidelity** | High. Preserves paragraph breaks and technical phrasing.15 | Inconsistent line breaks; merges paragraph breaks; prone to over-extraction.15 | Inconsistent with complex layouts; prone to word merging and content hallucinations.15 |
| **Single-Page Parse Latency** | **6.28 seconds**.15 | **51.06 seconds** (very slow; highly inefficient for large pipelines).15 | **~6.00 seconds**.15 |
| **50-Page Parse Latency** | **65.12 seconds** (demonstrates predictable, linear scaling).15 | **141.02 seconds** (non-linear scaling issues).15 | **~6.00 seconds** (highly optimized cloud extraction).15 |
| **Markdown Structural Output** | Good. Clear section headers, though nested headings are sometimes flattened to ##.15 | Basic structure extraction; struggles with multi-level section hierarchies.15 | Struggles with deep layout hierarchies; merges section boundaries into blocks.15 |
| **Downstream Architectural Fit** | Best for processing technical documents and dense financial reports.15 | Suitable for batch-oriented ETL data normalization but restricted by latency limitations.15 | Best for high-velocity, lightweight agent workflows where speed is prioritized over table accuracy.15 |

## **Source Authority and Provenance Models**

A mature enterprise corpus must account for the reality that organizational documents carry varying levels of authority.10 Treating a casual conversation or an unapproved draft with the same weight as a validated system-of-record entry introduces serious compliance and factual risks.10

### **Source Authority Model**

To solve this, the system assigns a quantitative **Source Authority Score** (As in the range [0.0, 1.0]) to every ingested resource.10 At query time, this score is used as a weight factor during vector retrieval scoring.  
The table below defines the authority tiers of enterprise knowledge sources:

| Source Class | Target System | As | Volatility | Ownership and Stewards | Compliance Auditing | Appropriate Context Use Case |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Tier 1: Enterprise System of Record** | Production PostgreSQL DB, Core SAP, Salesforce.6 | 1.00 | Extremely Low | IT Database Administrators, Security Officers. | Complete ACID transaction logs and security reviews. | Verifying customer transaction values, current product stock, and billing amounts. |
| **Tier 2: Certified Corporate Policy** | Legal DMS, Approved HR Handbooks, Regulatory Manuals.14 | 0.90 | Low | Legal Counsel, Department Stewards.14 | Annual reviews and formal revision control.14 | Authoritative regulatory compliance answers and security protocols. |
| **Tier 3: Product Specifications** | GitHub Releases, Jira Epic specs, Product databases. | 0.75 | Moderate | Product Managers, Engineering Leads. | Code repository history and commit signatures. | Resolving current product hardware dimensions and API schemas. |
| **Tier 4: Dynamic Team Wikis** | Confluence, Notion Teams, Shared Google Docs.13 | 0.50 | High | Collaborative team creators. | Basic document modification history. | Finding internal team operational guides and system deployment tips. |
| **Tier 5: External Web Scrapes** | Scraped partner APIs, public vendor blogs.4 | 0.30 | Very High | Automated crawl agents. | Excluded from primary compliance boundaries. | Competitor pricing trends and industry news aggregation.4 |
| **Tier 6: User-Generated Uploads** | Active chat uploads, unclassified local PDFs.13 | 0.20 | Dynamic | End-user during active session.13 | Sandbox scanning and DLP inspection logs.13 | Dynamic analysis and summarization of local user files.13 |
| **Tier 7: Generated Summaries** | Background summaries, model claim extractions.5 | 0.10 | Instantaneous | Background AI processors, verification agents. | Complete PROV-O lineage tracking.18 | Accelerating broad topic discovery and semantic link indexing. |

### **Provenance and Lineage tracking via PROV-O**

To guarantee complete auditability, the corpus platform must map every step of document discovery, parsing, translation, and chunking using the W3C Provenance Ontology (PROV-O).18 PROV-O uses a tripartite structure: **Entities** (assets, chunks, or claims), **Activities** (parser runs, chunk splits, or translation steps), and **Agents** (software systems or curators).29  
The RDF Turtle block below illustrates a complete lineage trail from a raw PDF document to a retrieved citation 18:

Code snippet  
@prefix prov: <http://www.w3.org/ns/prov#>.  
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>.  
@prefix corp: <http://enterprise.ai/corpus/>.  
@prefix agent: <http://enterprise.ai/agents/>.  
@prefix act: <http://enterprise.ai/activities/>.

# 1. Raw Source Entity representation  
corp:raw_hr_policy_pdf  
    a prov:Entity ;  
    prov:wasAttributedTo agent:hr_policy_director ;  
    prov:generatedAtTime "2026-01-15T09:00:00Z"^^xsd:dateTime.

# 2. Parsing Activity  
act:docling_parser_run_452  
    a prov:Activity ;  
    prov:startedAtTime "2026-06-06T10:00:00Z"^^xsd:dateTime ;  
    prov:endedAtTime "2026-06-06T10:00:06Z"^^xsd:dateTime ;  
    prov:wasAssociatedWith agent:docling_parser_engine.

# 3. Normalized Document Entity  
corp:normalized_hr_policy_md  
    a prov:Entity ;  
    prov:wasDerivedFrom corp:raw_hr_policy_pdf ;  
    prov:wasGeneratedBy act:docling_parser_run_452.

# 4. Chunking Activity  
act:recursive_splitter_run_893  
    a prov:Activity ;  
    prov:startedAtTime "2026-06-06T10:01:00Z"^^xsd:dateTime ;  
    prov:wasAssociatedWith agent:chunking_service.

# 5. Canonical Chunk Entity  
corp:canonical_chunk_segment_12  
    a prov:Entity ;  
    prov:wasDerivedFrom corp:normalized_hr_policy_md ;  
    prov:wasGeneratedBy act:recursive_splitter_run_893.

# 6. RAG Retrieval Activity  
act:vector_retrieval_query_901  
    a prov:Activity ;  
    prov:startedAtTime "2026-06-06T13:45:00Z"^^xsd:dateTime ;  
    prov:wasAssociatedWith agent:retrieval_orchestrator.

# 7. Context Object Entity  
corp:context_object_claim_901  
    a prov:Entity ;  
    prov:wasDerivedFrom corp:canonical_chunk_segment_12 ;  
    prov:wasGeneratedBy act:vector_retrieval_query_901.

# Agents  
agent:hr_policy_director a prov:Agent.  
agent:docling_parser_engine a prov:Agent ; prov:actedOnBehalfOf agent:it_data_platform_steward.  
agent:chunking_service a prov:Agent.  
agent:retrieval_orchestrator a prov:Agent.  
agent:it_data_platform_steward a prov:Agent.

Using this structural ledger, a downstream model or auditor can perform recursive graph queries to trace a generated claim back to its raw source.18 If a model cites context_object_claim_901, the system can trace the chain back through canonical_chunk_segment_12 and normalized_hr_policy_md to confirm that the source was raw_hr_policy_pdf, which was created by hr_policy_director.18

## **Normalization, Deduplication, and Canonicalization**

Unstructured enterprise information frequently contains layout differences, formatting noise, and duplicate content.20 If ignored, this noise pollutes semantic indexes and causes retrieved results to contain redundant or conflicting sections.1

```
Raw Sources ──► Normalization (UTF-8, ISO 8601, SI units)  
                      │  
                      ▼  
            MinHash LSH Indexing ──► ──► Retain higher-authority version   
                      │  
               
                      │  
                      ▼  
             Proxy-Pointer RAG ──► Build Entity Profile ──► strict LLM reconciliation   
                                                                        │  
                                                                        ▼  
                                                             Knowledge Graph update
```

### **Multi-Level Normalization Protocol**

The ingestion pipeline enforces strict normalization standards to ensure uniform metadata filtering and clean text formatting:

* **Document and Text Normalization**: Raw inputs are processed, characters are converted to strict UTF-8, and non-printable elements are stripped. Headings are mapped to standard Markdown # structures.15  
* **Temporal Normalization**: All document dates are parsed and converted to ISO 8601 UTC format (YYYY-MM-DDTHH:MM:SSZ).1  
* **Unit and Schema Normalization**: Measurements are converted to standard SI base units, and structural metadata parameters are mapped to strict schema types (e.g., UUID format for identifier fields).  
* **Language and Locale Handling**: Documents are analyzed via language detection models and tagged with ISO 639-1 codes. Multilingual files map to parallel translation references.  
* **Citation Normalization**: Author fields are parsed and formatted to a uniform corporate format, ensuring consistency in downstream citation generation.

### **Near-Duplicate Detection via MinHash LSH**

The pipeline uses MinHash Locality-Sensitive Hashing (LSH) to identify near-duplicate documents and chunks, preventing redundant files from cluttering the index.1  
For document token sets A and B, the system calculates their Jaccard similarity:  
J(A, B) = |A intersect B| / |A union B|  
MinHash approximates this similarity by applying independent hash functions to represent token permutations.22 The probability that a signature matches is mathematically equivalent to the Jaccard similarity:  
P(MinHash(A) = MinHash(B)) = J(A, B)  
By building a signature matrix from k independent hash functions (where k = 128\) and splitting the rows into b bands of r rows, the probability that a document pair hashes to the same bucket in at least one band is expressed as:  
P_match = 1 - (1 - s^r)^b  
where s = J(A, B).22 The system uses b = 16 and r = 8 to set a Jaccard similarity threshold T = 0.80.1 If a document's Jaccard score exceeds this threshold, it is flagged as a duplicate.  
The system resolves duplicate flags using the following logic:

* **Exact Metadata Match**: If the duplicate file carries the exact same version and timestamp as an existing entry, the system discards the duplicate to save index space.  
* **Variant with Version Difference**: If the duplicate contains minor updates or a different version string, the system retains the higher-tier version and marks the older entry as superseded.14  
* **Variant with System-of-Record Discrepancy**: If near-duplicate content is found across different systems of record, the entry from the higher-authority source (As) is retained, and the other is flagged for human review.

### **Canonical Entity and Alias Model**

When documents refer to a single entity using different names (e.g., *"Sony"*, *"Sony Interactive"*, *"SIE"*), standard vector searches return fragmented contexts.9 To prevent this, systems must implement **Proxy-Pointer RAG**.9


```                      
                              │  
                     Extract Entity Profile   
                              │  
                    Decompose Sub-Queries:  
       - Raw Entity Name: "Sony"  
       - Relationship Checks: "owns PlayStation trademark"   
                              │  
                              ▼  
                     Vector Database Search  
                              │  
                   
                              │  
                              v  
                   
                              │  
                              v  
                   
                    - Merges variations to Canonical ID  
                    - Maps localized graph neighborhood coordinates
```

Under this model, the system processes entities as follows:

1. **Entity Profile Construction**: When a new document is ingested, an upstream model extracts entity names and builds an **Entity Profile** that captures the surrounding context and relations.9  
2. **Multi-Track Query Decomposition**: The query engine decomposes this profile into targeted sub-queries, checking raw entity names, specific relationship definitions, and broader semantic contexts.9  
3. **Pointers to Parent Sections**: When a vector hit is returned, the system ignores the localized chunk text. It reads the chunk's metadata to locate its parent document and loads the **entire, structurally intact parent section** to preserve context.9  
4. **LLM-Driven Reconciliation**: A specialized Reconciler LLM evaluates the parent section against existing knowledge graph coordinates to merge variations to a single Canonical ID.9 This resolves alias variations at the database layer before storage, providing the graph with precise, localized neighborhood coordinates and eliminating entity sprawl.9

## **Permission-Aware Indexing Model and Sensitivity Inheritance**

A major vulnerability in enterprise AI search is data exposure through retrieved context.7 Unmanaged vector searches can pull sensitive text (such as salary tables or private communications) and pass them to unauthorized users simply because their semantic vectors match the query.7  
To resolve this, the system must enforce access control checks **before** vector similarity scoring takes place.8 Applying permissions after retrieval is insecure; it allows unauthorized data to be fetched into memory and exposes security side-channels, such as observing differences in vector search latencies or utilizing unauthorized rerank scores to deduce private text content.8

```
User Query + JWT Token  
         │  
         ├──> Native Token-Based Query (Entra ID) ──> Extract claims (Roles, Groups)   
         │                                        ──> Match index security metadata   
         │                                        ──> Filter out unauthorized vectors BEFORE ANN search   
         │  
         └──> Security Filter Query Plan (Cerbos) ──> Evaluate ABAC/RBAC policy   
                                                  ──> Generate native SQL/Vector pre-filters   
                                                  ──> Execute secure, constrained retrieval 
```

### **Security Propagation and Sensitivity Inheritance**

To prevent security gaps, any downstream summary, metadata field, extracted claim, evaluation dataset, prompt cache, or memory block derived from a parent source **inherits the identical security classification and ACL constraints of that source** unless explicitly downgraded by a certified administrative workflow.7  
If a parent document is classified as confidential, all derived chunks and summaries inherit the confidential tag. This sensitivity propagation ensures that restricted data cannot be accidentally laundered through summaries, embeddings, vector indexes, or memory writes.

### **Secure Ingestion and Retrieval Patterns**

The following Python implementation patterns demonstrate how to enforce permission-aware indexing and secure query filtering:

#### **1. Security Metadata and RBAC Pre-Filtering**

At ingestion time, every vector chunk is tagged with its tenant, allowed security groups, and classifications.8 The search engine enforces RBAC prior to approximate nearest neighbor (ANN) matching.8

Python  
from dataclasses import dataclass  
from typing import List, Dict, Any, Optional

@dataclass  
class SecureChunk:  
    id: str  
    text: str  
    tenant_id: str  
    allowed_roles: List[str]  
    department: str  
    classification: str  
    doc_version: str

class SecureRetrievalEngine:  
    def __init__(self, vector_store_client: Any):  
        self.client = vector_store_client

    def rbac_prefilter(self, tenant_id: str, user_roles: List[str]) -> Dict[str, Any]:  
        """  
        Generates database-native pre-filtering conditions.  
        Enforces that the tenant matches and the user possesses at least one matching role.  
        """  
        return {  
            "and": [  
                {"field": "tenant_id", "equals": tenant_id},  
                {"field": "allowed_roles", "in": user_roles}  
            ]  
        }

    def secure_search(self, query_vector: List[float], tenant_id: str, user_roles: List[str], top_k: int = 10) -> List:  
        # Generate the pre-filter block  
        filter_conditions = self.rbac_prefilter(tenant_id, user_roles)  
          
        # Execute the ANN vector search with pre-filtering enforced natively  
        results = self.client.search(  
            vector=query_vector,  
            filter=filter_conditions,  
            limit=top_k  
        )  
        return results

#### **2. Attribute-Based Access Control (ABAC) Filtering**

For fine-grained, dynamic rules (e.g., checking user geography, department match, and clearance level simultaneously), the system utilizes an ABAC filter.8

Python  
@dataclass  
class UserContext:  
    identity_id: str  
    tenant_id: str  
    departments: List[str]  
    clearance_level: str  # e.g., 'public', 'internal', 'confidential'

class ABACEnforcer:  
    @staticmethod  
    def construct_abac_filter(user: UserContext) -> Dict[str, Any]:  
        """  
        Ensures users can only retrieve chunks matching their department,   
        their exact tenant, and at or below their active clearance level.  
        """  
        clearance_hierarchy = {  
            "public": ["public"],  
            "internal": ["public", "internal"],  
            "confidential": ["public", "internal", "confidential"]  
        }  
          
        allowed_clearances = clearance_hierarchy.get(user.clearance_level, ["public"])  
          
        return {  
            "and": [  
                {"field": "tenant_id", "equals": user.tenant_id},  
                {"field": "department", "in": user.departments},  
                {"field": "classification", "in": allowed_clearances}  
            ]  
        }

#### **3. Row-Level Security (RLS) for Database-Backed Memory and Agents**

For agents executing structured SQL queries via Text2SQL, PostgreSQL Row-Level Security must be enforced on the connection session to prevent query injection from leaking cross-tenant data.8

SQL  
-- Enable RLS on the core agent memory table  
ALTER TABLE agent_memory_store ENABLE ROW LEVEL SECURITY;

-- Establish the tenant isolation policy using session variables  
CREATE POLICY tenant_isolation_policy   
ON agent_memory_store  
FOR ALL  
USING (tenant_id = NULLIF(current_setting('app.current_tenant', true), ''));

The database adapter execution pattern maps the authenticated user principal to this session variable prior to query processing 8:

Python  
from sqlalchemy import text  
from sqlalchemy.orm import Session

def execute_agent_query(db_session: Session, user: UserContext, generated_sql: str) -> List:  
    """  
    Executes an LLM-generated SQL query safely.  
    Enforces Row-Level Security by setting the session variable before execution.  
    """  
    try:  
        # Enforce the active tenant parameter in the PostgreSQL transaction context  
        db_session.execute(  
            text("SET LOCAL app.current_tenant = :tenant"),  
            {"tenant": user.tenant_id}  
        )  
          
        # Execute the generated SQL query. RLS is enforced natively at the database engine level.  
        result = db_session.execute(text(generated_sql))  
        return [dict(row) for row in result.mappings()]  
    except Exception as e:  
        db_session.rollback()  
        raise SecurityException(f"SQL execution failed or blocked by policy engine: {str(e)}")

## **Versioning, Retention, Redaction, and Archival Policy Map**

Enterprise document repositories are dynamic environments.6 A document's legal authority changes as laws adapt, product lines deprecate, and retention schedules expire.14

```
 ──► [Approved] ──► [Active] ──► ──► ──► [Archived] ──►  
                                │                                            │  
                                └───(Legal Hold Active)──────────────────────┴──►
```

### **Versioning and Supersession Rules**

The corpus lifecycle engine enforces distinct state transitions to manage the valid lifespan of documents:

* **Draft**: The document is under revision and is completely excluded from active retrieval.  
* **Approved**: The document is verified and queued for index loading.  
* **Active**: The current standard reference. This document is eligible for standard retrieval.  
* **Deprecated**: The document's valid timestamp has expired, but it remains searchable for historical query cases.  
* **Superseded**: A newer version ID replaces this document. Active retrieval queries are automatically rerouted to the new file, while the older version remains linked for audit trail purposes.  
* **Archived**: The document is moved to low-performance cold storage and excluded from standard search indexes.14  
* **Legal Hold**: The document is marked with an active legal hold tag, freezing automatic deletion and retention triggers for audit compliance.14  
* **Redacted Derivative**: A version with PII and sensitive terms masked or replaced, allowing it to be used in lower-clearance contexts.13  
* **Deleted**: The document is cryptographically erased from all physical storage, memory caches, and vector indexes.

### **Records Management Alignment**

To support defensible deletion and structural archival, the corpus lifecycle engine must integrate standards derived from the United States Department of Defense records management strategies 14:

* **DoD Instruction 5015.02 / DoD 8180.01 Compliance**: Systems must prioritize programmatic records management over platform-centric architectures, recognizing that organizational records outlast any single software application's lifecycle.14  
* **DTM-22-001 Guidelines**: The database must support safe-harbor retention periods, secure metadata-based deletion schedules, and guarantee that data remains completely interoperable across storage layers.24

The policy map below defines the retention and redaction rules for enterprise knowledge classes:

| Ingestion Category | Retention Trigger Type | Retention Lifespan | Redaction Requirement | Archival State Destination |
| :---- | :---- | :---- | :---- | :---- |
| **Financial Ledgers** | Event-Based: Fiscal Year Closure.14 | 7 Years.14 | Mask individual account IDs and customer PII.13 | Encrypted database table; excluded from active RAG indexes. |
| **HR Employee Files** | Metadata-Based: Employee Termination. | 10 Years. | Complete redaction of SSNs, home addresses, and private medical histories.13 | Placed under read-only schema lock; restricted to authorized HR admins.8 |
| **Legal Contracts** | Time-Based: Expiration of terms. | 20 Years. | Redaction of third-party business names and trade secrets before vector load.13 | Retained on active index under strict legal-access scopes.8 |
| **Product Manuals** | Version-Based: Product End-of-Life. | Indefinite. | Low-risk; zero redaction required. | Moved to "Legacy Archive" index; matching queries receive a "Superseded" banner. |
| **Customer Tickets** | Metadata-Based: Resolution Status. | 1 Year. | Automatic extraction and deletion of customer credentials and billing tokens. | Cleared for automatic deletion from index unless marked with a Legal_Hold tag.14 |

## **Conflict Resolution and Epistemic Belief Revision**

When retrieval systems ingest conflicting inputs (such as an old API manual and a newer specification, or contradictory policy guidelines), standard RAG blindly merges the text.12 This forces the model into a **Context-Compliance Regime**, where it outputs incorrect or hallucinated claims because the retrieved context dominates its parametric memory.12

```
 ──► Step 1: Contextual Extraction (a_ctx)  
                              │  
                       Step 2: Parametric Extraction (a_param)  
                              │  
                       Step 3: Divergence Check (Compare a_ctx & a_param)  
                              │  
                              ├──(Divergence Detected?)──> Yes ──> Step 4: Premise Isolation  
                              │                                      │  
                              │                                    Step 5: Resolution Loop   
                              │                                      │  
                              │                                    [Output resolved, audited answer]  
                              │  
                              └── No ──>
```

### **Distinguishing Contradiction from Scoped Variation**

A contradiction is not always a quality failure. Two conflicting statements can both be true if their valid operational domains differ. The corpus engine must evaluate three core variables to determine whether a conflict represents a structural error or a scoped variation:

1. **Temporal Domain**: Statement X is valid for Fiscal Year 2025; Statement Y is valid for Fiscal Year 2026.  
2. **Jurisdiction / Region**: Statement X applies in California (CA); Statement Y applies in the European Union (EU).  
3. **Product / Customer Scope**: Statement X governs Enterprise customers; Statement Y governs Basic tier accounts.

If these scopes overlap entirely, a true contradiction exists, and the system must flag the objects as disputed.10

### **Context-Driven Decomposition (CDD) Framework**

To resolve these epistemic knowledge conflicts, systems must implement the **Context-Driven Decomposition (CDD)** belief-revision framework.12 CDD structures the inference process into five distinct steps to detect and arbitrate factual divergence:

* **Step 1: Contextual Extraction**: The model extracts the answer strictly as asserted by the retrieved context (a_ctx), ignoring its own parametric knowledge.12  
* **Step 2: Parametric Extraction**: The model generates the answer using only its pre-trained parametric memory (a_param).12  
* **Step 3: Divergence Check**: The system compares a_ctx and a_param to evaluate if they represent conflicting facts.12  
* **Step 4: Premise Isolation**: If a divergence is detected, the system extracts the discrete premises from the retrieved context C that drive the contradiction.12  
* **Step 5: Resolution**: The isolated premises are evaluated against source authority metrics, temporal timestamps, and metadata to output the final resolved answer, accompanied by an explicit conflict log.12

### **CDD Performance under Adversarial Evaluation**

The CDD framework significantly improves RAG accuracy under knowledge stress tests compared to standard retrieval baselines 12:

| Evaluation Setting | Standard RAG Accuracy | CDD Accuracy | Mistake-Injection Causal Sensitivity |
| :---- | :---- | :---- | :---- |
| **Entity Swap Attacks** | 58.4% 12 | **88.0%** 12 | Corrects malicious entity substitutions. |
| **Logical Contradictions** | 56.0% 12 | **83.2%** 12 | Identifies logical inconsistencies. |
| **Temporal Shifts (Drift)** | 68.8% 12 | **71.3%** 11 | Restores accuracy under historical drifts. |
| **Noisy Distractor Context** | 68.8% 12 | **69.9%** 11 | Ignores irrelevancies and out-of-scope files. |
| **TruthfulQA Misconceptions** | 15.0% 11 | **78.1%** 12 | Blocks model compliance with falsehoods. |

By treating knowledge conflict as an inference-time observable, CDD keeps the trace of factual dispute visible to compliance teams, ensuring that the final answer is auditable.10

## **Corpus Quality Evaluation and Observability**

Managing a continuous ingestion pipeline requires strict observability. Data drift, parser regressions, and metadata omissions will corrupt retrieval indexing if left unmonitored.2  
The corpus platform must monitor and evaluate its data quality against these fifteen critical metrics:

| Metric Name | Mathematical Definition / Formulation | Target SLA | Failure Mitigation Strategy |
| :---- | :---- | :---- | :---- |
| **1. Provenance Completeness** | (Objects with Valid source_uri) / (Total Ingested Objects) 3 | 100.00% | Reject ingestion of files missing verifiable parent URIs.3 |
| **2. Metadata Coverage** | (Objects with non-empty Mandatory Keys) / (Total Ingested Objects) | >99.90% | Route files with incomplete tags to human metadata-tagging queues.4 |
| **3. Exact Duplicate Rate** | (Identical Lexical Hash Matches) / (Total Ingested Documents) 21 | <1.00% | Discard identical files during Stage 8 pipeline execution.1 |
| **4. Near-Duplicate Rate** | (MinHash Jaccard Matches at T >= 0.85) / (Total Ingested Documents) 1 | <3.00% | Merge similar drafts; retain only the highest-authority version.1 |
| **5. Entity Resolution Accuracy** | (Correct Entity merges to Canonical ID) / (Total Extracted Entities) 9 | >95.00% | Refine entity profile builder templates; adjust LLM merge logic.9 |
| **6. Alias Coverage Rate** | (Resolved Entity Aliases) / (Total Entity Synonyms Extracted) 9 | >90.00% | Run background batch entity extraction and alias table updates.16 |
| **7. Permission Leakage Rate** | (Chunks retrieved without valid Role match) / (Total Evaluated Retrievals) 8 | 0.00% | Enforce security checks before vector scoring is calculated.8 |
| **8. Redaction Propagation** | (Derived elements correctly redacting term t) / (Total derived elements from parent D) | 100.00% | Cascade parent redactions instantly using parent-child lineage paths. |
| **9. Citation Fidelity** | (Generations verified by Cited Source) / (Total Citations Generated) 11 | >98.00% | Block output if cited context lacks semantic warrant for claims.11 |
| **10. Parse Error Rate** | (Mangled paragraph / layout outputs) / (Total Parsed Pages) 2 | <2.00% | Upgrade parsing pipelines to structure-aware layout engines.2 |
| **11. OCR/Table Read Accuracy** | (Correctly extracted cells) / (Total cells in evaluation set) 15 | >95.00% | Route documents with multi-row tables to Docling/TableFormer.15 |
| **12. Stale-Source Rate** | (Active Objects past valid_until date) / (Total Active Ingested Objects) | <0.50% | Trigger automated deprecation workflows; notify content owner. |
| **13. Conflict Detection Accuracy** | (True Contradictions Identified) / (Total Contradictions in System) 10 | >95.00% | Implement Context-Driven Decomposition (CDD) at query time.12 |
| **14. Lineage Complete Rate** | (Objects with complete PROV-O trail) / (Total Ingested Objects) 18 | 100.00% | Block ingestion of objects with incomplete activity or agent steps. |
| **15. Retrieval Eligibility** | (Candidate objects matching active scopes) / (Total Candidate set returned by index) | 100.00% | Reject candidates that violate geographic, version, or tenant scopes. |

## **Knowledge Hygiene Checklist**

Data preparation sets the quality ceiling for all downstream retrieval and generation processes.2 Ingestion teams must run this operational audit checklist prior to moving any document into active search indexes:

| Hygiene Step | Audit Criteria & Validation Rule | Operational System Trigger | Verification State |
| :---- | :---- | :---- | :---- |
| **Parser Validation** | Inspect 20 representative parsing outputs.2 Confirm no multi-column text merges horizontally.15 | Run parser comparison test.2 | [ ] Pending |
| **OCR/Table Check** | Verify multi-row table cells map to correct parents.15 | TableFormer structural analysis run.15 | [ ] Pending |
| **Encoding Cleanup** | Strip non-printable characters; repair broken UTF-8 bytes. | Regex syntax validation script. | [ ] Pending |
| **Boilerplate Removal** | Confirm headers, footers, and side navs are completely removed.2 | Sentence frequency analysis pass.1 | [ ] Pending |
| **Metadata Verification** | Enforce non-empty tags for mandatory schema keys.1 | Schema validator engine pass.1 | [ ] Pending |
| **Deduplication Pass** | Run exact byte hashing and MinHash duplicate checking.1 | Jaccard threshold evaluation pass.2 | [ ] Pending |
| **Alias Resolution** | Confirm entity variants map to stable Canonical IDs.9 | Reconciler LLM check.9 | [ ] Pending |
| **Redaction Execution** | Verify PII and credentials are masked or removed.2 | Automated DLP scanner check.13 | [ ] Pending |
| **Source Authority** | Verify authority tiers are assigned correctly (As).10 | Connector source authority mapping match.10 | [ ] Pending |
| **Version Verification** | Confirm document is marked active and has not expired.14 | Temporal validity window verification check.14 | [ ] Pending |
| **Permission Check** | Confirm SharePoint/NTFS ACLs match database parameters.23 | Entra ID query test run.8 | [ ] Pending |
| **Citation Registration** | Verify document carries a valid citation format.3 | Schema citation tag check.11 | [ ] Pending |

## **Cross-Canon Handoff Map**

Corpus engineering defines the governed data substrate beneath retrieval mechanics, freshness policies, context design, and compliance systems. The following table establishes how this report coordinates with other components of the AI systems canon.

| Upstream Report Reference | Downstream Target Report | System-Level Interface Mechanics | Canon Integration Objective |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | AI-ENG-D — Corpus Engineering | Context-side objects inherit the temporal validity, tenant scope, and owner fields defined in the corpus schema. | Connects context-compilation boundaries directly to source data governance. |
| **AI-ENG-C — Inference Economics** | AI-ENG-D — Corpus Engineering | Minimizes chunk bloat, duplicate files, and boilerplate text at the ingestion layer to control context costs.1 | Protects system margin by removing expensive, redundant context tokens.20 |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-E — Retrieval Mechanics** | Delivers normalized, permission-aware, and deduplicated candidate sets to the retrieval engine.8 | Establishes the secure dataset that query-writing and embedding models search.8 |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-F — Knowledge Freshness** | Provides version, supersession, and review triggers to control knowledge-base deprecation and freshness.14 | Drives the background routines that rebuild stale chunks and clear deprecated vectors. |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-T — Tenant Isolation** | Maps the explicit tenant and partition boundaries required to prevent multi-tenant data leaks.8 | Natively isolates user access before vector searches touch physical memory.8 |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-U — Supply-Chain Security** | Enforces sensitivity inheritance and redaction on derived assets to prevent data laundering.7 | Prevents high-classification data from leaking into lower-security models. |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-Z — Systems Telemetry** | Records execution timings, PROV-O traces, and parser performance to systems telemetry dashboards.7 | Monitors pipeline latency, extraction failures, and near-duplicate bloat in real time. |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-AA — Retrieval Evals** | Supplies ground-truth citation links and verified claims for retrieval and grounding evaluations.5 | Evaluates citation accuracy against direct source document coordinates.11 |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-AB — Auditability Trails** | Maintains a complete, immutable lineage record from raw source to generated output.18 | Satisfies legal compliance audits and forensic query reconstruction.14 |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-AD — Data Governance** | Enforces organizational compliance, retention rules, and DoD-grade records management standards.14 | Aligns the AI platform with corporate risk, legal discovery, and legal compliance policies. |
| **AI-ENG-D — Corpus Engineering** | **AI-ENG-AH — Build/Buy Strategy** | Compares parsing speed, table accuracy, and costs (such as Docling vs. LlamaParse APIs).15 | Guides resource allocation between local systems and vendor platforms. |

## **Durable Principles of Corpus Engineering**

* **The Relevance Fallacy**: Semantic relevance is downstream of authority, validity, and permission.8 A passage can be highly relevant to a query but still be unapproved, out-of-scope, or completely restricted under organizational access control policies.7  
* **The Lineage Mandate**: No derivative asset (chunk, summary, memory, embedding) may exist without an unbroken, machine-readable PROV-O lineage trail linking it back to its parent source file.18  
* **Pre-Retrieval Enforcement**: User access rights and group memberships must be evaluated and filtered **before** vector scoring takes place.8 Post-hoc filtering of retrieved vectors is a security vulnerability.7  
* **The Sensitivity Inheritance Rule**: Derived items must inherit the identical classification and access restriction parameters of their source material, preventing the accidental laundering of private data.7  
* **The Parser Quality Ceiling**: No downstream model optimizations can compensate for corrupted, misaligned, or poorly extracted document structures.2 Invest in layout-aware, cell-accurate parsing models.15  
* **The Epistemic Isolation Rule**: Contradictions between data sources must not be flattened into a single, merged answer.12 Systems must use belief-decomposition to isolate, flag, and log factual variations.12  
* **The System Independence Axiom**: Corporate records and their associated retention rules will outlive the lifespan of any single AI tool or vector database.14 Design systems for interoperable metadata storage and defensible erasure.14

#### **Works cited**

1. Build an unstructured data pipeline for RAG | Databricks on AWS, accessed June 6, 2026, [https://docs.databricks.com/aws/en/generative-ai/tutorials/ai-cookbook/quality-data-pipeline-rag](https://docs.databricks.com/aws/en/generative-ai/tutorials/ai-cookbook/quality-data-pipeline-rag)  
2. RAG Data Preparation: The Foundation That Makes or Breaks Your AI System | sph.sh, accessed June 6, 2026, [https://sph.sh/en/posts/rag-data-preparation/](https://sph.sh/en/posts/rag-data-preparation/)  
3. Your tasks: Data provenance - RDMkit, accessed June 6, 2026, [https://rdmkit.elixir-europe.org/data_provenance](https://rdmkit.elixir-europe.org/data_provenance)  
4. Unstructured Data Extraction: Turn Documents into Insights - LlamaIndex, accessed June 6, 2026, [https://www.llamaindex.ai/blog/unstructured-data-extraction](https://www.llamaindex.ai/blog/unstructured-data-extraction)  
5. Extract Data From PDF Tables: Row-Level Guide - LlamaIndex, accessed June 6, 2026, [https://www.llamaindex.ai/blog/extracting-repeating-entities-from-documents](https://www.llamaindex.ai/blog/extracting-repeating-entities-from-documents)  
6. Introducing ACL Hydration: secure knowledge workflows for agentic ..., accessed June 6, 2026, [https://www.datarobot.com/blog/acl-hydration/](https://www.datarobot.com/blog/acl-hydration/)  
7. Permissions-Aware Authorization for RAG Pipelines - Cerbos, accessed June 6, 2026, [https://www.cerbos.dev/features-benefits-and-use-cases/access-control-for-rag](https://www.cerbos.dev/features-benefits-and-use-cases/access-control-for-rag)  
8. Secure RAG: Authorisation-Aware Retrieval and Row-Level Security ..., accessed June 6, 2026, [https://photokheecher.medium.com/secure-rag-authorisation-aware-retrieval-and-row-level-security-c6542500ec21](https://photokheecher.medium.com/secure-rag-authorisation-aware-retrieval-and-row-level-security-c6542500ec21)  
9. Proxy-Pointer RAG: Solving Entity and Relationship Sprawl in Large ..., accessed June 6, 2026, [https://towardsdatascience.com/proxy-pointer-rag-solving-entity-and-relationship-sprawl-in-large-knowledge-graphs/](https://towardsdatascience.com/proxy-pointer-rag-solving-entity-and-relationship-sprawl-in-large-knowledge-graphs/)  
10. What's the best way to handle conflicting sources in a RAG system? - Reddit, accessed June 6, 2026, [https://www.reddit.com/r/Rag/comments/1r6i4m3/whats_the_best_way_to_handle_conflicting_sources/](https://www.reddit.com/r/Rag/comments/1r6i4m3/whats_the_best_way_to_handle_conflicting_sources/)  
11. Shuhuai Lin - CatalyzeX, accessed June 6, 2026, [https://www.catalyzex.com/author/Shuhuai%20Lin](https://www.catalyzex.com/author/Shuhuai%20Lin)  
12. Does RAG Know When Retrieval Is Wrong? Diagnosing Context Compliance under Knowledge Conflict - arXiv, accessed June 6, 2026, [https://arxiv.org/html/2605.14473v1](https://arxiv.org/html/2605.14473v1)  
13. Secure AI Governance & Permission-Aware RAG - FileOrbis, accessed June 6, 2026, [https://www.fileorbis.com/platform/ai-governance/](https://www.fileorbis.com/platform/ai-governance/)  
14. DoD 5015.02 Certified Software for Records Management - ZL Technologies, accessed June 6, 2026, [https://www.zlti.com/regulations/dod-certified-software/](https://www.zlti.com/regulations/dod-certified-software/)  
15. PDF Data Extraction Benchmark 2025: Comparing Docling ..., accessed June 6, 2026, [https://procycons.com/en/blogs/pdf-data-extraction-benchmark/](https://procycons.com/en/blogs/pdf-data-extraction-benchmark/)  
16. Entity resolution with Elasticsearch & LLMs, Part 2: Matching entities with LLM judgment and semantic search, accessed June 6, 2026, [https://www.elastic.co/search-labs/blog/elasticsearch-entity-resolution-llm-semantic-search](https://www.elastic.co/search-labs/blog/elasticsearch-entity-resolution-llm-semantic-search)  
17. Multi-Agent RAG Framework for Entity Resolution: Advancing Beyond Single-LLM Approaches with Specialized Agent Coordination - ResearchGate, accessed June 6, 2026, [https://www.researchgate.net/publication/398342865_Multi-Agent_RAG_Framework_for_Entity_Resolution_Advancing_Beyond_Single-LLM_Approaches_with_Specialized_Agent_Coordination](https://www.researchgate.net/publication/398342865_Multi-Agent_RAG_Framework_for_Entity_Resolution_Advancing_Beyond_Single-LLM_Approaches_with_Specialized_Agent_Coordination)  
18. PROV - Metadata Standards Catalog, accessed June 6, 2026, [https://rdamsc.bath.ac.uk/msc/m33](https://rdamsc.bath.ac.uk/msc/m33)  
19. ProvONE: A PROV Extension Data Model for Scientific Workflow Provenance - DataONE PURLs, accessed June 6, 2026, [https://purl.dataone.org/provone-v1-dev](https://purl.dataone.org/provone-v1-dev)  
20. Byte-Exact Deduplication in Retrieval-Augmented Generation: A Three-Regime Empirical Analysis Across Public Benchmarks - arXiv, accessed June 6, 2026, [https://arxiv.org/html/2605.09611v1](https://arxiv.org/html/2605.09611v1)  
21. Reducing Redundancy in Retrieval-Augmented Generation through Chunk Filtering - arXiv, accessed June 6, 2026, [https://arxiv.org/html/2604.24334v1](https://arxiv.org/html/2604.24334v1)  
22. Improve MinhashLSH for Deduplication on Large Scale Dataset, accessed June 6, 2026, [https://tech.preferred.jp/en/blog/improve-minhashlsh-for-deduplication-on-large-scale-dataset/](https://tech.preferred.jp/en/blog/improve-minhashlsh-for-deduplication-on-large-scale-dataset/)  
23. Document-Level Access Control - Azure AI Search | Microsoft Learn, accessed June 6, 2026, [https://learn.microsoft.com/en-us/azure/search/search-document-level-access-overview](https://learn.microsoft.com/en-us/azure/search/search-document-level-access-overview)  
24. DoD 8180.01, 5015.02 and DTM-22-001 – Everything You Need to ..., accessed June 6, 2026, [https://www.laserfiche.com/resources/blog/dod-8180-01-5015-02-and-dtm-22-001-everything-you-need-to-know/](https://www.laserfiche.com/resources/blog/dod-8180-01-5015-02-and-dtm-22-001-everything-you-need-to-know/)  
25. DoDI 5015.02, February 24, 2015, Incorporating Change 1 on August 17, 2017 - Executive Services Directorate, accessed June 6, 2026, [https://www.esd.whs.mil/portals/54/documents/dd/issuances/dodi/501502p.pdf](https://www.esd.whs.mil/portals/54/documents/dd/issuances/dodi/501502p.pdf)  
26. Does RAG Know When Retrieval Is Wrong? Diagnosing Context Compliance under Knowledge Conflict - arXiv, accessed June 6, 2026, [https://arxiv.org/html/2605.14473](https://arxiv.org/html/2605.14473)  
27. LlamaParse vs. Unstructured: Which platform delivers better document parsing?, accessed June 6, 2026, [https://www.llamaindex.ai/compare/llamaparse-vs-unstructured](https://www.llamaindex.ai/compare/llamaparse-vs-unstructured)  
28. Document Parsing Benchmarks: Unstructured vs. the Field, accessed June 6, 2026, [https://unstructured.io/benchmarks](https://unstructured.io/benchmarks)  
29. Attribution Metadata Standard and Use Case Examples - Research Data Alliance (RDA), accessed June 6, 2026, [https://www.rd-alliance.org/system/files/RDA%20Attribution%20Metadata%20Standard%20and%20Use%20Cases.pdf](https://www.rd-alliance.org/system/files/RDA%20Attribution%20Metadata%20Standard%20and%20Use%20Cases.pdf)  
30. Does RAG Know When Retrieval Is Wrong? Diagnosing Context Compliance under Knowledge Conflict - arXiv, accessed June 6, 2026, [https://arxiv.org/pdf/2605.14473](https://arxiv.org/pdf/2605.14473)  
31. [2605.14473] Does RAG Know When Retrieval Is Wrong? Diagnosing Context Compliance under Knowledge Conflict - arXiv, accessed June 6, 2026, [https://arxiv.org/abs/2605.14473](https://arxiv.org/abs/2605.14473)

---

[← Back to Canon Map](../canon-map.md)