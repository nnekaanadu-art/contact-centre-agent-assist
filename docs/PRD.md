# Product Requirements Document: Contact Centre Agent Assist

**Product:** Contact Centre Agent Assist  
**Product Type:** AI Proof of Concept  
**Product Lifecycle Stage:** Build & Validate  
**Author:** Nneka Anadu  
**Original Date:** November 1, 2026  
**Last Updated:** November 13, 2026  
**Status:** Prototype evaluated; additional validation planned

## 1. Problem and Product Summary

Contact centre agents spend time after customer interactions preparing notes, identifying follow-up actions, and determining whether complaints or potential customer vulnerabilities require additional attention.

These activities can be inconsistent, particularly in high-volume environments.

Contact Centre Agent Assist explores whether generative AI can support after-call work by analyzing completed transcripts and producing five structured outputs:

1. Call summary
2. Customer intent
3. Complaint flag
4. Potential vulnerability flag
5. Next best action

All outputs are recommendations intended for human review. The agent remains responsible for confirming the information and determining the appropriate action.

The proof of concept uses Gemini through Google Colab and was evaluated using 20 synthetic transcripts representing interactions with a fictional financial institution, Northbridge Bank.

No real customer information is used.

## 2. Product Objectives

The prototype aims to:

- Explore the feasibility of AI-assisted after-call documentation.
- Evaluate the accuracy of structured AI-generated outputs.
- Identify opportunities to improve complaint and vulnerability detection.
- Test whether prompt refinements improve performance.
- Establish measurable acceptance criteria and human oversight requirements.
- Identify risks and validation needs before any operational pilot.

The prototype does not establish production readiness or demonstrate realized operational efficiency improvements.

## 3. Users and Scope

**Primary user:** Front-line retail banking agent completing after-call work.

**Secondary users:** Team leads, quality assurance analysts, complaints teams, and vulnerable customer support teams.

| In scope | Out of scope |
|---|---|
| Completed text transcripts | Real-time assistance during calls |
| English-language interactions | Speech-to-text and multilingual support |
| 11 approved intent categories | Automatically executing actions in banking systems |
| Structured AI-generated outputs | Autonomous customer decisions |
| Synthetic banking scenarios | Real customer information |
| Prompt evaluation and comparison | Production integrations |

## 4. Success Measures

### Proposed Acceptance Targets

These targets represent product requirements for further validation, not achieved production performance.

| Metric | Measurement | Target |
|---|---|---|
| Complaint recall | Correctly identified complaints divided by all actual complaints | 100% |
| Vulnerability recall | Correctly identified vulnerable cases divided by all actual vulnerable cases | 100% |
| Flag precision | Correct Yes classifications divided by all Yes classifications, separately for each flag | At least 80% |
| Intent accuracy | Matches the answer key | At least 90% |
| Next best action accuracy | Matches the answer key | At least 85% |
| Summary quality | Average reviewer score using a 1–5 rubric | At least 4.0 |
| Output validity | Responses containing all five required fields in valid JSON | 100% |
| Holdout performance | Difference between evaluation and holdout performance for each applicable metric | No more than 10 percentage points lower |

The planned holdout evaluation will use five previously unseen synthetic transcripts.

Because the holdout contains only five cases, each classification error can change accuracy by 20 percentage points. Results should therefore be treated as directional evidence rather than statistically reliable estimates.

Recall is prioritized for complaint and vulnerability detection because missed cases can have more serious consequences than additional agent review.

However, false positives also have operational costs and should be monitored.

### Observed Prototype Results

The following results were recorded during the initial 20-transcript evaluation.

| Measure | Version 1 | Version 2 |
|---|---|---|
| Intent accuracy | 100% | 100% |
| Complaint accuracy | 100% | 100% |
| Vulnerability accuracy | 95% | 95% |
| Next best action accuracy | 100% | 100% |
| All four fields correct | 95% | 95% |
| Average summary score | 4.35 / 5 | 4.50 / 5 |
| Summaries scoring 5 | 10 / 20 | 11 / 20 |

These are observed accuracy results, not substitutes for the proposed recall and precision measures.

### Future Business Measures

A production-oriented pilot would also evaluate:

- Average after-call work time
- Agent acceptance and override rates
- Complaint capture rate
- Referral quality for potential vulnerability cases
- Agent satisfaction and workflow impact

These business outcomes have not been measured by the current prototype.

## 5. AI Output Acceptance Criteria

An output is accepted only when it meets the applicable field requirements and the overall response conforms to the expected JSON structure.

| Field | Acceptance criteria |
|---|---|
| Summary | Three to five sentences covering the reason for the call, actions taken, relevant context, and unresolved matters. Must not introduce unsupported facts or expose full account or card numbers. |
| Intent | Exactly one approved intent label, consistent with the answer key |
| Complaint flag | Yes or No. Yes when dissatisfaction concerns the bank's fees, service, or errors, or the customer asks for a complaint to be recorded. Merchant-only
