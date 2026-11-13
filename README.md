# Contact Centre Agent Assist (AI Proof of Concept)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nnekaanadu-art/contact-centre-agent-assist/blob/main/agent_assist.ipynb)

## The Problem
After each call, contact centre agents spend time writing summaries and deciding what to do next. Important signals, such as a complaint or a customer in a vulnerable situation, can be missed when agents are busy.

## What This Proof of Concept Does
For each call transcript, the proof of concept uses Gemini to produce:
- **Summary:** a short description of the call
- **Intent:** the main reason the customer called
- **Complaint flag:** whether the customer expressed a complaint
- **Vulnerability flag:** whether the customer may be in a vulnerable situation
- **Next best action:** the recommended next step for the agent

## How It Was Tested
I created 20 sample call transcripts for a fictional bank and wrote an answer key with the expected result for each one. The proof of concept's outputs were compared to the answer key to measure accuracy, and each summary was scored on a scale of 1 to 5. Version 2 added clear definitions of a complaint and a vulnerable customer to the instructions given to Gemini, and the full test was run again.

## Results
| Measure | Version 1 | Version 2 |
|---|---|---|
| Intent | 100% | 100% |
| Complaint | 100% | 100% |
| Vulnerability | 95% | 95% |
| Next best action | 100% | 100% |
| All four fields correct | 95% | 95% |
| Average summary score (out of 5) | 4.35 | 4.50 |
| Summaries scoring 5 | 10 of 20 | 11 of 20 |

## Key Findings
- The proof of concept matched the answer key on 19 of 20 transcripts across all four fields in both versions.
- The same overall score hid a trade-off. Adding the vulnerability definition fixed one error (T15, a stolen wallet, was no longer flagged as vulnerable) but introduced a new one (T03, a customer tricked by a scam text, was now flagged as vulnerable). This shows why every change needs a full retest, not just a check of the problem it was meant to fix.
- Both vulnerability errors were false alarms rather than missed vulnerable customers. For this use case, that is the safer type of error, because a missed vulnerable customer could lead to harm, while a false alarm costs an agent a few seconds to review.
- Summary quality improved in Version 2, with more summaries including key details that Version 1 left out.

## Product Requirements
The [product requirements document](docs/PRD.md) covers success measures, acceptance criteria for AI output, human review points, failure handling, and AI risks mapped to the NIST AI Risk Management Framework.

## Tools
Google Colab | Gemini API (free tier) | Google Sheets | Python

## Files
- `agent_assist.ipynb`: the notebook that runs the proof of concept
- `data/results_v2.csv`: transcripts, answer key, Gemini outputs, and accuracy for Version 2
- `data/results_v1.csv`: results from the first version
- `data/prompt_versions.csv`: how the instructions to Gemini changed between versions
- `docs/PRD.md`: one-page product requirements document

## Data
All transcripts are synthetic and were created for this proof of concept. No real customer or company data is used.

## Limitations and Next Steps
- Tested on a small sample of 20 transcripts that were written for this test; real calls are longer and less predictable, so accuracy would likely be lower
- With only 20 transcripts, a single error changes a score by 5 percentage points
- The free Gemini tier sometimes returns a "service busy" (503) error; failed calls were retried
- Next step: refine the vulnerability definition so it handles scam cases like T03 correctly, then retest all 20 transcripts
- Next step: test on a separate set of new transcripts the prompt has never seen, to check the results hold up

---

**Created by Nneka Anadu**

[View My Product Leadership Portfolio](https://nnekaanadu-art.github.io/) | [Connect on LinkedIn](https://www.linkedin.com/in/NnekaAnadu)


© 2026 Nneka Anadu. All rights reserved.
