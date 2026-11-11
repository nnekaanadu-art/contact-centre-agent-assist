# PRD: Contact Centre Agent Assist Prototype

Nneka Anadu · November 1, 2026

## Problem and summary

Contact centre agents spend several minutes after each call writing notes and choosing next steps, and complaints and vulnerable customers can be missed on busy days. The agent assist prototype reads a call transcript and returns five outputs for the agent: a summary, an intent, a complaint flag, a vulnerability flag, and a next best action. Every output is a recommendation; the agent always makes the final decision.

This is a learning prototype built with free tools (Google AI Studio on the Gemini free tier, Google Colab, Google Sheets). It is tested on 20 fictional transcripts for a fictional bank, Northbridge Bank. A five call holdout set of new transcripts is planned as the next step. No real customer data is used.

## Users and scope

The primary user is a front line retail banking agent completing after call work. Secondary users are team leads and quality assurance (QA) analysts who review outputs, and the complaints and vulnerable customer teams who receive referrals.

| In scope | Out of scope |
| --- | --- |
| After call use on completed text transcripts | Real time suggestions during the call |
| English language calls | Speech to text and other languages |
| 11 approved intents and their next best actions | Any action taken automatically in bank systems |
| Fictional data only | Real customer data or production integration |

## Success metrics

The prototype succeeds when it meets every target below on the 20 call evaluation set and holds within 10 points on the holdout set. Targets are proposed starting points; confirm them after the v1 baseline. With 20 calls, one miss moves accuracy by 5 points.

| Metric | How it is measured | Target |
| --- | --- | --- |
| Complaint recall | Share of answer key complaints the model flags | 100% |
| Vulnerability recall | Share of answer key vulnerable customers the model flags | 100% |
| Flag precision | Share of Yes flags that are correct (each flag) | 80% or higher |
| Intent accuracy | Matches answer key | 90% or higher |
| Next best action accuracy | Matches answer key | 85% or higher |
| Summary quality | Average reviewer score, 1 to 5 rubric | 4.0 or higher |
| Output validity | Responses that parse with all five fields | 100% |
| Holdout gap | Evaluation score minus holdout score, per field | 10 points or less |

Recall is weighted above precision for both flags because a missed complaint or vulnerable customer is a regulatory and customer harm, while a false flag costs the agent a few seconds. In production, the business measures would be after call work time, agent acceptance rate of suggestions, and complaint capture rate; these cannot be measured in the prototype.

## Acceptance criteria for AI output

An output is accepted only when all five fields meet the criteria below and the response is valid JSON.

| Field | Acceptance criteria |
| --- | --- |
| Summary | 3 to 5 sentences. Covers the reason for the call, the actions the agent took (including whether a complaint was recorded), relevant context such as a repeat issue, and what remains open. States no fact that is not in the transcript. Contains no full card or account numbers. |
| Intent | Exactly one of the 11 approved intent labels. Matches the answer key. |
| Complaint flag | Yes or No only. Yes when the customer expresses dissatisfaction with the bank itself (fees, service, or errors) or asks for a complaint to be recorded. Frustration with a merchant alone is No. |
| Vulnerability flag | Yes or No only. Yes when the customer mentions something that may reduce their ability to manage their finances: bereavement, illness, age related confusion, job loss, or being targeted by a scam. |
| Next best action | Exactly one of the approved actions. Consistent with the intent mapping in the answer key. Phrased as a recommendation, never as a completed action. |

Stability check: the same transcript run three times returns the same intent, flags, and next best action. Summary wording may vary, but every required fact must appear in each run.

## Human review points

A person reviews every output before it affects a customer or a record; the tool never acts on its own.

| Review point | Who | What they do |
| --- | --- | --- |
| Summary | Agent | Reads and edits before saving to case notes. Nothing saves automatically. |
| Next best action | Agent | Confirms or overrides before taking any action. |
| Complaint flag | Agent, then complaints team | Confirms a Yes and logs it through the complaint process. A No never stops an agent from logging a complaint. |
| Vulnerability flag | Agent, then team lead | Confirms a Yes and makes a referral. The flag is never shown or read to the customer. |
| Ongoing quality | QA analyst | Scores a weekly sample against the rubric and reviews every flag the agent overrode. |
| Prompt or model change | Product Owner | Approves release only after the full evaluation and holdout sets are rerun and meet targets. |

## Failure handling

When the AI fails, the agent sees a clear message and completes the work manually; the tool never shows a partial or invalid answer.

| Failure | How it is detected | Response |
| --- | --- | --- |
| Service error, timeout, or free tier rate limit | API error code | Retry once, then show "Assist unavailable". Agent writes notes manually. |
| Invalid JSON or a missing field | Schema check on every response | Retry once, then show no output. |
| Intent or action not on the approved list | Value checked against the list | Discard the value and show "Agent to select". |
| Unclear or multiple issue call | Model returns "unclear" as instructed | Agent selects intent and action. |
| Summary states something not in the transcript | Agent edit, QA sample | Agent corrects it. QA logs the case as a defect for prompt review. |
| Accuracy falls below target after a change | Full evaluation rerun | Block the release and keep the last approved prompt version. |

## Risks and NIST AI RMF mapping

The top risk is a missed complaint or vulnerable customer, so recall targets and agent review carry the most weight. The NIST AI RMF 1.0 organizes risk work into four functions, and this PRD covers each: Govern (owners and release approval), Map (users, scope, context), Measure (metrics and evaluation), and Manage (review points and failure handling). Risk names below come from the NIST Generative AI Profile (NIST AI 600-1).

| Risk | NIST AI 600-1 risk | Main functions | Mitigation |
| --- | --- | --- | --- |
| Complaint or vulnerable customer is missed | Human-AI configuration (over-reliance on a No flag) | Measure, Manage | 100% recall targets. Agents can always log a complaint or referral without a flag. |
| Summary states facts that are not in the call | Confabulation | Measure, Manage | No new facts criterion, agent edit before saving, QA sampling. |
| Agents accept suggestions without checking | Human-AI configuration (automation bias) | Govern, Manage | Outputs worded as recommendations, required confirmation, QA review of overrides. |
| Customer personal data reaches a third party model | Data privacy | Govern, Map | Fictional data only. Production would need redaction, an approved vendor, and Canadian data residency review. |
| Lower accuracy for some customer groups | Harmful bias and homogenization | Map, Measure | Add varied speaking styles to the evaluation set and score results by group. |
| Text in a transcript tries to override the prompt | Information security | Measure, Manage | Schema and approved list checks. Add a test transcript with injected instructions. |
| Free tier model changes without notice | Value chain and component integration | Govern, Manage | Log the model version with each run. Rerun the full evaluation on any change. |
| Results do not hold beyond 20 calls | Not a profile risk; trustworthy characteristic "valid and reliable" | Measure | Test a five call holdout set next. Grow to 100 or more calls before any pilot. |

As of October 2026, NIST notes the AI RMF 1.0 is under revision; recheck the mapping when a new version is published.

Sources: [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework), [NIST AI 600-1 Generative AI Profile (PDF)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf), [Modulos summary of the 12 profile risks](https://docs.modulos.ai/frameworks/nist-ai-rmf/generative-ai-profile).
