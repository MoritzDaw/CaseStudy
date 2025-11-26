# Procurement Risk Intelligence - AI Case Study
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](
https://colab.research.google.com/github/MoritzDaw/procurementPilot/blob/main/notebooks/00_getting_started.ipynb)

### 1. Business Scenario
A large global industrial manufacturing company operates multiple production facilities across Europe, Asia, and North America. Every year, the organization processes more than 7.000 purchase orders (POs) from a diverse supplier base. The supply chain is complex, with long lead times, fluctuating material availability, and variable supplier performance.

In recent years, the company has experienced increasing supply chain disruptions, including:
- delayed deliveries,
- quality issues,
- non-responsive suppliers,
- unstable pricing and material shortages.

These disruptions have led to production delays, increased operational costs, and reduced delivery performance. As part of a broader digital transformation initiative, the CFO and CTO are sponsoring the development of an AI-driven early-warning system that identifies potentially risky purchase orders as early as possible.

The goal is to enable procurement and production teams to:
- escalate critical orders sooner,
- activate alternative suppliers,
- increase safety stock where necessary,
- reduce the risk of production downtime.

## What was done in recent projects:
The procurement specialists are communicating with the suppliers. For every Mail interaction, a PO_EVENT is created and a conversation summary is stored in the "Comment" field of PO_EVENTS. A different model then classifies the comment to one of six event types. The severity of the problem is then ranked by a human individually. 
Hint: You might want to use the combination of severity and Event Type to label your data set and train your model respectively.

### 2. Your Role as a Forward Deployed AI Engineer
As a Forward Deployed AI Engineer, you collaborate directly with customer stakeholders across Procurement, Supply Chain, and Production. You are expected to:
- understand the business processes and operational bottlenecks,
- analyze the data model and technical systems,
- design and prototype AI-driven solutions,
- communicate insights to both technical and executive audiences,
- deliver scalable, production-ready components as part of the Data Intelligence Factory.
- The customer provides access to a Supabase database with simplified, SAP-inspired procurement tables. Your task is to build a working prototype that predicts whether a purchase order is “at risk”.

### 3. Objective of the Case Study
Build a machine learning solution that predicts whether a PO is likely to become risky based on:

- PO header information,
- item-level information,
- supplier master data,
- event logs (e.g., delays, issues, escalations),
- free-text descriptions in multiple languages (English and German).
- The final model should classify each PO as:
- 0 = not at risk
- 1 = at risk

You will later present your results to a mock panel consisting of the CFO and CTO.

### 4. Tasks
#### 4.1 Data Extraction and Understanding
   
Using the given REST API:
1. Load the tables LFA1, EKKO, EKPO, and PO_EVENTS.
2. Explore each dataset’s content and structure.
3. Join the tables to construct a training dataset.
4. Conduct an exploratory data analysis (EDA), including:
- PO count and distribution,
- supplier distribution,
- risk ratio,
- text field analysis (header_text, item_text),
- event frequencies and severity,
- numerical feature distributions.

#### 4.2 Feature Engineering
You are expected to design a meaningful feature set combining:

- Text features (Text should be embedded using a SentenceTransformer model)
- Numerical features
- Categorical features

#### 4.3 Machine Learning Model
Train a suitable, supervised classification model.

#### 4.4 Evaluation
Evaluate your model using suitable metrics.

#### 5.1 Business Deliverables (!important!)
- How much money can we safe by using your implementation?
- Is there already something we should look out for when the system goes live (e.g. specific vendors performing poorly)
- etc.


#### 6 Final Deliverables
Condadates are expected to submit:

1. A fully working Google Colab notebook containing:
- data loading,
- feature engineering,
- preprocessing pipeline,
- model training,
- evaluation.

2. A short slide deck (5–8 slides) for a mock presentation to the CFO and CTO explaining:
- the business problem,
- your proposed AI solution,
- design decisions based on assumptions about the business,
- business value and expected impact
Use the Slide Master that was provided to you in our invitation.

Remark: Remember that you are talking to Top Level Management. They are more interested in business outcomes than technical details. The CTO might might want to dig into some technical details after the presentation. We are aware that the effort you can put into these tasks are well beyond two days. No matter how much time you will spend on solving these tasks, we will evaluate you by effort worth of exactly two days.

**Best of Luck!**





