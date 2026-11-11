# Week 3: Pipeline Decisions

## 1. Purpose and Requirements

The purpose of the Week 3 data pipeline is to prepare official CloudJoi ticketing information for use by TicketWise.

The pipeline decisions are guided by the following Topic 2 requirements:

| Requirement ID | Data pipeline need |
|---|---|
| FR1 | Prepare official ticketing information, including purchasing procedures, ticket retrieval information, ticketing terms and refund or exchange conditions. |
| FR2 | Preserve identifiable source information so that prepared content can be traced to its original document. |
| RAI1 | Preserve supporting information and source identity to support grounded answers and acknowledgement of insufficient evidence. |

## 2. Selected Data Sources

Three official CloudJoi PDF documents were selected:

| Source ID | Source file | Content |
|---|---|---|
| S01 | `cloudjoi_faq.pdf` | Frequently asked questions about ticket purchasing, ticket retrieval, refunds and online or on-demand viewing. |
| S02 | `cloudjoi_ticketing_terms.pdf` | Ticketing terms and conditions, including purchase conditions, admission, streaming, personal information and refunds. |
| S03 | `cloudjoi_refund_policy.pdf` | Refund, exchange and cancellation policy. |

These sources were selected because their contents directly support TicketWise's ticketing-information requirements.

All three files were publicly accessible from official CloudJoi webpages at the time of collection. Their publication versions and redistribution permissions have not been independently verified.

## 3. Data Preparation Pathway

**Selected pathway:** Pathway A - PDF/TXT document preparation.

All selected sources are PDF documents containing extractable text. The pypdf library was selected to extract text from the PDF files.

Other modality pathways were not selected because the project data does not contain images, audio, time-series data or structured API records requiring those pathways.

## 4. Preparation Method Comparison and Selection

Two initial preparation approaches were compared:

| Approach | Description | Observation |
|---|---|---|
| Fixed-size chunking | Divides extracted text into chunks of approximately 500 characters. | Simple to implement, but may split FAQ questions from their answers or divide related terms and conditions across chunks. |
| Structure-aware preparation | Organises the extracted content according to its existing document structure. | Better preserves FAQ question-and-answer groupings and Ticketing Terms sections. |

The structure-aware approach was selected for the final prepared dataset.

The implementation uses different record structures for the three documents:

| Source | Selected preparation method | Result |
|---|---|---|
| FAQ | Question-and-answer records, with the introduction stored separately. | 22 question-and-answer records and 1 introduction record. |
| Ticketing Terms | Section-aware records, including the introduction. | 17 section records. |
| Refund Policy | One cleaned document-level record. | 1 document-level record. |

The final prepared dataset contains **41 records**.

This is a project-specific adaptation of the initial chunking comparison. It was chosen because the source documents have meaningful structures that should be preserved. The method is not semantic chunking; it uses the identifiable structure of the source documents.

## 5. Pipeline Design Decisions

### 5.1 Processing approach: ETL

The pipeline follows an Extract, Transform and Load (ETL) approach.

- **Extract:** Read the three original PDF files and extract their text using pypdf.
- **Transform:** Clean extracted text and organise it into FAQ question-and-answer records, Ticketing Terms section records and a refund-policy document record.
- **Load:** Validate and save the prepared records as a JSONL file.

This approach was selected because the documents are prepared and validated before the output is stored for later use.

### 5.2 Processing mode: Batch

Batch processing was selected because the source documents are static PDF files that can be processed together in one pipeline run.

A streaming approach was not selected because the current project does not require continuous ingestion of live ticketing data or real-time document updates.

### 5.3 Output format: JSONL

The prepared records are stored in JSON Lines format (`.jsonl`), with one JSON record per line.

This format allows the prepared content to be stored as individual records with associated source information.

The Part B prepared output is:

`data/processed/prepared_text.jsonl`

The Part C executable pipeline output is:

`data/pipeline_v1/documents/prepared_text.jsonl`

The Part C pipeline generated 41 records in one prepared output file.

## 6. Metadata and Data Lineage Decisions

Source information and file identity are retained to support traceability.

SHA-256 checksums are used to identify the original source files and prepared output files. A lineage manifest records the relationship between the source documents and the prepared output.

Two manifests were generated:

| Manifest | Purpose |
|---|---|
| `data/week3_manifest.json` | Records the Part B source-to-output lineage. |
| `data/pipeline_v1/documents/pipeline_manifest.json` | Records information about the Part C executable pipeline run and its validation results. |

The three original PDF sources are registered against the prepared JSONL output.

The original PDFs are kept separate from the prepared data. They should not be committed to a public repository unless redistribution permission has been confirmed.

## 7. Validation Decisions

The preparation and pipeline outputs were checked using the following validation criteria:

| Validation check | Purpose | Result |
|---|---|---|
| Source file existence | Confirm that the selected PDF inputs are available. | Passed. |
| Text extraction | Confirm that the PDF pages contain extractable text. | Passed. All eight pages returned non-empty extracted text during initial inspection. |
| Prepared record count | Confirm that the expected number of records was generated. | Passed. 41 records. |
| Unique record IDs | Prevent duplicate record identifiers in the prepared dataset. | Passed. |
| Non-empty record text | Prevent empty records from being included in the prepared output. | Passed. |
| Required metadata | Confirm that prepared records contain the required source information. | Passed during Part B validation. |
| SHA-256 format | Check that recorded hashes have the expected format. | Passed during Part B validation. |
| Output file integrity | Confirm that the prepared output exists and is not empty. | Passed. |
| Pipeline repeatability | Compare source fingerprints, prepared-output fingerprints, output sizes and source-to-output mappings across two runs. | Passed. |

The Part C repeatability test produced matching source fingerprints, prepared-output fingerprints, output sizes and source-to-output mappings.

The final Part C verification also confirmed that the prepared output contained 41 non-empty records and that the pipeline manifest's validation information matched the recorded pipeline result.

## 8. Failure Points and Responses

| Pipeline stage | Possible failure | Response |
|---|---|---|
| Ingestion | A selected PDF is missing, unreadable or cannot be opened. | Stop processing and report the affected source file. Check the file path and re-upload or replace the source before rerunning the pipeline. |
| Preparation and validation | Text extraction produces empty content, or prepared records fail validation. | Stop the export rather than using incomplete data. Inspect the affected document or preparation logic, correct the issue and rerun validation. |

These failure points are included to make the pipeline's expected behaviour and recovery actions explicit.

## 9. Limitations and Trade-offs

- The pipeline processes the three selected PDF documents only. It does not retrieve updated documents automatically from CloudJoi.
- The extracted text may not preserve all original PDF formatting, paragraph boundaries or reading order.
- The structure-aware preparation uses the document structures identified during inspection. It does not perform semantic chunking.
- Some extracted website navigation text and layout artefacts required cleaning.
- The FAQ includes an incomplete support instruction in one answer. The preparation process does not supply missing information that is absent from the source.
- The publication versions of the FAQ and Ticketing Terms were not identified during inspection.
- Redistribution permissions for the source documents have not been independently verified.
- SHA-256 checksums establish file identity, not factual accuracy, content quality or legal permission.
- The prepared output is a data-preparation artefact for TicketWise. Its generation does not by itself establish that a live retrieval system or CloudJoi integration has been deployed.

## 10. Final Decision

The selected design uses a batch ETL pipeline to extract, clean, organise, validate and store three official CloudJoi PDF documents as 41 structure-aware JSONL records.

This approach was selected because it preserves useful document structures, maintains source traceability and produces a repeatable prepared-data output that supports TicketWise's FR1, FR2 and RAI1 requirements.