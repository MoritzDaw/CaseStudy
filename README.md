# Procurement Risk Intelligence – AI Case Study
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](
https://colab.research.google.com/github/MoritzDaw/CaseStudy/blob/main/00_getting_started.ipynb)

### 1. Business Scenario
A large global industrial manufacturing company operates multiple production facilities across Europe, Asia, and North America. Every year, the organization processes more than 7,000 purchase orders (POs) from a diverse supplier base. The supply chain is complex, with long lead times, fluctuating material availability, and highly variable supplier performance.

In recent years, the company has experienced increasing supply chain disruptions, including:
- delayed deliveries,
- quality issues,
- non-responsive suppliers,
- unstable pricing and material shortages.

These disruptions have led to production delays, increased operational costs, and reduced delivery performance. As part of a broader digital transformation initiative, the CFO and CTO are sponsoring the development of an AI-driven early-warning system that identifies potentially risky purchase orders as early as possible.

The goal is to enable procurement and production teams to:
- escalate critical orders earlier,
- activate alternative suppliers,
- adjust safety stocks proactively,
- reduce the risk of production downtime.

### 2. Your Role as a Forward Deployed AI Engineer
As a Forward Deployed AI Engineer, you collaborate directly with stakeholders across Procurement, Supply Chain, and Production. You are expected to:
- understand business processes and operational bottlenecks,
- analyze the data model and interfacing systems,
- design and prototype AI-driven solutions,
- communicate insights to both technical and executive audiences,
- deliver scalable, production-ready components as part of the Data Intelligence Factory.

The customer provides access to a Supabase database with simplified, SAP-inspired procurement tables. Your task is to build a working prototype that predicts whether a purchase order is “at risk”.

### 3. Objective of the Case Study
Build a machine learning solution that predicts whether a PO is likely to become risky based on:

- PO header information,
- item-level information,
- supplier master data,
- event logs (e.g., delays, escalations, supplier issues),
- free-text descriptions in English and German.

The final model should classify each PO as:
- **0 = not at risk**  
- **1 = at risk**

You will later present your results to a panel consisting of the CFO and CTO.

---

## What was done in recent projects (Build-up of PO_EVENTS)
Procurement specialists communicate regularly with suppliers. For every email interaction, a PO_EVENT is generated, and a conversation summary is stored in the `comment` field. An internal ML model classifies the comment into one of six event types, while the severity of each issue is manually assigned by a human reviewer.

---

### 4. Tasks

#### 4.1 Data Extraction and Understanding
Using the provided REST API:

1. Load the tables **LFA1**, **EKKO**, **EKPO**, and **PO_EVENTS**.
2. Explore and document each dataset’s content and structure.
3. Join the tables to construct a training dataset.
4. Conduct exploratory data analysis (EDA), including:
   - PO volume and distribution,
   - supplier distribution,
   - risk ratio,
   - text field analysis (`header_text`, `item_text`, `comment`),
   - event frequencies and severity patterns,
   - numerical feature distributions.

#### 4.2 Feature Engineering
Design a meaningful feature set combining:

- **Text features**  
  Embedded using a SentenceTransformer model.
- **Numerical features**  
  e.g., quantities, values, lead-time information.
- **Categorical features**  
  e.g., country, document type.

#### 4.3 Machine Learning Model
Train an appropriate supervised classification model to predict PO risk.

#### 4.4 Evaluation
Evaluate the model using suitable metrics.

---

### 5. Business Deliverables (Important)

In addition to the technical implementation, candidates should address business questions, such as:

- What is the estimated financial impact of deploying this solution?  

- Are there specific suppliers, materials, or regions that require particular attention once the system goes live?

- What risks or limitations should the organization be aware of before operationalizing this solution?

These aspects will be relevant for the management-level discussion. 
When answering these questions, please base your analysis on **historical data** rather than hypothetical events.

---

### 6. Final Deliverables

Candidates are expected to submit:

1. **A fully working Google Colab notebook** including:
   - data loading,
   - feature engineering,
   - preprocessing pipeline,
   - model training,
   - evaluation.

2. **A short slide deck (4–6 slides)** for a presentation to the CFO and CTO explaining:
   - the business problem,
   - your proposed AI solution,
   - key design decisions and assumptions,
   - expected business value and impact.  
Please use the slide master provided in our invitation.

**Note:** We understand that such an exercise could theoretically be expanded indefinitely. As guidance, we ask candidates to invest **no more than approximately two days** into the development of their solution and the preparation of the presentation. The evaluation will be based on the results and insights expected from this level of effort.

---

## EKKO
| Field             | Type     | Description                                                                 |
|------------------|----------|-----------------------------------------------------------------------------|
| ebeln            | text     | Purchase order number (unique identifier of PO header)                      |
| lifnr            | text     | Supplier number (vendor ID assigned in SAP)                                 |
| bukrs            | text     | Company code – identifies the legal entity                                   |
| bedat            | date     | Creation date of the purchase order                                          |
| bsart            | text     | Document type (e.g., NB – standard PO)                                       |
| release_indicator| bool     | Whether the PO is released/approved                                          |
| header_text      | text     | Free-text PO header notes                                                    |

## EKPO
| Field     | Type     | Description                                                                  |
|-----------|----------|------------------------------------------------------------------------------|
| ebeln     | text     | Purchase order number (links to `ekko`)                                      |
| ebelp     | int4     | PO item number (line number within the PO)                                   |
| matnr     | text     | Material number (identifier of the ordered material)                         |
| menge     | numeric  | Ordered quantity                                                             |
| eindt     | date     | Delivery date requested                                                       |
| price     | numeric  | Net price of the item                                                         |
| item_text | text     | Item-level text (e.g., description or notes)                                 |

## LFA1
| Field     | Type    | Description                                                                  |
|-----------|---------|------------------------------------------------------------------------------|
| lifnr     | text    | Supplier number (primary key, links to `ekko.lifnr`)                         |
| name1     | text    | Supplier name (company name)                                                 |
| country   | bpchar  | Country code (ISO-style, e.g., DE, US)                                       |
| risk_score| int4    | Custom risk attribute (e.g., 1–10 or risk categories)                        |

## PO_EVENTS
| Field      | Type      | Description                                                                 |
|------------|-----------|-----------------------------------------------------------------------------|
| id         | int8      | Unique event identifier                                                      |
| ebeln      | text      | Related purchase order number (links to `ekko`)                              |
| event_ts   | timestamp | Timestamp of the communication event                                         |
| event_type | text      | ML-classified event category (e.g., delay, confirmation, cancellation etc.) |
| severity   | int4      | Manually assigned severity level of the issue (ranking)                     |
| comment    | text      | Conversation summary extracted from email text                               |
