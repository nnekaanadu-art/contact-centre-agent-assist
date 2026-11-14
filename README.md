# Contact Centre Agent Assist | AI Proof of Concept

**An AI-powered contact centre proof of concept that explores how generative AI can support agents with call summaries, customer intent identification, risk signals, and recommended next actions.**

**Product Lifecycle Stage:** Build & Validate

**[Open Interactive Notebook in Google Colab](https://colab.research.google.com/github/nnekaanadu-art/contact-centre-agent-assist/blob/main/agent_assist.ipynb)**

## Overview

Contact centre agents must document customer interactions, identify important customer needs, and determine appropriate follow-up actions, often while managing high call volumes.

I developed Contact Centre Agent Assist to explore how generative AI could reduce manual after-call work while supporting consistent identification of customer needs and situations requiring additional attention.

The proof of concept uses Google's Gemini model to analyze fictional banking call transcripts and generate structured outputs for agent review.

The project demonstrates my approach to AI product experimentation, measurable validation, iterative improvement, and responsible AI design.

## The Problem

Contact centre operations face several challenges:

- Agents spend valuable time preparing summaries after customer calls.
- Customer intent and follow-up requirements may not be captured consistently.
- Complaints and potential customer vulnerability signals can be overlooked.
- Inconsistent documentation can affect follow-up quality.
- AI-generated recommendations require validation before they can be trusted in operational workflows.

## The Solution

An AI-assisted workflow that analyzes a call transcript and produces five structured outputs.

### Key Capabilities

**Call Summarization**

Generate a concise summary of the customer interaction to support after-call documentation.

**Customer Intent Identification**

Identify the primary reason the customer contacted the organization.

**Complaint Detection**

Flag interactions in which the customer expresses a complaint.

**Potential Vulnerability Detection**

Identify situations that may require additional sensitivity, support, or human review.

**Next Best Action**

Recommend an appropriate follow-up action for the agent to review.

These outputs are intended to support agent judgment, not replace it.

## How the Proof of Concept Was Tested

I created 20 synthetic call transcripts representing interactions with a fictional bank.

For each transcript, I developed an answer key containing the expected intent, complaint flag, vulnerability flag, and next best action.

The Gemini-generated outputs were compared against the answer key to evaluate performance.

Summary quality was assessed separately using a five-point scoring scale.

### Prompt Iteration

**Version 1:** Initial instructions used to generate structured call analysis.

**Version 2:** Updated instructions with clearer definitions of complaints and vulnerable customers.

The complete 20-transcript test was repeated after the prompt changes to assess whether the revised instructions improved overall performance or introduced new errors.

## Evaluation Results

| Measure | Version 1 | Version 2 |
|---|---|---|
| Intent accuracy | 100% | 100% |
| Complaint accuracy | 100% | 100% |
| Vulnerability accuracy | 95% | 95% |
| Next best action accuracy | 100% | 100% |
| All four fields correct | 95% | 95% |
| Average summary score (out of 5) | 4.35 | 4.50 |
| Summaries scoring 5 | 10 of 20 | 11 of 20 |

*Results are based on 20 synthetic transcripts and should not be interpreted as production-level accuracy.*

## Key Findings

**1. High accuracy on a small, controlled dataset**

Both versions matched the expected results across all four structured fields for 19 of the 20 transcripts.

This demonstrates promising initial performance but does not establish reliability on real-world contact centre interactions.

**2. Prompt changes introduced a measurable trade-off**

Version 2 corrected the vulnerability classification for transcript T15, involving a stolen wallet.

However, it introduced a new vulnerability classification error for T03, involving a customer deceived by a scam text.

Overall vulnerability accuracy remained at 95%.

This demonstrates why prompt changes require full regression testing rather than testing only the scenario a change was intended to fix.

**3. Error type matters, not just accuracy**

Both vulnerability classification errors were false positives rather than missed vulnerability cases.

In this use case, false positives may create additional agent review work, while false negatives could result in customers who need additional support being overlooked.

The evaluation therefore needs to consider the operational consequences of different error types, not accuracy alone.

**4. Summary quality improved**

Average summary quality increased from 4.35 to 4.50 out of 5.

The number of summaries receiving a score of 5 increased from 10 to 11.

Version 2 captured additional important details in some summaries, although further validation is needed.

## Product Approach

The proof of concept reflects several AI product management principles:

1. **Problem-led AI adoption:** Begin with a specific operational challenge rather than introducing AI without a defined use case.
2. **Measurable success:** Establish expected outputs and evaluation criteria before testing.
3. **Iterative validation:** Compare prompt versions using a consistent test dataset.
4. **Risk-based evaluation:** Consider the consequences of false positives and false negatives.
5. **Human oversight:** Position AI-generated summaries, flags, and recommendations as inputs for agent review.
6. **Transparent limitations:** Distinguish proof-of-concept results from production readiness.
7. **Responsible AI:** Document risks, human review points, and failure handling as part of product requirements.

## Product Requirements and AI Governance

The [Product Requirements Document](docs/PRD.md) provides additional detail on:

- Product objectives and success measures
- AI output acceptance criteria
- Human review and oversight
- Failure handling
- AI-related risks mapped to the NIST AI Risk Management Framework

The PRD complements the technical proof of concept by documenting the intended product behavior and operational safeguards.

## Technology and Development

This is an independent portfolio proof of concept developed through AI-assisted prototyping and iterative product direction.

**Tools and Technologies:**

- Python
- Google Colab
- Gemini API (free tier)
- Google Sheets

The development process included creating synthetic test data, defining expected outputs, testing structured AI responses, comparing prompt versions, and documenting results.

The focus is on demonstrating AI product discovery, evaluation, and validation rather than building a production contact centre platform.

## Repository Contents

| File | Description |
|---|---|
| `agent_assist.ipynb` | Google Colab notebook used to run the proof of concept |
| `data/results_v1.csv` | Initial evaluation results |
| `data/results_v2.csv` | Version 2 transcripts, expected outputs, generated results, and accuracy |
| `data/prompt_versions.csv` | Changes made to Gemini instructions between versions |
| `docs/PRD.md` | Product requirements, acceptance criteria, and AI risk considerations |

## How to Explore

1. Open the [Google Colab notebook](https://colab.research.google.com/github/nnekaanadu-art/contact-centre-agent-assist/blob/main/agent_assist.ipynb).
2. Review the notebook instructions and sample transcripts.
3. Review the evaluation results for Versions 1 and 2.
4. Compare prompt changes and the resulting classifications.
5. Read the [Product Requirements Document](docs/PRD.md) for the product design and governance considerations.

Running the notebook may require access to the Gemini API and appropriate configuration.

## Data and Privacy

All call transcripts were created specifically for this proof of concept.

The dataset is synthetic and represents fictional customer interactions.

No real customer or employer data is used.

The project is not connected to a live contact centre, customer account system, or banking platform.

## Limitations

- Evaluation used only 20 synthetic transcripts.
- Real customer interactions may contain longer conversations, incomplete information, ambiguity, and unpredictable language.
- With 20 test cases, one classification error changes the measured accuracy by five percentage points.
- The evaluation dataset was used during prompt refinement, limiting its usefulness as an independent measure of generalization.
- The Gemini free tier occasionally returned HTTP 503 service-busy errors, requiring retries.
- Results do not establish production reliability, operational readiness, or regulatory compliance.

## Future Enhancements

Potential next steps include:

- Refining vulnerability definitions to address scam-related classification errors.
- Repeating regression tests across all existing transcripts.
- Evaluating performance on a separate, previously unseen test dataset.
- Expanding the range and complexity of synthetic call scenarios.
- Introducing more detailed error analysis and evaluation metrics.
- Exploring an agent-facing interface with explicit human review and approval.
- Assessing operational performance, latency, and failure handling under realistic conditions.

These are proposed enhancements and are not represented as currently implemented functionality.

## Disclaimer

Contact Centre Agent Assist is an independent proof of concept created for educational, experimentation, and portfolio purposes.

All customer interactions are fictional.

The reported evaluation results reflect a limited synthetic dataset and should not be interpreted as validated production performance.

The proof of concept is not affiliated with any employer or financial institution and is not intended for live customer servicing or automated customer decisions.

---

**Product Thinking in Practice**

[Portfolio Home](https://nnekaanadu-art.github.io/) | [LinkedIn](https://www.linkedin.com/in/NnekaAnadu) | [GitHub](https://github.com/nnekaanadu-art)

© 2026 Nneka Anadu. All rights reserved.
