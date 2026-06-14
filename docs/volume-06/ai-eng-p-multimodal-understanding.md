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

[← Back to Canon Map](../canon-map.md)