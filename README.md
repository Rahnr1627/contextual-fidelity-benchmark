# contextual-fidelity-benchmark

> **A benchmark toolkit for evaluating contextual intent fidelity in large language models.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status: Active Research](https://img.shields.io/badge/Status-Active%20Research-green.svg)]()
[![Target: IEEE Access](https://img.shields.io/badge/Target-IEEE%20Access-orange.svg)]()
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)]()

---

## What This Is

Large language models give confidently wrong answers. Not because they hallucinate facts — but because they misread *what the human actually meant*.

A user asks: *"Can I take ibuprofen if I already took aspirin?"*
The model answers: *"The standard dose of ibuprofen is 400mg every 6 hours."*

Technically correct. Completely wrong for the human asking.

This is **contextual intent misalignment** — and it is the #1 reason AI systems cannot be fully trusted by everyday users, especially those who are not technical experts and cannot spot when the AI got it subtly wrong.

This repository provides:
- A **taxonomy** of contextual intent failure modes in conversational AI
- A **benchmark dataset** of 500 prompts across 5 domains, each labeled by failure mode
- An **evaluation pipeline** to test any LLM against the benchmark
- **Comparative results** across GPT-4o, Claude 3.5 Sonnet, and Gemini 1.5 Pro

This work is the empirical foundation for the paper:
**"Contextual Intent Misalignment in Large Language Models: A Taxonomy of Failure Modes in Enterprise Conversational AI"**
*Rahul Naredla, Independent Researcher — Target: IEEE Access, 2026*

---

## The Problem in Numbers

| Metric | Value |
|--------|-------|
| Enterprise AI market (2025) | $114.87 billion |
| Customer-facing AI spend share | ~39% of enterprise AI |
| Users most affected | Non-native English speakers, non-technical users, accessibility-limited populations |
| Current benchmark gap | No existing taxonomy targets *contextual intent* failure specifically |

---

## Taxonomy of Failure Modes

This benchmark identifies and tests 5 distinct failure modes:

### 1. Lexical Anchoring Failure (LAF)
The model responds to the most prominent keywords while ignoring contextual modifiers that change the user's actual intent.

> *Query:* "What should I NOT do after taking metformin?"
> *Failure:* Model describes what metformin is, not what to avoid.

### 2. Pragmatic Intent Failure (PIF)
The model interprets the literal meaning correctly but misses what the user actually needs as an outcome.

> *Query:* "Is this contract clause standard?"
> *Failure:* Model describes the clause instead of comparing it to industry norms.

### 3. Cultural-Linguistic Frame Failure (CLFF)
Queries with non-standard syntax, indirect phrasing, or cultural idioms produce responses calibrated to standard American English conventions — systematically disadvantaging diverse users.

> *Query:* "My manager is saying I should give notice but I don't want to leave. What to do?"
> *Failure:* Model gives resignation advice instead of addressing the underlying workplace conflict.

### 4. Expertise Asymmetry Failure (EAF)
The model assumes an expertise level that does not match the user, producing responses that are too technical or too simple without signaling the mismatch.

> *Query:* "How does RAG work?" (asked by a non-technical HR manager)
> *Failure:* Model launches into vector embeddings and cosine similarity without context.

### 5. Emotional Context Failure (ECF)
The model ignores affective signals — distress, urgency, confusion — and responds to informational content alone, producing technically accurate but contextually inappropriate responses.

> *Query:* "I keep getting errors and I have a demo in an hour please help"
> *Failure:* Model gives a generic debugging guide instead of prioritizing the fastest fix.

---

## Repository Structure

```
contextual-fidelity-benchmark/
│
├── data/
│   ├── benchmark_v1.json              # 500 prompts with labels and metadata
│   ├── domains/
│   │   ├── healthcare.json            # 100 healthcare information prompts
│   │   ├── financial_compliance.json  # 100 financial/compliance prompts
│   │   ├── hr_policy.json             # 100 HR policy prompts
│   │   ├── it_support.json            # 100 IT support prompts
│   │   └── legal_procedure.json       # 100 legal procedure prompts
│   └── annotations/
│       └── human_eval_sample.json     # Human-annotated subset (100 prompts)
│
├── evaluation/
│   ├── run_benchmark.py               # Main evaluation script
│   ├── llm_judge.py                   # LLM-as-judge scoring pipeline
│   ├── scoring_rubric.json            # Evaluation criteria per failure mode
│   └── results/
│       ├── gpt4o_results.json
│       ├── claude35_results.json
│       └── gemini15_results.json
│
├── taxonomy/
│   ├── taxonomy_v1.md                 # Full taxonomy with definitions and examples
│   └── annotation_guide.md           # Instructions for human annotators
│
├── analysis/
│   ├── statistical_analysis.py        # Inter-rater reliability, regression models
│   └── visualizations.py             # Result charts and tables
│
├── paper/
│   └── preprint.pdf                   # Preprint (available after submission)
│
├── requirements.txt
├── LICENSE
└── README.md
```

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/rahulnaredla/contextual-fidelity-benchmark.git
cd contextual-fidelity-benchmark

# Install dependencies
pip install -r requirements.txt

# Set your API keys
export OPENAI_API_KEY=your_key
export ANTHROPIC_API_KEY=your_key
export GOOGLE_API_KEY=your_key

# Run benchmark against one model and domain
python evaluation/run_benchmark.py --model gpt-4o --domain healthcare --n 100

# Run full evaluation across all models and domains
python evaluation/run_benchmark.py --model all --domain all
```

---

## Benchmark Prompt Schema

Each prompt in the dataset follows this structure:

```json
{
  "id": "hc_001",
  "domain": "healthcare",
  "prompt": "Can I take ibuprofen if I already took aspirin?",
  "failure_mode": "LAF",
  "user_expertise": "novice",
  "linguistic_complexity": "low",
  "emotional_valence": "neutral",
  "ground_truth_intent": "drug interaction risk assessment",
  "notes": "Surface keywords suggest dosage query; actual intent is safety/interaction"
}
```

---

## Evaluation Metrics

| Metric | Description |
|--------|-------------|
| **CIF Score** | Contextual Intent Fidelity — primary metric (0–1 scale) |
| **FM-Accuracy** | Failure mode classification accuracy per category |
| **IRR (Cohen's κ)** | Inter-rater reliability across human annotators |
| **Per-domain breakdown** | CIF scores across all 5 query domains |
| **Per-model comparison** | GPT-4o vs Claude 3.5 Sonnet vs Gemini 1.5 Pro |

---

## Project Status

| Component | Status |
|-----------|--------|
| Taxonomy v1 | ✅ Complete |
| Benchmark dataset (500 prompts) | 🔄 In progress |
| Evaluation pipeline | 🔄 In progress |
| Model evaluation runs | ⏳ Planned — Month 3 |
| Human annotation (100 sample) | ⏳ Planned — Month 3 |
| Statistical analysis | ⏳ Planned — Month 4 |
| Paper submission (IEEE Access) | ⏳ Target — Month 5 |
| Dataset public release | ⏳ On submission |

---

## Citing This Work

If you use this benchmark in your research, please cite:

```bibtex
@article{naredla2026contextual,
  title   = {Contextual Intent Misalignment in Large Language Models:
             A Taxonomy of Failure Modes in Enterprise Conversational AI},
  author  = {Naredla, Rahul},
  journal = {IEEE Access},
  year    = {2026},
  note    = {Under review}
}
```

---

## Related Work

This benchmark builds on and extends:
- [LMSYS-Chat-1M](https://huggingface.co/datasets/lmsys/lmsys-chat-1m) — large-scale real-world LLM conversations
- [WildChat](https://wildchat.allen.ai/) — diverse user interaction dataset
- [TruthfulQA](https://github.com/sylinrl/TruthfulQA) — LLM truthfulness benchmark
- [HaluEval](https://github.com/RUCAIBox/HaluEval) — hallucination evaluation benchmark

**Key distinction:** Unlike existing benchmarks that focus on factual hallucination, this work specifically targets *contextual intent failure* — a distinct and underexplored failure mode with direct implications for AI systems serving diverse, non-expert human populations.

---

## Contributing

Contributions are welcome. Priority areas:
- Additional prompts in underrepresented domains (education, accessibility)
- Non-English prompt variants for CLFF failure mode testing
- Evaluations of additional models (Llama 3, Mistral, Command R+)
- Human annotation contributions (see `taxonomy/annotation_guide.md`)

Please open an issue before submitting a pull request.

---

## Author

**Rahul Naredla**
Independent Researcher
MS Information Technology, University of Central Missouri
Jacksonville, FL

[Email](mailto:rahul.nr1627@gmail.com)

---

## License

MIT License — see [LICENSE](LICENSE) for details.
Dataset annotations released under CC BY 4.0.
