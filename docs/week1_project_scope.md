# Project Name

**TicketWise: AI-Powered Ticket Support Assistant for CloudJoi**

# Domain

Artificial Intelligence Engineering / Customer Support Automation / Natural Language Processing

# Problem Statement

Customers do not always resolve their questions through self-service support information. A Gartner survey found that only 14% of customer service issues were fully resolved through self-service. For ticketing enquiries, customers may need to understand policies, find their tickets, or check information about a particular order. Refund questions can also involve conditions such as event cancellation or postponement, request deadlines, and organiser approval.

This project aims to develop TicketWise, an AI-powered support assistant that answers ticketing questions using official CloudJoi information, checks simulated order statuses, and assesses refund conditions. The prototype will be tested to determine whether it selects the correct tools and provides accurate, source-supported answers. It will not access real customer accounts or process actual refunds.

# Evidence of the Problem

**Evidence 1:** A Gartner survey of 5,728 customers found that only 14% of customer service and support issues were fully resolved through self-service. This suggests that existing self-service options do not always resolve customer enquiries. However, the survey is not specific to CloudJoi.

Source / citation: Gartner (2024), *Gartner Survey Finds Only 14% of Customer Service Issues Are Fully Resolved in Self-Service*.

URL: https://www.gartner.com/en/newsroom/press-releases/2024-08-19-gartner-survey-finds-only-14-percent-of-customer-service-issues-are-fully-resolved-in-self-service

**Evidence 2:** CloudJoi's ticketing terms state that refund requests may depend on event cancellation or postponement, the refund-request period, and organiser authorisation. Certain complaints may take up to 30 working days to resolve. This demonstrates that some ticketing enquiries involve multiple conditions and may require follow-up, but it does not prove that CloudJoi customers are dissatisfied with its support.

Source / citation: CloudJoi, *Ticketing Terms & Conditions*.

URL: https://www.cloudjoi.com/ticketing-tnc

# Initial Related Work

**Study / solution 1: CloudJoi FAQ and Support Knowledge Base**

**Source / citation:**

- *Frequently asked questions - CloudJoi*. (n.d.). https://www.cloudjoi.com/frequently-asked-questions-faq
- *CloudJoi Support*. (n.d.). https://help.cloudjoi.com/

**Key limitation:** These resources provide official information, but users still need to find the relevant information themselves. They do not provide the proposed integrated workflow for answering questions and checking simulated order records.

**Study / solution 2: Retrieval-Augmented Generation (RAG)**

**Source / citation:**

- Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., Küttler, H., Lewis, M., Yih, W., Rocktäschel, T., Riedel, S., & Kiela, D. (2020). *Retrieval-Augmented Generation for Knowledge-Intensive NLP tasks*. https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html

**Key limitation:** RAG helps language models retrieve external information when generating answers, but retrieval alone does not check order-specific records or guarantee that every answer is correct.

**Study / solution 3: API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs**

**Source / citation:**

- Li, M., Zhao, Y., Yu, B., Song, F., Li, H., Yu, H., Li, Z., Huang, F., & Li, Y. (2023). API-Bank: a comprehensive benchmark for Tool-Augmented LLMs. *Conference on Empirical Methods in Natural Language Processing (EMNLP)*, 3102–3116. https://doi.org/10.18653/v1/2023.emnlp-main.187

**Key limitation:** API-Bank evaluates how language models use tools across different tasks. It is a benchmark rather than a ready-made ticket-support system, so TicketWise still needs its own tools, validation, and evaluation.

# Engineering Gap / Limitation

Existing support pages provide information for users to read, while RAG and tool-using language models provide approaches for retrieving information and performing tasks. The proposed gap is to combine document-grounded answers with controlled tool calling in one ticket-support workflow. TicketWise will retrieve policy information, check simulated order records, and assess refund conditions while validating tool inputs and identifying cases that require human confirmation.

# Initial Project Objectives

**O1:** To design a support assistant that retrieves relevant information from publicly available CloudJoi documents and provides source references.

**O2:** To implement tool calling for simulated order-status lookup and refund-condition checking, with input validation and appropriate handling of uncertain cases.

**O3:** To evaluate tool-selection accuracy, order-lookup correctness, policy-answer correctness, source support, and error handling using a prepared test set.

# Target Users

CloudJoi customers who need help with ticket retrieval, ticketing policies, order status, or refund conditions.

*(The prototype will use synthetic order data for testing and will not access real customer accounts.)*

# Data Sources

**Source 1: CloudJoi Frequently Asked Questions**

URL: https://www.cloudjoi.com/frequently-asked-questions-faq

**Source 2: CloudJoi Ticketing Terms & Conditions**

URL: https://www.cloudjoi.com/ticketing-tnc

**Source 3: CloudJoi Support article, "What is CloudJoi's refund and exchange policy?"**

URL: https://help.cloudjoi.com/articles/1746419291-what-is-cloud_joi-s-refund-and-exchange-policy

# Similar to EduAdvisor

Both systems answer users' questions using official information sources. Both retrieve relevant content, generate answers, and require evaluation for correctness and source support.

# Different from EduAdvisor

EduAdvisor focuses on academic advising using university documents. TicketWise focuses on ticketing support and includes controlled tool calling to check simulated order records and assess refund conditions. Its evaluation will also measure tool selection and input validation.

# Responsible AI Risks 

**Risk 1 - Hallucination:** The assistant may invent policy details or incorrectly explain refund eligibility.

*Mitigation: use retrieved official information, provide source references, and state when the answer cannot be confirmed.*

**Risk 2 - Incorrect tool use:** The assistant may select the wrong tool or use an invalid order ID, leading to misleading results.

*Mitigation: validate tool arguments, handle errors clearly, and test missing or unknown order IDs.*

**Risk 3 - Privacy:** Order information may contain personal data.

*Mitigation: use synthetic order records only and never store real customer information, credentials, or payment details.*
