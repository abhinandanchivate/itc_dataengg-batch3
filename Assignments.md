# Assignments: Data Engineering Prompt Engineering Case Study

**Companion document:** `Case_Study.md`  
**Suggested duration:** 2–3 hours  
**Working mode:** Individual or teams of 2–3

## Submission instructions

Use only the synthetic data, schemas, metrics, and constraints in `Case_Study.md`. For each assignment, submit your actual prompt, the LLM's output, and a short validation of that output. Do not claim database or Spark jobs were executed unless you genuinely run them in your own lab environment.

### Assignment 1 — Data quality analysis with a structured prompt

1. Write a **baseline prompt** asking an AI to review Dataset A for data quality issues. Do not give it a structured template yet.
2. Write an **improved prompt** using: Role, Context, Task, Input Data, Instructions, Constraints, Output Format, and Tone.
3. Instruct the model to identify duplicate transaction IDs, missing customer IDs, negative amounts, failed-status records, and candidate business-rule exceptions.
4. Ask for a table with `Issue | Transaction ID | Observation | Severity | Recommended Action` and a count summary.
5. Which observations are provable from the supplied rows? Which require external business rules?
6. Remove the constraints from your improved prompt and compare the responses. Does the AI assume negative amounts and failed statuses are invalid?
7. Document at least two unsupported claims or risky suggestions that a model could make.

**Deliverable:** Baseline prompt, structured prompt, two AI outputs, comparison and review notes.

### Assignment 2 — SQL query generation

1. Create a prompt directing an LLM to write **MySQL 8 SELECT-only queries** for the supplied `transactions` schema.
2. Request separate queries to find duplicate transaction IDs, rows with missing customer IDs, negative amounts, counts by transaction status, and counts by event date.
3. Require an explanation of every query and the expected result on Dataset A where applicable.
4. How will you prevent the LLM from inventing columns or assuming a primary key that was not defined?
5. Modify your prompt to request PostgreSQL-compatible SQL. Which queries are unchanged, and which, if any, need adaptation?
6. Add test cases for empty tables, NULL transaction IDs, and duplicate IDs with non-identical record contents.
7. Explain why auto-generating `DELETE` statements would be risky in this scenario.

**Deliverable:** Two prompts (MySQL and PostgreSQL), SQL drafts, and a validation checklist.

### Assignment 3 — PySpark data validation

1. Write a prompt asking the LLM to generate PySpark DataFrame API functions for Dataset A's schema.
2. Request separate checks for duplicate transaction IDs, missing customer IDs, negative amounts, total row count, and distinct transaction ID count.
3. Require reusable functions and a consolidated result summary.
4. Add constraints: no Pandas, no automatic deletion of records, and no full-dataset `collect()`.
5. Ask for unit tests covering a valid dataset, duplicates, NULLs, an empty DataFrame, and a negative amount that may represent a reversal.
6. Ask the LLM to explain the performance implications of multiple Spark actions such as `count()` and suggest a more efficient strategy.
7. Review the generated code. Does it handle empty data and schema mismatches? What could fail in production?

**Deliverable:** Prompt, generated PySpark code, test cases, and code-review findings.

### Assignment 4 — Pipeline performance troubleshooting

1. Write a prompt analyzing Dataset B's normal-run and incident-run metrics.
2. Ask the LLM for at least three plausible causes of the performance degradation.
3. Require the model to distinguish observed metrics from hypotheses and confirmed causes.
4. Require a table: `Observation | Hypothesis | Verification Step | Recommended Action | Confidence`.
5. Which Spark UI metrics, logs, or query plans would you request to validate each hypothesis?
6. Update the prompt so the assistant must *not* recommend scaling infrastructure without explaining the supporting evidence.
7. Compare the output from a generic troubleshooting prompt with an evidence-grounded prompt. Which better separates fact from speculation?

**Deliverable:** Two prompt versions, investigation matrix, and output comparison.

### Assignment 5 — Root-cause analysis and self-review

1. Build a prompt to produce a root-cause investigation report using Dataset B.
2. Require three competing hypotheses and evidence for and against each one.
3. Ask the model to specify missing information and tests needed to confirm or reject each hypothesis.
4. Add a self-review instruction: identify unsupported assertions and revise the final conclusion.
5. Which statements can the assistant make with confidence using only the supplied metrics?
6. What would qualify as a confirmed root cause rather than a plausible hypothesis?
7. How would you prevent an injected instruction inside an incident log from overriding the assistant's task constraints?

**Deliverable:** Investigation prompt, draft report, and human-reviewed conclusion.

### Assignment 6 — Structured JSON incident output

1. Create a prompt that returns **JSON only** for the Dataset B incident.
2. Specify these required properties:
   - `pipeline_name` (string)
   - `incident_category` (string)
   - `observations` (array of strings)
   - `possible_causes` (array of strings)
   - `verification_steps` (array of strings)
   - `recommended_actions` (array of strings)
   - `root_cause_confirmed` (boolean)
   - `human_review_required` (boolean)
3. Require the output to preserve the provided numeric facts and not invent logs or root-cause confirmations.
4. Design a JSON Schema or equivalent Pydantic model to validate the response.
5. Test malformed JSON, a missing required field, an incorrect field type, and unsupported invented facts.
6. Is valid JSON sufficient to guarantee a factually accurate report? Explain your answer.

**Deliverable:** JSON-oriented prompt, generated sample output, schema, and validation cases.

### Capstone assignment — Cross-system transaction reconciliation

Using Dataset C, prepare a prompt for an AI assistant helping a senior data engineer investigate the apparent discrepancy between core banking and payment gateway records.

1. Define the AI role, business context, precise task, inputs, constraints, output format, and intended tone.
2. Ask the assistant to identify which additional schemas, keys, grain definitions, date windows, and transaction statuses it needs before performing reliable reconciliation.
3. Require read-only SQL query templates for count checks, duplicate checks, and key-level comparisons. Clearly mark unknown schema details as placeholders.
4. Instruct the LLM not to equate the 5,000-row count difference with missing transactions.
5. Explain why the reported 3,000 duplicate rows and 2,000 NULL customer-ID rows cannot simply be added or subtracted without further evidence.
6. Specify how to account for transaction retries, reversals, and ingestion delays.
7. Request a table separating confirmed observations, unresolved questions, investigation queries, and recommended next steps.
8. Add safeguards preventing fabricated match results, destructive SQL, and unsupported claims about financial settlement.
9. Run a baseline prompt and an improved structured prompt. Compare them for accuracy, completeness, and safety.
10. Deliver a final investigation plan suitable for a human data engineering lead to approve.

**Capstone deliverables:** Final structured prompt, AI response, read-only query templates, verification plan, and baseline-versus-improved comparison.

## Final reflection questions

1. Which prompt components most improved the technical correctness of the results, and why?
2. When does giving the model a SQL schema reduce hallucination?
3. Why is a negative amount not automatically a data quality defect?
4. How should an AI assistant communicate uncertainty in a production incident?
5. What validations should be performed in application code rather than delegated only to a prompt?
6. What information should never be sent to a third-party LLM without appropriate organizational authorization?
7. When should the model ask for more evidence instead of generating a definitive answer?
8. How would you evaluate prompt quality systematically across many pipeline incidents?

## Assessment rubric (100 points)

| Criterion | Points |
|---|---:|
| Role, context, and business understanding | 15 |
| Clear, measurable task definition | 20 |
| Accurate handling of input data and schemas | 15 |
| Constraints and guardrails | 20 |
| Useful output structure | 10 |
| Evidence-based reasoning and uncertainty handling | 10 |
| Verification and testability | 10 |
| **Total** | **100** |

## Submission checklist

- [ ] All prompts are included, including the baseline and improved versions.
- [ ] LLM responses are included and labeled as generated outputs.
- [ ] SQL and PySpark outputs have been reviewed for correctness and safety.
- [ ] Observations and assumptions are clearly distinguished.
- [ ] At least three failure or edge cases have been tested or specified.
- [ ] A capstone investigation plan is included.
- [ ] No unverified database access, Spark execution, or production modification is claimed.
