# Data Engineering Prompt Engineering — Complete Case Study

Hands-on Training Case Study

Case Study Title: AI-Powered Data Pipeline Monitoring, Data Quality Analysis, and Incident Resolution

Industry: Banking / Financial Services\
Target Audience: Data Engineers, Senior Developers, Data Architects, and Data Platform Engineers\
Difficulty: Intermediate to Advanced\
Suggested Duration: 2–3 hours\
Technology: Python, SQL, Apache Airflow, PySpark, and an LLM (Gemini / ChatGPT)

## 1. Business problem statement

A banking organization processes millions of transactions every day through a data engineering pipeline.

The pipeline collects transactions from multiple sources, transforms them, validates data quality, and loads the results into an analytics platform.

One morning, the reporting team identifies the following issues:

- The daily transaction count is lower than expected.
- Some customer IDs are missing.
- Duplicate transactions have appeared.
- Some transaction amounts are negative.
- The pipeline completed 45 minutes later than expected.
- The management dashboard shows incorrect transaction totals.

Business challenge: The data engineering team must identify the problems, determine their causes, recommend solutions, and prepare an incident report using GenAI without exposing sensitive banking data.

## 2. Existing data engineering architecture

Data Sources

Core Banking DB • UPI Transactions • Payment Gateway

Ingestion Layer

Kafka / Airflow / Batch Ingestion

Processing Layer

PySpark / SQL Transformation

Data Quality Validation

Nulls • Duplicates • Reconciliation • Freshness

Data Warehouse

Curated Transaction Tables

Business Intelligence

Reports • Dashboards • Reconciliation

The GenAI assistant will help the data engineer analyze pipeline metadata, draft SQL checks, interpret sample data, generate troubleshooting recommendations, and prepare an incident report.

It will not independently modify production databases, approve financial corrections, or invent root causes.

## 3. Synthetic dataset for the case study

The following data represents a small sample of transactions from a much larger banking pipeline.

| transaction_id | customer_id | amount (₹) | status  | event_date |
| -------------- | ----------- | ---------- | ------- | ---------- |
| TXN1001        | C101        | 5,000      | SUCCESS | 2026-11-05 |
| TXN1002        | C102        | 2,000      | SUCCESS | 2026-11-05 |
| TXN1003        | NULL        | 3,500      | SUCCESS | 2026-11-05 |
| TXN1004        | C104        | -1,200     | SUCCESS | 2026-11-05 |
| TXN1002        | C102        | 2,000      | SUCCESS | 2026-11-05 |
| TXN1005        | C105        | 7,000      | FAILED  | 2026-11-05 |
| TXN1006        | C106        | 4,000      | SUCCESS | 2026-11-05 |

### Observed data quality findings

| Check                     | Result | Interpretation                   |
| ------------------------- | ------ | -------------------------------- |
| Total rows                | 7      | Sample records                   |
| Duplicate transaction IDs | 1      | TXN1002 appears twice            |
| Missing customer IDs      | 1      | TXN1003                          |
| Negative amounts          | 1      | TXN1004 requires investigation   |
| Failed transactions       | 1      | TXN1005; not necessarily invalid |
| Distinct transaction IDs  | 6      | One ID is repeated               |

A negative amount might be legitimate for certain transaction types, such as reversals. Similarly, a failed transaction is not automatically a data quality defect. A well-engineered prompt must instruct the LLM to validate such cases against business rules before classifying them as errors.

## 4. Prompt engineering assignments for data engineers

The case study is divided into six practical labs. Each lab introduces a different prompt engineering skill and produces an output that can be checked.

Learning progression

1. Lab 1 — Identify data quality issues using structured prompts
   
Scenario: Your transaction pipeline produced seven sample records, and the data quality team wants to identify anomalies before loading the data into the warehouse.
Learning objective: Practice Role, Context, Task, Input, Constraints, and Output Format.

Lab 2 — Generate SQL using prompt engineering
25 minutes
Scenario: The data engineer must generate SQL to validate the source table without altering production data.

Schema provided:
CREATE TABLE transactions (
    transaction_id VARCHAR(30),
    customer_id VARCHAR(30),
    amount DECIMAL(15,2),
    status VARCHAR(20),
    event_date DATE
);
4. Lab 3 — Generate PySpark transformations using constraints
5. Lab 4 — Troubleshoot a pipeline using evidence-grounded prompts
6. Lab 5 — Produce a root-cause analysis using multi-step prompts
7. Lab 6 — Generate an incident report using structured JSON output


