---
layout: post
title: "Multi-Agent Research Design in Practice"
date: 2026-01-23 18:00:00
description: How I used Virtual Lab to create fine-tuned AI agents that collaborate on Medicare payment policy analysis
tags: agents research data-engineering
categories: research
featured: true
---

I recently adapted the [Virtual Lab](https://github.com/zou-group/virtual-lab) framework—originally developed for drug discovery and [published in Nature](https://doi.org/10.1038/s41586-025-09442-9) by Swanson, Wu, Bulaong et al.—to study how hospitals respond to Medicare payment shocks.

This post shows how multi-agent collaboration transforms simple research prompts into rigorous econometric designs, why I ultimately skipped fine-tuning for this project, and how this connects to AI-assisted coding tools like Claude Code.

## From Simple Prompt to Rigorous Research Design

**Initial prompt to the agent team:**
> "Analyze changes in CMS Inpatient Prospective Payment System (IPPS) Impact Files. Identify quasi-experimental events related to Wage Index Cliffs and Border Discontinuities to understand how hospitals respond to payment shocks."

**What the agents produced after 5 rounds of discussion:**

**Final Research Question:**
> How do hospitals respond to Wage Index shocks, what role do cost-shifting behaviors play, how does Market Concentration (HHI) moderate these responses, and what are the implications for health equity?

**Final Econometric Specification:**

- **Design**: Staggered-adoption intensity event study using Sun & Abraham estimator
- **Treatment**: Baseline-frozen OMB delineation changes mapped to hospital-level payment shocks
- **Moderation**: HHI interaction terms to test "rent capture" hypothesis
- **Primary Outcome**: Change in wage index wedge (Δwi_wedge) - the cleanest behavioral signal in Impact Files
- **Validity Tests**: Pre-trend tests for both Shock and Shock×HHI, manipulation tests, support checks
- **Inference**: Wild bootstrap clustering at CBSA level

This evolution happened through structured debate among six specialized agents.

## The Agent Team

Each agent brought specific expertise:

1. **Principal Investigator**: Enforced focus on "Impact File-native" variables only
2. **Health Economist**: Designed difference-in-differences specifications
3. **Industrial Economist**: Analyzed market concentration effects, pushed for "rent capture" mechanism tests
4. **Policy Analyst**: Grounded analysis in CMS regulations and Federal Register documentation
5. **Causal Inference Expert**: Rejected standard TWFE under staggered adoption, insisted on Sun & Abraham
6. **Economics Critic**: Flagged when claims exceeded empirical support

## Example: Multi-Agent Debate on Methods

**Industrial Economist's proposal:**
> "Use difference-in-differences and regression discontinuity designs, with propensity score matching."

**Economics Critic's response:**
> "Caution against TWFE under staggered adoption—require Sun & Abraham or Callaway-Sant'Anna with diagnostics. Pre-trend gates must be pre-specified for both Shock and Shock×HHI. Do not over-interpret payment-on-exposure regressions without first-stage validation."

**Causal Inference Expert's synthesis:**
> "Main estimator should use Sun & Abraham interaction-weighted event study. Define time-invariant, baseline-frozen treatment intensity for clean identification. Include formal pre-trend gates for both main effects and interactions. Show nonparametric HHI bins."

**Result:** The team converged on modern causal inference methods, with explicit validity checks and robustness requirements that wouldn't have emerged from a single agent or typical researcher workflow.

## The Fine-Tuning Pipeline: How It Works

Virtual Lab includes a 7-phase pipeline to transform generic LLMs into domain specialists:

### Phase 1-2: Literature Search and Selection

**Agent receives task:**
> "Search PubMed for query 'CMS IPPS impact file policy change quasi-experimental hospital behavior', request 10 abstracts. Rigorously exclude papers about basic science, pure clinical trials without cost data, or non-US healthcare systems. Only select papers with direct relevance to US hospital economics, payment policy, or market structure."

**Example agent response (Industrial Economist):**

```json
[
  {
    "pmcid": "9175183",
    "title": "Adoption and Learning Across Hospitals: The Case of a Revenue-Generating Practice",
    "reason": "Studies hospital response to 2008 Medicare documentation reform
               affecting revenue—directly relevant to understanding how hospitals
               react to payment incentives and inter-hospital learning dynamics."
  }
]
```

The agent correctly filtered out conference abstracts and irrelevant papers, selecting only those studying US hospital behavioral responses to Medicare policy changes.

### Phase 3-4: Summarization and Training Data Generation

Papers are downloaded and summarized with extreme detail. Here's an excerpt from the Industrial Economist's summary of Garmon (2017) on hospital merger screening:

**Summary excerpt:**
> "The paper evaluates whether prospective merger 'screens' computed using pre-merger data actually predict post-merger price changes for 28 consummated hospital mergers. It compares traditional structural measures (HHI under different market definitions) versus 'direct' unilateral-effects measures (diversion ratios, WTP, UPP) and merger simulation... Key findings: Minimum ΔWTP threshold of 6% and UPP threshold of 4% outperform HHI-based screens. HRR-bed HHI produces many false negatives (flags only 4 of 9 significant price-increase mergers), confirming criticism that EH-like broad markets are too large..."

The summary continues for 15+ pages, covering:

- Policy problem and fiscal implications
- Institutional backdrop (why 1990s enforcement failed)
- Technical details of each screening method (WTP, UPP, diversion ratios)
- Data construction and causal design (synthetic control)
- All empirical findings with specific numbers
- Operational recommendations for local agencies

These detailed summaries are then converted to training examples in OpenAI's format:

**Training data structure:**

```json
{
  "messages": [
    {
      "role": "system",
      "content": "You are a Health Economist. Your expertise is in
                  fiscal impact analysis and hospital cost structures..."
    },
    {
      "role": "user",
      "content": "Explain the findings of 'Medicaid Disproportionate Share'
                  regarding hospital policy."
    },
    {
      "role": "assistant",
      "content": "## 1) What the document says UC DSH is *supposed* to do
                  vs. what it *became*

                  ### Original policy purpose...
                  The document frames Medicaid DSH as a supplemental payment
                  intended to help hospitals that 'serve a disproportionate
                  number of low-income patients'... [detailed response]"
    }
  ]
}
```

Each agent gets 50-200 training examples from their curated literature, creating an 80/20 train/validation split.

### Phase 5-7: Fine-Tuning and Evaluation

Training jobs are launched on `gpt-4o-mini`. To evaluate whether fine-tuning improved performance, the system generates synthetic QA pairs from validation papers.

**Example QA pair (from Neprash et al. 2015 on price measurement):**

```json
{
  "question": "What is the main measurement problem Neprash et al. (2015) address?",
  "correctAnswerIndex": 0,
  "options": [
    "How to construct valid, comparable commercial health care
     price measures across geographic markets",
    "How to predict individual patient mortality risk using DRG codes",
    "How to estimate hospital cost-to-charge ratios from Medicare cost reports",
    "How to set optimal Medicaid capitation rates"
  ]
}
```

**Another example on methodology:**

```json
{
  "question": "What is a major drawback of using a single 'sentinel service'
               price as a proxy for overall market prices?",
  "correctAnswerIndex": 0,
  "options": [
    "Sentinel service prices may be unrepresentative of overall market
     pricing and correlate weakly with other services",
    "Sentinel service prices require identical service baskets across all markets",
    "Sentinel service prices cannot be computed from noncapitated claims",
    "Sentinel service prices automatically adjust for case-mix differences"
  ]
}
```

The system generates 3-5 multiple-choice questions per validation paper, testing whether models understand:

- Main research questions and policy problems
- Methodological details and their limitations
- Key empirical findings with specific numbers
- Institutional context and fiscal implications

## Evaluation Results: Base vs Fine-Tuned Models

The evaluation compared base `gpt-4o-mini` against fine-tuned versions on the validation set QA pairs.

**How agents answered domain questions:**

When asked about hospital merger screening methods (from the validation set):

**Base Model Response:**
> "Hospital merger screening typically uses HHI thresholds from the Horizontal Merger Guidelines. Post-merger HHI > 2500 with Δ > 200 raises concerns. More recent approaches incorporate diversion ratios..."

**Fine-Tuned Industrial Economist:**
> "Garmon (2017) shows that minimum ΔWTP threshold of 6% and UPP threshold of 4% outperform HHI-based screens for predicting post-merger price changes. HRR-bed HHI produces false negatives—flagging only 4 of 9 significant price-increase mergers—confirming that EH-like broad markets are too large. The optimal screening pipeline should compute ΔWTP and UPP from discharge-based choice models, using conservative aggregation across models..."

The fine-tuned response included:

- Specific thresholds (6%, 4%) from the literature
- Empirical validation (4 of 9 mergers)
- Methodological critique (EH-like markets too broad)
- Actionable recommendations (discharge-based choice models)

**However, the results were mixed:**

On the hold-out validation set, base `GPT-4o` models often performed as well or better than fine-tuned `gpt-4o-mini` models. The base models' superior general reasoning sometimes compensated for less domain-specific knowledge.

## Why I Skipped Fine-Tuning

Despite implementing the full pipeline, I ran the project with base `GPT-4o` models. Here's why:

**1. Base GPT-4o outperformed fine-tuned gpt-4o-mini**

When evaluating on hold-out papers, base `GPT-4o` produced more balanced responses than fine-tuned `gpt-4o-mini`. The base model's broader reasoning capabilities and training data meant it already had strong health economics knowledge.

**2. Fine-tuning is iterative and incomplete**

The fine-tuning pipeline I implemented is a first pass. Improving performance would require:

- More training examples (currently 50-200 per agent)
- Better prompt engineering in training data
- Iterative refinement based on validation failures
- Potentially fine-tuning larger models (gpt-4o instead of gpt-4o-mini)

**3. Cost-benefit for this research stage**

For exploratory research design, the marginal benefit of fine-tuning didn't justify:

- Training costs (~$3/M tokens)
- Higher inference costs for fine-tuned models
- Maintenance burden (retraining as literature evolves)

## Connecting to AI-Assisted Coding

Virtual Lab and tools like Claude Code serve complementary roles:

### Virtual Lab: Research Design

Agents **design** the research through structured debate:

**Output from longitudinal modeling phase:**
> "Adopt an OMB delineation adoption design as the anchor event. Use Sun & Abraham staggered-adoption estimator with baseline-defined, time-invariant treatment intensity. Primary outcome: Δ(wi_wedge). Include HHI moderation via linear interactions and HHI-bin event studies. Exclude transition years by default unless modeling blend weights explicitly."

### Claude Code: Implementation

Take that specification and **implement** it:

Claude Code writes the implementation based on agent specifications. Agents provide **what** to build; Claude Code handles **how**.

There was some back and forth getting the code to work and explored expanding the time horizon.

### Ideal Workflow

1. **Virtual Lab agents debate design** → Produce specification
2. **Feed spec to Claude Code** → Generate implementation
3. **Claude Code runs analysis** → Produce results
4. **Virtual Lab agents critique** → Identify issues, suggest refinements
5. **Claude Code updates code** → Iterate to final analysis

## Key Lessons

**1. Start with base models, fine-tune only when necessary**

Test strong prompts first. Fine-tune only for specific, measurable deficiencies (usually style/format, not knowledge).

**2. Version control agent prompts**

Agent system prompts are code. Use git, code review, changelogs, and A/B testing.

**3. Validate agent output rigorously**

Agents can be confidently wrong. Always:

- Cross-check citations (agents sometimes hallucinate papers)
- Verify regulatory references against Federal Register
- Test generated code
- Run robustness checks on econometric strategies

**4. Log everything**

Virtual Lab saves all discussions as JSON. This audit trail is invaluable for understanding decisions and debugging recommendations.

**5. Human-in-the-loop for final decisions**

Let agents debate alternatives, but humans make final calls on:

- Research questions
- Data inclusion/exclusion
- Publication-ready claims
- Code deployment

## Looking Forward

I want to take another pass at finetuning agents on literature using this approach and test it.

As frontier models improve, the bottleneck shifts from "Can agents think at this level?" to "How do we structure agent collaboration effectively?"

Virtual Lab provides one answer: assemble specialists, let them debate, critique iteratively. May also explore ways of folding something like this into a custom claude code workflow.

---

**Resources:**

- [Virtual Lab framework](https://github.com/zou-group/virtual-lab) (Zou Group)
- [My presentation on agent tooling and MCP](https://github.com/daltonmaurice/dissc-agent-tooling)
- Hospital Impact Files project (example application to health economics)

**Credits:** Virtual Lab created by Swanson, K., Wu, W., Bulaong, N.L. et al., "The Virtual Lab of AI agents designs new SARS-CoV-2 nanobodies," *Nature* (2025). <https://doi.org/10.1038/s41586-025-09442-9>
