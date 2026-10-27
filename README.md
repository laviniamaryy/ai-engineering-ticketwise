
# TicketWise — AI-Powered Ticket Support Assistant for CloudJoi
> An AI support assistant that answers ticketing questions using official CloudJoi information and checks simulated order statuses and refund conditions.
<p align="center">
  <img src="docs/ticketwise_logo.png" alt="TicketWise Logo" width="700">
</p>

## Problem Statement
Customers do not always resolve their questions through self-service support information. A Gartner survey found that only 14% of customer service issues were fully resolved through self-service. CloudJoi's ticketing terms also show that refund requests can depend on event conditions, deadlines, and organiser approval.

TicketWise will be a prototype that answers policy questions using official information and uses controlled tools to check simulated order records and refund conditions. It will be evaluated for accuracy, tool selection, source support, and error handling.

## Evidence of the Problem
- **Evidence 1: Self-service support does not always resolve customer issues.**
Gartner reported that only 14% of customer service and support issues were fully resolved through self-service, based on a survey of 5,728 customers conducted in December 2023. This supports the broader need for more effective self-service support, but it is not specific to CloudJoi.

Reference: Gartner. (2024, August 19). Gartner survey finds only 14% of customer service issues are fully resolved in self-service. https://www.gartner.com/en/newsroom/press-releases/2024-08-19-gartner-survey-finds-only-14-percent-of-customer-service-issues-are-fully-resolved-in-self-service

- **Evidence 2: Ticketing enquiries can involve multiple conditions and steps.**
CloudJoi's official refund policy explains that refunds are generally unavailable unless an event is cancelled or postponed. Eligible refunds may also require a request form, submission within a specified period, and compliance with the applicable refund procedure. This demonstrates that answering refund enquiries requires checking specific conditions, although it does not establish that customers find the process difficult.

Reference: CloudJoi. (n.d.). What is CloudJoi's refund and exchange policy? https://help.cloudjoi.com/articles/1746419291-what-is-cloud_joi-s-refund-and-exchange-policy
 
## Initial Related Work / Existing Approaches
- **1. CloudJoi FAQ and Support Knowledge Base:** CloudJoi provides official FAQs and support articles covering ticket retrieval, ticketing policies, and refunds (CloudJoi, n.d.).
**Key limitation:** Users must find and interpret the relevant information themselves; whether this causes difficulties for customers has not been established.  

*Source: Frequently asked questions - CloudJoi. (n.d.). https://www.cloudjoi.com/frequently-asked-questions-faq* 
 
- **2. Retrieval-Augmented Generation (RAG):** Lewis et al. (2020) proposed RAG, which combines language generation with retrieval from external knowledge sources. TicketWise can use this approach to generate answers grounded in CloudJoi's public support documents.
**Key limitation:** Retrieved information does not guarantee that every generated answer will be correct or fully supported by its sources.  

*Source: Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., Küttler, H., Lewis, M., Yih, W., Rocktäschel, T., Riedel, S., & Kiela, D. (2020, May 22). Retrieval-Augmented Generation for Knowledge-Intensive NLP tasks. arXiv.org. https://arxiv.org/abs/2005.11401*

- **3. API-Bank: A Benchmark for Tool-Augmented LLMs:** Li et al. (2023) introduced a benchmark for evaluating language models that plan and use external tools. This is relevant to TicketWise's order-status and refund-condition tools.
**Key limitation:** The benchmark evaluates general tool-use capabilities rather than TicketWise's specific ticketing-support workflow.

*Source: Li, M., Zhao, Y., Yu, B., Song, F., Li, H., Yu, H., Li, Z., Huang, F., & Li, Y. (2023). API-Bank: a comprehensive benchmark for Tool-Augmented LLMs. Conference on Empirical Methods in Natural Language Processing (EMNLP), 3102–3116. https://doi.org/10.18653/v1/2023.emnlp-main.187*
 
## Project Objectives
- O1: Design a support assistant that retrieves relevant information from public CloudJoi documents and provides source references.

- O2: Implement tool calling for simulated order-status lookup and refund-condition checking, with input validation and appropriate handling of uncertain cases.

- O3: Evaluate tool-selection accuracy, order-lookup correctness, policy-answer correctness, source support, and error handling using a prepared test set.

## Target Users
CloudJoi customers seeking help with ticket retrieval, ticketing policies, order status, or refund conditions. Testing will use prepared questions and synthetic order records, not real customer accounts.

## Data Sources
- [Source 1: CloudJoi Frequently Asked Questions — https://www.cloudjoi.com/frequently-asked-questions-faq]
- [Source 2: CloudJoi Ticketing Terms & Conditions — https://www.cloudjoi.com/ticketing-tnc]
- [Source 3: CloudJoi Refund and Exchange Policy — https://help.cloudjoi.com/articles/1746419291-what-is-cloud_joi-s-refund-and-exchange-policy]

## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |


