# SCM Assistant – Supply Chain RAG Chatbot

## Public Chatbot URL

```text
https://cloud.flowiseai.com/chatbot/6c929c55-2627-47f9-89b2-c3ce046fa00c
https://cloud.flowiseai.com/chatbot/3adda62e-5b8c-41ac-98e5-1e4a6545243f
```

---

# Overview

SCM Assistant is a Retrieval-Augmented Generation (RAG) chatbot built using Flowise Cloud. The chatbot answers supplier governance, compliance, risk management, sustainability, disruption response, and procurement-related questions using:

supplier_performance_data.csv
SupplyChain_Governance_Policy_v3.2.pdf

The chatbot was deployed using Flowise Share Chatbot and made publicly accessible for evaluation.

---

# Technology Stack

| Component        | Technology                           |
| ---------------- | ------------------------------------ |
| Framework        | Flowise Cloud                        |
| LLM              | Gemini 2.5 Flash                     |
| Embedding Model  | HuggingFace Embeddings               |
| Vector Database  | Pinecone                             |
| Retrieval Method | RAG (Retrieval-Augmented Generation) |
| Data Sources     | CSV + PDF                            |
| Deployment       | Flowise Share Chatbot                |
| Version Control  | GitHub                               |


# Architecture

```text
CSV Dataset
            \
             → Embeddings → Pinecone → Retriever
            /
PDF Policy

Retriever + Gemini 2.5 Flash
            ↓
      SCM Assistant
```
# Screenshots

## Document Store
![Document Store](screenshots/document_store.png)

## Chunk Configuration 1
![Chunk Config 1](screenshots/chunk_config_1.png)

## Chunk Configuration 2
![Chunk Config 2](screenshots/chunk_config_2.png)

## Chatflow
![Chatflow](screenshots/chatflow.png)

## Chatbot Testing
![Chatbot Testing](screenshots/chatbot_test.png)

## Public Chatbot URL
![Public URL](screenshots/public_url.png)
---

# Chunking Experiments

## Configuration 1

| Parameter     | Value                             |
| ------------- | --------------------------------- |
| Splitter      | Recursive Character Text Splitter |
| Chunk Size    | 1000                              |
| Chunk Overlap | 200                               |

### Results

* PDF Chunks: 19
* Better retrieval of policy sections
* Moderate retrieval accuracy

---

## Configuration 2

| Parameter     | Value               |
| ------------- | ------------------- |
| Splitter      | Token Text Splitter |
| Chunk Size    | 500                 |
| Chunk Overlap | 100                 |

### Results

* More granular supplier-level retrieval
* Improved retrieval for supplier-specific questions
* Increased number of chunks

---

# Validation Results

## Question 1

### Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

### Expected

* 11 Tier-3 suppliers identified
* High Risk with active disruption flag
* Level 3 Activate response
* CPO escalation and alternate supplier allocation

### Retrieved

* Bohai Electronics
* Maghreb Castworks
* Retrieved only partial supplier list
* Level 2 and Level 3 responses identified

### Status

❌ Partial Match

---

## Question 2

### Which suppliers qualify for the annual Volume Rebate Program and how many are there?

### Expected

* 19 suppliers qualify
* Criteria:

  * Tier-1
  * OTD ≥ 93%
  * Defect Rate < 0.5%
  * Sustainability Score ≥ 85

### Retrieved

* No qualifying suppliers identified
* Returned result: 0 suppliers

### Status

❌ Not Matched

---

## Question 3

### Which region has the highest total PO value, and does it breach the concentration limit?

### Expected

* EMEA
* $193,987,179.91
* 48.5% of total spend
* Breaches concentration limit

### Retrieved

* APAC
* Approx. $9.8M
* 69.79% of retrieved spend
* Breaches concentration limit

### Status

❌ Not Matched

---

## Question 4

### Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

### Expected

* 11 suppliers
* Compliance Score < 60
* Restriction:

  * New PO issuance limited to 20% of prior quarter volume

### Retrieved

* No suppliers identified from retrieved context

### Status

❌ Not Matched

---

## Question 5

### Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

### Expected

* Mechanical Components
* Average Defect Rate: 2.12%
* Below Tier-2 threshold (2.50%)

### Retrieved

* Packaging Materials
* Average Defect Rate: 0.933%
* Below threshold

### Status

❌ Not Matched

---

# Analysis

The chatbot successfully retrieves policy information and supplier-specific records from the uploaded documents. However, several evaluation questions require aggregation across all 2,000 purchase orders and 116 suppliers.

Examples include:

* Highest regional spend
* Supplier qualification counts
* Watch list identification
* Average defect rate calculations

Traditional RAG retrieves only a subset of the most relevant chunks and therefore may not have visibility into the complete dataset during response generation.

---

# Future Improvement

## Integrate a DataFrame / SQL Agent

Instead of relying solely on RAG retrieval, a DataFrame Agent or SQL Agent can be integrated to perform analytical operations across the complete dataset.

Benefits:

* Full dataset visibility
* Accurate aggregations
* Better counting and ranking operations
* Improved supplier analytics


