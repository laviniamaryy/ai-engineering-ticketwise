# Data Quality, Bias, and Responsible Data Report — TicketWise

## 1. Data Quality Scorecard

| Dimension | Rating (1–5 / N.V.) | Evidence | Action / limitation |
|---|---:|---|---|
| Completeness | 3/5 | All three CloudJoi source documents are represented. The corpus contains 41 records covering ticket purchasing, ticket retrieval, refunds, streaming and admission. | Review whether important information is missing, especially within the document-level Refund Policy record. Page-level information is also unavailable. |
| Consistency | 4/5 | All 41 records passed the Pandera validation rules for required fields, unique record IDs and allowed processing values. | Automated checks confirm structural consistency but do not guarantee that the extracted content is consistent with the original PDFs. |
| Accuracy | N.V. | Three records were identified during manual inspection with potentially incomplete or incorrect wording. | Compare the affected records with the original PDFs before confirming whether the issues originated from the source documents or extraction process. |
| Timeliness | N.V. | The Refund Policy states that it was last updated on 4 May 2025. The publication dates of the FAQ and Ticketing Terms have not been verified. | Verify source versions and record retrieval dates. Check official CloudJoi information for updates. |
| Uniqueness | 5/5 | All 41 record IDs are unique. No exact duplicate text records were identified after whitespace normalisation. | Retain duplicate checks when new documents or updated versions are added. |

## 2. Validation and Data-Defect Findings

The prepared corpus was assessed using automated validation rules and manual inspection.

| Quality concern | Observable condition | Failures | Decision |
|---|---|---:|---|
| Missing required fields | Record ID, text, source, source fingerprint and processing method must not be empty. | 0/41 (0%) | Pass |
| Duplicate record IDs | Each record must have a unique record ID. | 0/41 (0%) | Pass |
| Invalid processing values | Processing values must match the permitted methods. | 0/41 (0%) | Pass |
| Empty text | Text must contain at least one character. | 0/41 (0%) | Pass |
| Corrupted text | No obvious replacement characters or corrupted text patterns were detected. | 0/41 (0%) | Pass |
| Excessive whitespace | No records with excessive whitespace were detected by the basic text checks. | 0/41 (0%) | Pass |
| Missing page information | The page field is empty for all records. | 41/41 (100%) | Warning / limitation |
| Uncertain content accuracy | Three records contain potentially incomplete or incorrect wording. | 3 records | Manual review required |

**Validation summary**

All 41 records passed the automated Pandera validation rules. No automated validation failures were recorded in `week4_validation_failures.csv`.

However, all 41 records have missing page information because the Topic 3 pipeline did not capture page numbers. This limits the ability to trace individual records back to specific PDF pages, although the source filenames and SHA-256 fingerprints are retained.

Manual inspection also identified three uncertain cases:

1. **FAQ-018:** The phrase "please contact us via for help" appears incomplete because the contact method is not specified.
2. **Ticketing Terms-006:** The phrase "Proof of identity may be required to shown" contains a possible grammatical error.
3. **Ticketing Terms-016:** The heading contains the spelling "Compliants" instead of "Complaints".

These cases have not been confirmed as source-document or extraction errors because the original PDFs have not yet been compared with the extracted records. They are retained for manual review rather than being automatically corrected or removed.

No confirmed stale records were identified. However, the publication dates of two source documents have not been verified, so their current accuracy cannot be guaranteed.

## 3. Duplicate and Anomaly Findings

Duplicate checking was performed using SHA-256 hashes of whitespace-normalised text.

- Original records: 41
- Cleaned records: 41
- Exact duplicate records removed: 0
- Unique record IDs: 41

No exact duplicate text records were identified. Therefore, all 41 records were retained in the cleaned corpus.

The three uncertain records identified during manual inspection were documented for review. They were not removed because their accuracy has not yet been confirmed.

The cleaned JSONL and PKL files preserve the original record information, including record IDs, source filenames, source fingerprints, processing methods and page fields. This helps maintain provenance when the records are used in later development.

No confirmed contradictions or suspicious outliers were established during the current assessment. However, a complete content-level comparison of all source documents has not been performed.

## 4. Representation, Coverage, and Bias Assessment

The prepared corpus contains information from three official CloudJoi documents.

| Source document | Number of records | Percentage |
|---|---:|---:|
| CloudJoi FAQ | 23 | 56.10% |
| CloudJoi Ticketing Terms | 17 | 41.46% |
| CloudJoi Refund Policy | 1 | 2.44% |
| **Total** | **41** | **100%** |

The records cover five main ticketing-related areas:

| Topic | Records containing relevant keywords |
|---|---:|
| Ticket Purchasing | 25 |
| Ticket Retrieval | 4 |
| Refunds and Exchanges | 12 |
| Streaming and On-Demand | 6 |
| Admission | 6 |

These topic counts are based on keyword matching. A record may appear under more than one topic, and the counts do not prove that the information is complete.

**Coverage assessment**

The corpus contains information relevant to the main TicketWise requirements, including ticket purchasing, ticket retrieval, refunds and exchanges. It also retains source information to support identifying the document from which each record originated.

However, the Refund Policy is stored as one document-level record, unlike the smaller records used for the FAQ and Ticketing Terms. Its lower record count does not automatically mean that refund information is missing. Further content-level inspection is needed to confirm whether its important conditions can be identified and used effectively.

**Identified bias risks**

| Bias type | Evidence and likely origin | Propagation path | Potential user-facing effect | Mitigation |
|---|---|---|---|---|
| Representation bias | The FAQ has 23 records, Ticketing Terms has 17, and the Refund Policy has only 1 document-level record. This difference comes from source selection and document processing methods. | Source documents and processing decisions → Uneven record representation → Different opportunities for later retrieval → Important refund information may be overlooked. | Users asking about refunds may receive incomplete or less relevant information. | Review document segmentation and ensure important refund conditions remain identifiable. Test questions from all three sources in Topic 8. |
| Measurement bias | Topic coverage was measured using predefined keywords for five ticketing topics. This method may not identify relevant information that uses different wording. | Keyword selection → Incomplete coverage measurement → Relevant records may be overlooked during evaluation → The system's available evidence may be incorrectly assessed. | Users may receive incomplete answers if relevant information is not identified or retrieved. | Combine keyword matching with manual inspection and improve the keyword list when necessary. |
| Historical bias | The current corpus contains general organisational documents rather than historical customer or demographic records. Historical bias cannot be assessed from the available evidence. | Available source material → Limited evidence for historical assessment → Historical bias remains unknown → Potential risks cannot yet be determined. | The project cannot make claims about historical fairness based on this corpus. | Avoid unsupported assumptions and reassess this risk if relevant historical data is introduced in future. |

**Downstream propagation and future testing**

The identified risks may affect future TicketWise retrieval and answer generation.

For example, if specific refund conditions are difficult to retrieve because the Refund Policy is stored as one large record, the system may return incomplete information or rely on less relevant records from other sources.

Similarly, keyword-based coverage analysis may fail to identify relevant records that use different wording. This could lead to an incorrect assessment of the available evidence.

The pipeline can help reduce these risks through document inspection, suitable text segmentation, source traceability and improved validation methods.

In Topic 8, the implemented system should be tested using questions from different ticketing topics and all three source documents. Testing should examine retrieval relevance, source representation, answer completeness and whether generated answers are supported by the retrieved evidence.

These are potential risks, not evidence of actual unfairness in a completed AI system. No final system fairness conclusion can be made at this stage.

## 5. Privacy, Consent, Rights/Access, and Data Governance

**Source ownership and provenance**

The corpus was prepared from three publicly accessible CloudJoi documents: the FAQ, Ticketing Terms and Refund Policy.

The source filenames and SHA-256 fingerprints are retained in the prepared records. The Refund Policy states that it was last updated on 4 May 2025. However, the publication dates of the FAQ and Ticketing Terms have not been verified.

**Rights and access**

Public accessibility does not automatically mean that the documents are free to redistribute or reuse without restrictions. Their copyright and redistribution permissions have not been independently verified.

Therefore, the original documents should not be publicly redistributed until the relevant permissions have been checked.

**Personal and sensitive information**

The inspected corpus contains general ticketing information. No actual CloudJoi customer records, private order details or personal customer information were identified in the prepared records.

Although the Ticketing Terms discuss the collection of customer personal information, this does not mean that such information is included in the current corpus.

Actual customer records and unnecessary personal information should remain excluded from the academic prototype.

**Consent and collection basis**

The current corpus uses publicly accessible organisational documents and does not involve collecting actual customer personal data. Therefore, individual customer consent is not applicable to the current corpus.

If personal data is introduced in future development, its collection basis, consent requirements and privacy safeguards must be assessed separately.

**Version control and governance**

Source filenames and fingerprints are retained to support traceability and integrity checks. However, a formal schedule for checking source updates and handling superseded documents has not yet been established.

The project team should record retrieval dates, verify available publication dates and review the corpus when official CloudJoi information changes.

## 6. Mitigation Plan and Residual Risk

| Issue | Evidence | Action | Owner / timing | Residual risk |
|---|---|---|---|---|
| Missing page information | All 41 records have null page values. | Improve page-level extraction if supported by the source documents and pipeline. | Project team / Before final corpus improvement | Page-level traceability may remain unavailable for existing records. |
| Uncertain extracted content | Three records were flagged during manual inspection. | Compare the records with the original PDFs and correct only confirmed extraction errors. | Project team / Before final system evaluation | Source-document errors may still exist. |
| Refund Policy segmentation | The Refund Policy is stored as one document-level record. | Review its content and consider splitting it into meaningful sections while preserving source information. | Project team / Before Topic 8 | Some refund conditions may remain difficult to identify. |
| Unverified source dates | Publication dates for the FAQ and Ticketing Terms are unknown. | Verify source versions and record retrieval dates. | Project team / Before final evaluation | Official information may change after the review. |
| Potential redistribution restrictions | Copyright and redistribution permissions have not been verified. | Check the relevant permissions before publicly sharing original documents. | Project team / Before public distribution | Usage restrictions may still limit redistribution. |
| Potential measurement bias | Keyword-based topic analysis may miss relevant records. | Combine keyword checks with manual content inspection and later retrieval testing. | Project team / During Topic 8 | Some relevant information may still be missed. |
| Future data privacy risks | The current corpus does not contain actual customer records. | Continue excluding unnecessary personal information and reassess privacy requirements if personal data is introduced. | Project team / Throughout development | Future integrations may introduce additional privacy risks. |

## 7. Impact on Topic 2 Requirements / Evaluation Criteria

The identified data quality issues may affect the following TicketWise requirements:

| Topic 2 requirement | Potential impact | Required action |
|---|---|---|
| **FR1:** Use official CloudJoi information to answer ticket-related enquiries. | Missing, outdated or incorrectly extracted information may cause TicketWise to provide incomplete or inaccurate answers. | Review uncertain records, verify source versions and test answers against the original documents. |
| **FR2:** Preserve source information and identify insufficient evidence. | Missing page numbers limit detailed source traceability. Incomplete or poorly segmented records may also make it harder to identify the supporting information. | Preserve record IDs, source filenames and fingerprints. Improve page-level traceability where possible and ensure insufficient evidence is recognised. |

The Responsible AI requirements are also affected.

- **Accuracy and reliability:** Unverified wording and outdated information may affect the correctness of generated answers.
- **Transparency:** Missing page numbers may limit the detail of supporting source references.
- **Privacy:** Future use of actual customer information would require appropriate privacy safeguards and authorisation.
- **Accountability:** Source versions, validation findings and unresolved issues should remain documented.
- **Fairness:** Uneven source representation and keyword-based measurement may affect which information is available for later retrieval.

These issues could affect future evaluation results, particularly answer accuracy, source relevance, answer completeness and the ability to identify insufficient evidence.

The Topic 4 findings should therefore be considered when designing and evaluating the later TicketWise system.

## 8. Topic 4 Quality Gate Decision

**Decision: GO WITH CONDITIONS**

The prepared CloudJoi corpus can proceed to the next academic development stage because all 41 records passed the automated structural validation rules, all three source documents are represented, and no exact duplicate text records were identified.

However, this decision does not mean that the dataset is fully accurate, complete or free from risk.

The following conditions must be maintained:

1. The three uncertain records must remain documented and should be compared with the original PDFs before final system evaluation.
2. The missing page information must be recognised as a traceability limitation.
3. The Refund Policy should undergo further content-level inspection to assess whether its important conditions are adequately represented.
4. Source versions and retrieval dates should be verified and recorded whenever available.
5. Original CloudJoi documents must not be publicly redistributed until the relevant permissions have been verified.
6. Actual customer personal information must not be introduced without appropriate authorisation and privacy safeguards.

The cleaned corpus is suitable for continued academic prototype development under these conditions. Its limitations and residual risks must be considered during subsequent retrieval, answer generation and system evaluation.

**Final assessment:** The Topic 4 quality gate is passed with conditions. Further data review and testing are required before making claims about the final system's accuracy, reliability or fairness.