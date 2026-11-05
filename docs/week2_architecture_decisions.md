# Week 2: Architecture Decisions and Requirements Traceability

## 1. Architecture Decision Record (ADR)

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include Knowledge Access | FR1, FR2, NFR2 and RAI1: Answers must be based on official CloudJoi documents, include source references and handle insufficient information. | Base language model only; static search; dynamic knowledge access. | Include a Knowledge Access component connected to official CloudJoi knowledge sources, with source tracking and evidence handling. | Adds system complexity and retrieval processing, but improves answer grounding, source traceability and the ability to identify insufficient supporting information. |
| Include Scope / Safety Control | RAI2 and NFR4: The system must handle out-of-scope enquiries and missing or unavailable information appropriately. | Rely on language model instructions only; include an explicit scope / safety control. | Include an explicit Scope / Safety Control to identify out-of-scope enquiries and guide appropriate responses. The system also handles insufficient supporting information through its knowledge access and evidence-handling process. | Adds an additional control and may cause some valid enquiries to be incorrectly rejected, but provides a clearer way to manage unsupported enquiries. |
| Use Controlled Tools for Order and Refund Checks | FR4, FR5 and NFR3: Order statuses and refund conditions must be checked accurately using simulated records. RAI4: The system must clarify that final refund approval depends on the event organiser. | Allow the language model to generate results directly; use controlled tools connected to simulated order records. | Use Controlled Tools for order-status and refund-eligibility checks, connected to Simulated Order Records and predefined refund conditions. | Requires additional tool logic and simulated records, but provides more consistent and testable results than relying on generated answers alone. |

## 2. Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR1, FR2, NFR2, RAI1 | Ticketing Answer Correctness; Source Traceability and Grounding | Knowledge Access, CloudJoi Knowledge Sources, Language Model Service and Scope / Safety Control | Results from 30 ticket-related questions and 20 document-related questions, including answer correctness, source-reference accuracy and handling of insufficient evidence. |
| O2 | FR4, FR5, NFR3, RAI4 | Simulated Order-Status Accuracy; Refund-Condition Assessment | Controlled Tools and Simulated Order Records | Results from 10 simulated order-status scenarios and 10 simulated refund scenarios, including correct results and refund-authority communication. |
| O3 | RAI2, NFR4, NFR1 | Out-of-Scope Enquiry Handling; Response Latency | Scope / Safety Control, TicketWise Application / Interface and Evaluation Point | Results from 20 out-of-scope enquiries and response-time measurements across 30 test enquiries. |

## 3. Architecture Review

The V1 conceptual architecture was reviewed against the requirements, evaluation criteria and architecture decisions.

- Each major component is included to support a specific functional, non-functional or Responsible AI requirement.
- Knowledge Access and CloudJoi Knowledge Sources support grounded answers and source traceability.
- Controlled Tools and Simulated Order Records support reliable order-status and refund-condition checks.
- Scope / Safety Control supports the handling of out-of-scope enquiries.
- The Evaluation Point supports the testing and measurement of system performance and behaviour.
- No specific language model, database, retrieval framework or deployment technology has been selected at this stage.

The architecture remains conceptual and may be refined as the project progresses.

## 4. Reflection

The architecture decision involving the most important trade-off was including Knowledge Access. This decision was driven by FR1, FR2, NFR2 and RAI1, which require TicketWise to provide accurate answers based on official CloudJoi information and include verifiable source references. Although Knowledge Access adds system complexity and retrieval processing, I accepted this trade-off because relying only on the language model could result in unsupported or inaccurate answers. Grounding responses in official documents is important to ensure that users receive more reliable ticketing information.