# [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems

[Volume 6. P-R Multimodal and Interface-Controlling Systems]
  - [AI-ENG-P — Multimodal Understanding - Documents, Images, Tables, Charts & Video](#ai-eng-p--multimodal-understanding---documents-images-tables-charts--video)
  - [AI-ENG-Q — Speech, Voice, and Real-Time Interaction Systems](#ai-eng-q--speech-voice-and-real-time-interaction-systems)  
  - [AI-ENG-R — UI Agents - Browser Control, Desktop Automation & Visual State](#ai-eng-r--ui-agents---browser-control-desktop-automation--visual-state)  

---

# AI-ENG-P — Multimodal Understanding - Documents, Images, Tables, Charts & Video

## **I. Doctrinal Foundations: The Evidence-Grounded Paradigm**

The structural integrity of a multimodal AI system depends on a fundamental rule:

**Visual, spatial, tabular, and temporal structure is semantic content.**

Documents, images, charts, tables, diagrams, forms, screenshots, and videos are not merely containers for text. Their meaning is carried by coordinates, layout, sequence, grouping, scale, orientation, color, timing, and visual adjacency. Flattening these artifacts into unstructured text destroys evidence.

A multimodal system has not understood an artifact merely because the artifact entered the model context. Understanding requires the system to identify the precise evidence unit supporting a claim and preserve the coordinate chain back to the source.

That evidence unit may be:

* a page region
* a paragraph bounding box
* a table cell plus row and column headers
* a form label-value pair
* a chart axis, legend, tick, and mark
* a visual object bounding box
* a video frame or timestamp interval
* a UI element coordinate
* a multi-page continuation structure
* a suppressed-but-retained header, footer, footnote, caption, or watermark

The evidence-grounded paradigm distinguishes several operations that are often blurred together:

| Operation | Meaning | Failure if Conflated |
| :--- | :--- | :--- |
| **Ingest** | Load the artifact and identify its media type. | The system may possess the file but understand nothing about its structure. |
| **Route** | Select the correct processing path by modality and quality. | Text-native documents may be over-processed; visual documents may be under-processed. |
| **Parse** | Extract text, layout, visual objects, tables, charts, or temporal events. | Structure may collapse into misleading text. |
| **Localize** | Identify coordinates of relevant evidence. | The answer may cite a document but not the supporting region. |
| **Inspect** | Verify the crop, page, frame, or region is adequate for the task. | Tiny text, missing headers, or cropped footnotes may produce false certainty. |
| **Extract** | Convert evidence into structured data. | A number may be separated from its units, headers, or footnotes. |
| **Verify** | Check that the extracted data supports the claim. | A plausible summary may outrun the visual evidence. |
| **Cite** | Return source coordinates and provenance. | The result becomes unauditable. |
| **Replay** | Preserve enough artifacts to reproduce the claim. | Regression testing and incident review become impossible. |

Plausible text is not visual grounding. A system that describes a chart without reading the axes, summarizes a table without binding headers, or answers from a video without locating the relevant event is producing ungrounded synthesis.

This report extends prior canon doctrines into multimodal evidence systems:

| Prior Canon Module | Multimodal Extension |
| :--- | :--- |
| **AI-ENG-B — Context Tenure and State Governance** | Visual artifacts, page crops, frame selections, and coordinate maps require lifespan, versioning, and authority boundaries. |
| **AI-ENG-D — Corpus Engineering and Source Authority** | Multimodal sources require provenance, file hashes, rendering versions, parser versions, and media-quality metadata. |
| **AI-ENG-E — Retrieval Pipelines** | Retrieval must select modality-appropriate evidence packets rather than flattening all evidence into text chunks. |
| **AI-ENG-L — Serving Architecture** | Multimodal rendering, OCR, visual retrieval, and video sampling create distinct latency, memory, and cost envelopes. |
| **AI-ENG-O — Action Verification** | User-facing claims must not outrun verified visual, spatial, tabular, or temporal evidence. |
| **AI-ENG-AB — Auditability and Replay** | Multimodal claims require replayable coordinate chains, page images, crops, parser versions, and extraction records. |

### **Artifact 1: Conceptual Glossary**

| Term | Definition | Operational Signal |
| :--- | :--- | :--- |
| **Multimodal Understanding** | Evidence-grounded interpretation of visual, spatial, tabular, document, image, or temporal artifacts. | Claim-to-evidence grounding ratio. |
| **Evidence Unit** | The smallest source region sufficient to support a claim. | Page box, cell, chart mark, frame range, object box. |
| **Coordinate Chain** | The preserved mapping from generated claim back to source artifact coordinates. | Source ID -> page/frame -> region -> crop -> extraction. |
| **Layout Preservation** | Retaining page hierarchy, reading order, columns, captions, headers, footnotes, and object relations. | Reading-order F1, layout IoU, hierarchy preservation rate. |
| **Spatial Grounding** | Binding claims to explicit coordinates or bounding boxes. | IoU@threshold, region hit rate, localization precision. |
| **Tabular Grounding** | Binding values to table cells, row headers, column headers, units, captions, and footnotes. | Cell-header binding accuracy, GriTS/TEDS, numeric exactness. |
| **Chart Derendering** | Converting chart pixels into structured series, axes, legends, and values. | Relative error tolerance, axis/legend extraction accuracy. |
| **Temporal Grounding** | Binding video claims to timestamp ranges, selected frames, and event boundaries. | Temporal IoU, frame selection sensitivity, event recall. |
| **Inspection Adequacy** | Determining whether selected evidence is sufficiently complete, legible, and contextualized. | Resolution, crop coverage, header/footnote inclusion, uncertainty flags. |
| **Citation Fidelity** | Precision of attribution from claim to source evidence. | Document, page, region, cell, mark, frame, or timestamp-level citation. |
| **Modality Routing** | Selecting the correct parser/retriever based on file properties and query intent. | Route accuracy, fallback rate, cost avoided, failure rate. |

### **Artifact 2: Multimodal Evidence Pipeline**

```text
+--------------------------------------------------------------------------------
| MULTIMODAL EVIDENCE PIPELINE
+--------------------------------------------------------------------------------
|
| [ Source Artifact ]
|   PDF | scan | image | form | table | chart | screenshot | video
|        |
|        v
| [ Ingestion and Fingerprinting ]
|   file hash | media type | page count | duration | embedded text | image coverage
|        |
|        v
| [ Modality Routing ]
|   text-native path | visual document path | table path | chart path | video path
|        |
|        v
| [ Parsing and Structural Extraction ]
|   text spans | layout graph | OCR boxes | objects | cells | axes | frames
|        |
|        v
| [ Coordinate Registry ]
|   page boxes | cell boxes | chart marks | object boxes | timestamp ranges
|        |
|        v
| [ Retrieval and Localization ]
|   lexical | dense | multi-vector | spatial | table | chart | temporal
|        |
|        v
| [ Inspection Adequacy Gate ]
|   resolution | crop coverage | context completeness | uncertainty | fallback
|        |
|        v
| [ High-Precision Extraction ]
|   form fields | table cells | chart data | visual objects | temporal events
|        |
|        v
| [ Evidence Verification ]
|   headers | units | axis scale | labels | tenant/source | version | time range
|        |
|        v
| [ Grounded Synthesis ]
|   answer generated only from selected, adequate, cited evidence
|        |
|        v
| [ Citation and Replay Package ]
|   source hash | page/frame | coordinates | crops | parser version | evidence map
|
+--------------------------------------------------------------------------------
| Invariant:
|   No factual multimodal claim should be emitted without a coordinate-bearing
|   evidence packet or an explicit uncertainty/degraded-mode statement.
+--------------------------------------------------------------------------------
```

### **Evidence-Grounded Output Rule**

A multimodal answer should carry three things:

| Required Output | Purpose |
| :--- | :--- |
| **Claim** | What the system asserts. |
| **Evidence packet** | What region, cell, chart mark, or frame supports it. |
| **Adequacy status** | Whether the evidence was legible, complete, and verified enough to support the claim. |

When evidence is insufficient, the system should say so directly:

```text
The available crop does not include the table header, so the value cannot be
safely attributed to a column. Retrieve the full table region or adjacent page.
```

## **II. Document Ingestion, OCR, and OCR-Free VLM Pipelines**

Document ingestion is the boundary where raw files become structured evidence. A PDF, scan, or image must be classified before parsing because the correct processing path depends on document properties, not merely file extension.

A brittle routing rule such as “more than 100 extracted characters means text-native” is insufficient. A document may contain extractable text while still having broken font encodings, scrambled reading order, image-only tables, scanned appendices, rotated pages, hidden OCR layers, or layout-dependent meaning.

### **Artifact 3: Document Ingestion Routing Classifier**

A robust ingestion classifier uses multiple signals:

| Signal | What It Detects | Routing Implication |
| :--- | :--- | :--- |
| **Extractable text density** | Whether the file contains native text. | High density may route to text-native parsing, but only if quality checks pass. |
| **Glyph map validity** | Whether extracted characters map correctly. | Invalid encodings route to visual/OCR path. |
| **Reading-order confidence** | Whether extracted text preserves logical order. | Low confidence triggers layout-aware parsing. |
| **Raster/image coverage** | Whether page content is mostly embedded images. | High coverage routes to visual page rendering. |
| **Table/figure density** | Whether layout carries meaning. | High density triggers structural extraction even for text-native files. |
| **Font size and contrast** | Whether small text can be resolved. | Microtext or low contrast triggers higher DPI rendering. |
| **Rotation/skew** | Whether page orientation disrupts extraction. | Triggers deskewing and orientation correction. |
| **Language/script profile** | OCR and parser suitability. | Routes to appropriate OCR/VLM or human-review fallback. |
| **Security/provenance flags** | Macros, embedded files, strange streams, untrusted origin. | Routes to sandboxed parsing or quarantine. |

### **Document Parsing Stack Model**

| Layer | Input | Processing Function | Output Artifact | Fallback Trigger |
| :--- | :--- | :--- | :--- | :--- |
| **1. File Ingestion** | PDF, image, scan, screenshot, office export. | Fingerprint, hash, media-type detection, page count, metadata capture. | Source artifact record. | Quarantine malformed, encrypted, or unsafe files. |
| **2. Routing Classification** | Source artifact record. | Multi-signal routing classifier. | Parse route: text-native, visual, hybrid, table-heavy, chart-heavy, form, video. | Route to hybrid visual/text path if confidence is low. |
| **3. Text-Native Extraction** | Digital PDF or office export. | Extract text spans, font metadata, page coordinates, links, annotations. | Text span graph with coordinates. | Broken glyphs, scrambled order, missing layout, or low confidence. |
| **4. Page Rendering** | Pages requiring visual processing. | Render page images at task-appropriate DPI. | Page image store with render metadata. | Increase DPI for microtext, low contrast, stamps, or handwriting. |
| **5. OCR / VLM Layout Parsing** | Page images and text spans. | OCR, OCR-free VLM parsing, layout detection, structural tagging. | Layout graph: blocks, tables, figures, captions, form fields. | Fallback to alternate OCR/VLM engine or human review. |
| **6. Reading-Order Reconstruction** | Layout graph. | Resolve columns, marginalia, footnotes, headers, captions, and equations. | Ordered document tree. | Low reading-order confidence or conflicting layout paths. |
| **7. Coordinate Registry** | Ordered document tree and page images. | Assign stable IDs to spans, regions, cells, figures, captions, and suppressed metadata. | Coordinate-bearing evidence registry. | Missing coordinates, invalid boxes, or page mismatch. |
| **8. Evidence Packet Compiler** | Coordinate registry. | Produce retrieval-ready text, visual, spatial, table, and citation packets. | Multimodal evidence packets. | Fallback to page-level packet with uncertainty flag. |

### **Artifact 4: Layout Preservation Framework**

```text
+--------------------------------------------------------------------------------
| LAYOUT PRESERVATION FRAMEWORK
+--------------------------------------------------------------------------------
|
| Source Page:
|   page_id: p001
|   coordinate_system: normalized_0_1000
|   width: 1000
|   height: 1000
|
| Layout Graph:
|
|   header_001
|     class: running_header
|     text: "Journal of Systems Architecture"
|     bbox: [60, 25, 940, 60]
|     stream_policy: suppress_from_main_text
|     retention_policy: retain_as_metadata
|
|   col_001
|     class: column
|     bbox: [70, 110, 470, 860]
|     children:
|       h1_001
|         class: heading
|         text: "1. Introduction"
|         bbox: [75, 115, 460, 150]
|       para_001
|         class: paragraph
|         text: "The visual structures..."
|         bbox: [75, 165, 460, 315]
|       eq_001
|         class: equation
|         latex: "S(q,d)=..."
|         bbox: [95, 330, 430, 375]
|       table_001
|         class: table
|         bbox: [75, 405, 465, 745]
|         caption_ref: caption_001
|
|   col_002
|     class: column
|     bbox: [530, 110, 930, 860]
|     children:
|       para_002
|         class: paragraph
|         text: "The experimental results..."
|         bbox: [535, 115, 920, 260]
|       fig_001
|         class: figure
|         bbox: [540, 285, 915, 610]
|       caption_001
|         class: figure_caption
|         text: "Figure 2: Grounding pipeline."
|         bbox: [540, 620, 915, 675]
|         parent_ref: fig_001
|
|   footer_001
|     class: page_footer
|     text: "Page 1 of 15"
|     bbox: [420, 945, 580, 970]
|     stream_policy: suppress_from_main_text
|     retention_policy: retain_as_metadata
|
+--------------------------------------------------------------------------------
| Rule:
|   Suppressed is not deleted. Headers, footers, watermarks, confidentiality
|   labels, footnotes, and captions may be omitted from the main reading stream
|   while still retained as coordinate-bearing metadata.
+--------------------------------------------------------------------------------
```

### **Layout Node Schema**

```json
{
  "node_id": "para_001",
  "page_id": "p001",
  "class": "paragraph",
  "bbox": [75, 165, 460, 315],
  "text": "The visual structures...",
  "reading_order_index": 3,
  "parent_id": "col_001",
  "children": [],
  "stream_policy": "include_in_main_text",
  "confidence": 0.97,
  "parser": {
    "name": "layout_parser",
    "version": "v1.4.2"
  }
}
```

### **Document Parsing Invariant**

The parser should never produce a plain text stream alone. At minimum, it must also produce:

* page ID
* bounding boxes
* reading-order indices
* structural classes
* parser version
* confidence scores
* suppressed metadata records
* source hash
* rendering metadata when rasterization was used

A document without coordinates is not yet evidence. It is just text wearing a fake mustache.

## **III. Forms and Field Understanding: Spatial-Semantic Association**

Forms are spatial contracts. Their meaning depends on label-value relationships, checkboxes, signatures, stamps, handwriting, tables, alignment, repeated sections, and intentional blank space.

A form parser must not simply extract text. It must bind each value to the correct label, state, field type, coordinate region, and confidence score.

### **Artifact 5: Form Understanding Model**

```json
{
  "form_id": "invoice_form_2026_0042",
  "source_document_id": "doc_9ab3",
  "page_id": "p001",
  "coordinate_system": {
    "type": "normalized",
    "width": 1000,
    "height": 1000
  },
  "page_orientation_degrees": 0.0,
  "template_match": {
    "template_id": "vendor_invoice_v3",
    "confidence": 0.94
  },
  "fields": [
    {
      "field_id": "vendor_name",
      "field_type": "text",
      "state": "present",
      "label": {
        "text": "Vendor Name",
        "bbox": [70, 118, 215, 145],
        "confidence": 0.99
      },
      "value": {
        "text": "ACME Industrial Corp.",
        "bbox": [240, 116, 610, 148],
        "extraction_modality": "native_text",
        "confidence": 0.99
      }
    },
    {
      "field_id": "invoice_date",
      "field_type": "handwritten_text",
      "state": "present",
      "label": {
        "text": "Invoice Date",
        "bbox": [70, 168, 220, 195],
        "confidence": 0.98
      },
      "value": {
        "text": "04/05/2026",
        "bbox": [245, 162, 430, 205],
        "extraction_modality": "handwriting_ocr",
        "confidence": 0.895
      }
    },
    {
      "field_id": "tax_exempt_status",
      "field_type": "checkbox",
      "state": "checked",
      "label": {
        "text": "Tax Exempt",
        "bbox": [70, 228, 215, 255],
        "confidence": 0.97
      },
      "value": {
        "bbox": [238, 224, 260, 246],
        "active_mark_ratio": 0.782,
        "local_background_adjusted": true,
        "confidence": 0.981
      }
    },
    {
      "field_id": "authorized_signature",
      "field_type": "signature",
      "state": "present",
      "label": {
        "text": "Authorized Signature",
        "bbox": [70, 765, 300, 792],
        "confidence": 0.96
      },
      "value": {
        "bbox": [315, 735, 720, 815],
        "ink_pixel_count": 14203,
        "signature_presence_class": "scribble_present",
        "confidence": 0.994
      }
    },
    {
      "field_id": "purchase_order_number",
      "field_type": "text",
      "state": "intentionally_empty",
      "label": {
        "text": "PO Number",
        "bbox": [70, 285, 195, 312],
        "confidence": 0.96
      },
      "value": {
        "text": null,
        "bbox": [240, 280, 520, 318],
        "blank_region_confidence": 0.93
      }
    }
  ],
  "verification_logs": [
    {
      "check": "stamp_or_seal_presence",
      "state": "present",
      "bbox": [745, 710, 895, 835],
      "confidence": 0.97
    },
    {
      "check": "required_fields_present",
      "state": "passed",
      "missing_required_fields": []
    }
  ]
}
```

### **Field State Taxonomy**

| Field State | Meaning | System Handling |
| :--- | :--- | :--- |
| **present** | Value or mark detected with sufficient confidence. | Commit extracted value with coordinates. |
| **absent** | Expected field not visually present in the form instance. | Flag template mismatch or missing field. |
| **checked** | Checkbox/radio/select mark is active. | Commit boolean/choice value with confidence. |
| **unchecked** | Field exists and mark area is empty. | Commit explicit false/unchecked state. |
| **ambiguous** | Mark, handwriting, or label-value binding is uncertain. | Route to fallback extraction or human review. |
| **occluded** | Region is covered, cut off, blurred, or obstructed. | Mark unresolved; do not infer value. |
| **extraction_failed** | Region exists but parser could not read it. | Preserve coordinates and failure reason. |
| **intentionally_empty** | Field exists and region appears blank. | Commit blank state only when region was inspected. |

### **Form Failure Mode Map**

| Failure Mode | Symptom | Root Cause | Mitigation |
| :--- | :--- | :--- | :--- |
| **Label-Value Cross-Binding** | Value assigned to wrong label. | Multi-column or irregular layout misleads nearest-neighbor binding. | Use template anchors, reading-order graph, and label-value vector scoring. |
| **Checkbox Inversion** | Checked box read as unchecked or vice versa. | Noise, compression, light pencil marks, or form artifacts. | Use local background-normalized mark ratio and ambiguity threshold. |
| **Signature / Stamp Omission** | Valid authorization mark ignored. | OCR-only pipeline treats non-text marks as noise. | Add visual object heads for signatures, stamps, seals, and handwritten marks. |
| **Intentional Blank Collapse** | Empty field omitted and mistaken for parser failure. | Parser only emits detected text. | Emit all template fields with explicit inspected blank states. |
| **Template Drift** | Fields shifted or renamed across form versions. | Template changed without schema update. | Version templates and use geometry + text anchors. |
| **Handwriting Ambiguity** | Low-confidence handwritten value becomes false fact. | OCR uncertainty below threshold. | Preserve uncertain text, route to human review, or require second model pass. |

### **Form Evidence Rule**

A form value is not evidence unless it carries:

```text
field_id
field_type
label_bbox
value_bbox
field_state
extraction_modality
confidence
template_version
source_page
```

The parser must record blanks, ambiguity, and extraction failure explicitly. Silence is not a field state.

## **IV. Tables as Structured Evidence: Multi-Page Recognition, Split-Merge, and Graph-Based Transformations**

Tables are structured evidence. A number inside a table is not meaningful unless the system preserves its row header, column header, units, spanning cells, footnotes, caption, page context, and continuation state.

Flattening a table into prose destroys the relationships that make the data true.

### **Artifact 6: Table Evidence Lifecycle**

```text
+--------------------------------------------------------------------------------
| TABLE EVIDENCE LIFECYCLE
+--------------------------------------------------------------------------------
|
| [ Page or Crop ]
|      |
|      v
| [ Table Detection ]
|   locate table region | caption | footer | nearby notes
|      |
|      v
| [ Structure Recognition ]
|   rows | columns | spanning cells | projected headers | merged cells
|      |
|      v
| [ Cell Content Extraction ]
|   text | numbers | units | confidence | OCR/native source
|      |
|      v
| [ Header Binding ]
|   row headers | column headers | superheaders | units | footnotes
|      |
|      v
| [ Continuity Resolution ]
|   page N/N+1 relation | repeated header | row offset | terminal totals
|      |
|      v
| [ Table Object Compilation ]
|   cells with coordinates, values, headers, provenance, confidence
|      |
|      v
| [ Claim-Level Table Citation ]
|   cite value + row header + column header + table/caption/footnote
|
+--------------------------------------------------------------------------------
```

### **Table Extraction Architecture**

| Architectural Path | Best Fit | Processing Paradigm | Output Artifact | Verification Metric |
| :--- | :--- | :--- | :--- | :--- |
| **Full-Page Table Graph Recognition** | Tables embedded in reports with captions, footers, and surrounding explanatory text. | Detect table objects and structural relations in full-page context. | Table graph with parent-child relationships, caption, footer, cells. | GriTS, TEDS, cell-header binding accuracy. |
| **Split-Merge Grid Recognition** | Dense regular grids, complex merged cells, and large financial/statistical tables. | Detect row/column separators, then classify merged cells. | Grid object with split lines, merge regions, and cell IDs. | TEDS-Struc, merge accuracy, cell adjacency F1. |
| **Native Digital Table Extraction** | Born-digital PDFs or spreadsheets with recoverable text and coordinates. | Extract text spans and infer grid from coordinates and ruling lines. | Coordinate-bearing table cells. | Exact text match, cell order, row/column alignment. |
| **Hybrid Table Extraction** | Mixed native text and scanned visual structures. | Combine visual structure detection with native text span assignment. | Table graph with OCR/native source flags. | Header binding and content preservation. |

### **Table Object Schema**

```json
{
  "table_id": "tbl_p012_001",
  "source_document_id": "doc_9ab3",
  "page_ids": ["p012", "p013"],
  "bbox": [80, 145, 930, 850],
  "caption": {
    "text": "Table 4. Quarterly operating margin by segment.",
    "bbox": [80, 105, 930, 135]
  },
  "continuation": {
    "is_continued": true,
    "continued_from": null,
    "continued_to": "tbl_p013_001",
    "confidence": 0.91
  },
  "cells": [
    {
      "cell_id": "r4_c3",
      "page_id": "p012",
      "bbox": [545, 430, 665, 462],
      "value": "14.2%",
      "row_headers": ["Industrial Machinery"],
      "column_headers": ["Q2 2026", "Operating Margin"],
      "units": "percent",
      "footnote_refs": [],
      "confidence": 0.982
    }
  ]
}
```

### **Multi-Page Continuity Resolution**

Multi-page table joining should be treated as an inference with evidence, not an automatic merge.

```text
+--------------------------------------------------------------------------------
| MULTI-PAGE TABLE CONTINUITY RESOLUTION
+--------------------------------------------------------------------------------
|
| [ Table Candidate on Page N ]
|   table_id: tbl_p012_001
|   columns: 5
|   terminal_total_row: absent
|   bottom_margin_cutoff: true
|        |
|        v
| [ Table Candidate on Page N+1 ]
|   table_id: tbl_p013_001
|   columns: 5
|   repeated_header_similarity: 0.94
|   caption: "Table 4 continued"
|        |
|        v
| [ Continuity Feature Scoring ]
|   column_count_match: true
|   column_x_alignment_score: 0.96
|   repeated_header_score: 0.94
|   row_sequence_continuity: 0.89
|   terminal_marker_absent_on_page_N: true
|   caption_continuation_signal: true
|        |
|        v
| [ Decision ]
|   continuation_state: likely_continued
|   confidence: 0.91
|   action: merge_with_row_offset_and_preserve_page_ids
|
+--------------------------------------------------------------------------------
```

### **Continuity Decision States**

| State | Meaning | Handling |
| :--- | :--- | :--- |
| **not_continued** | Evidence indicates separate tables. | Keep separate table IDs. |
| **likely_continued** | Features strongly support continuation. | Merge with page-aware row offsets and confidence. |
| **possible_continuation** | Some features match but evidence is incomplete. | Keep linked but unmerged; request inspection. |
| **conflict** | Features disagree. | Route to review or alternate parser. |
| **unknown** | Insufficient evidence. | Do not merge automatically. |

### **Table Citation Rule**

A tabular claim must cite more than the cell value. It should cite:

```text
table_id
page_id
cell_id
cell_bbox
row_headers
column_headers
units
caption
footnotes
continuation_state
parser_version
```

A cell without headers is not a fact. It is a number waiting to betray someone in finance.

## **V. Charts, Plots, and Visualized Data: Modality Conversion and Symbolic Reasoning**

Charts encode data visually. To answer questions about a chart, the system must identify chart type, parse axes, detect scale, bind legends to series, locate marks, convert pixels to values, preserve uncertainty, and execute calculations over the extracted data.

A chart interpretation system should not rely on global visual impressions for quantitative answers. It should convert visual marks into structured data, then reason over the structured data.

### **Artifact 7: Chart Interpretation Pipeline**

```text
+--------------------------------------------------------------------------------
| CHART INTERPRETATION PIPELINE
+--------------------------------------------------------------------------------
|
| [ Input Chart Image ]
|      |
|      v
| [ Chart Type Classifier ]
|   line | bar | stacked bar | scatter | pie | area | mixed | unknown
|      |
|      v
| [ Chart Region Segmentation ]
|   plot area | title | axes | ticks | legends | labels | annotations
|      |
|      v
| [ Axis and Scale Parser ]
|   x-axis type | y-axis type | tick values | units | log/linear scale
|      |
|      v
| [ Legend and Series Binder ]
|   colors | markers | line styles | series names | confidence
|      |
|      v
| [ Visual Mark Extractor ]
|   bars | points | lines | segments | slices | error bars
|      |
|      v
| [ Pixel-to-Value Conversion ]
|   coordinate transform | scale transform | uncertainty estimate
|      |
|      v
| [ Linearized Data Table ]
|   series | x | y | units | source mark bbox | confidence
|      |
|      v
| [ Programmatic / Symbolic Reasoning ]
|   calculations over extracted table, not raw image impressions
|      |
|      v
| [ Grounded Chart Claim ]
|   claim + chart element citations + extraction uncertainty
|
+--------------------------------------------------------------------------------
```

### **Chart Data Object Schema**

```json
{
  "chart_id": "chart_p007_001",
  "source_document_id": "doc_9ab3",
  "page_id": "p007",
  "chart_type": "line",
  "bbox": [90, 130, 915, 760],
  "title": {
    "text": "Operating Margin by Quarter",
    "bbox": [260, 85, 740, 122]
  },
  "axes": {
    "x": {
      "label": "Quarter",
      "scale": "categorical",
      "tick_values": ["Q1 2026", "Q2 2026", "Q3 2026"],
      "bbox": [140, 700, 850, 742]
    },
    "y": {
      "label": "Operating Margin (%)",
      "scale": "linear",
      "tick_values": [0, 5, 10, 15, 20],
      "bbox": [92, 150, 140, 700]
    }
  },
  "series": [
    {
      "name": "Industrial Machinery",
      "legend_bbox": [720, 160, 895, 190],
      "marks": [
        {
          "x": "Q2 2026",
          "y": 14.2,
          "units": "percent",
          "mark_bbox": [430, 278, 445, 293],
          "value_confidence": 0.94
        }
      ]
    }
  ]
}
```

### **Chart Verification Checks**

| Check | Purpose |
| :--- | :--- |
| **Chart-type confidence** | Prevents using line-chart logic on stacked bars or mixed charts. |
| **Axis-label extraction** | Preserves units and variable names. |
| **Tick interval consistency** | Detects nonlinear, logarithmic, truncated, or broken axes. |
| **Legend binding** | Prevents series-color confusion. |
| **Mark localization confidence** | Determines whether visual marks were actually found. |
| **Pixel-to-value uncertainty** | Quantifies extraction precision. |
| **Data-table validation** | Checks extracted values against visual layout and summary statistics. |
| **Citation binding** | Links every numeric claim to chart region, axis, legend, and mark. |

### **Symbolic Reasoning Rule**

Use programmatic or symbolic calculation over extracted chart data:

```text
extracted_chart_table -> calculation trace -> grounded answer
```

Do not encode “chain-of-thought” as a production artifact. The durable artifact is a calculation trace over extracted data:

```json
{
  "operation": "difference",
  "inputs": [
    { "series": "Industrial Machinery", "x": "Q2 2026", "y": 14.2 },
    { "series": "Industrial Machinery", "x": "Q1 2026", "y": 12.9 }
  ],
  "result": 1.3,
  "units": "percentage points"
}
```

### **Chart Failure Modes**

| Failure Mode | Symptom | Mitigation |
| :--- | :--- | :--- |
| **Axis Scale Misread** | Linear value inferred from log or truncated axis. | Parse scale and tick transform before extraction. |
| **Legend Confusion** | Series values swapped. | Bind color/marker/line style to legend entries. |
| **Overlapping Label Loss** | Tick or data labels omitted. | Use high-resolution crop and label-specific OCR. |
| **Pie Slice Approximation Error** | Percentage inferred from visual area alone. | Prefer data labels or derender with uncertainty. |
| **Chart Type Ambiguity** | Mixed chart treated as single type. | Decompose chart into mark layers. |
| **Uncited Numeric Claim** | Number appears in answer with no mark/axis citation. | Block claim or route to inspection. |

## **VI. Images, Visual Grounding, and Region Evidence: Spatial Coordinate Mapping**

Visual grounding maps claims to spatial regions. A global image caption is not enough for high-stakes document, diagram, inspection, medical, financial, engineering, or interface tasks. The system must identify which pixels support the claim.

### **Artifact 8: Visual Grounding Model**

```text
+--------------------------------------------------------------------------------
| VISUAL GROUNDING MODEL
+--------------------------------------------------------------------------------
|
| Source:
|   document_id: doc_pressure_manual
|   page_id: p012
|   coordinate_system: normalized_0_1000
|
| Evidence Regions:
|
|   region_text_limit
|     class: text_block
|     bbox: [72, 118, 612, 176]
|     text: "The operating pressure must not exceed 150 PSI under standard load."
|     confidence: 0.985
|
|   region_diagram
|     class: technical_diagram
|     bbox: [95, 245, 895, 720]
|     confidence: 0.991
|
|   region_gauge_needle
|     class: visual_mark
|     parent: region_diagram
|     bbox: [646, 438, 706, 512]
|     attribute: needle pointing into red zone near 180 PSI
|     confidence: 0.942
|
| Grounded Claim:
|   "The equipment is shown operating above the stated 150 PSI limit."
|
| Required Support:
|   text limit region + gauge needle region + gauge scale context
|
+--------------------------------------------------------------------------------
```

### **Grounding Coordination Object**

```json
{
  "claim_id": "claim_pressure_over_limit",
  "claim": "The equipment is shown operating above the stated 150 PSI limit.",
  "source": {
    "document_id": "doc_pressure_manual",
    "page_id": "p012",
    "document_hash": "sha256:abc123"
  },
  "coordinate_system": {
    "type": "normalized",
    "width": 1000,
    "height": 1000
  },
  "evidence_regions": [
    {
      "region_id": "region_text_limit",
      "modality": "text",
      "region_class": "text_block",
      "bbox": [72, 118, 612, 176],
      "extracted_text": "The operating pressure must not exceed 150 PSI under standard load.",
      "confidence": 0.985
    },
    {
      "region_id": "region_gauge_needle",
      "modality": "visual",
      "region_class": "visual_mark",
      "bbox": [646, 438, 706, 512],
      "visual_attribute": "needle points into red zone near 180 PSI",
      "confidence": 0.942
    },
    {
      "region_id": "region_gauge_scale",
      "modality": "visual_text",
      "region_class": "scale_labels",
      "bbox": [575, 360, 790, 555],
      "extracted_text": "150 180 PSI",
      "confidence": 0.91
    }
  ],
  "grounding": {
    "grounding_good_ratio": 0.963,
    "spatial_semantic_alignment_score": 0.947,
    "inspection_adequacy": "sufficient"
  }
}
```

### **Patch-to-Region Relevance Propagation**

Page-level visual retrieval can find relevant pages but often fails to identify the precise evidence region. Spatially grounded retrieval maps patch-level relevance onto OCR or layout boxes.

Let:

| Symbol | Meaning |
| :--- | :--- |
| `W`, `H` | Page image width and height. |
| `W_grid`, `H_grid` | Patch grid width and height. |
| `j` | Patch index. |
| `P_j` | Patch coordinate box. |
| `q_i` | Query token vector. |
| `p_j` | Page patch vector. |
| `B` | OCR/layout bounding box. |

Patch coordinates:

```text
patch_width  = W / W_grid
patch_height = H / H_grid

x1_j = (j mod W_grid) * patch_width
y1_j = floor(j / W_grid) * patch_height
x2_j = x1_j + patch_width
y2_j = y1_j + patch_height

P_j = [x1_j, y1_j, x2_j, y2_j]
```

Patch score:

```text
score_patch(j) = max_i cosine(q_i, p_j)
```

Region score:

```text
score_region(B) =
  sum over patches j overlapping B:
    IoU(P_j, B) * score_patch(j)
```

### **Grounding Adequacy Rules**

| Requirement | Reason |
| :--- | :--- |
| **Target region present** | Claim must be anchored to visible evidence. |
| **Context region present** | Neighboring labels, scales, captions, or legends may define meaning. |
| **Resolution sufficient** | Small text and visual marks require readable crops. |
| **Coordinate chain intact** | Claim must map back to source page and region. |
| **Uncertainty represented** | Ambiguous visual interpretation should not become confident text. |

A grounded image claim should answer: what region supports this, what surrounding context defines it, and how confident is the localization?

## **VII. Video Evidence Sampling and Temporal Grounding: Event-Aware Structures**

Video adds time to visual evidence. A video claim may depend on event order, duration, motion, causality, speaker identity, object persistence, or a brief transient frame. Uniform sampling can miss the event that makes the claim true.

Video understanding requires temporal grounding: the system must locate the relevant time range and preserve the frames or clips that support the answer.

### **Artifact 9: Temporal Evidence Pipeline**

```text
+--------------------------------------------------------------------------------
| VIDEO TEMPORAL EVIDENCE PIPELINE
+--------------------------------------------------------------------------------
|
| [ Source Video ]
|   duration | fps | audio | subtitles | scene metadata | hash
|        |
|        v
| [ Low-Cost Prepass ]
|   shot boundaries | audio segments | subtitles | visual embeddings
|        |
|        v
| [ Event Segmentation ]
|   coherent temporal segments with start/end timestamps
|        |
|        v
| [ Query-Aware Sampling Policy ]
|   semantic | motion | hybrid | global summary | dialogue
|        |
|        v
| [ Candidate Frame / Clip Selection ]
|   anchor frames | refinement frames | dense motion windows
|        |
|        v
| [ Temporal Inspection Adequacy Gate ]
|   enough frames? event covered? order preserved? motion visible?
|        |
|        v
| [ Event Verification ]
|   object/action/speaker/sequence verification over selected interval
|        |
|        v
| [ Grounded Temporal Claim ]
|   claim + timestamp interval + frame IDs + uncertainty
|
+--------------------------------------------------------------------------------
```

### **Sampling Strategy Matrix**

| Query Type | Evidence Need | Sampling Strategy | Failure Risk |
| :--- | :--- | :--- | :--- |
| **Semantic-only** | Static objects, people, locations, scene contents. | Diverse keyframes from visually distinct segments. | Missing small objects or text if resolution is poor. |
| **Motion-only** | Movement, direction, speed, repeated action, physical sequence. | Dense sampling inside candidate temporal windows. | Uniform sparse sampling may reverse or miss action. |
| **Semantic + Motion** | Actor/object identity plus temporal action. | Anchor frames plus dense local windows around event. | Object identity or action sequence may be incomplete. |
| **Dialogue / Speaker** | Spoken content, speaker turns, subtitle alignment. | Audio/subtitle alignment plus visual speaker frames. | Speaker attribution errors. |
| **Global Summary** | Broad video overview. | Hierarchical segment sampling across beginning/middle/end plus salient events. | Over-selection and token bloat. |
| **Rare Event Detection** | Brief safety, defect, anomaly, UI flash, or hand motion. | High-frequency pass around candidate anomaly windows. | Event may be missed entirely. |

### **Temporal Evidence Object**

```json
{
  "video_id": "vid_2026_0042",
  "source_hash": "sha256:abc123",
  "claim_id": "claim_button_before_gauge",
  "claim": "The technician presses the red button twice before checking the gauge.",
  "evidence_intervals": [
    {
      "interval_id": "evt_001",
      "start_sec": 43.7,
      "end_sec": 46.2,
      "event": "first button press",
      "keyframes": [
        { "frame_id": "f_0045_100", "timestamp_sec": 45.1, "bbox": [420, 380, 510, 460] }
      ],
      "confidence": 0.94
    },
    {
      "interval_id": "evt_002",
      "start_sec": 88.9,
      "end_sec": 91.4,
      "event": "second button press",
      "keyframes": [
        { "frame_id": "f_0090_300", "timestamp_sec": 90.3, "bbox": [415, 376, 512, 464] }
      ],
      "confidence": 0.91
    },
    {
      "interval_id": "evt_003",
      "start_sec": 153.2,
      "end_sec": 158.8,
      "event": "gauge inspection",
      "keyframes": [
        { "frame_id": "f_0155_000", "timestamp_sec": 155.0, "bbox": [610, 260, 790, 480] }
      ],
      "confidence": 0.88
    }
  ],
  "temporal_order_verified": true
}
```

### **Video Evaluation Diagnostics**

| Diagnostic | Meaning | Use |
| :--- | :--- | :--- |
| **Frame Selection Sensitivity** | Accuracy delta between relevant and irrelevant frame sets. | Detects whether benchmark/model actually depends on visual evidence. |
| **Language Independence Score** | Accuracy without frames. | Detects language-prior shortcuts. |
| **Temporal IoU** | Overlap between predicted and ground-truth time ranges. | Measures temporal localization. |
| **Event Recall** | Fraction of required events included in selected frames/clips. | Detects missed-event failures. |
| **Order Accuracy** | Whether event sequence is preserved. | Detects reversal and causality errors. |
| **Frame Budget Efficiency** | Evidence coverage per selected frame/token. | Controls cost and context size. |

### **Temporal Grounding Rule**

A video claim should not cite the whole video unless the whole video is the evidence. It should cite:

```text
video_id
timestamp interval
selected frame IDs
event labels
object/person boxes when relevant
sampling policy
confidence
```

A video answer without time is just vibes with a progress bar.

## **VIII. Multimodal Embeddings, Retrieval, and Indexing: Hybrid Storage**

A production multimodal system cannot rely on one flattened vector index. Enterprise corpora contain text-native PDFs, scanned documents, forms, tables, charts, engineering diagrams, screenshots, and videos. Each modality requires different retrieval signals and different evidence packets.

The retrieval architecture should preserve parallel representations:

* text spans
* layout nodes
* page images
* OCR boxes
* table cells
* chart marks
* visual patch embeddings
* video keyframes
* temporal segments
* source provenance

### **Artifact 10: Multimodal Indexing Strategy**

```text
+--------------------------------------------------------------------------------
| MULTIMODAL STORAGE AND INDEXING CLUSTER
+--------------------------------------------------------------------------------
|
| Object Store
|   source files
|   rendered page images
|   region crops
|   chart crops
|   video keyframes
|   parser artifacts
|        |
|        v
| Relational Metadata Store
|   document records
|   page records
|   layout nodes
|   bounding boxes
|   table cells
|   chart marks
|   video segments
|   parser versions
|        |
|        v
| Text Retrieval Index
|   BM25 / sparse
|   dense text embeddings
|   metadata filters
|        |
|        v
| Visual Retrieval Index
|   single-vector page/image embeddings for candidate retrieval
|   multi-vector page patch embeddings for late-interaction reranking
|        |
|        v
| Specialized Evidence Indexes
|   table index
|   chart data index
|   form field index
|   object/region index
|   temporal video index
|
+--------------------------------------------------------------------------------
```

### **Late-Interaction Retrieval Posture**

Late-interaction systems often store multiple vectors per page or image. Approximate nearest-neighbor search may be used to retrieve candidates through pooled or representative vectors, but **MaxSim is a scoring/reranking operation over multi-vectors**, not a normal single-vector HNSW distance metric in the ordinary dense-vector sense.

A safe retrieval pattern is:

```text
1. Use metadata, lexical search, or pooled dense vectors to retrieve candidates.

2. Load candidate page/image multi-vectors.

3. Compute late-interaction MaxSim score over query tokens and candidate patches.

4. Rerank candidates.

5. Propagate patch scores to regions, boxes, or OCR/layout nodes.

6. Return localized evidence packets, not just whole pages.
```

### **Hybrid Index Components**

| Index | Stored Representation | Best For | Return Artifact |
| :--- | :--- | :--- | :--- |
| **Text-Native Index** | Extracted spans, chunks, section headers, metadata. | Exact entities, legal clauses, titles, IDs. | Text evidence packet with coordinates. |
| **Layout Node Index** | Page objects, reading order, captions, footnotes, suppressed metadata. | Layout-dependent documents. | Layout graph packet. |
| **Visual Page Index** | Page/image embeddings and patch vectors. | Visually rich pages, diagrams, scanned pages. | Candidate pages and patch relevance map. |
| **Spatial Coordinate Registry** | Bounding boxes for spans, figures, tables, cells, fields, marks. | Localized evidence and citations. | Coordinate-bearing region packet. |
| **Table Index** | Tables, cells, row/column headers, units, footnotes. | Quantitative table QA. | Cell evidence packet. |
| **Chart Index** | Chart type, axes, series, marks, extracted data table. | Chart QA and numeric reasoning. | Chart data packet. |
| **Form Field Index** | Form templates, label-value pairs, checkbox states, signatures. | Invoice, receipt, administrative forms. | Field evidence packet. |
| **Video Temporal Index** | Segments, keyframes, subtitles, object tracks, embeddings. | Video QA, event search, temporal grounding. | Timestamp/frame evidence packet. |

### **Artifact 11: Multimodal Retrieval Decision Model**

| Query Profile | Target Artifact | Primary Route | Retrieval Method | Post-Processing |
| :--- | :--- | :--- | :--- | :--- |
| **Lexical / Entity-Specific** | Text-native document. | Text index. | BM25/exact metadata + dense rerank. | Section and coordinate citation. |
| **Visual-Spatial Layout** | Presentation, manual, diagram, screenshot. | Visual page index. | Candidate retrieval + late-interaction rerank. | Region localization and crop extraction. |
| **Table Fact** | Scanned or digital table. | Table index + coordinate registry. | Table/cell retrieval with header binding. | Cell, row, column, caption citation. |
| **Chart Numeric Question** | Chart or plot. | Chart index / chart crop route. | Chart detection and derendering. | Extracted data table + calculation trace. |
| **Form Extraction** | Invoice, receipt, administrative form. | Form field index. | Template matching + label-value binding. | Field object with state and confidence. |
| **Temporal Action Sequence** | Video. | Temporal video index. | Segment retrieval + query-aware sampling. | Timestamp interval and frame citation. |
| **Uncertain / Mixed Modality** | Unknown or hybrid artifact. | Multi-route fanout with budget cap. | Run cheap routes first, escalate selectively. | Evidence adequacy gate chooses final packet. |

### **Ingestion Routing Flow**

```text
+--------------------------------------------------------------------------------
| INGESTION ROUTING CLASSIFIER FLOW
+--------------------------------------------------------------------------------
|
| [ Source Artifact ]
|      |
|      v
| [ Fingerprint and Inspect ]
|   file type | hash | pages | duration | embedded text | image coverage
|      |
|      v
| [ Route Decision ]
|      |
|      +--> text quality high, layout simple
|      |       -> text-native parser + coordinate spans
|      |
|      +--> embedded text present but layout complex
|      |       -> hybrid text + layout parser
|      |
|      +--> image-heavy or scanned
|      |       -> render + OCR/VLM visual parser
|      |
|      +--> table-heavy
|      |       -> table detector + structure recognizer
|      |
|      +--> chart-heavy
|      |       -> chart detector + derendering path
|      |
|      +--> video
|              -> temporal segmentation + keyframe index
|
+--------------------------------------------------------------------------------
```

Routing policies should be measured by cost saved, extraction accuracy, fallback frequency, and downstream answer grounding. Cheap extraction is good only when it preserves the evidence needed for the task.

## **IX. Evidence Selection and Inspection Adequacy**

Retrieved evidence is not automatically adequate evidence. A page, crop, table cell, chart mark, or video frame may be relevant but still insufficient to support the final claim.

The system must evaluate whether the selected evidence is legible, complete, contextualized, and appropriate for the task before generating an answer.

### **Artifact 12: Evidence Selection Quality Framework**

| Adequacy Dimension | Failure Signal | Corrective Action |
| :--- | :--- | :--- |
| **Resolution** | Small text unreadable, OCR confidence low, crop too compressed. | Re-render at higher DPI, upscale crop, or retrieve full page. |
| **Crop Coverage** | Headers, axis labels, footnotes, legend, caption, or surrounding context missing. | Expand crop or retrieve neighboring layout nodes. |
| **Coordinate Validity** | Bounding box outside page, zero-area box, wrong page ID. | Recompute coordinates or route to parser fallback. |
| **Reading Order** | Multi-column text interleaves or skips clauses. | Use layout-aware parser and reading-order reconstruction. |
| **Header Binding** | Table cell lacks row/column header. | Retrieve header cells, spanning cells, caption, and units. |
| **Axis / Legend Binding** | Chart mark lacks scale or series identity. | Retrieve axis, legend, ticks, and plot area. |
| **Temporal Coverage** | Video frames miss action start/end or sequence order. | Increase sampling density around candidate event interval. |
| **Source Freshness** | Artifact version hash does not match active source. | Re-ingest source and invalidate stale index entries. |
| **Permission Scope** | Evidence region belongs to unauthorized tenant or document. | Block evidence and reroute retrieval under active permission. |
| **Uncertainty** | Confidence below threshold or conflicting parser outputs. | Use fallback parser, second-pass inspection, or human review. |

### **Task-Specific Minimum Evidence Matrix**

| Task Category | Minimum Evidence Units | Required Coordinate Targets | Adequacy Checks |
| :--- | :--- | :--- | :--- |
| **Contract / Legal Answer** | Target clause, adjacent definitions, footnotes, cross-references, page context. | Paragraph box, definition box, footnote box, page ID. | Reading-order confidence, version status, full clause coverage. |
| **Invoice / Receipt / Financial Audit** | Label-value pairs, totals, tax markers, vendor/customer identifiers, signature/stamp if relevant. | Label bbox, value bbox, total bbox, signature/stamp bbox. | OCR confidence, arithmetic consistency, field-state completeness. |
| **Tabular Quantitative Extraction** | Cell value, row header, column header, units, caption, footnotes, continuation status. | Cell bbox, header bboxes, table bbox, caption bbox. | Header binding, GriTS/TEDS, continuation confidence. |
| **Chart / Plot Interpretation** | Chart title, axes, tick labels, legend, marks, annotations. | Plot area, axis boxes, legend boxes, mark boxes. | Scale detection, legend binding, extraction uncertainty. |
| **Engineering Diagram Analysis** | Component crop, surrounding schematic context, labels, legends, scale/reference notes. | Component bbox, label bbox, diagram bbox. | Object localization confidence, context coverage, resolution. |
| **Form Understanding** | Field label, field value, checkbox/signature/stamp regions, template metadata. | Label bbox, value bbox, mark bbox. | Label-value binding, field state, template confidence. |
| **Video Sequence QA** | Event keyframes/clips, timestamp interval, subtitles/audio when needed, object/person boxes. | Time interval, frame IDs, frame-level boxes. | Temporal order, event coverage, frame selection sensitivity. |
| **Screen / GUI Understanding** | Target UI element, label, surrounding container, active state, cursor/focus context. | Element bbox, label bbox, viewport coordinates. | Coordinate precision, click safety, stale screenshot check. |

### **Inspection Decision Policy**

| Adequacy State | Meaning | System Behavior |
| :--- | :--- | :--- |
| **sufficient** | Evidence is complete and legible enough for the claim. | Generate grounded answer with citation. |
| **insufficient_resolution** | Evidence is relevant but unreadable. | Re-render/upscale or ask for better source. |
| **insufficient_context** | Evidence lacks surrounding headers, labels, scale, or footnotes. | Expand crop or retrieve adjacent regions. |
| **conflicting_evidence** | Evidence sources disagree. | Surface conflict and route to source-authority logic. |
| **unauthorized** | Evidence violates active access scope. | Block and log. |
| **unverifiable** | Source lacks adequate coordinate or verification path. | Report uncertainty or route to review. |
| **human_review_required** | Automated inspection cannot safely decide. | Create review packet with crops and candidate extraction. |

### **Evidence Adequacy Rule**

```text
Relevant is not enough.
Visible is not enough.
Cited is not enough.

The evidence must be adequate for the claim being made.
```

## **X. Evidence Provenance, Citation, and Production Observability**

A production multimodal system has not understood an artifact unless it can show the exact evidence that supports its answer. The system must preserve a coordinate chain from the generated claim back to the original source artifact.

This requires structured citations, failure-mode tracking, and production metrics for OCR, layout, retrieval, extraction, grounding, and citation fidelity.

### **Artifact 14: Multimodal Evidence Citation Model**

```json
{
  "$schema": "https://ai-engineering.canon/schemas/multimodal-citation-v1.json",
  "citation_id": "cite_2026_06_10_08912",
  "source_document": {
    "document_uuid": "doc_8f92a10c-3b9e-4a8f-b9c2-7d8e9f0a1b2c",
    "document_hash": "sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
    "filename": "Q2_2026_financial_report.pdf",
    "source_version": "v2026.06.10"
  },
  "rendering": {
    "renderer": "page_renderer",
    "renderer_version": "1.8.0",
    "dpi": 300,
    "coordinate_system": {
      "type": "normalized",
      "width": 1000,
      "height": 1000
    }
  },
  "grounded_claims": [
    {
      "claim_id": "claim_1",
      "text": "Operating margins for the Industrial Machinery segment rose to 14.2% in Q2 2026.",
      "evidence_class": "structured_table_cell",
      "confidence": 0.982,
      "evidence_regions": [
        {
          "region_id": "tbl_12_1",
          "page_number": 12,
          "region_class": "table_block",
          "bounding_box": [78, 142, 930, 842],
          "context_markup": "<Table id='tbl_12_1'>",
          "parser": "table_extractor_v3"
        },
        {
          "region_id": "cell_row_4_col_3",
          "page_number": 12,
          "region_class": "table_cell",
          "bounding_box": [545, 430, 665, 462],
          "cell_value": "14.2%",
          "cell_headers": [
            "Operating Margin",
            "Q2 2026",
            "Industrial Machinery"
          ],
          "units": "percent"
        }
      ]
    },
    {
      "claim_id": "claim_2",
      "text": "The increase was associated with the ACME-400 valve configuration.",
      "evidence_class": "annotated_figure",
      "confidence": 0.941,
      "evidence_regions": [
        {
          "region_id": "fig_43_2",
          "page_number": 43,
          "region_class": "figure_image",
          "bounding_box": [112, 185, 890, 710],
          "context_markup": "<Figure id='fig_43_2'>"
        },
        {
          "region_id": "component_annotation_valve",
          "page_number": 43,
          "region_class": "figure_annotation",
          "bounding_box": [624, 390, 812, 438],
          "annotated_value": "ACME-400 Valve Assembly"
        }
      ]
    }
  ],
  "replay": {
    "page_image_ids": ["p012_render_300dpi", "p043_render_300dpi"],
    "crop_ids": ["crop_tbl_12_1", "crop_cell_row_4_col_3", "crop_fig_43_2"],
    "index_version": "mm_index_2026_06_10",
    "parser_manifest_id": "parser_manifest_v14"
  }
}
```

### **Artifact 15: Multimodal Failure Mode Map**

| Failure Mode | Symptom | Root Cause | Detection Signal | Mitigation |
| :--- | :--- | :--- | :--- | :--- |
| **OCR Drift** | Alphanumeric values misread. | Low resolution, poor contrast, degraded fonts. | CER/WER increase; token confidence drop. | Re-render, enhance contrast, use alternate OCR, route to review. |
| **Layout Collapse** | Columns, captions, or footnotes serialized incorrectly. | Text-only extraction ignores visual layout. | Reading-order F1 drops; section references mismatch. | Use layout graph parser and coordinate-aware reconstruction. |
| **Table Flattening** | Cell values lose headers and units. | Table parsed as prose. | Header-binding failure, low table QA accuracy. | Use table structure recognition and cell citation packets. |
| **Chart Scale Misread** | Numeric chart answers wrong. | Axis/legend/scale not parsed. | High relative error on chart QA. | Derender to data table and calculate programmatically. |
| **Form Field Misbinding** | Values assigned to wrong labels. | Spatial association failure. | Field validator mismatch. | Template anchors, label-value scoring, ambiguity states. |
| **Temporal Reversal** | Video event order wrong. | Sparse or unordered frame sampling. | Low temporal-order accuracy. | Use event segmentation and timestamp-aware grounding. |
| **Wrong Crop Retrieval** | Retrieved crop lacks answer. | Patch-to-region propagation mismatch. | Region hit rate near random. | Recalibrate coordinate grid and rerank localized regions. |
| **Low-Resolution Crop** | Small text invented or guessed. | Crop too small for visual encoder/OCR. | OCR confidence drop, unresolved characters. | Upscale/re-render or retrieve larger region. |
| **Visual Similarity Confusion** | Wrong customer/date/template retrieved. | Visual embedding overweights layout similarity. | Metadata mismatch or exact text mismatch. | Hybrid visual + lexical + metadata retrieval. |
| **Figure-Caption Separation** | Figure interpreted without caption. | Missing parent-child layout edges. | Caption recall drops. | Preserve figure-caption graph links. |
| **Missed Video Frame** | Brief event omitted. | Sampling interval too coarse. | Low event recall / FSS. | Dense sampling around candidate event windows. |
| **Stale Media Ingestion** | Old visual version cited. | Index not invalidated after source update. | Source hash mismatch. | Version visual embeddings and invalidate on file change. |
| **Source Provenance Loss** | Citation lacks page/crop/cell/frame. | Coordinate mappings discarded. | Citation fidelity failure. | Require coordinate-bearing evidence packets. |
| **Evidence Over-Selection** | Context flooded with redundant images. | Page-level retrieval without localization. | Cost/latency spike, answer dilution. | Return targeted crops and evidence packets. |
| **Evidence Under-Selection** | Missing headers, units, or footnotes. | Crop too narrow. | Adequacy gate failure. | Expand region or retrieve adjacent layout nodes. |
| **Unsupported Visual Inference** | Confident claim lacks localized support. | Model answers from impression. | Grounding ratio below threshold. | Block claim or require reinspection. |
| **Prompt Injection in Visual Text** | OCR text contains instructions to model. | Retrieved visual text treated as instruction. | Command-like content in evidence. | Wrap visual text as data and sanitize before generation. |

### **Artifact 16: Multimodal Evaluation and Observability Guidance**

| Evaluation Area | Metric | Collection Method | Failure Trigger |
| :--- | :--- | :--- | :--- |
| **OCR Accuracy** | CER / WER / token confidence. | Compare extracted text to labeled samples or high-confidence native text. | Confidence below task threshold. |
| **Reading Order** | Reading-order F1. | Compare layout sequence to labeled document paths. | Column or footnote ordering collapse. |
| **Layout Detection** | Region IoU / class F1. | Compare detected blocks to annotated regions. | Missing tables, figures, captions, headers. |
| **Table Structure** | GriTS, TEDS, header-binding accuracy. | Compare extracted table graph to ground truth. | Cell/header mismatch or continuation error. |
| **Chart Extraction** | Relative error tolerance, axis/legend accuracy. | Compare extracted chart table to ground truth. | Numeric claim outside tolerance. |
| **Form Extraction** | Field exact match, field-state accuracy. | Compare label-value extraction to labeled forms. | Required field missing or misbound. |
| **Visual Grounding** | IoU@0.5, region hit rate, grounding good ratio. | Compare cited regions to labeled evidence boxes. | Unsupported claim or wrong crop. |
| **Video Grounding** | Temporal IoU, event recall, FSS delta. | Compare selected intervals to annotated events. | Missed event or language-prior shortcut. |
| **Citation Fidelity** | Claim-to-coordinate completeness. | Audit generated claims for source hash, page/frame, bbox/cell/time. | Claim lacks evidence packet. |
| **Replayability** | Replay package completeness. | Check parser versions, crops, page images, index versions, source hashes. | Missing artifact prevents reproduction. |

Production observability should separate multimodal stages:

```text
ingestion_latency
render_latency
ocr_latency
layout_parse_latency
visual_retrieval_latency
crop_generation_latency
inspection_failure_rate
grounding_failure_rate
citation_completeness_rate
```

The system should not hide visual retrieval failures inside generic generation latency. That is how greml—nope, not saying it. That is how nonsense gets a dashboard.

## **XI. Architectural Handoffs and Degraded-Mode Fallbacks**

Multimodal understanding is a substrate layer for downstream agents, interface-control systems, audit systems, and user-facing evidence displays. If the system cannot verify what visual evidence it inspected, it cannot safely click interfaces, extract financial data, summarize legal documents, interpret diagrams, or report status as grounded truth.

### **Artifact 17: Cross-Canon Handoff Map**

| Target Module | Handoff Artifact | Operational Dependency |
| :--- | :--- | :--- |
| **AI-ENG-Q — Screen Understanding / Computer Use** | UI element boxes, viewport coordinates, OCR labels, click confidence, screenshot freshness. | Blocks unsafe clicks when coordinate confidence or screen freshness is insufficient. |
| **AI-ENG-R — Browser / GUI Automation** | Page layout nodes, DOM/visual alignment, form fields, button states, visual confirmation. | Enables agents to act on interfaces without relying on hallucinated UI state. |
| **AI-ENG-S — Production Pathologies** | OCR drift, parser failures, retrieval misses, grounding failures, rendering latency. | Diagnoses multimodal pipeline failures and cost spikes. |
| **AI-ENG-T — Prompt Injection and Tenant Isolation** | Visual text streams, OCR instructions, document provenance, tenant-scoped coordinates. | Prevents visual/text prompt injection and cross-tenant evidence leakage. |
| **AI-ENG-U — Parser and Tool Dependency Risk** | Parser versions, fallback engines, sandbox settings, third-party OCR/VLM dependency state. | Manages parser outages, model regressions, and dependency degradation. |
| **AI-ENG-W — Fallback and Degraded Modes** | Evidence adequacy state, fallback route, parser confidence, unavailable modality flags. | Determines whether to use degraded extraction, ask for better source, or refuse. |
| **AI-ENG-X — Transparent User-Facing Evidence** | Claim-level citations, bounding boxes, crop IDs, timestamp intervals, confidence. | Renders visual highlights and evidence cards to users. |
| **AI-ENG-Y — Human Review** | Low-confidence crops, ambiguous fields, conflicting parser outputs, review packets. | Routes unresolved multimodal evidence to human adjudication. |
| **AI-ENG-Z — Telemetry** | Stage-level latency, grounding metrics, citation completeness, parser failure rates. | Powers multimodal observability dashboards. |
| **AI-ENG-AA — Multimodal Evaluations** | Ground-truth boxes, chart tables, table cells, frame intervals, replay packages. | Evaluates multimodal grounding and extraction accuracy. |
| **AI-ENG-AB — Audit and Replay** | Source hashes, page images, crops, coordinates, parser manifests, index versions. | Reconstructs generated claims from source evidence. |
| **AI-ENG-AC — Incident Response** | Fault indicators, poisoned artifacts, parser crashes, coordinate inversions, citation failures. | Quarantines bad documents and triggers recovery workflows. |
| **AI-ENG-AD — Governance and Accountability** | Evidence retention policies, visual PII rules, regulated document handling, audit obligations. | Ensures multimodal evidence use complies with organizational policy. |
| **AI-ENG-AJ — Reference Architectures** | Multi-index retrieval blueprint, coordinate registry schema, citation schema, fallback patterns. | Provides implementation templates for multimodal systems. |

### **Degraded-Mode Fallbacks**

| Failure / Constraint | Degraded Mode | User-Facing Status |
| :--- | :--- | :--- |
| **Low DPI / poor scan** | Deskew, contrast enhance, re-render/upscale, alternate OCR. | “Source quality is low; extraction confidence is reduced.” |
| **Unreadable microtext** | Retrieve larger crop, higher DPI page, or request original file. | “The visible text is too small to verify safely.” |
| **Layout parser failure** | Fall back to text-native extraction plus coordinate uncertainty. | “Layout preservation failed; answer limited to text evidence.” |
| **VLM/OCR API unavailable** | Use local OCR/parser route if allowed. | “Using degraded local extraction; accuracy may be lower.” |
| **Table structure uncertain** | Preserve page crop and table region; avoid cell-specific claims. | “Table structure could not be verified.” |
| **Chart derendering failure** | Provide qualitative description only if chart regions are visible; block numeric claims. | “Numeric chart values could not be extracted reliably.” |
| **Video too long for dense processing** | Segment hierarchically; inspect only query-relevant intervals. | “Video processed by sampled segments; uninspected intervals remain.” |
| **Coordinate chain broken** | Return document/page-level citation only with explicit warning. | “Precise evidence coordinates are unavailable.” |
| **Permission conflict** | Exclude unauthorized evidence and reroute retrieval. | “Some evidence is outside the active permission scope.” |
| **Conflicting parser outputs** | Route to human review or second-pass adjudication. | “Extraction is ambiguous and requires review.” |

### **Fallback Rule**

```text
Degraded mode must degrade the claim, not just the pipeline.

If the system loses resolution, layout, coordinates, or verification,
the answer must expose that loss rather than pretending full confidence.
```

## **XII. Durable Principles of Multimodal Understanding**

### **I. Evidence Before Answer Generation**

A multimodal system must select, inspect, and verify visual, spatial, tabular, or temporal evidence before generating factual claims. Generation without evidence selection is visual hallucination with better lighting.

### **II. Structure Is Semantic Content**

Spatial layout, row/column grids, axis scales, legends, captions, footnotes, coordinates, and frame order carry meaning. Flattening structure into plain text destroys evidence.

### **III. Coordinates Are Part of the Claim**

A grounded multimodal claim should map to page boxes, cell coordinates, chart marks, visual regions, UI elements, or timestamp intervals. Document-level citation is not enough for high-stakes visual reasoning.

### **IV. Preserve the Coordinate Chain**

The system must preserve mappings through ingestion, rendering, parsing, cropping, embedding, extraction, synthesis, citation, and replay. Any break in the chain lowers citation fidelity.

### **V. Route by Modality and Task**

Do not force every artifact through the same parser or index. Text-native clauses, scanned forms, dense tables, chart images, diagrams, screenshots, and videos require different processing routes.

### **VI. Inspect Adequacy Before Trusting Evidence**

A retrieved crop may be relevant but insufficient. The system must check resolution, context, headers, axis labels, legends, footnotes, frame coverage, and uncertainty before answering.

### **VII. Prefer Structured Extraction for Quantitative Claims**

Tables and charts should be converted into structured data before calculations. Numeric claims should come from cells, axes, marks, or extracted data tables, not impressions.

### **VIII. Treat Readable Text in Images as Data, Not Instruction**

OCR text, screenshots, scanned documents, and visual prompts may contain instruction-like content. Downstream models must treat this material as evidence data, not executable commands.

### **IX. Degraded Mode Must Be Honest**

When OCR, layout parsing, visual grounding, video sampling, or citation mapping degrades, the answer must expose the limitation. Reduced evidence quality must reduce claim strength.

### **X. Multimodal Claims Must Be Replayable**

Auditors and regression tests must be able to reconstruct the claim from source hash, parser version, rendered page/frame, crop, coordinates, extraction result, and citation object.

### **XI. Suppressed Metadata Is Still Evidence**

Headers, footers, watermarks, confidentiality labels, page numbers, footnotes, and captions may be omitted from the main reading stream, but they should not be discarded. They often carry version, authority, or interpretive context.

### **XII. Plausible Text Is Not Visual Grounding**

The final invariant:

```text
No coordinate chain, no grounded multimodal claim.
No adequate inspection, no confident answer.
No verified evidence, no user-facing certainty.
```

#### **Works cited**

1. Visual RAG Toolkit: Scaling Multi-Vector Visual Retrieval with Training-Free Pooling and Multi-Stage Search - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2602.12510v1](https://arxiv.org/html/2602.12510v1)  
2. Multimodal RAG: Retrieving from Images, PDFs, and Tables | Tensoria, accessed June 10, 2026, [https://tensoria.fr/en/blog/multimodal-rag-images-pdfs-tables](https://tensoria.fr/en/blog/multimodal-rag-images-pdfs-tables)  
3. From Raw PDF to Qdrant Search Engine: Choosing the Right Document Parser for Your RAG Pipeline | by M K Pavan Kumar - Towards AI, accessed June 10, 2026, [https://pub.towardsai.net/from-raw-pdf-to-qdrant-search-engine-choosing-the-right-document-parser-for-your-rag-pipeline-5933bbbc7213](https://pub.towardsai.net/from-raw-pdf-to-qdrant-search-engine-choosing-the-right-document-parser-for-your-rag-pipeline-5933bbbc7213)  
4. What is Table Extraction OCR? - LlamaIndex, accessed June 10, 2026, [https://www.llamaindex.ai/glossary/table-extraction-ocr](https://www.llamaindex.ai/glossary/table-extraction-ocr)  
5. Revolutionizing Document Intelligence: The Strategic Advantage of IBM Granite-Docling VLM | by Mustafa Genc | Medium, accessed June 10, 2026, [https://medium.com/@mustafa.gencc94/revolutionizing-document-intelligence-the-strategic-advantage-of-ibm-granite-docling-vlm-71f8bf1357e4](https://medium.com/@mustafa.gencc94/revolutionizing-document-intelligence-the-strategic-advantage-of-ibm-granite-docling-vlm-71f8bf1357e4)  
6. Spatially-Grounded Document Retrieval via Patch-to-Region Relevance Propagation - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2512.02660v2](https://arxiv.org/html/2512.02660v2)  
7. [Literature Review] Spatially-Grounded Document Retrieval via Patch-to-Region Relevance Propagation, accessed June 10, 2026, [https://www.themoonlight.io/en/review/spatially-grounded-document-retrieval-via-patch-to-region-relevance-propagation](https://www.themoonlight.io/en/review/spatially-grounded-document-retrieval-via-patch-to-region-relevance-propagation)  
8. BBox-DocVQA: A Large-Scale Bounding-Box–Grounded Dataset for Enhancing Reasoning in Document Visual Question Answer - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2511.15090v1](https://arxiv.org/html/2511.15090v1)  
9. BBox DocVQA: Spatially Grounded DocVQA - Emergent Mind, accessed June 10, 2026, [https://www.emergentmind.com/topics/bbox-docvqa](https://www.emergentmind.com/topics/bbox-docvqa)  
10. The PDF Parser That Refuses to Be Helpful — And Why That's the Point - Towards AI, accessed June 10, 2026, [https://pub.towardsai.net/the-pdf-parser-that-refuses-to-be-helpful-and-why-thats-the-point-82c21508f214](https://pub.towardsai.net/the-pdf-parser-that-refuses-to-be-helpful-and-why-thats-the-point-82c21508f214)  
11. LensVLM: Selective Context Expansion for Compressed Visual Representation of Text, accessed June 10, 2026, [https://arxiv.org/html/2605.07019v1](https://arxiv.org/html/2605.07019v1)  
12. POTATR: A Lightweight Image-to-Graph Model for Page-Level Table Extraction - arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2606.09788](https://arxiv.org/pdf/2606.09788)  
13. [2212.10505] DePlot: One-shot visual language reasoning by plot-to-table translation - ar5iv, accessed June 10, 2026, [https://ar5iv.labs.arxiv.org/html/2212.10505](https://ar5iv.labs.arxiv.org/html/2212.10505)  
14. Enhancing Question Answering on Charts Through Effective Pre-training Tasks - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2406.10085v1](https://arxiv.org/html/2406.10085v1)  
15. VideoITG: Multimodal Video Understanding with Instructed Temporal Grounding - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2507.13353v2](https://arxiv.org/html/2507.13353v2)  
16. VideoITG: Multimodal Video Understanding with Instructed Temporal Grounding - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2507.13353v1](https://arxiv.org/html/2507.13353v1)  
17. ColPali: Efficient Document Retrieval with Vision Language Models - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2407.01449v2](https://arxiv.org/html/2407.01449v2)  
18. ColPali and Multimodal Document RAG on GPU Cloud: Visual PDF Retrieval Without OCR (2026) | Spheron Blog, accessed June 10, 2026, [https://www.spheron.network/blog/colpali-multimodal-document-rag-gpu-cloud/](https://www.spheron.network/blog/colpali-multimodal-document-rag-gpu-cloud/)  
19. ColPali for RAG (2025): Theory, implementation, and production tips | by Issa Hammoud, accessed June 10, 2026, [https://medium.com/@intuitivedl/rag-with-colpali-everything-you-need-to-know-46b7bd50901b](https://medium.com/@intuitivedl/rag-with-colpali-everything-you-need-to-know-46b7bd50901b)  
20. [2512.02660] Spatially-Grounded Document Retrieval via Patch-to-Region Relevance Propagation - arXiv, accessed June 10, 2026, [https://arxiv.org/abs/2512.02660](https://arxiv.org/abs/2512.02660)  
21. PubTables-v2: A new large-scale dataset for full-page and multi-page table extraction - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2512.10888v1](https://arxiv.org/html/2512.10888v1)  
22. Introducing PubTables-v2: Kensho's New Dataset Empowering Next-Gen AI for Table Extraction, accessed June 10, 2026, [https://kensho.com/news/introducing-pubtables-v2-kenshos-new-dataset-empowering-next-gen-ai-for-table-extraction](https://kensho.com/news/introducing-pubtables-v2-kenshos-new-dataset-empowering-next-gen-ai-for-table-extraction)  
23. TABLET: Table Structure Recognition using Encoder-only Transformers - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2506.07015v1](https://arxiv.org/html/2506.07015v1)  
24. [Literature Review] TABLET: Table Structure Recognition using Encoder-only Transformers, accessed June 10, 2026, [https://www.themoonlight.io/en/review/tablet-table-structure-recognition-using-encoder-only-transformers](https://www.themoonlight.io/en/review/tablet-table-structure-recognition-using-encoder-only-transformers)  
25. DEPLOT: One-shot visual language reasoning by plot-to-table translation - ACL Anthology, accessed June 10, 2026, [https://aclanthology.org/2023.findings-acl.660.pdf](https://aclanthology.org/2023.findings-acl.660.pdf)  
26. Event-Anchored Frame Selection for Effective Long-Video Understanding - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2603.00983v1](https://arxiv.org/html/2603.00983v1)  
27. [2603.00983] Event-Anchored Frame Selection for Effective Long-Video Understanding - arXiv, accessed June 10, 2026, [https://arxiv.org/abs/2603.00983](https://arxiv.org/abs/2603.00983)  
28. TempCore: Are Video QA Benchmarks Temporally Grounded? A Frame Selection Sensitivity Analysis and Benchmark - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2509.01167v2](https://arxiv.org/html/2509.01167v2)  
29. Best PDF Parsers for AI and RAG Workflows in 2026 - Firecrawl, accessed June 10, 2026, [https://www.firecrawl.dev/blog/best-pdf-parsers](https://www.firecrawl.dev/blog/best-pdf-parsers)  
30. Optimal RAG Chunking : 8 Strategies & Real Benchmarks - Anas Rabhi, accessed June 10, 2026, [https://ianas.fr/en/blog/2026/04/15/chunking-optimal-rag/](https://ianas.fr/en/blog/2026/04/15/chunking-optimal-rag/)  
31. Python PDF text extraction with PyMuPDF: Scanned PDFs, tables, and OCR - Nutrient iOS, accessed June 10, 2026, [https://www.nutrient.io/blog/extract-text-from-pdf-pymupdf/](https://www.nutrient.io/blog/extract-text-from-pdf-pymupdf/)  
32. olmOCR – Open-Source OCR for Accurate Document Conversion - Allen Institute for Artificial Intelligence, accessed June 10, 2026, [https://olmocr.allenai.org/](https://olmocr.allenai.org/)  
33. docling-project/granite-docling-2stage-258m - Hugging Face, accessed June 10, 2026, [https://huggingface.co/docling-project/granite-docling-2stage-258m](https://huggingface.co/docling-project/granite-docling-2stage-258m)  
34. [2603.13032] Multimodal OCR: Parse Anything from Documents - arXiv, accessed June 10, 2026, [https://arxiv.org/abs/2603.13032](https://arxiv.org/abs/2603.13032)  
35. allenai/olmocr: Toolkit for linearizing PDFs for LLM datasets/training - GitHub, accessed June 10, 2026, [https://github.com/allenai/olmocr](https://github.com/allenai/olmocr)  
36. Unsiloed Achieves #1 Rank on olmOCR-Bench, accessed June 10, 2026, [https://www.unsiloed.ai/blog/unsiloed-ai-achieves-1-rank-on-olmocr-bench-2](https://www.unsiloed.ai/blog/unsiloed-ai-achieves-1-rank-on-olmocr-bench-2)  
37. AI OCR & Document Processing Tools Comparison - Artificial Analysis, accessed June 10, 2026, [https://artificialanalysis.ai/agents/ocr](https://artificialanalysis.ai/agents/ocr)  
38. POTATR: A Lightweight Image-to-Graph Model for Page-Level Table Extraction - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2606.09788](https://arxiv.org/html/2606.09788)  
39. PubTables-v2: A new large-scale dataset for full-page and multi-page table extraction - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2512.10888v2](https://arxiv.org/html/2512.10888v2)  
40. Daily Papers - Hugging Face, accessed June 10, 2026, [https://huggingface.co/papers?q=Encoder-only](https://huggingface.co/papers?q=Encoder-only)  
41. A Comparison of Image-based, Text-based and Multimodal Models in the Table Structure Recognition Task - OpenReview, accessed June 10, 2026, [https://openreview.net/pdf/a586861b6d8335e7c04ff733d5d53c3c34384629.pdf](https://openreview.net/pdf/a586861b6d8335e7c04ff733d5d53c3c34384629.pdf)  
42. Table Transformer (TATR) - Table Structure Recognition - AIKosh, accessed June 10, 2026, [https://aikosh.indiaai.gov.in/home/models/details/table_transformer_tatr_table_structure_recognition.html](https://aikosh.indiaai.gov.in/home/models/details/table_transformer_tatr_table_structure_recognition.html)  
43. LightOnOCR-bbox-bench: Document Image Localization - Emergent Mind, accessed June 10, 2026, [https://www.emergentmind.com/topics/lightonocr-bbox-bench](https://www.emergentmind.com/topics/lightonocr-bbox-bench)  
44. PubTables-v2: A new large-scale dataset for full-page and multi-page table extraction - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2512.10888v3](https://arxiv.org/html/2512.10888v3)  
45. We improved table extraction in DocParse with a new AI model - Aryn, accessed June 10, 2026, [https://www.aryn.ai/post/we-improved-table-extraction-in-docparse-with-a-new-ai-model](https://www.aryn.ai/post/we-improved-table-extraction-in-docparse-with-a-new-ai-model)  
46. GenPlot: Increasing the Scale and Diversity of Chart Derendering Data - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2306.11699](https://arxiv.org/html/2306.11699)  
47. Spatially-Grounded Document Retrieval via Patch-to-Region Relevance Propagation - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2512.02660v1](https://arxiv.org/html/2512.02660v1)  
48. Wang Chen's research works | Fuzhou University and other places - ResearchGate, accessed June 10, 2026, [https://www.researchgate.net/scientific-contributions/Wang-Chen-2264289193](https://www.researchgate.net/scientific-contributions/Wang-Chen-2264289193)  
49. Luojun Lin - CatalyzeX, accessed June 10, 2026, [https://www.catalyzex.com/author/Luojun%20Lin](https://www.catalyzex.com/author/Luojun%20Lin)  
50. Late interaction models: How to scale & optimize in Elasticsearch, accessed June 10, 2026, [https://www.elastic.co/search-labs/blog/late-interaction-model-colpali-scale](https://www.elastic.co/search-labs/blog/late-interaction-model-colpali-scale)  
51. Grounding is All You Need? Dual Temporal Grounding for Video Dialog - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2410.05767v2](https://arxiv.org/html/2410.05767v2)

---

# AI-ENG-Q — Speech, Voice, and Real-Time Interaction Systems

Real-time voice systems represent a fundamental departure from traditional request-response text architectures. In a text-based system, information arrives as a complete, static artifact with explicit, deterministic boundaries. In contrast, spoken human interaction is a continuous, bi-directional, temporal stream where timing, pauses, overlaps, pitch, and physical acoustics carry critical semantic meaning.1 A real-time voice interaction system cannot be built merely by wrapping a text-based Large Language Model (LLM) in batch transcription and text-to-speech APIs.4 When voice interfaces are treated as text generation with audio input/output bolted on, the resulting systems fail under human conversational norms, manifesting high latency, false endpointing, fragile interruption mechanics, and unsafe tool execution on unstable transcripts.4  
The foundational architecture of a high-dimensional real-time voice system is therefore defined as a temporal state-coordination system. This architecture must process continuous streaming media under strict latency budgets, manage the conversational floor across overlapping speaker turns, filter background noise and echo to allow natural user barge-in, repair verbal misunderstandings without degrading progressivity, verify identity, and protect agentic tools from executing on partial or misheard intents.4  
This report establishes the system architecture, state models, and operational profiles required to build resilient, latency-bounded, and safe real-time voice systems.

## **Conceptual Glossary**

The following glossary defines the core terms and operational metrics governing high-dimensional speech, voice, and real-time interaction systems.

| Term | Technical Definition | Primary Operational Metric | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **Speech-to-Text (STT)** | The conversion of continuous acoustic audio frames into structured text tokens or word hypotheses via streaming neural decoders.10 | Word Error Rate (WER) / Entity-specific WER 12 | < 5% English WER, < 2% Entity-WER 12 |
| **Text-to-Speech (TTS)** | The synthesis of structured text tokens into audible streaming waveforms with specified prosodic and expressive parameters.10 | Time-to-First-Audio (TTFA) / Mean Opinion Score (MOS) 4 | < 200 ms TTFA, > 4.2 MOS 4 |
| **Realtime Session** | A persistent, stateful, bi-directional network connection handling concurrent media streaming, event signaling, and state updates.15 | Jitter / Packet Loss / Session Reconnection Latency 14 | < 10 ms Jitter, Jitter < 10 ms; near-zero effective packet loss after concealment; media reconnect target defined by deployment profile. |
| **Streaming Transcription** | The ongoing emission of transcripts as audio frames arrive, returning both unstable intermediate guesses and locked historical frames.10 | Partial-to-Final Latency / Revision Rate 6 | < 150 ms Partial Latency, < 5% Revision Rate 12 |
| **Partial Transcript** | A speculative, volatile transcript hypothesis generated from incomplete audio context, subject to downstream correction by the STT engine.4 | Stability index / Token volatility 6 | Display-only; never sent to high-risk tools 4 |
| **Final Transcript** | An immutable, locked transcript segment emitted once the STT decoder establishes high confidence over the accumulated acoustic context.4 | Segment Commit Latency 4 | 300 ms to 500 ms post-utterance 4 |
| **Transcript Stability** | A probabilistic metric indicating the likelihood that currently generated partial transcript tokens will match the final emitted tokens.6 | Stability Confidence Score 6 | Threshold > 0.85 for speculative execution 6 |
| **Voice Activity Detection (VAD)** | A lightweight binary classification process identifying the presence or absence of human speech within short, sequential audio frames.1 | Frame-level Precision/Recall, False Trigger Rate 18 | Precision > 0.98, False Trigger < 2 per hour 1 |
| **Endpointing** | The decision logic determining whether a speaker has completed a conversational turn based on silence, prosodic cues, and semantic completeness.1 | Turn-End Detection Latency / Cutoff Error Rate 4 | < 300 ms Latency, < 1.5% Cutoff Error 4 |
| **Turn-Taking** | The protocol and state machine governing conversational floor transitions, preventing simultaneous speech while maintaining responsiveness.22 | Turn Gap Duration / Overtalk Overlap Time 22 | 200 ms to 450 ms Turn Gap 5 |
| **Backchannel** | Minimal vocal gestures (e.g., "uh-huh", "mm-hmm") emitted to indicate active listening and floor preservation without claiming the floor. | Backchannel Precision / False Interruption Rate | Precision > 0.90 under noise |
| **Barge-In** | The user action of speaking while the agent is playing audio, requiring the agent to immediately halt output and process the new input.5 | Interruption Detection Latency / Playback Flush Time 5 | < 150 ms from Speech Onset to TTS Flush 5 |
| **Interruption** | A conversational event where one speaker's active turn is terminated prematurely by the speech onset of another speaker.5 | Interruption False Alarm Rate | < 1.0% on environmental noise/laughter |
| **Floor Holding** | Acoustic and linguistic signals used by a speaker to retain the conversational floor during pauses, preventing premature agent endpointing.22 | Hold vs Shift Classification Accuracy 22 | Hold classification > 0.95 on trailing fillers |
| **Conversational Repair** | Interactive sequences where the system or user resolves speaking, hearing, or understanding breakdowns to restore mutual comprehension.7 | Repair Completion Rate / Conversation Abandonment Rate 26 | > 92% Repair Success, < 3% Abandonment 26 |
| **Speaker Diarization** | The process of partitioning an audio stream into distinct segments associated with individual speaker identities ("who spoke when").3 | Diarization Error Rate (DER) / Turn-Boundary DER 3 | DER < 10% in multi-speaker environments 3 |
| **Speaker Recognition** | The biometric verification or identification of a speaker based on the unique acoustic characteristics of their voice profile.3 | Equal Error Rate (EER) / False Acceptance Rate (FAR) 27 | EER < 0.1% under clean conditions 27 |
| **Voice Identity** | The unique, auditable representation of a speaker's vocal characteristics used for access control, styling, or authentication.3 | Identity Match Confidence / Authentication F1-score 27 | F1-score > 0.99 for gated mutations 27 |
| **Synthetic Voice** | Programmatically synthesized speech designed to sound like a specific human or styled persona.10 | Naturalness MOS / Persona Alignment Score 10 | MOS > 4.3 10 |
| **Latency Budget** | The end-to-end allocation of time segments across all processing components in a streaming interaction loop.4 | End-to-End Latency (T_streaming) 4 | Target < 700 ms, Hard Limit < 1000 ms 4 |
| **Voice-to-Action Gating** | The protective verification layers preventing unconfirmed, volatile, or unauthorized speech from triggering high-impact system mutations. | Gating Precision / Unintended Action Rate | 100% precision on irreversible tools |

## **Realtime Voice Interaction Pipeline**

A production-grade real-time voice system operates as a distributed, stream-aligned architecture. The end-to-end pipeline must coordinate multiple asynchronous systems, each running on distinct clocks, over low-latency media transport protocols.4

```
+--------------------------------------------------------------------------------
| REALTIME VOICE INTERACTION PIPELINE
+--------------------------------------------------------------------------------
|
| Client Media Plane
|
|   [ Microphone Capture ]
|          |
|          v
|   [ Acoustic Preprocessing ]
|     AEC / noise suppression / AGC / local VAD
|          |
|          v
|   [ Opus Encode + WebRTC Uplink ]
|          |
|          v
|
| Edge Media Plane
|
|   [ WebRTC Termination ]
|     ICE / DTLS / SRTP / decrypt / jitter management
|          |
|          +--> [ Barge-In Path ]
|          |       client VAD -> playback mute -> truncate signal
|          |
|          v
|   [ PCM Audio Stream ]
|     timestamped frames + session sequence IDs
|          |
|          v
|
| Perception Plane
|
|   [ Streaming STT ]
|          |
|          +--> partial hypotheses
|          |       captions | prefetch | speculative read-only planning
|          |
|          +--> final segments
|                  locked transcript | word timings | confidence
|          |
|          v
|
| Dialogue / Control Plane
|
|   [ Turn-Taking State Machine ]
|     VAD + endpointing + floor ownership + repair state
|          |
|          v
|   [ Agent Reasoning and Policy Gate ]
|     context assembly | tool gating | confirmation | action verification
|          |
|          v
|
| Output Media Plane
|
|   [ Streaming LLM Tokens ]
|          |
|          v
|   [ Sentence / Clause Buffer ]
|          |
|          v
|   [ Streaming TTS ]
|          |
|          v
|   [ Playback Jitter Buffer ]
|          |
|          v
|   [ Speaker Output ]
|
+--------------------------------------------------------------------------------
| Invariant:
|   Partial transcripts may assist speculation.
|   Finalized turns may drive reasoning.
|   Verified actions may drive completion claims.
+--------------------------------------------------------------------------------
```

### **Media Transport Infrastructure and Session Edge Architecture**

Traditional request-response architectures terminate traffic using standard stateless HTTP endpoints. For real-time voice, bidirectional transport must bypass standard TCP handshakes to avoid the latency penalties of congestion control and packet retransmission.4 Production systems implement a hybrid architecture of WebRTC and WebSockets 31:

* **WebSockets**: Effective for control signaling, session setup, and low-concurrency streaming where packet-loss handling (TCP) does not introduce latency cliffs under clean network conditions.4  
* **WebRTC**: The standard for production-grade, high-scale bidirectional media transport.31 WebRTC utilizes UDP as its base transport, wrapping packets in Secure Real-time Transport Protocol (SRTP) for media and Datagram Transport Layer Security (DTLS) for key exchange.30

To avoid the overhead of multi-party Selective Forwarding Units (SFUs) where the AI acts as a participant in a group session, production systems for 1:1 interactions use a **Transceiver Edge Model**.30 In this model, a geographically distributed edge service terminates downstream WebRTC connections directly, handling Interactive Connectivity Establishment (ICE), DTLS handshakes, and SRTP decryption at the edge.30 The decrypted media is then converted into lightweight, internal protocols (such as raw gRPC or flat UDP streams) for upstream routing to model inference and orchestration clusters.30  
To route incoming UDP media packets to the correct stateful transceiver instance without an expensive database lookup on every packet, systems route on **ICE Credentials**.30 During Session Description Protocol (SDP) negotiation, the edge service generates a unique ICE username fragment (ufrag) containing encoded routing metadata.30 When the client sends its first STUN binding request to establish connectivity, the stateless network load balancers parse the ufrag from the STUN header and steer the UDP stream to the designated transceiver container.30 If a transceiver or relay restarts, the next incoming STUN packet rebuilds the path instantly based on the ufrag routing hint.30

### **Audio Evidence Provenance**

To maintain consistency and auditability, real-time voice systems must preserve a continuous trace mapping generated text tokens and tool calls back to the raw source audio segments. Every audio frame ingested by the transceiver edge is assigned a monotonically increasing timestamp and sequence identifier. The streaming STT engine maps word-level start and end times back to these frame offsets.4  
When the dialogue coordinator transitions its state or triggers an external tool contract, it logs the precise audio segment boundaries and transcript stability states that justified the action. This ensures that every mutation can be audited back to the raw physical signal, protecting against hallucinations or errors where a tool executes on ungrounded text.

### **Media Plane and Signal Plane Execution Pipeline**

The execution flow of the real-time voice pipeline translates physical acoustic pressure into state transformations and back into physical acoustics.4 The pipeline segments these responsibilities into distinct software layers.

| Sequence Stage | Local Pipeline Step | Executing Infrastructure Layer | Local Protocol / Format | Critical Failure Risk |
| :---- | :---- | :---- | :---- | :---- |
| **1. Capture** | Microphone Audio Ingestion | Client-side App (Native SDK) | Linear PCM 16-bit, 16kHz, Mono 33 | Audio capture permission block 28 |
| **2. Preprocess** | WebRTC AEC3 Echo & Noise Filtering | Client AudioWorklet (WebAssembly) 34 | Float32 Internal -> PCM conversion 34 | Adaptive filter divergence on Android 35 |
| **3. Encode** | Audio Frame Compression | Client Opus Encoder | Opus packets over UDP (WebRTC) 34 | Packet dropped due to local thread lag 34 |
| **4. Transport** | Secure Packet Uplink Transit | Public/Transit IP Network | DTLS/SRTP via WebRTC Transceiver 30 | Network jitter, cross-region routing delay 4 |
| **5. Terminate** | WebRTC termination & Decryption | Server Edge (Transceiver Cluster) 30 | Decrypted PCM Int16 raw buffer 30 | Ephemeral port exhaustion under Kubernetes 30 |
| **6. Local VAD** | Client/Server Double VAD pass | Server Edge (Silero Wasm Thread) 36 | 10/20ms Frame classification 18 | High False Positives on street background noise 5 |
| **7. Stream STT** | Acoustic-to-Linguistic Decoding | STT Engine (WebSocket Pool) 11 | Bidirectional WebSocket JSON 11 | Model drift, phonetic spelling failure 10 |
| **8. Revise** | Partial Transcript update cycle | Dialogue Context Manager | Interim results collection (is_final=False) 6 | Premature execution on highly volatile phrases 6 |
| **9. Endpoint** | Spoken turn completion parsing | Semantic Turn Classifier | Semantic & Prosodic Feature Scoring | Cutting off users with assistive stutters |
| **10. Reason** | Think-Before-Speak execution | LLM Agent Execution Cluster 38 | In-memory token representation 38 | Context window token ceiling, high TTFT |
| **11. Gate** | Spoken action authorization check | Security & Policy Layer | Action schema-to-transcript mapping | Executing destructive tool on misheard entity 9 |
| **12. Synthesize** | Neural speech output compilation | Streaming TTS Cluster (Sonic Engine) 5 | Opus/PCM stream segment output 5 | Audio gaps, mechanical voice sounding 4 |
| **13. Playback** | Waveform output conversion | Client App Playback Engine | PCM 16-bit, 24kHz, Mono 33 | Glitching "chipmunk effect" on mismatch 33 |
| **14. Log & Trace** | Comprehensive Session auditing | Observability Storage Platform 28 | Structured OpenTelemetry session JSON 28 | Exposing raw audio leaks, PI data breach 28 |

## **Speech-to-Text and Streaming Transcription**

Real-time transcription is more than batch speech recognition optimized for speed; it is an active perception pipeline that processes incomplete, overlapping, and changing audio structures.

### **Streaming STT State Model**

The streaming STT decoder operates as a stateful transition engine over rolling audio frames.6 It translates continuous acoustic feature vectors into structured linguistic hypotheses, distinguishing between unstable partial guesses and locked historical frames.6

| Origin State | Transition Trigger Event | Destination State | Retained State Metadata | Downstream System Action |
| :---- | :---- | :---- | :---- | :---- |
| **Acoustic Idle** | Energy level crosses VAD threshold 1 | **Active Frame Ingestion** | Prefix buffer, timestamp onset 5 | Open active STT decoder socket 11 |
| **Active Frame Ingestion** | First frame sequence completed (> 100 ms) 40 | **Partial Hypothesis** (is_final=False) 6 | Speculative text array, confidence score 6 | Render real-time UI captions 34 |
| **Partial Hypothesis** | Subsequent audio frame arrival 6 | **Hypothesis Revision Loop** | Word-level coordinate shifts 32 | Refine speculative intent classification 1 |
| **Hypothesis Revision Loop** | Acoustic probability entropy decreases below threshold 6 | **Locked Segment** (is_final=True) 6 | Solidified token sequence, word timestamps 41 | Append immutable text to LLM context 4 |
| **Locked Segment** | VAD registers silence > threshold 39 | **Utterance Boundary** (speech_final=True) 6 | Final turn sequence number, semantic tag 6 | Trigger model turn reasoning thread 4 |
| **Utterance Boundary** | Core agent response generated 4 | **Acoustic Idle** | Historical dialogue logs, tool receipts 28 | Transition client state back to listening 5 |

### **Transcript Stability Policy**

Because interim results are volatile, acting on them prematurely introduces systemic risk.6 For instance, if a user says, "I want to cancel... my cancellation request," a system acting on the first partial transcript ("I want to cancel") might trigger a destructive account mutation before the corrective phrase ("my cancellation request") is ever processed.6 To mitigate this, real-time architectures enforce a strict **Transcript Stability Policy** that maps permitted system behaviors to transcript states.

| Transcript State | Stability Criteria | Permitted Actions | Prohibited Actions | Recovery Protocol |
| :---- | :---- | :---- | :---- | :---- |
| **Volatile Partial** (is_final: false, Stability < 0.70) 6 | Initial phoneme matching, highly subject to downstream linguistic revision.6 | Local UI caption rendering, speculative intent classification.6 | LLM context serialization, tool execution, external state mutations. | Silently discard the segment if updated frames diverge.6 |
| **Stable Speculative** (is_final: false, Stability >= 0.70) 6 | High acoustic confidence, but waiting for linguistic boundary resolution.11 | Pre-fetching read-only API data, pre-warming vector indexes.4 | Dispatching user replies, committing payments, updating external records. | Flush speculative state cache if final result differs.5 |
| **Locked Segment** (is_final: true, speech_final: false) 6 | Decoder has finalized the phoneme-to-text mapping.6 | Appending text to conversational history, routing to reasoning LLM.4 | Triggering irreversible tools without explicit spoken confirmation. | Wait for final utterance boundary or explicit pause.6 |
| **Utterance Complete** (is_final: true, speech_final: true) 6 | Segment is finalized and the user has executed a valid turn boundary.6 | Executing safe read-only queries, triggering standard dialog response.42 | Mutating critical user accounts or executing high-impact tools. | Validate turn completion against semantic VAD before speaking. |

### **Multilingual Dynamics and Keyterm Prompting**

Under multilingual and code-switching constraints, the STT engine must run dynamic, low-latency streaming language identification.12 If the speaker switches from English to Spanish mid-sentence, a monolingual decoder will hallucinate phonetic approximations, introducing transcription errors.12 Production systems employ parallel acoustic language-identification heads that dynamically adjust the decoder's language model matrix.12  
To maintain high accuracy for specialized, domain-specific vocabularies (such as product names, medical codes, or user-specific contact lists), the STT connection is initialized with a **Keyterm Prompting List**.12 This dynamically biases the decoder's decoding graph towards specified proper nouns or acronyms, substantially reducing entity-specific WER on prioritized domain terms when the keyterm list is well maintained.12

## **Voice Activity Detection, Endpointing, and Turn Boundaries**

Voice Activity Detection (VAD) and Endpointing are distinct control systems within the real-time interaction loop.20 VAD is a low-level, frame-by-frame acoustic classifier that determines the presence or absence of human speech.1 Endpointing is a higher-level, context-aware decision process that determines whether the user has completed their conversational turn and yielded the floor to the assistant.1

### **Endpointing and Turn-Detection Model**

Standard energy-based VAD systems rely on simple volume thresholds: if the audio signal amplitude drops below a specified level for a fixed duration, the system assumes the user has stopped speaking. In production environments, this fails in both directions. Background noise can prevent endpointing, while thoughtful pauses, breath timing, stutters, or mid-sentence planning pauses can trigger premature endpointing.

Modern architectures deploy a **Hybrid Semantic-Acoustic Endpointing Model**. This system combines low-latency acoustic VAD with semantic and prosodic turn-completion features.

The model evaluates:

* **Acoustic speech probability:** frame-level estimate of whether human speech is present.
* **Silence duration:** how long the current gap has lasted.
* **Linguistic completeness:** whether the transcript appears syntactically or semantically complete.
* **Prosodic contour:** whether pitch, rhythm, and trailing intonation imply continuation or completion.
* **Floor-holding signals:** fillers, conjunctions, discourse markers, stutters, repeated syllables, and unfinished clauses.
* **Risk context:** whether the turn may trigger a high-impact action requiring extra confirmation.

```text
+--------------------------------------------------------------------------------
| HYBRID SEMANTIC-ACOUSTIC ENDPOINTING MODEL
+--------------------------------------------------------------------------------
|
| [ Continuous Audio Frames ]
|          |
|          v
| [ Acoustic VAD ]
|   frame speech probability p_speech
|          |
|          +--> speech detected
|          |       reset silence timer
|          |       continue listening
|          |
|          +--> silence detected
|                  start / continue silence timer
|                  |
|                  v
|          [ Endpoint Candidate ]
|                  |
|                  v
|          [ Turn Completion Classifier ]
|            transcript completeness
|            prosody / intonation
|            filler and floor-holding markers
|            accessibility profile
|            action risk profile
|                  |
|          +-------+-----------------------------+
|          |                                     |
|          v                                     v
| [ Complete / Yield Floor ]          [ Incomplete / Hold Floor ]
|   emit endpoint                       extend hangover window
|   lock turn                           wait for more speech
|   trigger reasoning                   preserve active floor
|
+--------------------------------------------------------------------------------
| Policy:
|   VAD detects speech.
|   Endpointing decides whether the turn is complete.
|   Silence alone is not a turn boundary.
+--------------------------------------------------------------------------------
```

When silence is detected acoustically, the system calculates a dynamic hangover window:

| Condition | Endpointing Behavior |
| :--- | :--- |
| Transcript is complete and low-risk | Endpoint quickly after a short silence window. |
| Transcript ends with filler, conjunction, or unfinished phrase | Extend silence window and preserve user floor. |
| User profile indicates stutter, aphasia, dysarthria, or slow pacing | Use accessibility profile and longer endpoint window. |
| Environment is noisy | Require stronger speech/silence confidence and semantic confirmation. |
| High-impact action is possible | Require final transcript, semantic completeness, and explicit confirmation. |
| Barge-in or correction is detected | Yield assistant floor immediately and enter repair/interruption path. |

To programmatically control this behavior, production platforms expose VAD and endpointing parameters such as acoustic threshold, prefix padding, silence duration, semantic eagerness, min/max turn silence, and interruption delay. These parameters should be treated as **policy profiles**, not universal constants.

### **Endpointing Policy Profiles**

A single endpointing configuration cannot satisfy all conversational contexts. A fast response is desirable for home automation commands, but causes frustration during complex cognitive tasks or for users with speech impairments.1 Architectures must implement specialized **Endpointing Policy Profiles**.1

| Profile Name | Target Use Case | Silence Threshold (T_silence) | VAD Sensitivity / Energy Threshold | Semantic Guarding Policy | Target User Segment / Context |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Command & Control** | Voice-driven utility execution (e.g., "turn on lights"). | 200 ms - 300 ms 19 | High sensitivity / Low energy threshold (aggressive).39 | Disabled; execute instantly on keyword detection.39 | Hands-busy environments, low cognitive overhead.46 |
| **Casual Conversation** | Natural, open-ended dialog.4 | 400 ms - 600 ms 19 | Moderate sensitivity (balance noise vs speech).18 | Enabled; verify clause resolution before yielding floor. | Standard customer service, informational voicebots.4 |
| **Dictation / Transcription** | Continuous spoken capture, long paragraphs.10 | 1200 ms - 2000 ms 21 | High sensitivity; long hangover padding.17 | Disabled; wait for explicit "period" keyword or long pause.21 | Legal, medical, or administrative text entry.10 |
| **High-Impact Action** | Financial mutations, secure database changes.28 | 800 ms - 1200 ms 19 | Low sensitivity (avoid false starts on noise).39 | Strict; require full grammatical resolution plus readback. | Banking, secure credential updates, account changes.28 |
| **Inclusive / Accessibility** | Support stutters, slow pacing, dysarthria.46 | 1500 ms - 3000 ms 19 | High tolerance; ignore rapid repetitive syllable bursts. | Strict; disable endpointing on repetitive sound patterns. | Users with neurological speech differences.46 |
| **Noisy Environment** | Cafes, open offices, industrial floors.17 | 600 ms - 1000 ms 19 | Low sensitivity / High energy threshold (WebRTC level 3).19 | Enabled; filter out sudden ambient sound spikes.1 | Mobile users, field operations, public transport.17 |
| **Group Conversation** | Multi-speaker business meetings, panels.3 | 800 ms - 1500 ms 3 | High sensitivity + Speaker Diarization mapping.3 | Strict; keep turn open if primary speaker pauses but holds floor.1 | Corporate boardrooms, multi-party calls.3 |

## **Turn-Taking and Dialogue State**

Turn-taking is managed by a state machine that tracks floor ownership. The system must prevent overtalk (where both speak simultaneously) and dead air (where neither speaks), transitioning states based on VAD, transcript stability, and semantic completions.22

### **Turn-Taking State Machine**

The following matrix defines the state transitions of the Turn-Taking State Machine.

| Source State | Input Event Trigger | Destination State | Triggering Mechanism | State Machine System Actions |
| :---- | :---- | :---- | :---- | :---- |
| **IDLE** | VAD Speech Started | **LISTENING** | Audio energy level crosses VAD threshold 1 | Open streaming ASR connection, start context buffering.4 |
| **LISTENING** | Partial Transcript Received | **PROVISIONAL_TRANSCRIPT** | Interim results payload parsed from socket 6 | Render real-time visual captions, pre-warm caches.4 |
| **PROVISIONAL_TRANSCRIPT** | VAD Silence > threshold | **ENDPOINT_CANDIDATE** | Local silence timer passes 300 ms window 19 | Halt active transcript appending, trigger turn completion model. |
| **ENDPOINT_CANDIDATE** | Semantic Resolution Completed | **USER_TURN_FINAL** | Turn completeness model returns probability > 0.85 | Lock finalized ASR transcript segment, compile prompt.6 |
| **ENDPOINT_CANDIDATE** | New Speech Detected | **LISTENING** | Audio energy level crosses threshold before timeout 1 | Reset local silence timer, continue transcript stream.1 |
| **ENDPOINT_CANDIDATE** | Timeout Exceeded | **USER_TURN_FINAL** | Silence duration passes forced floor timeout 21 | Force close the active speaking turn.21 |
| **USER_TURN_FINAL** | Dialogue Reasoning Invoked | **THINKING** | Prompt dispatched to core agent LLM 4 | Execute tool gating checks, initiate reasoning thread.9 |
| **THINKING** | API/Tool Execution Invoked | **TOOL_CHECKING** | LLM outputs speculative tool schema 4 | Suspend spoken generation, route parameters to tool runner.28 |
| **TOOL_CHECKING** | Tool Return Verified | **THINKING** | API response parsed and validated 4 | Append tool payload to context, resume linguistic path.4 |
| **THINKING** | First Linguistic Token Emitted | **SPEAKING** | LLM outputs structural response token | Launch streaming TTS pipeline, initialize client playback. |
| **SPEAKING** | LLM Stream Paused Mid-Turn | **BACKCHANNELING** | LLM outputs explicit wait or placeholder token | Play light acoustic feedback gesture ("uh-huh"). |
| **SPEAKING** | VAD Speech Started (Barge-In) | **INTERRUPTED** | User interrupts playing assistant stream 5 | Flush client-side audio buffer, cancel reasoning.5 |
| **SPEAKING** | Synthesis Complete | **IDLE** | Audio playback playhead reaches termination marker 1 | Reset dialogue state flags, return to passive listening.5 |
| **INTERRUPTED** | Interruption Processing Complete | **REPAIR** | Truncation playhead timestamp aligned 5 | Log partial response, load repair conversational patterns.5 |
| **REPAIR** | Misunderstanding Resolved | **THINKING** | User confirmation text committed 8 | Compile corrected prompt, restart generation thread.4 |
| **IDLE / LISTENING** | Critical Protocol Loss | **TERMINATED** | Media socket connection drops > 30 s 16 | Tear down WebRTC transceiver paths, log metrics.30 |

### **Conversational Dynamics**

To maintain natural dialogue flow, the dialogue coordinator must distinguish between different communicative actions:

* **User Turn**: The user has the floor and is transmitting semantic intent.4  
* **Assistant Turn**: The agent has the floor and is playing audio.  
* **Backchannel**: A non-intrusive vocal gesture (e.g., "uh-huh") emitted by the listener. When the user is speaking and emits a brief pause, the assistant may issue a backchannel to signal active listening. This does not trigger an assistant turn transition.  
* **Floor Holding**: The user pauses but emits acoustic filler signals ("uhhh") or grammatical continuity markers ("because...") indicating they intend to retain the floor.  
* **Turn Yield**: The active speaker completes their statement and explicitly yields the floor, marked by falling pitch contours and silence.22  
* **Barge-In / Overlap**: Both speakers speak simultaneously.5 The coordinator must classify this event: if the user's speech is a brief backchannel, the assistant continues speaking; if it is a structural correction or stop command, the assistant yields the floor immediately.  
* **Conversational Repair**: A sub-dialogue loop triggered when either party detects a communication breakdown, holding progressivity until mutual understanding is restored.7  
* **Confirmation Turn**: A dedicated turn where the system asks the user to confirm a high-risk entity or tool payload.  
* **Status Turn**: A temporary turn where the system informs the user of a background execution delay to prevent awkward silences.  
* **Handoff Turn**: An administrative turn transitioning floor control and context data to a human operator or fallback channel.

If the turn-taking state machine fails, several user-visible pathologies occur: **Overtalk** (the agent speaks over the user, causing frustration), **Dead Air** (the agent hesitates, leaving the user unsure if the connection is active), and **Clipped Speech** (the agent speaks too quickly, cutting off the end of the user's words).22

## **Latency Budgets and Realtime System Margins**

Human conversational turn-taking operates on a 200 ms to 500 ms rhythm.22 If a voice agent exceeds a 700 ms response delay, the interaction feels lagging or unresponsive.4 If it exceeds 1000 ms, the conversation breaks down as users assume a system error and repeat themselves, triggering race conditions.31

### **Realtime Latency Budget Model**

The perceived latency of a real-time voice system is not simply the sum of every component in the pipeline. Some stages are sequential, but many are pipelined or concurrent. P50 and P95 component latencies should therefore be used for budget allocation and bottleneck diagnosis, not blindly added as if all stages block the same critical path.

A useful latency model distinguishes:

| Latency Type | Meaning |
| :--- | :--- |
| **End-of-speech to first audio** | User stops speaking; assistant begins audible response. |
| **Partial-speech to speculative work** | User is still speaking; system prefetches, prewarms, or plans safely. |
| **Turn-complete to first token** | Finalized turn enters reasoning; model emits first linguistic token. |
| **First token to first audio** | Text stream begins; sentence/clause buffer and TTS produce audio. |
| **Barge-in latency** | User starts interrupting; assistant playback stops. |

```text
+--------------------------------------------------------------------------------
| REALTIME LATENCY BUDGET MODEL
+--------------------------------------------------------------------------------
|
| Sequential Critical Path: End of User Speech -> First Assistant Audio
|
|   [ Endpointing Decision ]
|          |
|          v
|   [ Context Assembly ]
|          |
|          v
|   [ LLM First Useful Tokens ]
|          |
|          v
|   [ Clause / Sentence Buffer ]
|          |
|          v
|   [ TTS First Audio Chunk ]
|          |
|          v
|   [ Downlink + Playback Buffer ]
|          |
|          v
|   [ User Hears Response ]
|
| Concurrent / Overlapped Work:
|
|   audio capture and uplink       overlap with streaming STT
|   partial transcripts            overlap with prefetch and route planning
|   LLM generation after first chunk overlaps with TTS synthesis
|   downstream playback            overlaps with continued generation
|
+--------------------------------------------------------------------------------
| Target:
|   Keep ordinary conversational end-of-speech -> first-audio latency near
|   the human turn-taking band where possible.
|   Treat >1000 ms as a hard warning zone for interactive voice UX.
+--------------------------------------------------------------------------------
```

To maintain this budget under real-world network conditions, latency margins must be allocated across system components:

| Processing Component | Technical Processing Step | Latency Margin (P50) | Latency Margin (P95) | Architectural Optimization Strategy |
| :---- | :---- | :---- | :---- | :---- |
| **Media Capture** | Microphone hardware capture, client buffering. | 10 ms | 20 ms | Bind audio capture to native client thread; restrict frame size to small buffers. |
| **Acoustic Preprocessing** | Echo cancellation, noise suppression, automatic gain control. | 15 ms | 30 ms | Run local AEC/noise suppression close to the microphone. |
| **Uplink Transport** | Opus encoding, DTLS/SRTP transit. | 40 ms | 110 ms | Route media to geographically close relay or edge ingress. |
| **VAD & Endpointing** | Acoustic VAD plus semantic/prosodic turn decision. | 35 ms | 75 ms | Keep endpointing lightweight and profile-aware. |
| **Streaming STT** | Acoustic-to-token decoding. | 150 ms | 320 ms | Use persistent streaming sessions; avoid batch transcription APIs. |
| **Dialogue State Update** | Context assembly and prompt/event formatting. | 5 ms | 12 ms | Keep active dialogue state in memory. |
| **Model Reasoning** | First useful token or first sentence plan. | 120 ms | 250 ms | Use low-latency model route for voice turns; prewarm where safe. |
| **Read-Only Tool Lookup** | Optional external lookup. | 180 ms | 380 ms | Prefetch only when safe; do not block filler/status speech unless required. |
| **Streaming TTS** | First audio chunk generation. | 90 ms | 180 ms | Stream audio chunks; avoid waiting for full response text. |
| **Downlink Transport** | Audio frame return to client. | 35 ms | 95 ms | Match packetization to client playback buffer. |
| **Playback Buffer** | Client jitter buffer and output. | 30 ms | 70 ms | Keep adaptive buffer small while avoiding underruns. |
| **Interruption Stop** | User barge-in to local playback mute. | 12 ms | 25 ms | Detect locally and mute before server round-trip. |
| **Recovery Latency** | Repair state setup after interruption. | 65 ms | 140 ms | Preserve playhead and partial-response markers. |

### **Pipeline Concurrency and Stream-Pipelining**

The critical path of a voice agent is optimized through stream-pipelining. In a sequential pipeline, the user waits for each stage to complete:

```text
T_sequential =
  T_STT_final
+ T_LLM_full_response
+ T_TTS_full_response
```

This can easily exceed natural turn-taking rhythm.

In a streaming pipeline, stages overlap:

```text
audio frames -> streaming STT -> partial/final turns
finalized turn -> LLM first tokens -> clause buffer
clause buffer -> TTS first chunk -> playback
LLM continues -> TTS continues -> playback continues
```

The perceived latency is closer to:

```text
T_first_audio =
  T_endpoint
+ T_context_assembly
+ T_LLM_first_useful_tokens
+ T_clause_buffer
+ T_TTS_first_audio
+ T_downlink_playback
```

Speculative work may begin earlier from stable partials, but high-risk actions must wait for final transcript, confirmation, and tool verification.

The practical doctrine:

```text
Pipeline aggressively for perception.
Gate conservatively for action.
Never let low latency become fast wrongness with a microphone.
```

## **Streaming Generation and Speech Output Policy**

When an agent begins playing back audio, it commits to a public trajectory. If the language model subsequently changes its reasoning or encounters a tool error, the agent cannot retroactively edit its spoken output without a jarring verbal correction. Systems require a strict **Streaming Response Policy** to balance generation speed with truthfulness and action-safety boundaries.

### **Streaming Response Policy**

When an agent begins playing audio, it commits to a public trajectory. Spoken output is harder to revise than text: the user hears it in time, reacts in time, and may act on it before the system can correct itself. Real-time voice systems therefore need a response policy that balances speed, truthfulness, interruption safety, and action verification.

```text
+--------------------------------------------------------------------------------
| STREAMING RESPONSE POLICY
+--------------------------------------------------------------------------------
|
| [ User Turn Finalized ]
|          |
|          v
| [ Determine Response Dependency ]
|          |
|          +--> no external dependency
|          |       stream answer immediately
|          |
|          +--> read-only lookup
|          |       speak brief status / filler if useful
|          |       execute lookup
|          |       speak result after observation is received
|          |
|          +--> low-risk reversible mutation
|          |       validate payload
|          |       optionally confirm if ambiguity exists
|          |       execute
|          |       speak completion only after verified success
|          |
|          +--> high-risk or irreversible mutation
|                  read back payload
|                  require explicit confirmation
|                  execute through tool contract
|                  verify resulting state
|                  speak completion only after verification
|
+--------------------------------------------------------------------------------
| Rule:
|   The agent may speak status before verification.
|   The agent may not speak completion before verification.
+--------------------------------------------------------------------------------
```

The dialogue engine should choose among four response paths:

| Response Path | When to Use | Allowed Speech Before Tool Result | Completion Claim Rule |
| :--- | :--- | :--- | :--- |
| **Immediate Conversational Streaming** | Low-risk conversation, explanation, brainstorming, or summarization from available context. | Full streaming response. | No external completion claim involved. |
| **Read-Only Lookup Path** | Order status, weather, schedule, inventory, account balance, or public/current lookup. | Filler/status such as “Let me check that.” | Speak result only after lookup observation is received and parsed. |
| **Low-Risk Mutation Path** | Reversible or local actions such as adding a note, draft, reminder, or checklist item. | Confirmation/status if needed. | Speak “done” only after execution result is verified or at least confirmed by authoritative tool result. |
| **High-Risk Mutation Path** | Money movement, deletion, external message send, access change, deployment, account mutation. | Clarification, readback, confirmation, and status only. | Speak completion only after action verification confirms the state change. |

Examples:

| Unsafe Spoken Output | Safer Spoken Output |
| :--- | :--- |
| “Your payment has been sent” before payment verification. | “I’m submitting the payment now. I’ll confirm once it’s verified.” |
| “I deleted the file” before filesystem readback. | “I’m processing the deletion. I’ll verify the file state before confirming.” |
| “Your appointment is booked” after only a pending calendar API response. | “The calendar request was accepted. I’m verifying the event before I call it booked.” |
| “I sent that email” before provider acknowledgment. | “I’m sending the email now. I’ll confirm once the mail service accepts it.” |

### **Speech Output UX Model**

Text formatted for visual layout is often unsuitable for speech. The speech generation engine should pass content through a **Speech Output Normalization Layer** before synthesis.

| Normalization Task | Purpose |
| :--- | :--- |
| **Numerical normalization** | Speak numbers according to context: amount, code, phone number, account number, date, or measurement. |
| **Sensitive data filtering** | Avoid reading full secrets, passwords, tokens, account numbers, or private details aloud in unsafe contexts. |
| **Structural cleanup** | Remove Markdown syntax, raw URLs, HTML, JSON noise, and terminal artifacts. |
| **Alternate channel routing** | Send dense tables, long lists, code blocks, and private values to screen/text instead of voice. |
| **Prosodic segmentation** | Break long answers into short spoken clauses with pause points and interruption boundaries. |
| **Barge-in safety** | Track which text has been spoken so interruption recovery can resume or repair accurately. |

The spoken channel should be concise, truthful, interruptible, and grounded in verified state. It should not perform a dramatic live reading of a JSON blob unless everyone involved has made very poor life choices.

### **Speech Output UX Model**

Text formatted for visual layout is often unsuited for speech. Reading long code blocks, nested tables, markdown links, or dense raw numbers aloud increases cognitive load and causes conversational abandonment.17  
The speech generation engine must pass raw text through a **Speech Output Normalization Layer** before synthesis:

* **Numerical Normalization**: Translating raw digits into spoken representations based on context.10 For example, "Your account balance is $1520.50" is normalized to "Your account balance is fifteen twenty dollars and fifty cents," whereas "Your account number is 1520" is normalized to "Your account number is one five two zero".10  
* **Content Filtering**: Suppressing structural metadata.30 Markdown markers, image URLs, raw HTML, and complex terminal outputs are filtered out.10  
* **Alternative Presentation**: If the system must present dense multi-column tables, long bulleted lists, or sensitive personal data (such as passwords or full credit card numbers), the agent must redirect the content to a visual display or private text channel instead of reading it aloud.46

## **Text-to-Speech, Prosody, and Voice Behavior**

Text-to-Speech (TTS) models determine the vocal personality, warmth, and authority of the voice agent. Prosody (the rhythm, melody, and intonation of speech) carries emotional and syntactic weight, shaping user trust and understanding.10

### **TTS and Prosody Control Model**

To control synthetic voice output, the synthesis engine structures its commands using specialized parameters 10:

| Parameter Target | Control Value / Range | Algorithmic Mechanism | Target Output Behavior |
| :---- | :---- | :---- | :---- |
| **Voice Selection** | Approved brand voice profiles 27 | Neural speaker embedding mapping | Anchor brand personality.27 |
| **Speaking Rate** | 130 - 180 words/min 17 | Linear time-scaling DSP filters | Slow for numbers, fast for summaries.46 |
| **Pitch Contour** | -12 to +12 semitones 36 | F0 frequency-domain pitch shifting | Dynamic intonation peaks. |
| **Intonation Style** | Rising / Falling contours | Prosodic pitch trajectory modulation | Signal question vs firm statement.44 |
| **Pause Insertion** | 100 ms - 1200 ms 36 | Frame zero-padding injection 36 | Mimic breathing, separate sentences.36 |
| **Emphasis Index** | 1.0 to 1.5 scale factor | Dynamic amplitude & pitch scaling | Highlight critical dates or terms.25 |
| **Pronunciation** | Lexicon/IPA mappings 11 | Grapheme-to-phoneme override dictionaries | Prevent misreading industry jargon.11 |
| **Spelling Mode** | Character-by-character spelling 6 | Interleaved spelling character strings | Speak codes/names letter-by-letter.6 |
| **Chunking Logic** | Sentence boundary locks | Sentence-buffer tokenizer filters | Interruption-safe streaming audio. |
| **Emotional Tone** | empathetic, serious, excited 10 | Multi-style style-space embeddings 10 | Match caller sentiment.10 |
| **Multilingual Voice** | Native language profiles 11 | Dynamic cross-lingual phoneme sharing | Code-switch without accent drift.11 |
| **Audio Quality** | 24 kHz PCM mono 33 | High-fidelity sample rate rendering 33 | Studio clarity over WebRTC.34 |
| **Fallback Voice** | On-device SFSpeech synthesizers 36 | Dynamic on-client system switch | Retain function under network loss.36 |

### **Identity Transparency and Behavioral Boundaries**

Human-like synthetic voices can lead to overtrust, where users assume an agent has cognitive, emotional, or moral capabilities it does not possess.48 To maintain safety, systems must enforce **Identity Transparency Boundaries**:

* **Explicit Disclosure**: The agent must identify itself as an artificial system at the beginning of the interaction or whenever asked.  
* **Consent and Cloned Voices**: Voice cloning is prohibited unless the target individual has provided explicit, cryptographically verifiable authorization.27  
* **Public Impersonation Bans**: The system must block the synthesis of celebrity, public official, or unauthorized third-party voices, enforcing safety filters at the model level to reject unauthorized voice models.27

## **Interruption, Barge-In, and Cancellation**

A natural voice system must allow the user to interrupt the agent at any point during playback.5 If the user has to wait for the agent to finish reading a long response before they can correct an error, the interface becomes unusable.5

### **Interruption and Barge-In Model**

Managing interruption is difficult because the system must capture the user’s speech while playing its own audio through the device speakers. This creates acoustic echo: the assistant’s output enters the microphone and can be misclassified as user speech.

A production voice system therefore needs a hybrid client-server interruption pipeline.

```text
+--------------------------------------------------------------------------------
| HYBRID INTERRUPTION COORDINATION
+--------------------------------------------------------------------------------
|
| Client Side
|
|   [ Assistant Audio Playback ]
|          |
|          +--> reference signal ------------------------------+
|                                                               |
|   [ Microphone Capture ]                                      |
|          |                                                    |
|          v                                                    |
|   [ WebRTC AEC / Echo Suppression ] <-------------------------+
|          |
|          v
|   [ Client VAD ]
|          |
|          +--> sustained user speech detected
|                  |
|                  v
|          [ Immediate Local Playback Mute ]
|                  |
|                  v
|          [ High-Priority Barge-In / Truncate Signal ]
|                  |
|                  v
|
| Server Side
|
|   [ Dialogue Coordinator Receives Truncate Signal ]
|          |
|          +--> halt streaming TTS
|          +--> cancel or pause LLM generation
|          +--> preserve spoken playhead timestamp
|          +--> classify interruption
|          +--> route to repair, stop, new task, or resume
|
+--------------------------------------------------------------------------------
| UX rule:
|   Local playback should stop before the server round-trip completes.
|   Server state should then catch up and reconcile the interrupted turn.
+--------------------------------------------------------------------------------
```

1. **Acoustic Echo Cancellation:** The client or server uses the playback reference signal to suppress the assistant’s own voice from microphone input.
2. **Client-Side Edge VAD:** Lightweight local VAD detects likely human speech without waiting for network round-trip.
3. **Duration Guard:** The system requires sustained speech rather than a click, cough, breath, or room impulse.
4. **Immediate Local Mute:** Playback stops locally so the user experiences immediate interruption.
5. **Upstream Signaling:** A high-priority barge-in/truncate message is sent to the backend.
6. **Server Cancellation:** The dialogue coordinator halts streaming TTS and cancels or pauses LLM generation.
7. **Playhead Alignment:** The system maps interruption time to the text/audio segment already spoken.
8. **Recovery Classification:** The interruption is classified as correction, stop, new task, clarification, backchannel, ambient noise, assistant echo, or bystander speech.

### **Interruption Classification and Dialogue Recovery**

| Interruption Category | Audio/Text Pattern | Classification Marker | State Machine Action | Playback Recovery Action |
| :---- | :---- | :---- | :---- | :---- |
| **Correction** | “No, Tuesday.” | High-confidence correction of previous content. | Enter repair state and update target parameter. | Purge remaining audio and synthesize corrected path. |
| **Stop Request** | “Stop.” “Hold on.” | Critical intent classification. | Transition to paused state. | Stop audio and wait for next user instruction. |
| **New Task** | “Actually, book Wednesday.” | Intent shift from current response. | Cancel current response path and restart planning. | Drop remaining playback. |
| **Clarification** | “Wait, what?” | Question or confusion marker. | Enter repair/explanation state. | Stop or soften playback; provide shorter clarification. |
| **Backchannel** | “mm-hmm,” “okay,” brief acknowledgment. | Short duration, low semantic takeover intent. | Preserve assistant floor. | Continue playback. |
| **Ambient Noise** | Cough, click, door slam. | Low speech probability or no linguistic content. | Reject interruption. | Continue playback. |
| **Assistant Echo** | Assistant’s own audio leaks into microphone. | High correlation with playback reference. | Reject interruption. | Continue or unmute if muted speculatively. |
| **Bystander Speech** | Background voice not matching active speaker. | Diarization or speaker mismatch. | Ignore or suppress segment. | Continue playback unless safety policy requires pause. |

### **Interruption Classification and Dialogue Recovery**

Not all incoming speech during agent playback represents an intent to seize the conversational floor. The system must classify the interruption to determine the appropriate recovery state.

| Interruption Category | Audio/Text Pattern | Classification Marker | State Machine Action | Playback Recovery Action |
| :---- | :---- | :---- | :---- | :---- |
| **Correction** | "No, Tuesday." 1 | High confidence word mismatch | Rollback, load repair prompt | Purge audio buffer; synthesize alternate path.5 |
| **Stop Request** | "Stop.", "Hold on." 1 | Critical intent classification | Transition to **PAUSED** state | Cessation of all audio output; wait for new trigger.5 |
| **New Task** | "Actually, book Wednesday." | Semantic intent shift 1 | Purge current session cache | Terminate running model threads, start new execution.5 |
| **Clarification** | "Wait, what?" 1 | Question pattern detection | Transition to **REPAIR** state | Generate explanatory sidebar text immediately.25 |
| **Backchannel** | "mm-hmm", "okay" 1 | Short acoustic/duration signature | Ignore interruption; keep floor | Maintain current playback; do not halt audio buffer.1 |
| **Ambient Noise** | Cough, click, door slam, | Low speech probability from VAD | Reject classification 1 | Maintain current playback.1 |
| **Assistant Echo** | Agent's own words leaking | AEC linear filter correlation | Reject classification 35 | Unmute client playback if muted speculatively.34 |
| **Bystander Speech** | Multi-speaker chatter 3 | Diarization ID mismatch 3 | Ignore segment 3 | Maintain current playback.3 |

### **Cancellation Semantics**

When an interruption occurs, the dialogue coordinator must evaluate the state of pending tool calls:

* If the agent was interrupted while simply speaking, the system discards the remainder of the generated text and updates its context to reflect the user's interruption.5  
* If a tool call has been dispatched to the execution environment, the system must consult the tool contract.4 If the tool is **Idempotent / Read-Only** (e.g., a search query), the coordinator cancels the execution thread immediately.42 If the tool is **Non-Idempotent / Side-Effecting** (e.g., a financial ledger write or database insertion) and execution has already started, **the operation cannot be cancelled**.9 The system must allow the database transaction to resolve, capture the resulting state, and present a verbal status update to the user, ensuring the agent's spoken statements align with the database state.9

## **Conversational Repair and Misunderstanding Recovery**

Because acoustic environments are imperfect and spoken language is often ambiguous, speech systems will frequently mishear entities, names, numbers, or intents.26 A production-ready voice agent must be capable of identifying communication breakdowns and resolving them through conversational repair without forcing the user to repeat the entire interaction.7

### **Conversational Repair Framework**

Conversational analysis classifies repairs into two categories based on who initiates the process and who resolves the issue 8:

* **Self-Initiated Repair**: The speaker notices their own error and corrects it mid-sentence (e.g., "Please book the flight to London... sorry, I meant Paris").8  
* **Other-Initiated Repair**: The listener flags a comprehension breakdown and requests clarification.8

In human dialogue, these are managed using specific, structured patterns 25:

```
                  +-----------------------------------------+  
                  |      Comprehension / Confidence Gate    |  
                  +-----------------------------------------+  
                                       |  
                   ┌───────────────────┼───────────────────┐  
                   | (High Confidence) | (Low Confidence/  | (Ambiguous  
                   |                   |  Entity Error)    |  Resolution)  
                   v                   v                   v  
          +------------------+ +------------------+ +------------------+  
          | Proceed to Turn  | | Explicit Request | | Restricted Offer |  
          | Completion       | | "Can you spell?" | | "Do you mean A?"   
          +------------------+ +------------------+ +------------------+  
                                       |                   |  
                                       v                   v  
                               +---------------------------------------+  
                               |     Process User Correction / Response|  
                               +---------------------------------------+  
                                                   |  
                                                   v  
                               +---------------------------------------+  
                               |      Update Active Dialogue State     |  
                               +---------------------------------------+
```

To implement these repair dynamics, production architectures run three primary repair patterns based on confidence scoring and entity classification 25:

| Repair Modality | Triggering Condition | Algorithmic Mechanism | Verbal Prompts / Patterns | Escalation Threshold |
| :---- | :---- | :---- | :---- | :---- |
| **Confidence Clarification** | Overall STT confidence drops below threshold (0.50 - 0.70) 41 | Open request prompting for repetition | "I'm sorry, I didn't quite catch that. Could you please repeat what you said?" 25 | Strike 1: Prompt again. Strike 2: Slow down output tempo.46 |
| **Explicit Readback** | Critical alphanumeric capture completed 6 | Repeat exact parameter with confirm request | "I heard your account number is four five six seven. Is that correct?" 9 | Target parameter rejection resets input field buffer.9 |
| **Spelling Mode** | Phonetic spelling mismatch on proper nouns 8 | Character-by-character grapheme collector | "Could you please spell your last name letter-by-letter?" 8 | Trigger automatic lookup matching phonetic dictionaries.11 |
| **Digit-by-Digit Mode** | Alphanumeric string segmentation fail 10 | Disfluent digit capture filter | "Please repeat the routing code, saying each number slowly and separately." | Limit field capture size, fallback to DTMF/Keypad.46 |
| **Entity Disambiguation** | Multiple phonetic matches detected 25 | Restricted binary selection offering | "Did you mean June twelfth or July twelfth?" 25 | Strike 3: Divert to human specialist queue. |
| **Paraphrase Confirmation** | Multi-step logical intent inferred | Semantic summarization comparison | "Just to make sure we're on the same page, you want me to transfer five hundred dollars to John?" 9 | Rejection restarts planning loop; does not execute tool.9 |

## **Voice-to-Action Gating**

When voice interfaces connect to agentic tools, speech uncertainty becomes execution risk. An unstable transcript, misheard entity, spoofed speaker, or ambiguous intent must never trigger a permanent mutation.

Voice-to-action gating binds every tool invocation to transcript finality, confidence, speaker/session authorization, action risk class, confirmation requirements, and post-action verification.

### **Voice-to-Action Gating Model**

```text
+--------------------------------------------------------------------------------
| VOICE-TO-ACTION GATING MODEL
+--------------------------------------------------------------------------------
|
| [ Proposed Spoken Command ]
|          |
|          v
| [ Transcript State Check ]
|   partial | stable speculative | locked segment | utterance complete
|          |
|          v
| [ Intent and Entity Confidence Check ]
|   action | target | amount | recipient | date | account | object ID
|          |
|          v
| [ Speaker / Session Authorization Check ]
|   active user | diarization | liveness | MFA if required
|          |
|          v
| [ Tool Risk Classification ]
|   read-only | local mutation | external action | high-risk | critical
|          |
|          v
| [ Confirmation Policy ]
|   none | lightweight | explicit intent | strict readback | dual control
|          |
|          v
| [ Tool Contract Execution ]
|   schema | authorization | idempotency | side-effect class
|          |
|          v
| [ Action Verification ]
|   observation | authoritative state | completion claim
|
+--------------------------------------------------------------------------------
| Rule:
|   Voice can register intent quickly.
|   Voice should mutate state only after transcript, identity, policy,
|   confirmation, and verification requirements are satisfied.
+--------------------------------------------------------------------------------
```

### **Action Risk Classes**

| Action Risk Class | Target Operations | Transcript Requirement | Confidence / Identity Requirement | Spoken Confirmation Policy | Execution Rule |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Read-Only / Public Informational** | Weather, public facts, general Q&A, non-sensitive lookup. | Final utterance preferred; stable partial may prefetch. | Normal session identity. | None. | Execute or prefetch when safe; speak result after observation. |
| **Read-Only / Private Data** | Account balances, order status, medical/financial status, private documents. | Final utterance. | Active user/session authorization; speaker check where required. | May require lightweight confirmation in public/noisy contexts. | Do not read sensitive details aloud if environment/privacy risk is high. |
| **Low-Risk Local Mutation** | Local checklist item, draft note, non-shared reminder. | Final utterance. | Sufficient entity confidence; active session. | Usually none or lightweight clarification. | Execute if reversible and target is unambiguous. |
| **Low-Impact External Action** | Drafting/sending low-risk team messages, non-sensitive scheduling, simple CRM notes. | Final utterance. | Entity confidence high; recipient/target verified. | Explicit intent confirmation when external recipient or time matters. | Prefer draft/review mode when ambiguity exists. |
| **Medium-Risk Mutation** | Calendar events with invitees, customer-visible updates, ticket routing, account preference changes. | Final utterance plus complete entities. | Speaker/session authorization and target validation. | Explicit readback of target and consequence. | Execute through tool contract; verify result before saying complete. |
| **High-Risk / Irreversible** | Payments, file deletion, legal commitments, production deploys, credential changes. | Final utterance plus explicit confirmation turn. | Strong identity, liveness/MFA where applicable. | Strict payload readback. | Execute only after confirmation; verify state before completion claim. |
| **Critical Mutation** | Large transfers/refunds, security access changes, destructive data operations. | Voice may register intent only. | Strong identity plus secondary authorization. | Dual control or out-of-band approval. | Voice alone cannot execute; route to approved workflow. |

### **Confirmation Policy Ladder**

| Confirmation Level | Example Prompt | Use Case |
| :--- | :--- | :--- |
| **None** | No confirmation. | Low-risk public read-only response. |
| **Lightweight Clarification** | “Do you mean the Chicago office?” | Ambiguous but low-risk target. |
| **Explicit Intent Confirmation** | “Do you want me to send that message?” | External but low-impact actions. |
| **Strict Payload Readback** | “Confirm: transfer five hundred dollars to John Smith ending in 4421?” | High-risk mutation. |
| **Dual Control / Out-of-Band Approval** | “I’ve prepared the request. It requires administrator approval before execution.” | Critical mutation. |

### **Transcript and Entity Stability Requirements**

| Speech Artifact | Safe Uses |
| :--- | :--- |
| **Volatile partial transcript** | UI captions, speculation, prewarming. |
| **Stable speculative transcript** | Read-only prefetch only; no user-visible claim. |
| **Locked segment** | Reasoning and low-risk planning. |
| **Utterance complete** | Read-only response and low-risk tool execution. |
| **Confirmed utterance** | Mutating or high-risk tool execution. |
| **Readback-approved payload** | High-risk action submission. |

### **Structured Planning and Control Policy**

To avoid executing tools on conversational filler, production voice agents should use a hidden structured planning/control layer before speech or action. This layer does not need to expose chain-of-thought. It emits compact control state that the orchestrator can validate.

Instead of treating the model’s hidden reasoning as an artifact, the system should require a structured action policy object:

```json
{
  "turn_id": "turn_1042",
  "detected_intent": "transfer_funds",
  "risk_class": "HIGH_RISK_IRREVERSIBLE",
  "transcript_state": "utterance_complete",
  "required_entities": {
    "amount": {
      "value": "500.00",
      "confidence": 0.99
    },
    "recipient": {
      "value": "John Smith ending 4421",
      "confidence": 0.97
    }
  },
  "required_confirmation": "strict_payload_readback",
  "allowed_next_action": "request_confirmation",
  "tool_execution_allowed": false
}
```

The control layer may classify the turn, detect missing entities, request confirmation, choose a safe response path, or route to repair. It should not be described as a production dependency on visible chain-of-thought tokens.

### **Voice Action Invariant**

```text
Partial speech may prepare.
Final speech may plan.
Confirmed speech may authorize.
Verified system state may complete.
```

## **Voice Identity, Consent, and Security**

Voice interaction surfaces are vulnerable to identity spoofing, biometric replication, replay attacks, bystander command injection, and unauthorized synthetic voice use. A robust real-time voice system must distinguish between:

* **speech recognition:** what was said
* **speaker recognition:** who said it
* **liveness detection:** whether the audio likely came from a live human source
* **authorization:** whether that speaker/session may perform the requested action
* **voice synthesis governance:** which voices may be generated and under what consent

Voice biometrics should be treated as one signal in a defense-in-depth system, not as a magical identity wand with a waveform moustache.

### **Biometric Security and Cloning Prevention**

As voice synthesis improves, recorded or generated speech can challenge legacy voice-biometric systems. Security architectures should combine several controls.

#### **1. Real-Time Liveness and Anti-Spoofing**

The system analyzes incoming audio for signs of replay, synthetic generation, codec artifacts, spectral inconsistencies, phase artifacts, and unnatural temporal dynamics. A suspicious result should not automatically prove fraud, but it should downgrade trust and require additional authentication.

Typical response:

```text
anti_spoofing_confidence low
  -> block high-risk voice execution
  -> require MFA or registered-device confirmation
  -> preserve trace for security review
```

#### **2. Synthetic Audio Watermark Detection**

Watermark detection can identify synthetic audio from cooperating generation systems that embed durable marks. It should be used when available, but it must not be treated as universal proof. Many synthetic or replayed attacks will not contain a recognizable watermark.

Watermark policy:

| Detection Result | System Interpretation |
| :--- | :--- |
| **Known watermark detected** | Treat as synthetic or system-generated; block biometric authentication. |
| **No watermark detected** | Does not prove human origin. Continue with liveness, speaker verification, and MFA policy. |
| **Unknown watermark / uncertain signal** | Downgrade trust and route high-risk actions to stronger authentication. |

#### **3. Cryptographic Provenance for Generated Audio**

For generated outbound audio, systems can attach signed provenance metadata to session recordings, audio chunks, or completed media artifacts. In real-time streams, provenance is usually represented through signed session manifests, chunk hashes, or post-call artifacts rather than a single static file manifest.

```text
+--------------------------------------------------------------------------------
| STREAMING AUDIO PROVENANCE MODEL
+--------------------------------------------------------------------------------
|
| [ Generated Audio Chunks ]
|          |
|          v
| [ Chunk Hashes ]
|          |
|          v
| [ Session Manifest ]
|   model ID | voice profile ID | timestamp range | chunk hash chain
|          |
|          v
| [ Signature ]
|   KMS/HSM signing key | organization identity | policy assertions
|          |
|          v
| [ Replay / Audit Verification ]
|
+--------------------------------------------------------------------------------
```

Provenance metadata can support audit, disclosure, and origin verification. It cannot prevent every downstream recording, remix, compression artifact, or unauthorized copy. Useful? Yes. Omnipotent? Alas, no.

### **Voice Identity and Consent Model**

| Governance Parameter | Operational Mechanism | Security Control | Target Boundary |
| :---- | :---- | :---- | :---- |
| **Speech vs Speaker Recognition** | Keep STT output separate from speaker-biometric verification. | Isolated pipelines and least-privilege feature storage. | Prevent transcript text from becoming identity proof. |
| **Synthetic Voice Disclosure** | Verbal disclosure, UI disclosure, metadata assertion, or policy-controlled session banner. | Signed session or media provenance where available. | Prevent user deception and overtrust. |
| **Consent and Licensing** | Store explicit authorization for cloned or licensed voices. | Consent records, access controls, revocation path, audit trail. | Prevent unauthorized voice replica creation. |
| **Anti-Spoofing Filters** | Liveness, replay detection, synthetic audio detection, and anomaly scoring. | MFA fallback and high-risk action blocking. | Prevent voice spoofing from triggering access. |
| **Watermark Handling** | Detect known synthetic watermarks in inbound audio; watermark outbound generated audio when feasible. | Detection logs and policy gates. | Identify cooperating synthetic sources and generated outputs. |
| **Biometric Privacy** | Avoid retaining raw audio unless required; store protected embeddings with retention limits. | Encryption, deletion policy, regional compliance controls. | Reduce biometric privacy exposure. |
| **Corporate Voice Governance** | Restrict brand voices to approved profiles and use cases. | RBAC, approval workflows, generation logs. | Prevent brand voice hijack or misuse. |
| **High-Risk Authorization** | Require more than voice for critical operations. | MFA, registered-device confirmation, human approval, dual control. | Prevent irreversible actions from relying on voice alone. |

### **Consent Rules for Synthetic Voice**

A production system should enforce:

* no unauthorized cloning of private individuals
* no impersonation of public figures, officials, employees, or customers without authorization
* revocable consent records for licensed voices
* visible or audible disclosure when appropriate
* audit logs for voice profile usage
* policy review for emotionally manipulative or deceptive voice behavior
* safety gates for voice profiles used in regulated or high-trust contexts

## **Accessibility and Inclusive Voice Interaction**

Voice-first design must never mean voice-only.46 While real-time speech interfaces provide unprecedented accessibility for individuals with visual or motor impairments, they introduce barriers for individuals with stutters, aphasia, dysarthria, or hearing impairments.46

### **Inclusive Design Adaptations**

To ensure universal accessibility, real-time voice architectures must integrate inclusive-speech layers:

```
                  +-----------------------------------------+  
                  |         Incoming User Audio             |  
                  +-----------------------------------------+  
                                       |  
                                       v  
                  +-----------------------------------------+  
                  |        Speech Classifier Mode           |  
                  +-----------------------------------------+  
                                       |  
                     ├─────────────────┼─────────────────┐  
                     | (Standard       | (Stutter /      | (Non-Verbal /  
                     |  Speech)        |  Dysarthria)    |  AAC Input)  
                     v                 v                 v  
          +------------------+ +------------------+ +------------------+  
          | Standard VAD /   | | Extend Silence   | | Redirect to      |  
          | Fast Turn-Taking | | Hangover (1500ms)| | Text Chat /      |  
          | (400ms Silence)  | | Bypass Repeats   | | Visual UI Form   |   
          +------------------+ +------------------+ +------------------+
```

* **Stutter-Aware Endpointing**: Users who stutter experience involuntary sound repetitions, prolongations, or silent blocks. Standard endpointing cutoffs will segment their sentences prematurely. The dialogue coordinator must detect repetitive syllable patterns or acoustic blocks and automatically extend the max silence threshold to 2500 ms - 4000 ms, preventing premature interruption and allowing the speaker to finish their thought at their own pace.  
* **Atypical Speech Models**: Classic STT decoders trained on standard pronunciation frequently misidentify slurred, strained, or atypically paced speech caused by dysarthria, cerebral palsy, or Parkinson's disease. Production pipelines route atypical audio through specialized speech recognition models (e.g., Voiceitt's atypical speech API) that adapt dynamically to individual vocal patterns, converting disjointed acoustics into clear, reconstructed text.  
* **Multimodal Fallbacks**: For hearing-impaired users or non-verbal individuals, the interface must present synchronized visual captions, interactive text-entry alternatives, and support for Augmentative and Alternative Communication (AAC) device speech output. The system must allow users to transition between voice, touch, and text typing at any point during the session without losing conversation state or history.

### **Accessibility and Inclusive Voice Interaction Model**

Implementing accessible interfaces requires programmatic constraints to support diverse speech and language capabilities.46

| Target Disability Category | Specific Acoustic Challenge | Technical Mitigation Strategy | System Fallback Mechanism |
| :---- | :---- | :---- | :---- |
| **Motor & Physical** | Inability to press physical keys or hold devices. | Native voice wake-word + local VAD processing.39 | Dynamic push-to-talk toggling.54 |
| **Low Vision / Blind** | Inability to inspect screens or visual cards.46 | Strict verbal descriptive summaries of visual state.46 | Screen-reader compliant LaTeX / HTML output. |
| **Dyslexia / Cognitive** | Working memory load limits on long spoken sentences.46 | Clamp speaking rate below 140 words/min.46 | Push persistent summary transcripts to the client visual display.46 |
| **Hearing Impairments** | Inability to hear synthesized audio output.46 | Stream real-time visual closed-caption arrays.46 | Full download of session text transcript.46 |
| **Stutter / Stammer** | Involuntary blocks, prolongations, repetitions.51 | Extended semantic VAD thresholds, ignore syllable loops. | Toggle conversational pace setting to "Slow".39 |
| **Dysarthria / Slurred** | Muscular weakness altering consonant articulation.46 | Specialized acoustic training adapters (Voiceitt databases).55 | Refine input via local LLM grammar reconstruction.47 |
| **Aphasia** | Long pauses, word-retrieval struggle, fragments.46 | Extend silence hangover to > 3.0 s dynamically.46 | Use predictive word bank suggestions on visual displays.46 |
| **Temporary / Environmental** | Noisy cafés, laryngitis, dental recovery.46 | Noise-aware filtering + WebRTC AGC adjustments.35 | Switch dynamically to non-verbal text-chat messaging.46 |

## **Voice Privacy and Environment Model**

Because speech is an active acoustic waveform projected into physical space, voice systems interact directly with the user’s social and environmental surroundings.28 The system must respect privacy boundaries and adapt to the context of the user’s immediate physical space.30

| Operational Environment | Privacy Hazard | Technical Defense Protocol | Fallback / Safe State |
| :---- | :---- | :---- | :---- |
| **Public Place / Transport** | Shoulder-surfing acoustics, bystander overhearing.19 | Private Content Filter: Restrict financial/personal parameter readbacks.46 | Dynamic redirect to private smartphone display cards.46 |
| **Noisy Home / Office** | Accidental capture of background conversations or television.2 | Neural speaker-separation + diarization classification.3 | Ignore audio frames mapping to non-primary diarization IDs.3 |
| **Shared Workplace** | Capturing private background speech from coworkers.36 | Local wake-word validation + near-field microphone limits.39 | Shut down media plane if signal-to-noise ratio drops below 10 dB. |
| **Static Secure Room** | Eavesdropping via always-listening cloud connections.36 | Hardware Push-to-Talk (PTT) / physical mute triggers.36 | Terminate media stream socket; run local-only VAD.34 |
| **Compliance Monitored** | Accidental capture of PCI/PHI metadata.36 | Real-time regex and entity redaction on STT pipeline. | Quarantine target transcript blocks; raise security flag. |

## **Realtime Voice Failure Mode Map**

Real-time voice systems are vulnerable to failure modes that do not exist in text-based environments.1 This failure mode map outlines the typical symptoms, underlying causes, and mitigation strategies for production-grade systems.4

| Failure Mode | User-Visible Symptom | Technical Root Cause | Detection Signal / Trigger Metric | Mitigation & Architectural Prevention |
| :---- | :---- | :---- | :---- | :---- |
| **False Endpoint** | Agent cuts off the user mid-sentence or mid-thought, speaking over them.1 | Silence thresholds are too short or VAD registers silence during thoughtful pauses.1 | Post-interruption user correction rate / Overlap duration.1 | Deploy Hybrid Semantic-Acoustic VAD; extend pause thresholds on trailing conjunctions.20 |
| **Late Endpoint** | The agent takes 1.5 s - 2 s to respond, creating dead air.1 | Silence thresholds are set too high or wait for forced timeout.38 | Turn Latency / User repeat prompt rate.4 | Use punctuation-based turn detection or eager semantic classification.39 |
| **Clipped Speech** | The STT engine misses the last word of the user's sentence.1 | VAD closes the capture buffer immediately upon silence, chopping trailing phonemes.1 | Under-transcribed word error rate / Speech-stopped timestamp overlap. | Apply a minimum trailing padding (150 ms - 200 ms) after VAD silence detection.5 |
| **Overtalk / Overlap** | Both user and agent speak simultaneously, creating chaotic crosstalk.22 | State machine fails to yield floor or slow network delay.2 | Double-talk duration > 300 ms.2 | Calibrate WebRTC AEC3; enforce strict floor yield rules.5 |
| **Dead Air** | Conversation halts, neither party speaks, session feels broken.22 | Silence timeout fails to trigger or LLM reasoning hangs.4 | Continuous silence duration > 3.5 s.4 | Play comforting background hum or dispatch verbal status updates. |
| **False Barge-In** | Agent stops speaking prematurely on brief background noises (cough, click, door slam).5 | High-sensitivity VAD threshold with no duration guardrails.5 | Interruption rate / False interruption count.5 | Implement a minimum sustained-speech duration guard (150 ms - 250 ms) before triggering truncation.5 |
| **Missed Barge-In** | User says "stop" or "no", but the agent continues speaking over them.5 | Poor acoustic echo cancellation or high local client jitter buffer.5 | High overlap duration / User speech amplitude spike during output. | Calibrate WebRTC AEC3; implement immediate local speaker muting on client-side VAD speech onset.34 |
| **Echo Interruption** | The agent interrupts itself as soon as it begins speaking. | The agent's output leaks into the microphone and triggers the VAD.5 | Autocorrelation match between output and input streams.35 | Route speaker reference audio back to WebRTC AEC3; calibrate delay estimation parameters.35 |
| **Partial Action Mutation** | An API database change is executed on a sentence before it is finished.6 | Dialogue coordinator acts on an unstable partial transcript.6 | Database rollback count / API execution timestamp preceding speech_final.6 | Restrict mutation tool access to finalized, stable transcripts with explicit confirm gates. |
| **Transcript Revision** | The system processes a parameter (e.g., "$15" vs "$50") but the STT later revises the text after execution. | Over-optimistic parsing of interim results.6 | Post-execution revision count / Transcript mismatch rate.6 | Buffer state execution until is_final: true is returned by the decoder.6 |
| **Entity Miscapture** | The agent mishears proper nouns, names, or addresses, causing task failure.1 | Out-of-vocabulary phoneme mapping on specialized terms.11 | Downstream validation failures / Specific Entity WER.1 | Initialize STT connections with keyterm prompts and domain-specific pronunciation dictionaries.11 |
| **Wrong Speaker** | Commands from a background bystander are processed as user intent.3 | Lack of speaker diarization or biometric verification.3 | Speaker embedding distance > threshold on active turn.3 | Bind the active session context strictly to the authenticated user's voice footprint.3 |
| **Speaker Spoofing** | Unauthorized user gains access using a recorded or synthesized voice.27 | Vulnerable biometric voice verification system lacking anti-spoofing controls.27 | Verification security alert rate.27 | Run active liveness detection; verify synthetic watermarks on inputs; enforce multi-factor authentication.27 |
| **Accent Failure** | Accuracy drops dramatically for non-standard English accents.53 | Training data under-represents regional pronunciations.53 | Segment-level WER > 15% on accented accounts.53 | Implement multi-scale acoustic feature recalibration networks.43 |
| **Stutter Cutoff** | User with a stutter is cut off after repeating initial syllables.47 | Rapid syllable breaks are registered by VAD as silent turn-yielding pauses. | High session abandonment rate for accessibility segments.47 | Dynamically switch to the Inclusive/Accessibility profile; extend max turn silence.38 |
| **Noisy Room Fail** | System loops constantly, unable to process clean speech.36 | Environmental noise overrides energy thresholds.38 | Frame classification entropy > 0.8.38 | Calibrate adaptive noise filters; switch client settings to WebRTC level 3.35 |
| **Latency Cliff** | Conversations drop below human conversational pace (> 1.2 s).4 | Lack of stream-pipelining; slow model inference; geographic server-client distance.4 | End-to-End Latency distribution (P95 > 1000 ms).4 | Enforce concurrent sentence-buffered TTS streaming; peer transceivers close to major network hubs.30 |
| **TTS delay** | Long gap between LLM first token and audio playback onset. | Sentence buffer is set too large or TTS synthesis is too slow.4 | TTS Synthesis time-to-first-audio (TTFA) > 300 ms.4 | Utilize low-latency synthetic voices; stream first chunk before completing sentence.4 |
| **Streaming Contradiction** | Agent begins speaking a claim, then halts and corrects itself mid-sentence. | The streaming LLM changed its reasoning direction after partial tokens were already sent to synthesis. | Self-correction count in active speaker streams.8 | Implement a minimum lookahead token buffer before dispatching to TTS; enforce semantic sentence buffering.11 |
| **Tool Result Outrun** | Agent says "Your money has been sent" but the payment API fails 2 s later. | Claim generated before tool execution state was verified. | Claim-to-Reality validation failures. | Restrict vocalized completions until the execution engine returns a verified success payload.9 |
| **Under-Repair** | System repeatedly misinterprets input without launching clarification loops.8 | Lacks confidence threshold filters.1 | Repetitive intent correction loops.8 | Execute Targeted Restricted Requests on any entity confidence drops.25 |
| **Over-Confirmation** | Agent asks "Is that correct?" for basic, low-risk statements, frustrating the user.9 | Confirm gates applied globally without risk classification.9 | Task Completion Time (TCT) rises unnecessarily.2 | Align confirm prompts to action risk profiles.9 |
| **Creepy Voice** | User experiences discomfort or anxiety regarding the agent's emotional tone.48 | Excessive, inappropriate synthetic emotional matching or fake human claims.48 | Perceived creepy score in post-call evaluations. | Mandate identity transparency disclosures; clamp emotional synthesis parameters to appropriate ranges.48 |
| **Voice Impersonation** | Brand voice replica is compiled and posted to public domains.27 | Lack of server-side watermarking or signing.19 | Unauthorized brand asset detection alerts.48 | Embed permanent, spectral PerTH watermarks and C2PA manifests into all generated audio.19 |
| **Privacy Leak** | Agent reads highly private data aloud in a public, crowded space.1 | Failure to evaluate physical or environmental context. | Post-call privacy incident logs.1 | Restrict sensitive readbacks; redirect private parameters to visual display cards.46 |
| **Inaccessible Fallback** | Non-verbal user is locked out because the system requires voice confirmation. | System has no multimodal interfaces.46 | Accessibility failure report count.46 | Support parallel text typing, DTMF input, and non-voice alternatives.46 |

## **Evaluation and Observability**

A real-time voice system cannot be evaluated using offline Word Error Rate (WER) alone.1 While transcription accuracy is a key metric, the conversational success of an agent is determined by turn transitions, interruption responsiveness, and safety boundaries.5

### **Voice Evaluation and Observability Guidance**

Establishing continuous observability over live voice sessions requires tracking metrics across both the perception plane and the temporal interaction plane.1

| Metric Category | Technical Metric Identifier | Diagnostic Target | Production Target Threshold |
| :---- | :---- | :---- | :---- |
| **Speech Accuracy** | Word Error Rate (WER) 1 | General phonetic transcription accuracy | < 5.0% under standard conditions.11 |
| **Speech Accuracy** | Entity-specific WER 1 | Alphanumeric and proper noun accuracy | < 1.5% on prioritized codes.10 |
| **Transcription** | Transcript Stability Score 6 | Partial token volatility index | Stability confidence > 0.85.6 |
| **Transcription** | Partial-to-Final Revision Rate | Frequency of finalized text corrections | < 4.0% post-emission revision rate.6 |
| **Endpointing** | Turn-End Detection Latency 4 | Duration from speech stop to committed turn | 250 ms - 450 ms average turn gap.1 |
| **Endpointing** | Cutoff Error Rate 1 | Percentage of turns prematurely truncated | < 1.5% on conversational sessions.1 |
| **Endpointing** | False Endpoint Rate 1 | VAD silence false-positive triggers | < 2.0 instances per hour.1 |
| **Endpointing** | VAD False-Negative Rate 1 | Missed user speech frames | < 1.0% under noisy cafes.1 |
| **Barge-In** | Barge-In Detection Latency 5 | Duration to register user interruption | < 120 ms post-speech onset.5 |
| **Barge-In** | False Barge-In Rate | Agent stops on noise/coughs/clicks | < 1.0% on environmental noise. |
| **Barge-In** | Missed Barge-In Rate 5 | Failure of agent to yield floor | < 1.5% on explicit interruptions.5 |
| **Barge-In** | Interruption Recovery Time | Time to synthesize new path post-stop | < 150 ms total recovery window.5 |
| **Model Serving** | Model First-Token Latency | LLM Time-to-First-Token (TTFT) | < 150 ms from committed turn. |
| **Voice Synthesis** | TTS First-Audio Latency 4 | Synthesis Time-to-First-Audio (TTFA) | < 120 ms from sentence buffer.4 |
| **Media Playback** | Playback Jitter Buffer Delay 5 | Downlink client buffering lag | Keep dynamic size < 60 ms.5 |
| **E2E Dynamic** | Turn Latency (T_streaming) 16 | End-of-speech to start-of-audio | < 700 ms average conversational.4 |
| **Action Gating** | Tool-Gating Precision | Safe blocking of unauthorized tools | 100% precision on irreversible API mutations.9 |
| **Dialogue Repair** | Confirmation Success Rate 9 | Explicit readback validation match | > 98% confirm loop success.9 |
| **Dialogue Repair** | Repair Success Rate 8 | Resolution of phonetic misunderstandings | > 92% of clarification prompts resolved.8 |
| **Dialogue Repair** | User Correction Rate 8 | Frequency of user repeating commands | < 5.0% of active conversational turns.8 |
| **Dialogue Repair** | Repeat Request Rate | User says "what did you say?" | < 2.0% of active sessions. |
| **Accessibility** | Accessibility Success Rate 46 | Task completion for impaired speech | > 90% completion across atypical profiles.47 |
| **System Security** | Privacy Incident Rate 1 | Public data leaks / bystander capture | 0 verified leaks under public context.1 |
| **Conversational** | Abandonment Rate 26 | Call drop during breakdown sequences | < 3.0% of active calling sessions.26 |
| **Conversational** | User Satisfaction (MOS) 44 | Perceived naturalness / helpfulness | Mean Opinion Score > 4.2 out of 5. |

## **Cross-Canon Handoff Map**

The systems architecture of AI-ENG-Q interfaces directly with upstream and downstream reports across the AI Systems Engineering Canon. Its durable handoff is temporal state preservation: audio frames, transcripts, speaker turns, interruptions, confirmations, tool gates, and spoken claims must remain traceable across the full interaction lifecycle.

| Target Report ID | Target Report Domain | Operational Handoff | Dependency / Engineering Integration Rule |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context Tenure & State Governance | Audio-frame retention, transcript tenure, speaker-state lifetime. | Apply memory-clearing and retention rules to raw audio, partial transcripts, finalized transcripts, and biometric features. |
| **AI-ENG-C** | Cost, Latency & Margin Management | Voice latency budgets, TTS/STT cost envelopes, streaming compute margins. | Treat realtime voice as a margin-sensitive pipeline with separate media, model, and tool budgets. |
| **AI-ENG-L** | Serving Architecture | Stateful realtime session routing, media backpressure, transceiver edge placement. | Coordinate WebRTC/session infrastructure with serving pools and low-latency model routes. |
| **AI-ENG-M** | Agentic Orchestration | Turn states, interruption events, stop commands, repair loops. | Agent loops must respond immediately to barge-in, cancellation, and user correction. |
| **AI-ENG-N** | Tool Contracts | Voice-derived tool payloads, entity confidence, transcript finality. | Tool execution requires schema validation against finalized, authorized, and risk-gated spoken intent. |
| **AI-ENG-O** | Action Verification | Spoken completion claims, verified action state, status turns. | The assistant may speak status before verification but must not speak completion before verified state. |
| **AI-ENG-P** | Multimodal Understanding | Audio evidence provenance, timestamped transcript spans, synchronized captions. | Ground spoken claims and tool actions back to audio frame ranges and word-level timings. |
| **AI-ENG-S** | Production Pathologies | False endpointing, missed barge-in, tool-result outrun, transcript revision failures. | Diagnose realtime voice-specific production failure modes. |
| **AI-ENG-T** | Prompt Injection & Trust Boundaries | Spoken override attempts, OCR-to-speech injection, bystander commands. | Sanitize transcripts and bind commands to active speaker/session authority. |
| **AI-ENG-U** | Dependency and Tool Risk | STT/TTS/VAD provider health, local fallback engines, parser/library sandboxing. | Isolate and monitor third-party audio pipelines and realtime speech dependencies. |
| **AI-ENG-V** | Resource Abuse & Fraud | Voice spam, costly streaming loops, spoofed commands, denial-of-wallet patterns. | Enforce session quotas, liveness gates, and tool execution risk controls. |
| **AI-ENG-W** | Fallback & Degraded Modes | STT outage, TTS outage, high latency, noisy environment, accessibility fallback. | Switch to text, DTMF, local captions, slower endpointing, or human handoff when voice quality degrades. |
| **AI-ENG-X** | User Trust and Control | Visible transcript, correction UI, confirmation cards, spoken-status transparency. | Let users inspect, correct, interrupt, and approve voice-derived actions. |
| **AI-ENG-Y** | Human Review | Failed repair loops, biometric uncertainty, high-risk command escalation. | Divert to human operators when automated voice comprehension or authorization is insufficient. |
| **AI-ENG-Z** | Telemetry and Metrics | Voice onset, endpoint, STT finality, barge-in, TTFA, turn latency, repair rate. | Emit timestamped OpenTelemetry events for the full audio-to-action loop. |
| **AI-ENG-AA** | Speech & Voice Evaluations | Voice-agent benchmark traces, endpointing tests, barge-in tests, spoken-action safety suites. | Evaluate STT accuracy, turn-taking, interruption, repair, and action-gating behavior. |
| **AI-ENG-AB** | Audit and Replay | Audio-frame references, transcript revisions, spoken confirmations, session manifests. | Reconstruct the voice interaction from audio evidence, transcripts, timestamps, and policy decisions. |
| **AI-ENG-AC** | Incident Response | Spoofing alerts, privacy leaks, failed barge-in, false action execution, realtime outage traces. | Quarantine sessions, revoke risky actions, and trigger incident playbooks for voice-specific failures. |
| **AI-ENG-AD** | Governance, Policy, Compliance & Accountability | Voice consent, biometric retention, disclosure policy, high-risk approval ownership. | Govern synthetic voice usage, biometric privacy, user consent, and accountability for spoken actions. |
| **AI-ENG-AJ** | Reference Architectures | WebRTC transceiver edge, streaming STT/TTS, sentence buffers, tool-gated dialogue loop. | Provide deployable blueprints for production realtime voice systems. |

The durable handoff is this:

```text
AI-ENG-Q exports temporal interaction state:
who spoke, what was heard, when it stabilized, how the floor changed,
what was confirmed, what action was gated, what was spoken,
and what evidence supports replay.
```

## **Durable Principles of Real-Time Interaction Systems**

### **1. Timing is Semantic Content**

In real-time voice, pauses, turn transitions, intonation, and response latency carry equal semantic weight to the textual tokens themselves.1 A real-time voice system must treat conversational pace as a first-class optimization metric.4

### **2. Stream-Pipelining is Non-Negotiable**

To operate within human turn-taking expectations (200 ms - 500 ms), a voice agent cannot run sequentially.22 Every stage of the pipeline—from client-side audio capture, streaming transcription, token generation, clause-boundary sentence buffering, to TTS synthesis—must execute concurrently and stream outputs continuously to the next stage.4

### **3. Silence is Not the Same as an Endpoint**

Energy-based silence detection fails in real-world environments.1 Highly accurate turn-taking requires a hybrid semantic-acoustic model that combines frame-level acoustic VAD with real-time evaluation of linguistic completeness, prosodic contours, and conversational filler tokens, ensuring users are never prematurely cut off mid-thought.1

### **4. Spoken Claims Must Never Outrun Verified Reality**

When a voice interface is connected to external systems, unconfirmed spoken statements can create false user expectations. All non-idempotent or high-impact actions must be validated, and their execution state verified, before the agent vocalizes completion, enforcing a strict confirm-gate on unstable transcripts.

### **5. Voice-First Must Never Mean Voice-Only**

Speech is biologically and socially variable.46 Real-time voice systems must incorporate inclusive-design adaptations to support speech differences (stutters, dysarthria, aphasia) and mandate alternative visual captions, manual touch fallbacks, and text alternative paths to ensure accessibility under all environmental and physical conditions.46

#### **Works cited**

1. Tackling Turn Detection in Voice AI: Overcoming Noise and Interruption Challenges - Notch, accessed June 10, 2026, [https://www.notch.cx/post/turn-detection-in-voice-ai](https://www.notch.cx/post/turn-detection-in-voice-ai)  
2. Human Latency Conversational Turns for Spoken Avatar Systems - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2404.16053v1](https://arxiv.org/html/2404.16053v1)  
3. Streaming Speaker Diarization in Real Time: Guide - AssemblyAI, accessed June 10, 2026, [https://www.assemblyai.com/blog/streaming-speaker-diarization](https://www.assemblyai.com/blog/streaming-speaker-diarization)  
4. Building Enterprise Realtime Voice Agents from Scratch: A Technical Tutorial - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2603.05413v1](https://arxiv.org/html/2603.05413v1)  
5. Voice AI Barge-In and Turn-Taking: A 2026 Implementation Guide - Future AGI, accessed June 10, 2026, [https://futureagi.com/blog/voice-ai-barge-in-turn-taking-2026/](https://futureagi.com/blog/voice-ai-barge-in-turn-taking-2026/)  
6. Configure Endpointing and Interim Results - Deepgram's Docs, accessed June 10, 2026, [https://developers.deepgram.com/docs/understand-endpointing-interim-results](https://developers.deepgram.com/docs/understand-endpointing-interim-results)  
7. Repair: The Interface Between Interaction and Cognition - PMC, accessed June 10, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC6849777/](https://pmc.ncbi.nlm.nih.gov/articles/PMC6849777/)  
8. System and User Strategies to Repair Conversational Breakdowns of Spoken Dialogue Systems: A Scoping Review | Request PDF - ResearchGate, accessed June 10, 2026, [https://www.researchgate.net/publication/382076125_System_and_User_Strategies_to_Repair_Conversational_Breakdowns_of_Spoken_Dialogue_Systems_A_Scoping_Review](https://www.researchgate.net/publication/382076125_System_and_User_Strategies_to_Repair_Conversational_Breakdowns_of_Spoken_Dialogue_Systems_A_Scoping_Review)  
9. A Self-Reflection Voice Agentic Approach to Speech Recognition and Audio Reasoning with Omni Perception - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2601.09413v2](https://arxiv.org/html/2601.09413v2)  
10. Edge-Optimized Speech Workflows: Combining Deepgram Nova-3 STT with Fish Speech V1.5 TTS - GetStream.io, accessed June 10, 2026, [https://getstream.io/blog/edge-speech-deepgram-fish/](https://getstream.io/blog/edge-speech-deepgram-fish/)  
11. Live Audio - Deepgram's Docs, accessed June 10, 2026, [https://developers.deepgram.com/reference/speech-to-text/listen-streaming](https://developers.deepgram.com/reference/speech-to-text/listen-streaming)  
12. Best Speech-to-Text APIs for Developers Building Real-Time Voice AI in 2026, accessed June 10, 2026, [https://inworld.ai/resources/best-speech-to-text-apis](https://inworld.ai/resources/best-speech-to-text-apis)  
13. 10 Best speech-to text api You Should Know - Vatis Tech, accessed June 10, 2026, [https://vatis.tech/blog/best-speech-to-text-api-a00ab](https://vatis.tech/blog/best-speech-to-text-api-a00ab)  
14. Voice Agent API - AssemblyAI, accessed June 10, 2026, [https://www.assemblyai.com/products/voice-agent-api](https://www.assemblyai.com/products/voice-agent-api)  
15. How to Build a Voice Agent with Real-Time Translation Using OpenAI GPT Realtime 2, accessed June 10, 2026, [https://www.mindstudio.ai/blog/build-voice-agent-real-time-translation-gpt-realtime-2](https://www.mindstudio.ai/blog/build-voice-agent-real-time-translation-gpt-realtime-2)  
16. Real-time transcription in Python with Universal-Streaming - AssemblyAI, accessed June 10, 2026, [https://www.assemblyai.com/blog/real-time-transcription-in-python](https://www.assemblyai.com/blog/real-time-transcription-in-python)  
17. Voice Activity Detection: How VAD Powers AI Agents in 2026 - Parloa, accessed June 10, 2026, [https://www.parloa.com/blog/voice-activity-detection-vad/](https://www.parloa.com/blog/voice-activity-detection-vad/)  
18. VAD voice activity detection for clearer agent calls - Teammates.ai, accessed June 10, 2026, [https://teammates.ai/blog/vad-voice-activity-detection-for-clearer-agent-calls](https://teammates.ai/blog/vad-voice-activity-detection-for-clearer-agent-calls)  
19. Why VAD End-of-Speech Detection Is the Hardest Problem in Production Voice Agents, accessed June 10, 2026, [https://altersquare.medium.com/why-vad-end-of-speech-detection-is-the-hardest-problem-in-production-voice-agents-fee308e38cfc](https://altersquare.medium.com/why-vad-end-of-speech-detection-is-the-hardest-problem-in-production-voice-agents-fee308e38cfc)  
20. What Is Semantic VAD? (And Why It Matters for Voice Agents) - Inworld AI, accessed June 10, 2026, [https://inworld.ai/resources/what-is-semantic-vad](https://inworld.ai/resources/what-is-semantic-vad)  
21. Universal-3 Pro Streaming | AssemblyAI | Documentation, accessed June 10, 2026, [https://www.assemblyai.com/docs/streaming/universal-3-pro](https://www.assemblyai.com/docs/streaming/universal-3-pro)  
22. Turn-Taking Modelling in Conversational Systems: A Review of Recent Advances - MDPI, accessed June 10, 2026, [https://www.mdpi.com/2227-7080/13/12/591](https://www.mdpi.com/2227-7080/13/12/591)  
23. Barge-In – End-to-End Interruption Metrics Across ASR & TTS - Cekura, accessed June 10, 2026, [https://www.cekura.ai/discover/voice-ai-barge-in-testing-asr-latency-tts-overrun](https://www.cekura.ai/discover/voice-ai-barge-in-testing-asr-latency-tts-overrun)  
24. Turn Detection for Voice Agents: VAD, Endpointing, and Model-Based Detection | LiveKit, accessed June 10, 2026, [https://livekit.com/blog/turn-detection-voice-agents-vad-endpointing-model-based-detection](https://livekit.com/blog/turn-detection-voice-agents-vad-endpointing-model-based-detection)  
25. Conversational Repair and the Acquisition of Language - Stanford University, accessed June 10, 2026, [https://web.stanford.edu/class/cs379c/class_messages_listing/curriculum/Annotated_Readings/ClarkDISCOURSE-PROCESSES-20_Unannotated.pdf](https://web.stanford.edu/class/cs379c/class_messages_listing/curriculum/Annotated_Readings/ClarkDISCOURSE-PROCESSES-20_Unannotated.pdf)  
26. Analyzing Patterns of Conversational Breakdown in Human-Chatbot Customer Service Conversations - UU Research Portal, accessed June 10, 2026, [https://research-portal.uu.nl/ws/files/263067946/978-3-031-88045-2_1.pdf](https://research-portal.uu.nl/ws/files/263067946/978-3-031-88045-2_1.pdf)  
27. Top 10 Deepfake Audio Detection Tools for 2025 | Resemble AI, accessed June 10, 2026, [https://www.resemble.ai/resources/audio-deepfake-detection-tools](https://www.resemble.ai/resources/audio-deepfake-detection-tools)  
28. How to Build a Production-Ready Voice Agent Architecture with WebRTC - freeCodeCamp, accessed June 10, 2026, [https://www.freecodecamp.org/news/how-to-build-production-ready-voice-agents/](https://www.freecodecamp.org/news/how-to-build-production-ready-voice-agents/)  
29. How Real-Time Voice AI Actually Works (STT → LLM → TTS, Explained) | Retell AI, accessed June 10, 2026, [https://www.retellai.com/blog/how-real-time-voice-ai-works-stt-llm-tts](https://www.retellai.com/blog/how-real-time-voice-ai-works-stt-llm-tts)  
30. How OpenAI delivers low-latency voice AI at scale | OpenAI, accessed June 10, 2026, [https://openai.com/index/delivering-low-latency-voice-ai-at-scale/](https://openai.com/index/delivering-low-latency-voice-ai-at-scale/)  
31. How Real-Time Voice Agents Work: Architecture and Latency, accessed June 10, 2026, [https://gokuljs.com/blogs/real-time-voice-agent-infrastructure](https://gokuljs.com/blogs/real-time-voice-agent-infrastructure)  
32. Getting Started - Deepgram's Docs, accessed June 10, 2026, [https://developers.deepgram.com/docs/live-streaming-audio](https://developers.deepgram.com/docs/live-streaming-audio)  
33. Gemini Live API Proactive, in Next.js and React Native Expo | by Felipe Lujan - Medium, accessed June 10, 2026, [https://felipelujan.medium.com/gemini-live-api-proactive-in-next-js-and-react-native-expo-26d070dafff9?postPublishedType=initial](https://felipelujan.medium.com/gemini-live-api-proactive-in-next-js-and-react-native-expo-26d070dafff9?postPublishedType=initial)  
34. The Art of Interruption: VAD Strategies for Fluid AI Conversations - DEV Community, accessed June 10, 2026, [https://dev.to/deepak_mishra_35863517037/the-art-of-interruption-vad-strategies-for-fluid-ai-conversations-15bh](https://dev.to/deepak_mishra_35863517037/the-art-of-interruption-vad-strategies-for-fluid-ai-conversations-15bh)  
35. Acoustic Echo Cancellation: How WebRTC AEC3 Works | Switchboard Audio SDK, accessed June 10, 2026, [https://switchboard.audio/hub/how-webrtc-aec3-works/](https://switchboard.audio/hub/how-webrtc-aec3-works/)  
36. How to Implement Silence Trimming in Your iOS App (Swift + AVAudioEngine, 2026 Guide), accessed June 10, 2026, [https://www.forasoft.com/blog/article/how-to-implement-silence-trimming-feature-to-your-ios-app-1720](https://www.forasoft.com/blog/article/how-to-implement-silence-trimming-feature-to-your-ios-app-1720)  
37. Real-Time Voicemail Detection in Telephony Audio Using Temporal Speech Activity Features - arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2604.09675](https://arxiv.org/pdf/2604.09675)  
38. VoxMind: An End-to-End Agentic Spoken Dialogue System - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.15710v1](https://arxiv.org/html/2604.15710v1)  
39. Voice activity detection (VAD) | OpenAI API, accessed June 10, 2026, [https://developers.openai.com/api/docs/guides/realtime-vad](https://developers.openai.com/api/docs/guides/realtime-vad)  
40. Streaming Speech Recognition API for Real-Time Transcription - Deepgram, accessed June 10, 2026, [https://deepgram.com/learn/streaming-speech-recognition-api](https://deepgram.com/learn/streaming-speech-recognition-api)  
41. Speech-to-Text WebSocket messages - Telnyx, accessed June 10, 2026, [https://developers.telnyx.com/docs/voice/stt/websocket-streaming/responses](https://developers.telnyx.com/docs/voice/stt/websocket-streaming/responses)  
42. Build a voice agent with function calling - AssemblyAI, accessed June 10, 2026, [https://www.assemblyai.com/blog/build-voice-agent-function-calling](https://www.assemblyai.com/blog/build-voice-agent-function-calling)  
43. Artificial Intelligence for Speech Classification and Enhancement of Speech and Language Disorders: Techniques, Applications, an - IEEE Xplore, accessed June 10, 2026, [https://ieeexplore.ieee.org/iel8/6287639/10820123/11199322.pdf](https://ieeexplore.ieee.org/iel8/6287639/10820123/11199322.pdf)  
44. VAD vs. Turn-Taking Endpoints in Conversational AI | Retell AI, accessed June 10, 2026, [https://www.retellai.com/blog/vad-vs-turn-taking-end-point-in-conversational-ai](https://www.retellai.com/blog/vad-vs-turn-taking-end-point-in-conversational-ai)  
45. Real-Time Voicemail Detection in Telephony Audio Using Temporal Speech Activity Features - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.09675v1](https://arxiv.org/html/2604.09675v1)  
46. Speech Disabilities & Digital Accessibility - ExceedAbility, accessed June 10, 2026, [https://exceedability.com/speech-disabilities.html](https://exceedability.com/speech-disabilities.html)  
47. SpeechAgent: An End-to-End Mobile Infrastructure for Speech Impairment Assistance - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2510.20113v1](https://arxiv.org/html/2510.20113v1)  
48. C2PA Implementation Guidance, accessed June 10, 2026, [https://spec.c2pa.org/specifications/specifications/1.0/guidance/Guidance.html](https://spec.c2pa.org/specifications/specifications/1.0/guidance/Guidance.html)  
49. An Analysis of Dialogue Repair in Voice Assistants - arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2311.03952](https://arxiv.org/pdf/2311.03952)  
50. VoiceAgentBench: Are Voice Assistants ready for agentic tasks? - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2510.07978v1](https://arxiv.org/html/2510.07978v1)  
51. How Speech and Language Disabilities Affect Online Experiences & Accessibility Best Practices - AudioEye, accessed June 10, 2026, [https://www.audioeye.com/post/speech-and-language-disabilities/](https://www.audioeye.com/post/speech-and-language-disabilities/)  
52. Listener Ratings of Stuttering: Evaluating Two Auditory–Perceptual Scales - PMC, accessed June 10, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12806039/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12806039/)  
53. Inclusive AI Starts with Ethical Voice Data for Speech-Impaired and Atypical Speakers, accessed June 10, 2026, [https://blog.andovar.com/inclusive-ai-starts-with-ethical-voice-data-for-speech-impaired-and-atypical-speakers](https://blog.andovar.com/inclusive-ai-starts-with-ethical-voice-data-for-speech-impaired-and-atypical-speakers)  
54. Voice capability (TTS / STT / real-time audio) · Issue #116 - GitHub, accessed June 10, 2026, [https://github.com/pydantic/pydantic-ai-harness/issues/116](https://github.com/pydantic/pydantic-ai-harness/issues/116)  
55. Voiceitt - Inclusive Voice AI, accessed June 10, 2026, [https://www.voiceitt.com/](https://www.voiceitt.com/)

---

# AI-ENG-R — UI Agents - Browser Control, Desktop Automation & Visual State

The structural integrity of interface-controlling artificial intelligence systems depends on a fundamental law of operating boundaries: user interfaces are partially observable, drift-prone state machines. Traditional automation systems treat the target interface as a static, deterministic layout with permanent locators and predictable transition latencies. This simplified assumptions layer collapses under the execution patterns of production-grade deployments, where client-side state changes, asynchronous networking, dynamic Document Object Model (DOM) mutations, and layout shifts introduce continuous operational divergence.1  
To achieve reliable execution margins, UI agents must be designed as state-verifying interface operators. This architectural model dictates that every interaction—whether page navigation, semantic field inputs, visual targeting, or bulk file transfers—must be verified against the resulting physical, accessibility, and structural state transitions before the control thread executes subsequent operations.4 This report establishes the structural state models, perception pipelines, control-channel architectures, and containment protocols required to deploy resilient and safe UI agents.

## **Doctrinal Foundations: The Interface as a Stateful Object**

A high-dimensional UI agent cannot be built merely by mapping a model's textual outputs directly to keyboard and mouse coordinates.1 When UI control is treated as ungrounded coordinate-emitting actions, systems fail via misplaced events, duplicate form submissions, security-boundary leaks, and infinite failure-recovery loops.1  
The central task of UI agents is to act as state-verifying interface operators. These systems combine DOM, accessibility, screenshot, visual, browser, desktop, and action-history evidence to plan bounded software-interface actions, execute them through sandboxed control channels, verify each resulting UI state transition, and recover safely from interface drift.1

### **The Core Doctrine of Interface Control**

UI agents must treat interfaces as partially observable, drift-prone state machines. Every click, type, navigation, drag, upload, download, and submit action must be grounded in inspected UI state, executed through a controlled channel, and verified against resulting visual, DOM, accessibility, or application state before the agent continues.4  
This principle organizes the inquiry away from simple execution metrics to a continuous verification loop. The useful question is not "Can the agent click the screen?" but "Does the agent know what interface state it is in, what target it intends to manipulate, why that target is correct, what action is permitted, what state should change afterward, whether that change occurred, and what it must do if the interface drifts?".1 Interface agents fail when they confuse dispatched input events with verified task progress.5 A UI action is not complete when the click fires; it is complete when the interface state proves the intended transition occurred.5

### **Taxonomy of Interface Controllers**

To avoid overlapping execution profiles, systems must separate programmatic scripting from model-driven interface agency.1 This comparison details the execution assumptions, failure tolerances, and target interfaces of these controllers:

| System Category | Executive Definition | Execution Assumptions | Failure Tolerances | Primary Interaction Targets |
| :---- | :---- | :---- | :---- | :---- |
| **Browser Automation Script** | A programmatic instruction sequence designed to run a predefined path over a known web page.12 | Assumes static elements, fixed timeouts, and unyielding DOM targets.10 | Zero-tolerance; fails immediately on layout shifts or selector changes.10 | Pre-defined CSS selectors and XPaths.14 |
| **Test Runner** | A scripted execution environment that runs test cases alongside repeatable fixtures and assertions.5 | Assumes isolated local servers, repeatable mock data, and test environments.5 | Fails loudly; prioritizes pinpointing discrepancies over recovery.5 | Explicit test IDs and DOM elements.14 |
| **RPA Workflow** | A robotic process automation macro that automates semi-structured business workflows across legacy interfaces.1 | Assumes stable application windows, consistent screen coordinates, and fixed layouts.1 | Brittle; requires manual re-engineering on minor application updates.1 | screen coordinates, Win32 controls, and legacy properties, Win32 controls, and legacy properties.1 |
| **Browser Agent** | A model-driven agent that uses dynamic planning to operate web interfaces under variable layouts and flows.4 | Operates under dynamic layouts, changing user states, and unannounced redirects.4 | High; handles dynamic layout changes and performs automated recovery.4 | DOM structures, accessibility trees, and visual viewports.4 |
| **Desktop Agent** | A model-driven controller that plans and executes tasks across operating system windows and applications.1 | Operates across local files, multiple native windows, and global system settings.1 | High; manages focus transitions and handles system alerts.1 | Native OS accessibility APIs, system cursors, and display graphics.1 |
| **Computer-Use Agent** | A platform-agnostic agent that interacts with host operating systems via visual screenshots and synthetic inputs.1 | Assumes no direct DOM access, relying on pixel coordinates and screenshots.1 | Variable; handles visual elements but is susceptible to coordinate offsets.1 | Visual viewports and global screen coordinates.1 |

## **The Unified UI State Model**

To operate reliably, the UI agent must maintain a comprehensive, multi-channel representation of the interface. A screenshot is not just an image; it is a rendered snapshot of application state.1 A DOM is not the complete UI; it is a structural representation that may not match rendered truth.10 An accessibility tree may contain semantic controls unavailable from pixels alone.1  
The state representation must capture fifteen distinct dimensions to provide a complete picture of the UI:

| State Dimension | Physical Data Type | Extraction Mechanism | Operational Purpose | Structural Verification Signal |
| :---- | :---- | :---- | :---- | :---- |
| **1. Navigation State** | String (URI / App ID) 18 | browsingContext.get via WebDriver BiDi.18 | Confirms target domain alignment, tracking redirects and browser origins.7 | Matches the active URL path against expected domain patterns.5 |
| **2. Structural Graph** | Hierarchical markup tree 17 | CDP DOM extraction / raw XML traversal.19 | Maps parent-child element nodes and extracts HTML attributes.17 | Confirms that the target element is attached to the active DOM.5 |
| **3. Semantic Substrate** | Accessible role & name graph 8 | Native OS APIs (UIA, AX, AT-SPI2).8 | Resolves element roles, names, states, and descriptions.8 | Validates that roles match active application properties.8 |
| **4. Visual Capture** | viewport screenshot or rendered frame buffer 17 | Page.captureScreenshot / OS screen capture.1 | Analyzes raw pixels to check for visual changes and layout overlays.1 | Confirms rendering contrast and verifies element layouts.17 |
| **5. Spatial Coordinates** | Normalized array: [x1, y1, x2, y2]17 | getBoundingClientRect / AX position query.11 | Coordinates element bounds for visual click fallbacks.1 | Bounding box has a non-zero area and maps inside the viewport.5 |
| **6. Focus Element** | Element reference ID 11 | activeElement traversal via CDP Runtime.13 | Pinpoints the active input cursor location, preventing blind typing.5 | Focus matches the target element's active DOM reference.5 |
| **7. Input Values Map** | Map: Selector -> String 5 | Traverses input element attributes in the DOM.5 | Tracks field text to confirm data entry and clear operations.5 | Input strings match the expected field values.5 |
| **8. Validation Alerts** | Map: Selector -> String 5 | Evaluates aria-invalid attributes and text.5 | Identifies inline input errors and block constraints.5 | Error containers are absent or hidden after correction.5 |
| **9. Modal & Overlay State** | Map: Selector -> BoundingBox | Checks z-index layers and visible overlays.5 | Detects blocking dialogs, consent pop-ups, and screens.5 | Ensures the target element is not obscured by other layers.5 |
| **10. Viewport Offset** | Tuple: (Scroll_x, Scroll_y) 11 | Queries window scroll coordinates.11 | Checks scroll position to confirm element visibility.5 | Target element is positioned within the active viewport bounds.5 |
| **11. Network Traffic** | Integer (active request count) | Monitors requests via WebDriver BiDi.18 | Identifies loading states and pending transactions.5 | active requests settle or an app-specific readiness signal is observed.9 |
| **12. Runtime Console Logs** | List: ConsoleMessage 22 | LogInspector / console listener connections.22 | Catches Javascript exceptions and resource failures.22 | Zero uncaught runtime errors during action execution.22 |
| **13. Session Profile** | Map (cookies and local storage) 18 | storage.getCookies via BiDi.18 | Tracks login states, sessions, and tenant scopes.18 | Active session cookies match target auth configurations.7 |
| **14. Action History** | List: ActionRecord | Internal tracing logs. | Saves the sequence of completed steps to prevent loops. | Verifies execution progress matches the planned path. |
| **15. Risk Profile** | Enum (Low, Medium, High, Critical) | Internal configuration map. | Maps action risks to enforce safety constraints.7 | High-risk actions are gated and require explicit approvals.7 |

## **Dual-Channel UI Perception Model and Fusion Rules**

A robust UI agent cannot rely on a single channel for perception. DOM-only agents fail when elements are visually occluded, offscreen, hidden behind overlays, rendered in canvas, or detached during client-side re-rendering. Screenshot-only agents fail when semantic labels are missing from pixels, small text is unreadable, or keyboard/accessibility relationships are invisible to the visual model.

The agent must combine semantic, structural, visual, spatial, and runtime signals into a unified state model before choosing targets.

```text
+--------------------------------------------------------------------------------
| DUAL-CHANNEL UI PERCEPTION MODEL
+--------------------------------------------------------------------------------
|
| [ Active Interface State ]
|          |
|          +-------------------------------+
|          |                               |
|          v                               v
| [ Semantic / Structural Channel ]   [ Visual / Spatial Channel ]
|   DOM tree                          screenshot / framebuffer
|   accessibility tree                OCR boxes
|   roles and names                   icon detection
|   labels and form relations         visual bounding boxes
|   focus and enabled state           occlusion and contrast
|   selectors and test IDs            viewport coordinates
|          |                               |
|          +---------------+---------------+
|                          |
|                          v
| [ Perception Fusion Engine ]
|   reconcile DOM, AX, screenshot, hit-test, focus, and runtime signals
|                          |
|          +---------------+---------------+
|          |                               |
|          v                               v
| [ Verified Actionable Target ]   [ Conflict / Drift / Ambiguity ]
|   unique target                    re-observe, dismiss overlay,
|   visible                          re-query, inspect visually,
|   enabled                          or escalate
|   stable
|   receives events
|
+--------------------------------------------------------------------------------
| Invariant:
|   A target is actionable only when semantic identity, visual presence,
|   spatial coordinates, and event-receiving state agree.
+--------------------------------------------------------------------------------
```

To resolve structural conflicts, the fusion engine enforces five rules during state construction:

| Rule Category | Conflict Scenario | Detection Diagnostic | Unified Resolving Algorithm | Expected Outcome |
| :---- | :---- | :---- | :---- | :---- |
| **1. Visibility Guard** | DOM element exists but is hidden from the viewport. | DOM visibility, computed style, zero size, offscreen position. | Exclude element from targeting; mark as non-actionable. | Prevents actions on hidden layout elements. |
| **2. Occlusion Verification** | Target coordinates are covered by another element. | Hit-test returns another node; overlay captures pointer events. | Trace z-index and overlay boundaries; route to modal or overlay playbook. | Prevents misclicks into blocking dialogs or banners. |
| **3. Status Cross-Check** | DOM shows enabled, but rendered UI appears disabled. | Pixel grayout, disabled styling, AX disabled state, aria-disabled. | Query DOM, AX, and visual status; require agreement before action. | Prevents clicks on disabled or inert controls. |
| **4. Canvas Resolution** | Control is rendered inside canvas or custom graphics. | DOM exposes canvas or opaque container with no child controls. | Run OCR/icon detection and spatial matching over rendered region. | Produces coordinate target with lower confidence and stricter verification. |
| **5. Accessibility Audit** | DOM element lacks usable labels or stable identifiers. | Missing ID, text, placeholder, aria-label, or test ID. | Query accessibility tree for role, name, description, and relations. | Resolves semantic targets that DOM serialization misses. |

## **Semantic Targeting and Accessibility Tree Models**

UI agents should prefer stable semantic targets over volatile visual coordinates. A button’s accessible role and name usually survive layout shifts better than a pixel coordinate. A form field’s associated label is safer than a nearby text guess. A test ID is safer than a generated CSS class. Visual coordinates remain useful, but they should usually serve as verification and fallback rather than the primary identity of the target.

Windows, macOS, Linux, and browsers expose interface semantics through different APIs. A production UI agent should normalize these channels into a common semantic target object.

```json
{
  "$schema": "https://ai-engineering.canon/schemas/semantic-target-v1.json",
  "target_id": "tgt_2026_06_10_00412",
  "source_context": {
    "context_type": "browser",
    "origin": "https://billing.internal.enterprise",
    "viewport_id": "vp_001",
    "snapshot_id": "snap_2026_06_10_00412"
  },
  "role_descriptor": {
    "standardized_role": "button",
    "html_role": "button",
    "win32_uia_control": "ButtonControlPattern",
    "macos_ax_role": "AXButton",
    "linux_at_spi_role": "ROLE_PUSH_BUTTON"
  },
  "name_descriptor": {
    "accessible_name": "Place order",
    "visible_text": "Place order",
    "placeholder_text": null,
    "aria_label_value": "Submit and finalize your transaction"
  },
  "locators": {
    "test_id_selector": "data-testid=order-checkout-button",
    "role_selector": "getByRole('button', { name: 'Place order' })",
    "label_selector": null,
    "css_selector": "button.btn-submit-order",
    "xpath_selector": "//button[@type='submit' and contains(., 'order')]"
  },
  "spatial_metadata": {
    "bounding_box": [642, 518, 814, 568],
    "coordinate_system": "viewport_css_pixels",
    "device_pixel_ratio": 1.0,
    "viewport_attached": true,
    "center_point": [728, 543]
  },
  "actionability": {
    "is_visible": true,
    "is_stable": true,
    "is_enabled": true,
    "is_editable": false,
    "receives_pointer_events": true,
    "is_occluded": false
  },
  "risk": {
    "risk_classification": "critical_mutation",
    "confirmation_required": true,
    "post_action_verification_required": true
  },
  "targeting_heuristics": {
    "ambiguity_score": 0.0,
    "confidence_rating": 0.985,
    "resolution_channel": "test_id_plus_accessible_name",
    "fallback_channels": [
      "role_name",
      "visual_ocr",
      "spatial_neighborhood"
    ]
  }
}
```

### **Locator Robustness and Selection Strategy**

The targeting engine should evaluate element locators using a reliability hierarchy:

| Locator Strategy | Reliability | Best Use | Failure Mode |
| :--- | :---: | :--- | :--- |
| **Explicit Test IDs** | Highest | Production-owned interfaces, testable workflows, stable internal apps. | Missing if the app was not instrumented for automation. |
| **Accessible Roles and Names** | High | Buttons, links, menus, dialogs, checkboxes, form controls. | Breaks when accessibility metadata is poor or localized unexpectedly. |
| **Associated Form Labels** | High | Text fields, selects, checkboxes, radio groups. | Breaks when labels are missing or visually detached from fields. |
| **Stable Text Content / Placeholder** | Medium | Content pages, search boxes, simple forms. | Sensitive to localization, A/B copy, and dynamic text changes. |
| **DOM Structure / CSS Selector** | Medium-Low | Fallback when semantic locators are unavailable. | Brittle under component refactors and generated class names. |
| **XPath** | Low | Legacy pages or emergency fallback. | Brittle under hierarchy changes. |
| **Visual Coordinate Targeting** | Lowest as identity, useful as fallback | Canvas apps, remote desktops, screenshots, native apps with poor semantics. | Vulnerable to layout shift, scaling, viewport movement, and occlusion. |

Target selection should prefer the strongest available locator but verify the final candidate with visual and actionability checks. A valid locator is not enough if the target is hidden, overlapped, disabled, or outside the viewport.

UI agents should prefer stable semantic targets over volatile visual coordinates to prevent layout shifts or resolution differences from breaking automated workflows.1 By utilizing accessibility names, roles, and structural attributes, agents construct robust targets that survive minor interface changes.8  
Windows, macOS, and Linux manage accessibility trees using distinct, platform-specific APIs.1 To support cross-platform targeting, systems must abstract these variations into a unified semantic target schema:

```JSON  
{  
  "$schema": "https://ai-engineering.canon/schemas/semantic-target-v1.json",  
  "target_id": "tgt_2026_06_10_00412",  
  "role_descriptor": {  
    "standardized_role": "button",  
    "win32_uia_control": "ButtonControlPattern",  
    "macos_ax_role": "AXButton",  
    "linux_at_spi_role": "ROLE_PUSH_BUTTON"  
  },  
  "name_descriptor": {  
    "accessible_name": "Place order",  
    "placeholder_text": null,  
    "aria_label_value": "Submit and finalize your transaction"  
  },  
  "locators": {  
    "css_selector": "button.btn-submit-order",  
    "xpath_selector": "//button[@type='submit' and contains(., 'order')]",  
    "test_id_selector": "data-testid=order-checkout-button"  
  },  
  "spatial_metadata": {  
    "bounding_box": ,  
    "scaling_factor": 1.0,  
    "viewport_attached": true  
  },  
  "actionability": {  
    "is_visible": true,  
    "is_stable": true,  
    "is_enabled": true,  
    "receives_pointer_events": true  
  },  
  "targeting_heuristics": {  
    "ambiguity_score": 0.0,  
    "confidence_rating": 0.985,  
    "risk_classification": "critical_mutation"  
  }  
}
```

### **Locator Robustness and Selection Strategy**

To minimize failures during system updates, the targeting engine evaluates element locators using a strict reliability hierarchy :

1. **Explicit Test IDs (getByTestId)**: The most robust targeting method.14 It relies on dedicated data attributes (data-testid, data-qa) that remain unchanged during layout modifications.14  
2. **Accessible Roles & Names (getByRole)**: Highly resilient.14 It locates elements by how they are perceived by assistive technologies, ensuring the agent interacts with controls identically to human operators.14  
3. **Associated Form Labels (getByLabel)**: The preferred method for form inputs.14 It binds input controls to their visible labels, preventing input misalignment.14  
4. **Text Content & Placeholders (getByText, getByPlaceholder)**: Resilient for content pages.14 However, these are susceptible to localization changes and translation drift.14  
5. **Raw CSS Selectors & XPath (locator)**: Brittle and prone to breakage.15 Changes to utility classes or component hierarchies can disrupt lookups.15 This method is reserved as a fallback of last resort.15

## **Browser Control Architecture**

The browser control layer acts as the physical link between the agent's planning models and the active browser process. Modern high-dimensional systems select from multiple specialized control channels based on the required speed, latency, cross-browser compatibility, and level of introspection needed:

| Feature Dimension | WebDriver Classic | WebDriver BiDi | Chrome DevTools Protocol | Native Accessibility Bridge |
| :---- | :---- | :---- | :---- | :---- |
| **Transport Layer** | HTTP REST requests (stateless).12 | Persistent bidirectional WebSocket connection.18 | Persistent bidirectional WebSocket connection.19 | Native OS IPC and COM/CoreFoundation calls.8 |
| **Command Response Latency** | High; introduces network overhead per HTTP call.26 | Low; bidirectional websocket messaging.26 | Minimal; direct socket communication with browser process.26 | Single-digit millisecond-level native API calls.4 |
| **Event Streaming** | Lacks streaming; requires continuous HTTP polling.28 | Native streaming of lifecycle, console, and network events.18 | Native streaming of DOM, network, and security events.13 | Native OS event notifications (observers).8 |
| **Execution Performance** | Slower; execution requires driver translation.12 | Extremely fast; direct browser protocol execution.29 | Fast; removes protocol translations.26 | Extremely fast; executes directly on background windows.4 |
| **Introspection Depth** | High-level; limited to user actions and DOM states.12 | Mid-level; captures console, script contexts, and requests.18 | Complete low-level access (memory, profiler, network).13 | Visual controls, roles, positions, and element hierarchies.1 |
| **Cross-Browser Support** | Universal standard supported by all vendors.12 | Emerging standard; supported by major browsers.12 | Proprietary; limited to Chromium-based browsers.29 | Platform-specific; separate implementations required per OS.2 |
| **Context Isolation** | Managed via driver browser parameters. | Dedicated user contexts and sandboxed evaluations.18 | Supports multiple targets and isolated page sessions.13 | Window-level isolation; tracks Frontmost focus.16 |

## **Desktop Automation Boundary Model**

Operating outside the browser sandbox introduces critical safety concerns. A desktop automation agent may interact with local applications, filesystems, native dialogs, terminals, credential prompts, system settings, and global clipboard state. The system must therefore isolate execution, restrict host access, filter network egress, and verify native UI state before dispatching input events.

```text
+--------------------------------------------------------------------------------
| DESKTOP AUTOMATION BOUNDARY MODEL
+--------------------------------------------------------------------------------
|
| [ User / Orchestrator ]
|          |
|          v
| [ Policy and Permission Gate ]
|   allowed apps | allowed files | allowed domains | action risk class
|          |
|          v
| [ Isolated Execution Environment ]
|   VM / container / remote desktop / disposable user profile
|          |
|          +--> [ Filesystem Boundary ]
|          |      read/write workspace only
|          |      no ambient host home-directory access
|          |
|          +--> [ Credential Boundary ]
|          |      brokered credentials
|          |      no raw secrets in model context
|          |
|          +--> [ Network Boundary ]
|          |      egress allowlist
|          |      logging and block rules
|          |
|          +--> [ Clipboard Boundary ]
|          |      isolated clipboard
|          |      no host clipboard leakage
|          |
|          v
| [ Native UI Bridge ]
|   Windows UIA | macOS AX | Linux AT-SPI2 | screenshot fallback
|          |
|          v
| [ Action Dispatcher ]
|   click | type | shortcut | drag | file picker | app command
|          |
|          v
| [ Post-Action Verification ]
|   focus | window state | file state | app state | visual state
|
+--------------------------------------------------------------------------------
| Invariant:
|   Desktop control must never inherit ambient host authority.
|   It operates inside a bounded, observable, revocable environment.
+--------------------------------------------------------------------------------
```

To coordinate interaction across operating systems, desktop bridges use native APIs that present distinct structural features:

| Platform API | Win32 UI Automation (UIA) | macOS AXUIElement API | Linux AT-SPI2 Bridge |
| :---- | :---- | :---- | :---- |
| **Interface Technology** | COM interface via UI Automation client APIs. | CoreFoundation / AXUIElement interfaces. | DBus-based messaging layer over accessibility bus. |
| **Role Categorization** | Strongly typed roles mapping to fixed enums. | Role and subrole string classifications. | Standardized roles with custom application exposure. |
| **Query Mechanism** | Supports cache requests and bulk property fetching. | Element-by-element calls, with batching possible for attributes. | Element-by-element DBus messaging. |
| **Performance Profile** | Strong when cache requests are used. | Moderate; depends on batching and app responsiveness. | Can be slow due to DBus round trips. |
| **Query Optimization** | Cache filters and scoped tree queries. | Multi-attribute copy operations and subtree pruning. | Lazy evaluation and targeted role queries. |
| **Element Verification** | Validates roles, patterns, properties, and bounding rectangles. | Verifies supported actions such as press, focus, or value set. | Verifies available action strings and state sets. |
| **Stable Reference** | Cached snapshots can stale after UI changes. | AX references can stale after window/app updates. | Node changes require re-querying. |
| **Execution Tooling** | UIA clients, automation bridges, remote desktop controllers. | AX clients, app-specific scripting, remote desktop controllers. | AT-SPI2 clients and desktop automation bridges. |

Desktop automation is inherently higher-risk than browser-only automation because the control surface is broader. The default posture should be disposable session, least privilege, blocked host leakage, and strict post-action verification.

## **UI Action Taxonomy and Bounded Planning**

To simplify action planning, UI interactions are categorized into an exhaustive taxonomy of five operations. Actions must be explicitly bound to target elements and state preconditions:

| Action Category | Action Identifier | Input Parameters | Expected Interface Change | Precondition Criteria |
| :---- | :---- | :---- | :---- | :---- |
| **Observation** | ObserveState 1 | Viewport target.1 | Refreshes the active UI state model.4 | Target page context is active.18 |
|  | InspectElement 8 | Target element locator.14 | Captures structural attributes and coordinates.8 | Element node is attached to the DOM.5 |
| **Navigation** | NavigateTo 18 | Destination URL.18 | Target URL loads; page enters network idle.5 | Destination matches domain policies.7 |
|  | NavigateBack 18 | None. | Browser context rolls back to prior history state. | Browser history list contains past pages. |
|  | RefreshPage 18 | None. | Document reloads; active DOM cache is cleared.2 | Target page context is active.18 |
| **Interaction** | Click 11 | Target locator, position offset.11 | Dispatches pointer event; triggers layout change.5 | Element is visible, stable, and enabled.5 |
|  | DoubleClick 11 | Target locator, position offset.11 | Dispatches double-click sequence to target.11 | Element is visible, stable, and enabled.5 |
|  | Hover 35 | Target locator, hover delay.35 | Dispatches pointer moves; triggers tooltip rendering.5 | Target element is inside the viewport.5 |
|  | Type 35 | Target locator, character string.35 | Appends string values to target input.5 | Field is focused and editable.5 |
|  | ClearValue 11 | Target locator.11 | Selects and deletes active input strings.11 | Field is focused and editable.5 |
|  | SelectValue 21 | Target locator, option ID.21 | Selected property updates inside select node.5 | Select element is enabled and not read-only.5 |
|  | CheckControl 11 | Target checkbox locator.11 | Check state updates; aria-checked set to true.5 | Element is an unchecked checkbox.5 |
|  | DragAndDrop | Source locator, destination coordinates. | Source element is moved to target area. | Source and target elements are attached. |
| **System** | Upload 18 | Target input, file path.18 | File properties are updated inside upload node.5 | Target file exists in the sandbox.34 |
|  | Download 18 | Target action locator.18 | File is saved to the designated download path.18 | Host path is write-enabled and has space.34 |
|  | CopyText | Target source locator. | String is copied to the isolated sandbox clipboard. | Source element contains selectable text.5 |
|  | PasteText | Target destination locator. | Clipboard contents are pasted into input.5 | Destination input field is focused.5 |
|  | KeyboardShortcut 1 | Keyboard character combination.1 | System event is triggered inside active context.16 | Context window is active and focused.16 |

### **The Bounded Action Planning Model**

To prevent unverified execution paths, the planning engine evaluates every UI operation as a bounded action record. The record defines target identity, preconditions, execution payload, risk controls, post-action expectations, timeout policy, and recovery playbook.

```json
{
  "plan_record_id": "act_2026_06_10_00412",
  "target_domain": "billing.internal.enterprise",
  "active_action_type": "Click",
  "risk_class": "critical_mutation",
  "target": {
    "semantic_target_id": "tgt_confirm_payment",
    "preferred_locator": "getByRole('button', { name: 'Confirm payment' })",
    "fallback_locator": "data-testid=btn_confirm_payment",
    "expected_role": "button",
    "expected_name": "Confirm payment",
    "bounding_box": [642, 518, 814, 568],
    "coordinate_system": "viewport_css_pixels"
  },
  "preconditions": [
    "target.is_visible == true",
    "target.is_stable == true",
    "target.is_enabled == true",
    "target.receives_pointer_events == true",
    "target.is_occluded == false",
    "active_origin == 'https://billing.internal.enterprise'",
    "payload_hash == approved_payload_hash"
  ],
  "execution_payload": {
    "dispatch_method": "CDP:Input.dispatchMouseEvent",
    "click_point": [728, 543],
    "button": "left",
    "click_count": 1
  },
  "confirmation": {
    "required": true,
    "approval_id": "appr_2026_06_10_7781",
    "approval_scope": "single_execution",
    "expires_at": "2026-06-10T18:35:00Z"
  },
  "post_action_expectations": [
    "payment_request_submitted == true",
    "network_request_contains_idempotency_key == true",
    "success_or_pending_status_visible == true",
    "duplicate_warning_absent == true",
    "action_ledger_entry_created == true"
  ],
  "verification_policy": {
    "timeout_ms": 10000,
    "readiness_signal": "network_quiet_or_app_specific_completion",
    "requires_backend_verification": true,
    "requires_visual_verification": true
  },
  "error_recovery_playbook": "PLY_RECV_CLICK_NO_EFFECT",
  "stop_conditions": [
    "target_ambiguous",
    "approval_expired",
    "payload_mismatch",
    "cross_origin_redirect",
    "duplicate_transaction_warning"
  ]
}
```

A bounded action may dispatch only after all preconditions pass. If a precondition fails, the agent must re-observe, repair, ask for confirmation, or escalate. It must not improvise a nearby coordinate and hope the browser is feeling generous.

## **Click and Type Verification Framework**

The Click and Type Verification Framework acts as the system's execution-level gate. It ensures that inputs are verified directly against resulting UI state transitions before subsequent actions are permitted.

```
+---------------------------------------------------------------------------------------------------------+  
|                                    CLICK & TYPE VERIFICATION TIMELINE                                   |  
+---------------------------------------------------------------------------------------------------------+  
|                                                                                                         |  
|   0ms                     20ms                    100ms                   250ms                         |  
|   +-----------------------+-----------------------+-----------------------+-------------------------+   |  
|   | Pre-Action Check      | Event Dispatch        | Settle Delay          | Post-Action Verification|   |  
|   |                       |                       |                       |                         |   |  
|   | - Confirms visibility | - Programmatic click  | - Monitors page load  | - Audits DOM mutations  |   |  
|   | - Evaluates stability |   or key sequences    | - Tracks transitions  | - Checks input values   |   |  
|   | - Verifies enablement |   dispatched over     | - Waits for active    | - Confirms redirects    |   |  
|   | - Checks occlusion    |   active socket       |   network idle state  | - Validates toast text  |   |  
|   +-----------------------+-----------------------+-----------------------+-------------------------+   |  
|                                                                                                         |  
+---------------------------------------------------------------------------------------------------------+
```

To coordinate interaction across different UI elements, the verification pipeline executes across four sequential stages:

| Verification Phase | Pre-Action Checks | Action Dispatch Method | Settle & Sleep Checks | Post-Action Verification |
| :---- | :---- | :---- | :---- | :---- |
| **Click Verification** | Confirms element visibility, stability, and enablement. Verifies target is not obscured by overlay elements. | Dispatches pointer clicks via CDP input events or invokes platform AXPress. | Monitors CSS transitions and waits for animation frames to stabilize. | Verifies navigation changes, target element detachment, or success toast rendering. |
| **Type Verification** | Confirms field visibility, focus, and editable properties. | Sends character sequences via CDP insert text commands. | Waits for input events to trigger and monitors active client-side validators. | Audits resulting input values against expected strings. |
| **Select Verification** | Validates target select element and active dropdown choices. | Dispatches option selection events or updates selected DOM attributes. | Monitors select container for list collapses or overlay dismissals. | Verifies active option properties match targeted indices. |
| **Check Verification** | Validates checkbox elements and active check parameters. | Dispatches pointer events to checkboxes or updates checking properties. | Waits for toggle transitions to complete. | Evaluates resulting checkbox attributes (aria-checked="true"). |
| **Drag Verification** | Identifies source and destination coordinates. | Sends programmatic mouse-down, drag-path, and mouse-up events. | Waits for layout containers to adapt to element changes. | Verifies target element coordinate intersections inside destination boundaries. |
| **Upload Verification** | Confirms target input element has valid file upload types. | Dispatches file paths directly to target inputs via automation APIs. | Waits for processing and monitors form status. | Verifies uploaded file properties are recognized by the DOM tree. |
| **Download Verify** | Confirms download action controls are attached and enabled. | Dispatches pointer events to download triggers. | Monitors browser context for file download events. | Verifies downloaded files are written to the sandbox directory. |
| **Navigate Verify** | Confirms destination URLs match allowed domains. | Sends navigation requests over active browser connections. | Waits for page load completions and monitors redirects. | Checks resulting URL properties and audits console states. |
| **Submit Verification** | Executes Form Submission Safety checklist. | Sends click events to form submit buttons. | Waits for transitions and verifies active database state updates. | Confirms navigation changes or checks backend API records. |

### **Mitigating the React Re-render Race Condition**

A common automation failure occurs when React unmounts and replaces a DOM element between the agent's pre-action check and event dispatch.10 While the element remains visually identical, holding an outdated DOM reference throws an Element is not attached to the DOM exception 10:  
Race Window (T_race) = T_dispatch - T_check < 50ms  
This race window is typically under 50 milliseconds. The race condition is mitigated using two runtime design patterns:

* **Dynamic Re-Querying**: The execution engine must never store raw element references. Every action must accept a dynamic locator (e.g., page.getByRole("button", { name: "Place order" })) that re-queries and resolves the element target immediately before dispatching the event.10  
* **Web-First Retrying Assertions**: Verification loops must avoid immediate value checks (heading.textContent()) that fail on dynamic page loads.38 Instead, the system must utilize auto-retrying, polling assertions (e.g., expect(heading).toHaveText("Order completed")) that repeatedly evaluate criteria over a 5-second window to accommodate rendering delays.6

## **Form Submission Safety Model**

Form submissions are critical action boundaries because they often trigger durable side effects: database writes, purchases, message sends, file uploads, account changes, legal acknowledgments, or financial transactions. A UI agent must treat a form submit as a tool-like mutation, not as “just another click.”

```text
+--------------------------------------------------------------------------------
| FORM SUBMISSION SAFETY MODEL
+--------------------------------------------------------------------------------
|
| [ Draft Form State ]
|          |
|          v
| [ Field Verification ]
|   required fields | labels | values | masks | validation errors
|          |
|          v
| [ Origin and Frame Verification ]
|   allowed domain | frame ancestry | HTTPS | tenant/session scope
|          |
|          v
| [ Payload Compilation ]
|   normalized payload | target endpoint | recipient | amount | file list
|          |
|          v
| [ Duplicate and Idempotency Check ]
|   action history | idempotency key | prior submissions | warning banners
|          |
|          v
| [ Risk Evaluation ]
|   low | medium | high | critical
|          |
|          +--> low risk
|          |       submit after verification
|          |
|          +--> medium / high / critical
|                  render payload review
|                  require approval or out-of-band confirmation
|                  submit only if payload and approval match
|          |
|          v
| [ Submit Action ]
|   click | keyboard submit | programmatic form submit if allowed
|          |
|          v
| [ Post-Submit Verification ]
|   DOM state | network response | app status | backend/API/ledger when available
|
+--------------------------------------------------------------------------------
| Rule:
|   A form submit is complete only when the resulting state is verified.
|   A success toast is evidence, not proof.
+--------------------------------------------------------------------------------
```

To coordinate forms across processing states, the validation engine maps actions to distinct submission stages:

| Submission Stage | Required Verification Controls | Core Processing Algorithm | Expected Verification Signals | Exception Mitigation Protocol |
| :---- | :---- | :---- | :---- | :---- |
| **1. Draft** | Confirm active form, labels, focus order, and field editability. | Inspect form structure and bind labels to inputs. | Form container attached, visible, and scoped to expected origin. | Re-observe form or halt if structure diverges. |
| **2. Field Entry** | Verify field values, masks, dropdown states, checkboxes, and file attachments. | Compare normalized values against planned payload. | Input values match expected values after client-side formatting. | Clear and re-enter values; preserve validation errors. |
| **3. Ready** | Check required fields, inline errors, disabled submit state, and hidden required controls. | Evaluate DOM, accessibility state, and visible validation text. | Submit control is enabled; validation errors absent. | Resolve missing fields or route to repair. |
| **4. Payload Review** | Compile target recipient, amount, destination, endpoint, files, terms, and side-effect class. | Hash payload and compare against intended action. | Payload hash matches plan and user-visible summary. | Halt if payload differs from plan. |
| **5. Duplicate Guard** | Check action history, idempotency key, warning banners, and prior backend state. | Compare active payload to previous submissions. | No duplicate warning; no prior identical terminal transaction unless idempotent. | Cancel or escalate on duplicate risk. |
| **6. Confirmation** | Apply risk-based approval policy. | Require lightweight, explicit, strict readback, or dual-control confirmation. | Valid approval token bound to payload hash. | Halt if approval missing, expired, or mismatched. |
| **7. Submitted** | Dispatch submit action through verified target. | Click verified submit control or invoke allowed submit mechanism. | Submit event fires; UI enters loading/pending state. | Do not retry blindly; inspect for no-effect or duplicate state. |
| **8. Accepted** | Observe request acceptance or application acknowledgment. | Monitor network response, page transition, or server acknowledgment. | HTTP success, accepted status, or pending transaction ID. | Treat accepted as pending, not completed. |
| **9. Successful** | Verify durable state change. | Check DOM, app-specific state, backend/API/ledger where available. | Success state satisfies expected predicates. | Commit task progress only after verification. |
| **10. Failed** | Extract inline, modal, network, or server-side error. | Normalize failure into repairable/non-repairable category. | Error text, failed response, validation state, or unchanged backend state. | Repair, replan, or escalate. |
| **11. Unknown** | Execution result cannot be verified. | Preserve trace, payload hash, and possible submission state. | Timeout, lost connection, ambiguous UI, or unavailable backend. | Block duplicate submit until reconciliation. |

The submit button is not a magic portal to truth. It is a dangerous little rectangle and should be treated accordingly.

## **Browser Sandboxing, Permissions, and Containment**

When interacting with untrusted websites, the agent's browser runtime must be isolated to prevent credential theft, data leaks, and local process exploits.36 Modern configurations harden browser environments using process-level operating system sandboxes and remote browser isolation frameworks:

| Isolation Domain | Risk Surface | Programmatic Defense Configuration | Intended Containment Boundary |
| :---- | :---- | :---- | :---- |
| **Ephemeral Profiles** | Extends active session history, exposing cookie stores and local caches. | Launches browser using incognito context parameters (--incognito). | Wipes all browser data, cookies, and cache upon context teardown. |
| **Credential Masking** | Exposes passwords and tokens to the agent's prompt context. | Implements secure, headless credential brokers to inject inputs directly into the page. | Prevents credential leaks into context windows or logging outputs. |
| **Password Isolation** | Exposes local keychain vaults and system password managers. | Disables password autofill capabilities (--disable-save-password-bubble). | Blocks access to host credential vaults and system resources. |
| **Download Control** | Triggers drive-by malware downloads or writes to host filesystems. | Restricts download pathways to temporary sandboxed directories (/tmp/downloads). | Blocks execution of downloaded files on the host computer. |
| **Upload Restrictions** | Allows unauthorized exfiltration of sensitive host files. | Restricts upload access to a single, read-only workspace directory. | Prevents the agent from reading or uploading private host data. |
| **Clipboard Isolation** | Leaks sensitive clipboard data or system credentials. | Restricts sandbox browser clipboard APIs and access controls. | Prevents data exchange between host and sandbox clipboards. |
| **Extension Shield** | Vulnerable browser extensions intercept traffic and leak credentials. | Disables extensions on startup (--disable-extensions). | Limits execution to verified browser core components. |
| **Egress Filtering** | Initiates requests to malicious domains or exfiltrates data. | Implements container-level egress filters using domain whitelists. | Restricts outbound traffic to approved API and service endpoints. |
| **Permission Controls** | Accesses cameras, microphones, or location coordinates. | Rejects permission prompts automatically via browser preferences. | Prevents unauthorized monitoring and tracking. |
| **Pop-up Blocking** | Spawns multiple windows to bypass isolation boundaries. | Enforces single-tab restrictions and blocks window creation. | Restricts execution to the active, monitored browser tab. |
| **Cross-Origin Framing** | Frame injection attacks bypass security controls. | Enforces Site Isolation policies, running frames in separate processes. | Prevents malicious frames from accessing parent context data. |
| **Context Teardown** | Orphaned processes leak memory and retain session files. | Closes active connections and wipes temporary mount spaces. | Reclaims system resources and ensures zero persistent traces. |

## **Interface Drift Detection and Recovery**

Interface drift is a primary cause of automation failures. Layout shifts, A/B tests, localization changes, modal dialogs, session expiration, client-side re-rendering, and selector changes can all invalidate a previously valid plan.

The central recovery principle is:

**When the UI changes, the plan is stale. Re-observe before acting.**

```text
+--------------------------------------------------------------------------------
| INTERFACE DRIFT RECOVERY PROTOCOL
+--------------------------------------------------------------------------------
|
| [ Planned Next Action ]
|          |
|          v
| [ Pre-Action Verification Fails ]
|   missing target | ambiguity | occlusion | stale selector | unexpected page
|          |
|          v
| [ Pause Execution ]
|   stop dispatching input events
|   preserve current trace and action history
|          |
|          v
| [ Re-Observe Interface State ]
|   DOM | AX tree | screenshot | URL | focus | network | overlays
|          |
|          v
| [ Classify Drift Variant ]
|   selector | layout | modal | session | localization | network | A/B
|          |
|          v
| [ Recovery Attempt ]
|   re-query locator | dismiss overlay | wait | scroll | replan | ask user
|          |
|          +--> target resolved and verified
|          |       update plan and resume
|          |
|          +--> target unresolved or unsafe
|                  halt, escalate, or transfer control
|
+--------------------------------------------------------------------------------
```

To coordinate drift recovery, the system executes a structured Interface Drift Recovery Model:

| Drift Variant | Visual/DOM Root Cause | Detection Diagnostic Signal | First-Line Automated Recovery | Fallback Resolution Channel | Stop Condition Threshold |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **1. Stale Selector** | Dynamic class names or element hierarchy changed. | Locator fails to resolve in active DOM. | Re-query using accessible role/name and test IDs. | Visual coordinate matching with OCR/neighborhood check. | Timeout or 3 failed query attempts. |
| **2. Layout Shift** | Responsive styling, ads, lazy loading, or animation moves target. | Bounding box changes during actionability checks. | Wait for element position to stabilize. | Recalculate viewport-relative coordinates. | Target remains unstable after 2 seconds. |
| **3. A/B Variant** | Workflow structure or route changes unexpectedly. | URL is plausible but page structure diverges. | Re-evaluate visible workflow elements. | Replan from current state. | Replanning loop fails. |
| **4. Localization Shift** | Labels change language or copy. | Role structure exists but expected text missing. | Use test IDs, roles, and layout relations. | Resolve by structural placement and visual text. | Confidence below threshold. |
| **5. Responsive Hide** | Menus collapse on narrow/mobile viewport. | Element exists but is hidden/offscreen. | Resize, scroll, or open menu container. | Switch to mobile-specific workflow. | Target remains invisible. |
| **6. Dynamic Rearrange** | Search/table rows reorder during execution. | Row coordinates shift or unique key moves. | Locate row by stable cell key. | Filter/search table to isolate target row. | Target key missing. |
| **7. Cookie Banner** | Consent overlay blocks interaction. | Hit-test returns overlay element. | Find and dismiss allowed banner control. | Ask user or apply site policy. | Overlay persists after attempts. |
| **8. Modal Block** | Dialog intercepts viewport controls. | Active modal layer or z-index overlay. | Close/cancel if safe; otherwise ask user. | Escalate if modal is security/payment/permission related. | Modal remains or action risk rises. |
| **9. Progress Spinner** | Async requests delay content. | Spinner or loading state remains active. | Wait using readiness policy. | Refresh or re-observe after timeout. | Spinner exceeds timeout. |
| **10. Session Expired** | Redirect to login or auth page. | URL/cookies indicate auth failure. | Use approved credential broker or ask user. | Suspend workflow. | Login loops or MFA required. |
| **11. Input Mask Shift** | JS formats typed values. | Value verification mismatch. | Clear and retype using slower path or direct value set if safe. | Ask user or use alternate input mode. | Diverges after retries. |
| **12. Multi-Label Match** | Multiple identical controls. | Locator returns multiple nodes. | Scope by parent container and nearby labels. | Human review or visual selection. | Ambiguity remains after filters. |

## **UI Recovery Playbook Library**

The UI Recovery Playbook Library provides developers with predefined, deterministic routines to handle common automation exceptions and maintain workflow stability 4:

| Playbook Identifier | Exception Trigger Case | Action Sequence Strategy | Target Recover State | Escalation / Stop Conditions |
| :---- | :---- | :---- | :---- | :---- |
| **PLY_RECV_MISSING** | Target element is missing or selector fails to resolve.5 | 1. Pauses action loop for 1.5s.1 2. Clears element caches.2 3. Re-queries using accessible names.10 | Element successfully resolved and verified as active. | Aborts and escalates if element is missing after 3 attempts.4 |
| **PLY_RECV_AMBIGUOUS** | Locator resolves to multiple matching DOM elements.14 | 1. Filters results using visibility checks.14 2. Evaluates adjacent labels.6 3. Selects based on spatial coordinates.1 | Target is uniquely identified and isolated. | Immediately halts if targeting ambiguity remains.14 |
| **PLY_RECV_NO_EFFECT** | Click action succeeds but triggers no state changes.5 | 1. Retries click using visual coordinates.1 2. Attempts focus and sends Enter key inputs.5 3. Clears browser focus state.5 | Page state transition or visual mutation verified.5 | Aborts after 2 failed interaction retries. |
| **PLY_RECV_BAD_PAGE** | Navigation loads an unexpected page or error screen.5 | 1. Clears navigation context.18 2. Rewinds page history.18 3. Re-dispatches navigation requests.13 | Active context successfully loads target URL. | Halts if navigation fails twice.10 |
| **PLY_RECV_NAV_TIME** | Page navigation times out or network requests hang.5 | 1. Cancels pending requests.18 2. Refreshes active tab context.18 3. Evaluates current DOM structures.1 | Page settles on a usable, functional state. | Aborts if page fails to load within 30 seconds.10 |
| **PLY_RECV_SPINNER** | Dynamic page content is blocked by loading spinners.1 | 1. Polls loading spinner element status.9 2. Waits up to 10s for spinner unmount.9 3. Refreshes page if hang persists.18 | Progress indicators disappear; page content loads. | Escalates to user if spinner persists after page refresh.7 |
| **PLY_RECV_VALIDATE** | Input action triggers an inline validation error.5 | 1. Clears input field values.11 2. Re-types inputs slowly to trigger events.1 3. Dispatches focus blur events.5 | Validation markers are resolved and cleared. | Halts if inputs repeatedly trigger validation errors. |
| **PLY_RECV_LOGIN** | Page session expires and redirects to login.7 | 1. Identifies login form selectors. 2. Requests credentials from secure vault.7 3. Re-submits login form.7 | Target page session is restored and verified.7 | Aborts if login actions loop back to authentication screen.47 |
| **PLY_RECV_CAPTCHA** | Automation is blocked by a CAPTCHA verification prompt. | 1. Halts execution immediately.40 2. Saves the active session trace.1 3. Escalates control to user.7 | User resolves verification; session context restored. | Instantly halts and transfers control; no retries.40 |
| **PLY_RECV_PERM** | Context navigation triggers a browser permission prompt. | 1. Intercepts prompt using BiDi events.18 2. Rejects request following safety profile.18 3. Closes prompt window.18 | Context permission prompt is cleared and dismissed. | Rejects request and blocks camera/location access.46 |
| **PLY_RECV_PICKER** | Action triggers an unannounced native file picker. | 1. Scans context for active file picker events.18 2. Sets targets using file path controls.5 3. Dismisses native window.18 | File path successfully linked to upload control. | Aborts if native file dialog locks execution focus. |
| **PLY_RECV_DL_BLOCKED** | File download action is blocked by sandbox policy.34 | 1. Audits file metadata and extension types.18 2. Confirms file path matches safety scopes.34 3. Clears blocks if within bounds.18 | Download succeeds and writes to sandboxed directory. | Halts if downloaded file type violates safety limits.34 |
| **PLY_RECV_UP_FAIL** | File upload action fails or is rejected by form. | 1. Verifies local file exists and is readable.34 2. Validates format against form attributes.5 3. Re-dispatches upload path.5 | Form accepts file; upload progress finishes.5 | Aborts if form repeatedly rejects file format.5 |
| **PLY_RECV_OVERLAY** | Target click coordinate is blocked by an overlay.5 | 1. Identifies the overlay's close control.5 2. Sends clicks to dismiss pop-up. 3. Re-evaluates target actionability.5 | Overlay dismissed; target receives pointer events.5 | Aborts if overlay remains visible after 2 attempts. |
| **PLY_RECV_CRASH** | Page process crashes, displaying an error screen.46 | 1. Captures process crash dump logs.22 2. Re-creates the page context.13 3. Re-runs step sequence from history state. | Page context is restored and verified as functional. | Aborts if page crashes repeatedly during steps.46 |
| **PLY_RECV_NETWORK** | Requests fail due to local connection drops. | 1. Pauses execution and checks connection status. 2. Waits up to 10s for network to restore. 3. Retries failed navigation step.18 | Connection is restored; target page loads successfully. | Aborts if connection remains offline after 3 attempts. |
| **PLY_RECV_REDIRECT** | Navigation triggers unexpected cross-origin redirect.5 | 1. Audits target domain origin safety.7 2. Blocks inputs on unverified domains.7 3. Returns to prior history state.18 | Context is returned to a verified page domain. | Aborts if redirected domain violates safety policies.34 |
| **PLY_RECV_EXPIRED** | User session expires during workflow execution.47 | 1. Suspends active step sequence.4 2. Authenticates session via secure login broker.7 3. Restores execution context. | Active session is restored; target workflow resumes. | Halts if authentication attempts fail.47 |
| **PLY_RECV_SUB_ERR** | Form submission returns a server-side error.7 | 1. Extracts error text from target containers.5 2. Updates input fields with corrected data.11 3. Re-runs Form Submission safety checks.7 | Form accepts corrected payload; success confirmed.7 | Aborts if server returns repeatable 500 error codes. |
| **PLY_RECV_DUP_WARN** | Form returns duplicate transaction warning.7 | 1. Scans page for warning confirmation prompts.5 2. Verifies backend transaction logs.7 3. Cancels submit to prevent duplicates.7 | Submission is safely cancelled; status updated. | Halts and escalates to user for validation.7 |
| **PLY_RECV_CONFIRM** | Risky action triggers a confirmation dialog.7 | 1. Compares payload with planned variables.7 2. Scans for confirm button elements.5 3. Clicks to confirm if values match plan.5 | Dialog is dismissed; target mutation proceeds.5 | Halts and escalates if payload values diverge.7 |

## **UI Prompt Injection and Interface Instruction Collision**

UI agents read dynamic, untrusted page content: comments, ads, help text, documents, emails, chat messages, form labels, OCR text, hidden DOM nodes, and user-generated content. This creates a critical vulnerability: indirect prompt injection, where attacker-controlled interface content attempts to override the agent’s instructions, exfiltrate data, or trigger unauthorized actions.

The core security rule is:

**Page content is evidence, not authority.**

```text
+--------------------------------------------------------------------------------
| INDIRECT PROMPT INJECTION LIFECYCLE
+--------------------------------------------------------------------------------
|
| [ Attacker-Controlled UI Content ]
|   visible text | hidden DOM | comments | SVG | ads | documents | OCR text
|          |
|          v
| [ Extraction / Serialization ]
|   DOM text | accessibility tree | OCR | screenshot captions | page summary
|          |
|          v
| [ Context Assembly Risk ]
|   untrusted data placed near instructions or tool descriptions
|          |
|          v
| [ Instruction Collision ]
|   model may confuse page text with developer/system authority
|          |
|          +--> unsafe design
|          |       tool misuse | data exfiltration | credential leak | unauthorized action
|          |
|          +--> safe design
|                  data is delimited, sanitized, policy-gated,
|                  and blocked from granting tool authority
|
+--------------------------------------------------------------------------------
| Invariant:
|   Untrusted UI text can describe the world.
|   It cannot grant permissions, redefine goals, or authorize tools.
+--------------------------------------------------------------------------------
```

Attack delivery vectors:

| Attack Delivery Vector | Programmatic Concealment Method | Parser Extraction Vulnerability | Core System Security Guard |
| :---- | :---- | :---- | :---- |
| **1. Zero-Sizing** | Text hidden with `font-size: 0`, zero width/height, or offscreen positioning. | DOM extractors capture hidden strings; visual models miss them. | Filter by computed visibility, size, viewport intersection, and role relevance. |
| **2. CSS Hidden Divs** | `display: none`, `visibility: hidden`, opacity, clipping, or transform tricks. | Raw DOM serialization may include invisible instructions. | Exclude non-visible content unless specifically requested as code/data evidence. |
| **3. HTML Comments** | Payload placed inside `<!-- malicious instruction -->`. | Naive HTML-to-text pipelines may serialize comments. | Strip comments before model context unless auditing source markup. |
| **4. SVG / XML / CDATA** | Payload embedded inside SVG, XML metadata, or CDATA. | OCR or XML parsers may extract instructions without provenance. | Sanitize vector assets and mark extracted text as untrusted data. |
| **5. Semantic Blend** | Malicious instruction woven into normal page copy. | Model cannot separate content from authority by text alone. | Role separation, data delimiters, and tool-policy enforcement outside the model. |
| **6. Multilingual Override** | Same instruction repeated across languages. | Single-language filters miss alternate phrasing. | Multilingual classification and invariant policy enforcement. |
| **7. Serialization Breakout** | Payload contains strings such as `</untrusted-data>` or JSON/XML delimiters. | Context wrapper can be broken if text is not escaped. | Escape delimiters and serialize untrusted content as data, never raw control markup. |
| **8. Visual Prompt Injection** | Instructions rendered in image, screenshot, ad, or document. | OCR text enters prompt as if it were user/developer instruction. | Treat OCR output as evidence with source label and zero authority. |

### **Principles of System-Data Separation**

| Control | Purpose |
| :--- | :--- |
| **Role Separation** | System/developer instructions, user goals, page content, and tool results must remain distinct channels. |
| **Delimited Evidence Blocks** | Untrusted UI content should be wrapped as data with source labels and escaped syntax. |
| **Capability Sandboxing** | A read-only browsing task should not possess write, email, filesystem, or payment tools. |
| **Policy Enforcement Outside the Model** | Tool permissions, egress controls, credential access, and filesystem access must be enforced by runtime policy. |
| **Computed Visibility Filtering** | Hidden page text should not silently become instruction context. |
| **Source-Aware Summarization** | Summaries should retain whether content came from page text, OCR, metadata, or user instruction. |
| **Action Gating** | Page content cannot authorize actions; only user/system policy and verified approvals can. |
| **Injection Telemetry** | Suspected prompt injection payloads should be logged with source region and blocked capability path. |

A model trained to “ignore malicious instructions” is helpful. It is not a security boundary. Runtime authority boundaries are the boundary. The model is the intern with a clipboard; the sandbox is the locked door.

## **Deceptive Interface Risk Model**

UI agents can be tricked by deceptive design patterns, phishing pages, lookalike domains, fake browser chrome, clickjacking layers, hidden costs, and visual spoofing. A state-verifying UI agent must distinguish between genuine platform state and webpage-rendered imitation.

```text
+--------------------------------------------------------------------------------
| DECEPTIVE INTERFACE RISK MODEL
+--------------------------------------------------------------------------------
|
| [ Candidate Interface Action ]
|          |
|          v
| [ Origin and Transport Verification ]
|   URL | scheme | certificate | frame ancestry | redirect chain
|          |
|          v
| [ Native-vs-Web Prompt Classification ]
|   browser/OS dialog or webpage imitation?
|          |
|          v
| [ Semantic-Visual Consistency Check ]
|   DOM role | AX role | screenshot | hit-test | visible labels
|          |
|          v
| [ Interaction Safety Check ]
|   occlusion | clickjacking | hidden charges | prechecked options
|          |
|          v
| [ Execution Policy ]
|   allow | require confirmation | block | escalate
|
+--------------------------------------------------------------------------------
```

To secure agents against deceptive designs, developers must implement a multi-layered verification model:

| Threat Category | Spoofing Mechanism | Primary Detection Method | Core System Mitigation |
| :---- | :---- | :---- | :---- |
| **Phishing Pages** | Lookalike login screens steal credentials. | Verify active URL, domain allowlist, certificate, frame ancestry, and redirect chain. | Block credential broker on unverified origins. |
| **Clickjacking** | Transparent overlays intercept legitimate clicks. | Hit-test target coordinates and compare event receiver with intended target. | Reject clicks when another element receives pointer events. |
| **Hidden Costs** | Prechecked options, fees, or terms are visually minimized. | Scan form state, checkboxes, fine print, totals, and changed values. | Surface payload review and require approval for cost changes. |
| **Spoofed Native Prompts** | Webpage imitates OS/browser dialogs or security warnings. | Compare active dialog to native window handles / browser prompt APIs. | Ignore webpage-rendered fake system prompts; route real native prompts through permission policy. |
| **Malicious Downloads** | Drive-by file downloads or disguised executable files. | Monitor browser download events and file metadata. | Restrict downloads to sandbox; block execution and unsafe extensions. |
| **Credential Harvest Forms** | Page asks for passwords, tokens, MFA codes, or API keys outside approved flow. | Compare form origin and field purpose to credential policy. | Use credential broker only on allowlisted domains and workflows. |
| **Domain Confusion** | Unicode/homograph, typo-squat, or embedded third-party frame. | Normalize domain, inspect eTLD+1, detect punycode and frame origin mismatch. | Block or require explicit user confirmation. |
| **Visual Misdirection** | Buttons or labels visually imply one action while DOM submits another. | Compare visible text, form action, network endpoint, and click target. | Halt on mismatch and require review. |
| **Dark Pattern Confirmation** | UI nudges agent into unwanted add-ons, subscriptions, or consent. | Detect prechecked options, hidden recurring terms, and opt-out language. | Default to conservative choices and present user review. |

The agent should never enter credentials, approve payments, accept terms, download files, or submit forms on a page whose origin, prompt type, and payload semantics have not been verified.

## **UI Agent Evaluation and Observability Guidance**

Evaluating and debugging UI agents is difficult because interfaces are dynamic, nondeterministic, and partially observable. Task success rate alone is insufficient. A system can complete a task while misclicking, leaking credentials, relying on brittle selectors, ignoring prompt injection, or failing to verify the final state.

Production observability must track perception, targeting, execution, verification, safety, recovery, and replay.

```text
+--------------------------------------------------------------------------------
| UI AGENT OBSERVABILITY ARCHITECTURE
+--------------------------------------------------------------------------------
|
| [ Agent Run Trace ]
|          |
|          +--> [ Perception Artifacts ]
|          |      screenshots | DOM snapshots | AX tree | OCR boxes | focus state
|          |
|          +--> [ Targeting Artifacts ]
|          |      locator attempts | bounding boxes | hit-test results | ambiguity
|          |
|          +--> [ Execution Artifacts ]
|          |      clicks | typing | navigation | uploads | downloads | shortcuts
|          |
|          +--> [ Verification Artifacts ]
|          |      post-action DOM | visual diff | network result | app state | backend check
|          |
|          +--> [ Safety Artifacts ]
|          |      sandbox blocks | egress blocks | prompt-injection detections | approvals
|          |
|          +--> [ Replay Package ]
|                 ordered snapshots | action records | timing | browser profile | artifacts
|
+--------------------------------------------------------------------------------
| Invariant:
|   A UI-agent trace should let an auditor replay what the agent saw,
|   what it targeted, what it did, and how it knew the action worked.
+--------------------------------------------------------------------------------
```

Metrics:

| Metric Category | Technical Metric Identifier | Diagnostic Target | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **Perception** | UI OCR Character Error Rate | Accuracy of text extraction from rendered interfaces. | Task-specific threshold; lower for financial/legal fields. |
| **Perception** | Bounding Box IoU Accuracy | Precision of spatial element localization and grounding. | IoU >= 0.85 on target controls. |
| **Perception** | DOM-Screenshot Agreement | Agreement between DOM-visible text and rendered screenshot text. | > 98% on visible text nodes in controlled apps. |
| **Perception** | Accessibility Coverage Rate | Fraction of actionable controls with usable roles and names. | High for controlled apps; alert on regression. |
| **Targeting** | Grounding Hit Rate | Accuracy of selecting intended UI targets. | > 95% target resolution rate. |
| **Targeting** | Locator Resolution Failure Rate | Frequency of failed or ambiguous locator resolution. | < 2% of active execution steps where locators are expected. |
| **Targeting** | Locator Resolution Latency | Overhead from dynamic re-querying. | Kept within step latency budget. |
| **Targeting** | Occlusion Block Rate | Actions blocked because target is covered. | Nonzero is acceptable; spikes indicate UI drift or modal issues. |
| **Execution** | Step Success Rate | Percentage of interface actions completed without recovery. | > 98% per individual workflow step in stable apps. |
| **Execution** | Misclick Rate | Clicks whose event receiver differs from intended target. | Near zero. |
| **Execution** | Type Fidelity Rate | Inputs where final field values match expected strings. | 100% for controlled strings. |
| **Execution** | Action Retry Rate | Frequency of retries per action type. | Low and bounded; alert on loops. |
| **Verification** | State-Transition Match Rate | Post-action state satisfies expected predicates. | > 95%, task dependent. |
| **Verification** | Focus Verification Rate | Field focus confirmed before typing. | 100% of typing steps. |
| **Verification** | Form Submission Verification Rate | Submitted forms verified through UI/network/backend signal. | 100% for high-impact submits. |
| **Verification** | Unknown-State Rate | Actions where result could not be verified. | Must be low; high-impact unknown states require escalation. |
| **Safety** | Sandbox Isolation Violation Attempts | Attempts to access forbidden host resources. | Zero successful violations; attempted blocks should be investigated. |
| **Safety** | Egress Policy Block Count | Attempts to access unapproved domains. | Blocks may indicate controls working; alert on unexpected spikes. |
| **Safety** | Credential Exposure Events | Secrets exposed to model context, logs, screenshots, or page text. | Zero. |
| **Safety** | Prompt Injection Detection Rate | Suspected page instruction attacks. | Tracked; spikes trigger source review. |
| **Recovery** | Drift Recovery Success Rate | Automated recovery from layout, selector, or modal drift. | High on known apps; bounded attempts. |
| **Recovery** | Human Escalation Rate | Runs requiring user/operator intervention. | Context dependent; spikes indicate automation fragility. |
| **Replay** | Trace Completeness Rate | Runs with screenshots, DOM/AX state, action records, and verification artifacts. | 100% for production actions. |
| **Replay** | Deterministic Replay Success | Ability to reconstruct action sequence from stored artifacts. | Required for regulated or high-impact workflows. |
| **Telemetry** | Average Task Duration | End-to-end time to complete workflow. | Task-specific; monitor regressions. |

Evaluation harnesses should include:

| Test Class | Purpose |
| :--- | :--- |
| **Locator Drift Tests** | Verify target selection under class/name/layout changes. |
| **Occlusion Tests** | Confirm the agent blocks clicks under overlays. |
| **Re-render Race Tests** | Verify stale references are re-queried before dispatch. |
| **Prompt Injection Tests** | Ensure page content cannot grant tool authority. |
| **Sandbox Escape Tests** | Confirm filesystem, clipboard, credential, and egress boundaries. |
| **Form Submit Tests** | Validate duplicate prevention, confirmation, and post-submit verification. |
| **Replay Tests** | Confirm traces reconstruct perception, action, and verification. |

## **Cross-Canon Handoff Map**

The systems architecture of AI-ENG-R integrates with adjacent disciplines across the AI Engineering Systems Canon. Its durable handoff is verified interface state: what the agent saw, what it targeted, what it did, what changed, what failed, and what evidence proves the UI transition.

| Target Canon Report | Domain Area | Technical Handoff Parameters | Engineering Integration Rules |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context Tenure & State Governance | UI snapshots, browser profile state, cookies, local storage, action history. | Enforce retention, redaction, and cleanup rules for session artifacts and browser state. |
| **AI-ENG-C** | Cost, Latency & Margin Management | Step timeouts, rendering latency, locator retries, browser/session costs. | Coordinate action budgets with network readiness, rendering stability, and recovery loops. |
| **AI-ENG-L** | Serving Architecture | Stateful browser/desktop sessions, remote browser pools, backpressure controls. | Coordinate browser and desktop sessions with low-latency serving pools, queues, and capacity limits. |
| **AI-ENG-M** | Agentic Orchestration | UI action loops, stop states, retry budgets, escalation states. | Enforce bounded action sequences and terminate loops on repeated drift or ambiguity. |
| **AI-ENG-N** | Tool Contracts | Browser/desktop action schemas, dispatch payloads, side-effect class. | Treat UI actions as tool contracts with validation, authorization, idempotency, and traceability. |
| **AI-ENG-O** | Action Verification | Post-action UI state, backend/API verification, unknown-state handling. | Dispatch is not completion; state transition verification gates subsequent actions. |
| **AI-ENG-P** | Multimodal Understanding | UI element coordinates, screenshots, crops, OCR regions, visual grounding. | Use visual segmentation and OCR for fallback targeting and evidence citation. |
| **AI-ENG-Q** | Speech and Realtime Interaction | Voice-driven UI commands, spoken confirmations, transcript-to-target mapping. | Bind voice-derived actions to finalized transcript, UI target verification, and confirmation policy. |
| **AI-ENG-S** | Production Pathologies | Layout drift, stale selectors, browser crashes, prompt injection, recovery loops. | Monitor UI-specific failure modes and route recurring issues to incident/problem management. |
| **AI-ENG-T** | Prompt Injection and Trust Boundaries | Untrusted page text, hidden DOM, OCR instructions, data/authority separation. | Treat UI content as evidence, not instructions; enforce tool authority outside the model. |
| **AI-ENG-U** | Dependency Risk | Browser engines, automation protocols, OCR/VLM parsers, accessibility bridges. | Track protocol compatibility, parser drift, and sandbox dependency health. |
| **AI-ENG-V** | Resource Protection | Navigation loops, download abuse, egress blocks, sandbox violations. | Limit action volumes, domain access, file access, downloads, and recursive browsing. |
| **AI-ENG-W** | Fallback and Degraded Modes | DOM failure, visual fallback, human handoff, read-only degraded mode. | Activate visual coordinate matching, text-only fallback, or manual control when UI perception degrades. |
| **AI-ENG-X** | User Trust and Control | Action preview, visible target boxes, confirmation cards, progress display. | Let users inspect, approve, interrupt, and understand interface actions. |
| **AI-ENG-Y** | Human Review | Escalation packets, screenshots, target candidates, ambiguity records. | Transfer session context to users/operators on unresolved targeting or high-risk confirmation. |
| **AI-ENG-Z** | Telemetry and Metrics | UI traces, action spans, screenshots, DOM/AX snapshots, verification results. | Export standardized telemetry for perception, targeting, execution, verification, safety, and replay. |
| **AI-ENG-AA** | Evaluation | UI task benchmarks, drift tests, sandbox tests, injection tests. | Evaluate agents against web, desktop, visual targeting, and safety regression suites. |
| **AI-ENG-AB** | Audit and Replay | Signed traces, screenshots, DOM/AX snapshots, action records, verification artifacts. | Store replayable UI sessions for debugging, audits, and incident review. |
| **AI-ENG-AC** | Incident Response | Sandbox quarantine, credential exposure, malicious downloads, unsafe submits. | Isolate compromised sessions, revoke credentials, preserve evidence, and trigger UI-agent incident playbooks. |
| **AI-ENG-AD** | Governance, Policy, Compliance & Accountability | Approval rules, credential-use policy, data retention, regulated UI workflows. | Govern high-impact UI actions, credential handling, audit obligations, and accountability boundaries. |
| **AI-ENG-AJ** | Reference Architecture | Remote browser isolation, desktop sandbox clusters, UI state model, recovery library. | Provide implementation blueprints for production UI-agent platforms. |

The durable handoff is this:

```text
AI-ENG-R exports verified interface action state:
observed UI, selected target, dispatched action, resulting transition,
verification evidence, recovery path, and replay artifacts.
```

## **Durable Principles of UI Agents**

1. **Interfaces are State Machines**  
   A user interface is a partially observable, drift-prone state machine. Clicks, typing, uploads, downloads, and submits are hypotheses that must be verified against resulting visual, semantic, structural, application, or backend state before execution can proceed.

2. **Dispatch Is Not Completion**  
   A click event, keyboard event, or navigation request is not task progress by itself. Progress is established only when the expected interface transition has been verified.

3. **Dynamic Locators Are Mandatory**  
   Static element references stale under client-side rendering. Agents must re-query dynamic locators immediately before dispatch and verify actionability at the moment of execution.

4. **Semantic Targets Beat Coordinates Until They Don’t**  
   Roles, names, labels, test IDs, and accessibility structures should be preferred over raw coordinates. Coordinates remain necessary for canvas, remote desktop, screenshots, and visual fallback, but they require stricter verification.

5. **Visual State and Structural State Must Reconcile**  
   DOM truth, accessibility truth, and screenshot truth can diverge. Robust UI agents fuse all available channels and halt when the target is ambiguous, hidden, occluded, stale, or visually inconsistent.

6. **Forms Are Tool-Like Mutation Boundaries**  
   Form submissions can send money, messages, files, credentials, legal approvals, or irreversible account changes. They require payload review, duplicate prevention, approval policy, and post-submit verification proportional to risk.

7. **Untrusted UI Text Is Data, Not Authority**  
   Page content, comments, ads, OCR text, emails, documents, and hidden DOM nodes may contain hostile instructions. They can inform the task, but they cannot redefine policy, grant permissions, or authorize tools.

8. **Containment Is the Default Runtime Posture**  
   UI agents should run in disposable, isolated browser or desktop environments with restricted filesystem, clipboard, credential, permission, and network access. No agent should inherit the user’s ambient host authority.

9. **Recovery Must Be Bounded**  
   Interface drift, missing elements, modals, and stale selectors should trigger deterministic recovery playbooks with attempt limits. Infinite “try another click” loops are not resilience. They are chaos in a trench coat.

10. **High-Risk Actions Require Human-Visible Review**  
   Payments, submissions, deletes, credential operations, external messages, and regulated workflows require payload review and explicit approval when policy demands it.

11. **Every Production UI Action Should Be Replayable**  
   A production trace should preserve screenshots, DOM/AX snapshots, locators, bounding boxes, dispatched actions, timing, verification signals, recovery decisions, and final state.

12. **The Agent Controls the Interface; Policy Controls the Agent**  
   The model may plan and propose. The runtime validates, authorizes, dispatches, verifies, contains, logs, and stops. Authority lives in the system boundary, not in whatever the webpage whispered into the prompt.00

#### **Works cited**

1. Desktop Automation AI Agents: Beyond the Browser | by Oktay Ateş | May, 2026 | Medium, accessed June 10, 2026, [https://medium.com/@oktayates/desktop-automation-ai-agents-beyond-the-browser-031c52b0ec7a](https://medium.com/@oktayates/desktop-automation-ai-agents-beyond-the-browser-031c52b0ec7a)  
2. I built a library that gives AI agents structured UI access via accessibility APIs, like Playwright but for the entire OS - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/AI_Agents/comments/1s8o7gs/i_built_a_library_that_gives_ai_agents_structured/](https://www.reddit.com/r/AI_Agents/comments/1s8o7gs/i_built_a_library_that_gives_ai_agents_structured/)  
3. Learn to Adapt: Robust Drift Detection in Security Domain - arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2206.07581](https://arxiv.org/pdf/2206.07581)  
4. computer-use: overhaul for improved agentic desktop + browser interaction · Issue #4445 · daytonaio/daytona - GitHub, accessed June 10, 2026, [https://github.com/daytonaio/daytona/issues/4445](https://github.com/daytonaio/daytona/issues/4445)  
5. Auto-waiting - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/actionability](https://playwright.dev/docs/actionability)  
6. Best Practices - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/best-practices](https://playwright.dev/docs/best-practices)  
7. Indirect prompt injection through enterprise data is becoming a real attack surface - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/aiagents/comments/1tgjzfj/indirect_prompt_injection_through_enterprise_data/](https://www.reddit.com/r/aiagents/comments/1tgjzfj/indirect_prompt_injection_through_enterprise_data/)  
8. Cross-platform desktop automation through accessibility APIs ..., accessed June 10, 2026, [https://crowecawcaw.github.io/general/2026/05/30/accessibility-for-computer-use.html](https://crowecawcaw.github.io/general/2026/05/30/accessibility-for-computer-use.html)  
9. Mastering waits and timeouts in Playwright - CircleCI, accessed June 10, 2026, [https://circleci.com/blog/mastering-waits-and-timeouts-in-playwright/](https://circleci.com/blog/mastering-waits-and-timeouts-in-playwright/)  
10. Playwright auto-wait is great, until your component re-renders mid-action | Mergify, accessed June 10, 2026, [https://mergify.com/blog/playwright-auto-wait-element-rerenders](https://mergify.com/blog/playwright-auto-wait-element-rerenders)  
11. Locator - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/api/class-locator](https://playwright.dev/docs/api/class-locator)  
12. Automation Protocols - WebdriverIO, accessed June 10, 2026, [https://webdriver.io/docs/automationProtocols/](https://webdriver.io/docs/automationProtocols/)  
13. CDP vs. BiDi: Browser Automation Protocol Internals for Scrapers - Evomi Blog, accessed June 10, 2026, [https://evomi.com/blog/cdp-vs.-bidi-browser-automation-protocol-internals-for-scrapers](https://evomi.com/blog/cdp-vs.-bidi-browser-automation-protocol-internals-for-scrapers)  
14. Locators - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/locators](https://playwright.dev/docs/locators)  
15. React Playwright Testing: Complete Guide - Autonoma AI, accessed June 10, 2026, [https://getautonoma.com/blog/react-playwright-testing-guide](https://getautonoma.com/blog/react-playwright-testing-guide)  
16. Browser Control Layer: Programmatic Desktop Automation via macOS Accessibility API for AI Agent Swarms - Pubroot, accessed June 10, 2026, [https://pubroot.com/ai/agent-architecture/browser-control-layer-programmatic-desktop-automation-via-macos-accessib/](https://pubroot.com/ai/agent-architecture/browser-control-layer-programmatic-desktop-automation-via-macos-accessib/)  
17. AI-ENG-P — Multimodal Understanding - Documents, Images, Tables, Charts & Video  
18. WebDriver BiDi reference - MDN Web Docs, accessed June 10, 2026, [https://developer.mozilla.org/en-US/docs/Web/WebDriver/Reference/BiDi](https://developer.mozilla.org/en-US/docs/Web/WebDriver/Reference/BiDi)  
19. Chrome DevTools Protocol - GitHub Pages, accessed June 10, 2026, [https://chromedevtools.github.io/devtools-protocol/](https://chromedevtools.github.io/devtools-protocol/)  
20. Revisiting WebDriver and Selenium: Understanding History and Internals (CDP/BiDi) - Zenn, accessed June 10, 2026, [https://zenn.dev/mima_ita/articles/61c12757495cc0?locale=en](https://zenn.dev/mima_ita/articles/61c12757495cc0?locale=en)  
21. axuiautomation package - github.com/tmc/apple/x/axuiautomation - Go Packages, accessed June 10, 2026, [https://pkg.go.dev/github.com/tmc/apple/x/axuiautomation](https://pkg.go.dev/github.com/tmc/apple/x/axuiautomation)  
22. WebDriver BiDi: 2023 status update | Blog - Chrome for Developers, accessed June 10, 2026, [https://developer.chrome.com/blog/webdriver-bidi-2023](https://developer.chrome.com/blog/webdriver-bidi-2023)  
23. Selenium Chrome DevTools Protocol (CDP) API: How Does It Work? - Applitools, accessed June 10, 2026, [https://applitools.com/blog/selenium-chrome-devtools-protocol-cdp-how-does-it-work/](https://applitools.com/blog/selenium-chrome-devtools-protocol-cdp-how-does-it-work/)  
24. Prompt injection in AI agents: are your controls keeping up?, accessed June 10, 2026, [https://nhimg.org/community/agentic-ai-and-nhis/prompt-injection-in-ai-agents-are-your-controls-keeping-up/](https://nhimg.org/community/agentic-ai-and-nhis/prompt-injection-in-ai-agents-are-your-controls-keeping-up/)  
25. Trust Playwright's Auto-Waiting for End-to-End Testing - YouTube, accessed June 10, 2026, [https://www.youtube.com/shorts/eDdsynE9Xw0](https://www.youtube.com/shorts/eDdsynE9Xw0)  
26. WebDriver vs Chrome DevTools (Speed Test) - Abstracta, accessed June 10, 2026, [https://abstracta.us/blog/testing-tools/webdriver-vs-chrome-devtools-speed-test/](https://abstracta.us/blog/testing-tools/webdriver-vs-chrome-devtools-speed-test/)  
27. WebDriver BiDi - W3C, accessed June 10, 2026, [https://www.w3.org/TR/webdriver-bidi/](https://www.w3.org/TR/webdriver-bidi/)  
28. Chrome DevTools Protocol (CDP) - Pydoll, accessed June 10, 2026, [https://pydoll.tech/docs/deep-dive/fundamentals/cdp/](https://pydoll.tech/docs/deep-dive/fundamentals/cdp/)  
29. WebDriver BiDi - The future of cross-browser automation | Blog - Chrome for Developers, accessed June 10, 2026, [https://developer.chrome.com/blog/webdriver-bidi](https://developer.chrome.com/blog/webdriver-bidi)  
30. BiDirectional functionality - Selenium, accessed June 10, 2026, [https://www.selenium.dev/documentation/webdriver/bidi/](https://www.selenium.dev/documentation/webdriver/bidi/)  
31. IUIAutomationElement7::FindAllWithOptionsBuildCache (uiautomationclient.h) - Win32 apps, accessed June 10, 2026, [https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nf-uiautomationclient-iuiautomationelement7-findallwithoptionsbuildcache](https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nf-uiautomationclient-iuiautomationelement7-findallwithoptionsbuildcache)  
32. IUIAutomationCacheRequest (uiautomationclient.h) - Win32 apps - Microsoft Learn, accessed June 10, 2026, [https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nn-uiautomationclient-iuiautomationcacherequest](https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nn-uiautomationclient-iuiautomationcacherequest)  
33. Caching UI Automation Properties and Control Patterns - GitHub, accessed June 10, 2026, [https://github.com/MicrosoftDocs/win32/blob/docs/desktop-src/WinAuto/uiauto-cachingforclients.md](https://github.com/MicrosoftDocs/win32/blob/docs/desktop-src/WinAuto/uiauto-cachingforclients.md)  
34. AI Agent Sandbox: How to Safely Run Autonomous Agents in 2026 - Firecrawl, accessed June 10, 2026, [https://www.firecrawl.dev/blog/ai-agent-sandbox](https://www.firecrawl.dev/blog/ai-agent-sandbox)  
35. Locator | Playwright Python, accessed June 10, 2026, [https://playwright.dev/python/docs/api/class-locator](https://playwright.dev/python/docs/api/class-locator)  
36. What Is Remote Browser Isolation (RBI) and How Does It Work? - NordLayer, accessed June 10, 2026, [https://nordlayer.com/learn/browser-security/what-is-browser-isolation/](https://nordlayer.com/learn/browser-security/what-is-browser-isolation/)  
37. DevTools Event Listener, accessed June 10, 2026, [https://chromium.googlesource.com/chromium/src/+/refs/heads/main/chrome/test/chromedriver/docs/event_listener.md](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/chrome/test/chromedriver/docs/event_listener.md)  
38. Playwright Assertions: Avoid Race Conditions with This Simple Fix! - DEV Community, accessed June 10, 2026, [https://dev.to/playwright/playwright-assertions-avoid-race-conditions-with-this-simple-fix-dm1](https://dev.to/playwright/playwright-assertions-avoid-race-conditions-with-this-simple-fix-dm1)  
39. Playwright Assertions: Avoid Race Conditions with This Simple Fix! - YouTube, accessed June 10, 2026, [https://www.youtube.com/watch?v=1VxkHP8vfGg](https://www.youtube.com/watch?v=1VxkHP8vfGg)  
40. Fooling AI Agents: Web-Based Indirect Prompt Injection Observed in ..., accessed June 10, 2026, [https://unit42.paloaltonetworks.com/ai-agent-prompt-injection/](https://unit42.paloaltonetworks.com/ai-agent-prompt-injection/)  
41. Why Your AI Agents Need a Sandbox: OpenSandbox by Alibaba Explained - Emelia.io, accessed June 10, 2026, [https://emelia.io/hub/opensandbox-ai-agents-guide](https://emelia.io/hub/opensandbox-ai-agents-guide)  
42. Indirect Prompt Injection in Web-Browsing Agents | Promptfoo, accessed June 10, 2026, [https://www.promptfoo.dev/blog/indirect-prompt-injection-web-agents/](https://www.promptfoo.dev/blog/indirect-prompt-injection-web-agents/)  
43. Sandbox - Chromium Docs, accessed June 10, 2026, [https://chromium.googlesource.com/chromium/src/+/HEAD/docs/design/sandbox.md](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/design/sandbox.md)  
44. Site Isolation - The Chromium Projects, accessed June 10, 2026, [https://www.chromium.org/Home/chromium-security/site-isolation/](https://www.chromium.org/Home/chromium-security/site-isolation/)  
45. bureado/awesome-agent-runtime-security - GitHub, accessed June 10, 2026, [https://github.com/bureado/awesome-agent-runtime-security](https://github.com/bureado/awesome-agent-runtime-security)  
46. chrome://sandbox Diagnostics for Windows - The Chromium Projects, accessed June 10, 2026, [https://www.chromium.org/Home/chromium-security/articles/chrome-sandbox-diagnostics-for-windows/](https://www.chromium.org/Home/chromium-security/articles/chrome-sandbox-diagnostics-for-windows/)  
47. What is Remote Browser Isolation (RBI) | Nomios Group, accessed June 10, 2026, [https://www.nomios.com/resources/remote-browser-isolation-rbi/](https://www.nomios.com/resources/remote-browser-isolation-rbi/)  
48. Prompt Injection - OWASP Foundation, accessed June 10, 2026, [https://owasp.org/www-community/attacks/PromptInjection](https://owasp.org/www-community/attacks/PromptInjection)  
49. How are you handling prompt injection in AI agents that read untrusted content? - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/AskNetsec/comments/1rwywvu/how_are_you_handling_prompt_injection_in_ai/](https://www.reddit.com/r/AskNetsec/comments/1rwywvu/how_are_you_handling_prompt_injection_in_ai/)  
50. how-microsoft-defends-against-indirect-prompt-injection-attacks, accessed June 10, 2026, [https://www.microsoft.com/en-us/msrc/blog/2025/07/how-microsoft-defends-against-indirect-prompt-injection-attacks](https://www.microsoft.com/en-us/msrc/blog/2025/07/how-microsoft-defends-against-indirect-prompt-injection-attacks)  
51. Architectures for Agent Systems: A Survey of Isolation, Integration, and Governance | by yunwei37 | Medium, accessed June 10, 2026, [https://medium.com/@yunwei356/architectures-for-agent-systems-a-survey-of-isolation-integration-and-governance-59224d26e666](https://medium.com/@yunwei356/architectures-for-agent-systems-a-survey-of-isolation-integration-and-governance-59224d26e666)  
52. AI-ENG-Q — Speech, Voice, and Real-Time Interaction Systems

---