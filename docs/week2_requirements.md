# Section 0 - Project Framing & Application Scope Checkpoint

## Problem Evidence

### Evidence 1: Difficulties with Self-Service Customer Support

Gartner (2024) conducted a survey involving 5,728 customers and found that only 14% of customer service and support issues were fully resolved through self-service. The survey also reported that 43% of customers who experienced unsuccessful self-service attempts could not find content relevant to their issues. These findings highlight a general challenge in self-service support, where customers may struggle to locate the information needed to resolve their enquiries.

**Source:** Gartner (2024), *Gartner Survey Finds Only 14% of Customer Service Issues Are Fully Resolved in Self-Service*.

https://www.gartner.com/en/newsroom/press-releases/2024-08-19-gartner-survey-finds-only-14-percent-of-customer-service-issues-are-fully-resolved-in-self-service

### Evidence 2: Ticketing Enquiries and Policy Conditions in CloudJoi

CloudJoi's official FAQ identifies several customer enquiries, including difficulties retrieving tickets, locating tickets in purchase history and understanding ticket availability. In addition, CloudJoi's Ticketing Terms and Conditions explain that refund eligibility may depend on conditions such as event cancellation or postponement, the refund request period and authorisation from the event organiser. This shows that ticketing support involves different types of enquiries, some of which require customers to understand specific policy conditions.

**Sources:**

- CloudJoi, *Frequently Asked Questions (FAQ)*: https://www.cloudjoi.com/frequently-asked-questions-faq
- CloudJoi, *Ticketing Terms and Conditions*: https://www.cloudjoi.com/ticketing-tnc

### Overall Justification

These findings provide a basis for investigating an AI-powered ticket support assistant that helps customers retrieve relevant information from official ticketing documents and check order-related information through controlled tools.

## Existing Approaches

**1. Retrieval-Augmented Generation with Knowledge Graphs for Customer Service Question Answering**

Xu et al. (2024) proposed an approach that combines Retrieval-Augmented Generation (RAG) with knowledge graphs to improve customer-service question answering. Their approach retrieves relevant information from historical customer-service issue records and uses the retrieved information to generate answers. The system was also deployed within LinkedIn's customer-service team.

**2. τ-Bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains**

Yao et al. (2025) introduced τ-Bench, a benchmark designed to evaluate AI agents in customer-service scenarios. It focuses on retail and airline domains, where agents interact with users, use external APIs and follow domain-specific policies to complete tasks. The benchmark evaluates how reliably agents can perform these tasks.

**3. API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs**

Li et al. (2023) introduced API-Bank, a benchmark for evaluating the ability of large language models (LLMs) to use external tools. It assesses three main capabilities: planning the required actions, retrieving suitable APIs and correctly calling those APIs. The benchmark includes 73 API tools and 314 dialogues.

**Relevance to TicketWise**
These studies demonstrate existing approaches for customer-service question answering, evaluating AI agents that follow policies, and enabling LLMs to use external tools. They provide a foundation for the design and evaluation of TicketWise, particularly its document-based question answering and simulated order and refund checking functions.

**References**

- Xu, Z., et al. (2024). *Retrieval-Augmented Generation with Knowledge Graphs for Customer Service Question Answering*. https://arxiv.org/abs/2404.17723
- Yao, S., et al. (2025). *τ-Bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains*. https://proceedings.iclr.cc/paper_files/paper/2025/file/1b126cc38b8638e07bef37e7b2bb72bf-Paper-Conference.pdf
- Li, M., et al. (2023). *API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs*. https://doi.org/10.18653/v1/2023.emnlp-main.187

## Project / Engineering Gap

Existing studies have explored retrieval-based approaches for customer-service question answering and the use of external tools by AI agents. However, applying these approaches to a specific ticket-support context requires careful integration and control to ensure that the assistant provides answers grounded in reliable information and performs order-related tasks correctly.

The practical engineering challenge is to develop an AI assistant that retrieves relevant information from official CloudJoi documents, provides supporting sources for its answers, and uses controlled tools to check simulated order statuses and refund conditions. The assistant must also handle situations where information is missing or insufficient to avoid providing unsupported answers or incorrect results.

Therefore, this project focuses on developing and evaluating a bounded ticket-support application that addresses information grounding, source traceability and reliable tool usage using CloudJoi's published information and simulated order records.

## Project Objectives 

**Objective O1**
To design an AI-powered ticket support assistant that retrieves relevant information from official CloudJoi documents and identifies supporting sources to answer user enquiries.

**Objective O2**
To develop controlled tools for checking simulated ticket order statuses and refund conditions based on predefined rules and order records.

**Objective O3**
To evaluate the assistant's performance in answering ticket-related enquiries, selecting and using the appropriate tools, and handling missing or insufficient information.

## Intended Users

The intended users are individuals who have purchased or are interested in purchasing tickets through CloudJoi and need assistance with ticket-related enquiries. This includes users who want to understand ticketing policies, retrieve ticket information, check their order status or find out about refund conditions.

The prototype is designed for testing these support functions and is not an officially deployed CloudJoi customer-support system.

## Supported Tasks

TicketWise is designed to support the following tasks:

1. **Answer ticketing-related questions:** Retrieve relevant information from CloudJoi's official FAQ, Ticketing Terms and Conditions, and refund policy.

2. **Provide ticket retrieval guidance:** Explain how users can retrieve their tickets and locate them in their purchase history.

3. **Check simulated order status:** Use a controlled tool to retrieve the status of an order by checking the order ID provided by the user against simulated order records.

4. **Check refund conditions:** Use a controlled tool to assess whether a simulated order meets the predefined conditions for submitting a refund request, based on the available order information and CloudJoi's published refund policy.

5. **Handle missing or unsupported information:** Ask users for missing information when necessary, explain when information is unavailable, and direct users to the appropriate official CloudJoi support channel when the enquiry cannot be handled.

## Information / Knowledge Domain

TicketWise will use the following information sources:

1. **CloudJoi FAQ:** Publicly available information about ticket purchases, ticket retrieval, payment methods and frequently asked questions.

2. **CloudJoi Ticketing Terms and Conditions:** Official information about ticketing rules, purchase conditions, event cancellations, postponements and refund procedures.

3. **CloudJoi Refund and Exchange Policy:** Official information about refund and exchange conditions.

4. **Simulated order records:** Synthetic order data created for the prototype, containing information such as order IDs, payment statuses, event details and order statuses.

5. **Predefined refund conditions:** Structured rules derived from CloudJoi's published refund policy, used to check whether simulated orders meet the stated conditions for submitting a refund request and to identify situations requiring further confirmation from the event organiser.

The official CloudJoi documents will serve as the knowledge sources for answering ticket-related questions, while simulated order records will support the order-status and refund-condition checking functions. The prototype will not access actual CloudJoi customer accounts, private order records or live transaction systems.

## Operating Context

TicketWise will operate as a prototype AI-powered ticket support assistant with a conversational interface. Users will interact with the assistant by submitting text-based ticket-related enquiries.

The assistant will retrieve information from official CloudJoi documents and use controlled tools to check simulated order records and refund conditions. The prototype will operate independently without integration with CloudJoi's live platform, payment systems or customer databases.

The system will be evaluated in a controlled testing environment using predefined test questions and synthetic order data.

## Important Constraints

1. **Dependence on public information:** The assistant's answers will be limited to information available in CloudJoi's published documents. Unpublished or event-specific information may not be available.

2. **Changing policies:** CloudJoi's ticketing and refund policies may change over time. The knowledge sources will need to be reviewed and updated to maintain accuracy.

3. **Event-specific refund conditions:** Refund procedures and deadlines may vary depending on the event organiser and circumstances. The prototype will use simplified rules based on available policy information and simulated event data.

4. **Limited evaluation data:** The assistant will be evaluated using a predefined set of test questions and synthetic order records. The results may not represent every possible real-world customer enquiry.

5. **Development resources:** The implementation and evaluation will be limited by the available development time, computing resources and technical capabilities.

## Explicit Out-of-Scope Tasks

TicketWise is not designed to perform the following tasks:

1. **Actual ticket purchases:** The assistant will not purchase or book real event tickets on behalf of users.

2. **Payment processing:** The assistant will not process payments, issue refunds or handle financial transactions.

3. **Live account and order access:** The assistant will not access actual CloudJoi customer accounts, private order records or live transaction systems.

4. **Official refund approval:** The assistant will not make final refund approval decisions or override the event organiser's refund policy.

5. **Ticket cancellation or modification:** The assistant will not cancel, exchange, transfer or modify actual tickets.

6. **Direct communication with organisers:** The assistant will not contact event organisers or CloudJoi support on behalf of users.

7. **Unrelated enquiries:** The assistant will not handle enquiries unrelated to CloudJoi ticketing and the supported tasks defined in this project.

## Evaluation Focus
TicketWise will be evaluated based on the following aspects:

1. **Answer correctness:** Whether the assistant provides accurate answers to ticket-related questions based on the official CloudJoi documents.

2. **Information retrieval and source relevance:** Whether the assistant retrieves relevant information from the correct CloudJoi documents and identifies the supporting sources for its answers.

3. **Tool selection and execution:** Whether the assistant selects and uses the appropriate tools to check simulated order statuses and refund conditions.

4. **Result accuracy:** Whether the assistant returns the correct order status and assesses refund conditions accurately based on the simulated records and predefined rules.

5. **Handling missing or unsupported information:** Whether the assistant appropriately asks for missing information, identifies unavailable information and avoids making unsupported claims.

6. **Response time:** Whether the assistant can provide answers and complete tool-based tasks within a reasonable amount of time.

# Section 1: Functional Requirements

**Version:** 1.0

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|---|---|
| **FR1** | Answer users' ticket-related enquiries by providing information supported by the relevant CloudJoi FAQ, Ticketing Terms and Conditions, or Refund and Exchange Policy. | Invent ticketing policies or present unsupported information as official CloudJoi information. |
| **FR2** | Identify the relevant CloudJoi document and provide a supporting source reference for answers based on published ticketing information. | Present unsupported information as if it came from an official CloudJoi source. |
| **FR3** | Provide instructions on how users can retrieve their purchased tickets based on CloudJoi's published ticket retrieval procedures. | Access users' actual CloudJoi accounts or retrieve tickets directly from the live platform. |
| **FR4** | Check and report the status of a simulated ticket order by matching the order ID provided by the user against the available simulated order records. | Access or modify actual customer orders or live transaction records. |
| **FR5** | Assess whether a simulated ticket order meets the predefined conditions for submitting a refund request based on the available order information and published refund policy. | Approve refunds, process refund payments, or override decisions made by event organisers. |

# Section 2: Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | Performance | TicketWise shall return responses within 10 seconds for at least 90% of test enquiries under normal testing conditions. | Measure the end-to-end response time for a test set of 30 enquiries. | 90th percentile (p90) response time | ≤ 10 seconds |
| **NFR2** | Correctness | TicketWise shall provide answers that are factually consistent with the official CloudJoi documents. | Evaluate 30 ticket-related questions by comparing the assistant's answers against the official documents using a predefined scoring rubric. | Correct answer rate (%) | ≥ 85% |
| **NFR3** | Reliability | TicketWise shall return accurate results when checking simulated order statuses and refund conditions. | Test 20 scenarios, including both simulated order-status checks and refund-condition checks, and compare the results against the expected outcomes. | Correct tool result rate (%) | ≥ 95% |
| **NFR4** | Robustness | TicketWise shall appropriately handle enquiries when the required information is missing or outside its supported scope. | Test 10 scenarios involving missing information, unsupported questions, or unavailable records, and assess whether the assistant responds appropriately. | Appropriate handling rate (%) | ≥ 90% |

# Section 3: Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | Transparency / Grounding | TicketWise shall provide a supporting source reference for answers based on CloudJoi's published documents and indicate when sufficient supporting information cannot be found. | Test 20 document-related questions, including questions with insufficient information. Check whether the assistant provides correct source references or acknowledges missing evidence. | Correct source reference rate; appropriate missing-evidence response rate | ≥ 95% for both metrics |
| **RAI2** | Safety / Scope | TicketWise shall decline enquiries outside its defined ticket-support scope and provide an appropriate redirect where possible. | Test 20 out-of-scope enquiries and assess whether the assistant declines them appropriately. | Appropriate refusal rate (%) | ≥ 95% |
| **RAI3** | Privacy / Data Protection | TicketWise shall use simulated order records and shall not request real payment credentials or account passwords from users. | Inspect the prototype's input fields and test 10 scenarios involving requests for sensitive information. | Number of cases requesting real payment credentials or account passwords | 0 |
| **RAI4** | Transparency / Accountability | TicketWise shall clearly communicate that refund-condition checks are informational and do not represent final refund approval by an event organiser. | Test 10 simulated refund scenarios and check whether the assistant clearly distinguishes refund eligibility conditions from actual refund approval. | Correct refund-authority communication rate (%) | 100% |