---
title: MP3 Grounded Q&A
description: A cited Sherlock Holmes document assistant with abstention and evaluation
ms.date: 2026-10-24
ms.topic: overview
---

## Project overview

This Mini Project 3 submission extends the supplied Mini-RAG pipeline with
numbered citations, citation validation, retrieval-based abstention, and an
evaluation harness. It uses the five Sherlock Holmes summaries in
`learner/corpus/` and the OpenAI-compatible models configured in the learner
script.

## Setup

From the `learner` directory, create a virtual environment, install dependencies,
and configure credentials:

```bash
cd learner
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Edit `.env` with your API key and Qdrant configuration, then load the variables:

```bash
source .env
```

For the Vocareum OpenAI-compatible endpoint, set `OPENAI_BASE_URL` to
`https://openai.vocareum.com/v1`. The provided example config uses local
persistent Qdrant storage. You can instead configure `QDRANT_URL` and, if needed,
`QDRANT_API_KEY` for a Qdrant server.

## Run and verify

```bash
python mp3_grounded_qa.py selftest
python mp3_grounded_qa.py ingest
python mp3_grounded_qa.py ask
python mp3_grounded_qa.py eval | tee mp3_eval.txt
```

The final evaluation currently covers six answerable questions and three
questions that should abstain. It reports retrieval hit rate, grounded response
rate, abstention accuracy, and hallucination rate in `learner/mp3_eval.txt`.
The script retrieves ten chunks per question because the measured top-three run
found the correct story but sometimes omitted evidence needed to answer.

## Submission files

* `learner/mp3_grounded_qa.py`: implementation of the four required functions
* `learner/mp3_eval.txt`: saved evaluation output
* `learner/data/learner_questions.jsonl`: two answerable questions from different stories, including a multi-hop question, and one abstention question
* `learner/mp3_reflection.md`: reflection draft based on observed results; review and personalize it to match your own observations and course AI-use policy
* A 2–3 minute demo: record or present cited answers and one correct abstention

Secrets, virtual environments, and local Qdrant data are excluded by `.gitignore`.
