# Aryan Somayajula

**AI Engineer — agent reliability, LLM evaluation, production LLM systems.**
London, UK.

I work on the part of AI engineering that doesn't demo well: what agents do when a tool call
half-succeeds, when a model is confident and wrong, and when an action can't be undone. Most of my
public work is a benchmark, a reproduction, or a fix.

[Email](mailto:somayajulaaryan@gmail.com) ·
[LinkedIn](https://www.linkedin.com/in/somayajula-aryan-48852a1b5/) ·
[Paper (DOI)](https://doi.org/10.5281/zenodo.20426804)

---

## Work

### [kiri-gate](https://github.com/aryan597/kiri-gate) — a gate in front of your agent's tools

Acts on what can be undone, asks about what can't. Tools are classified at registration into `READ`,
`UNDOABLE`, `IRREVERSIBLE` and `EXTERNAL`, so no model can authorise an irreversible or external
action on its own. Zero dependencies, ships with an MCP proxy and an approval UI.

```python
@gate.tool(Class.IRREVERSIBLE)
def refund(order_id: str, amount: float) -> str:
    ...
```

### [ask-or-act](https://github.com/aryan597/ask-or-act) — does an agent know when to stop and ask?

A benchmark of 120 labelled decisions across six domains, run against four models from a small local
one to a frontier one. Built on "twins": near-identical action pairs where only the context differs,
so the benchmark measures judgement rather than keyword matching.

The finding: better models are better at *spotting* risk and no better at *avoiding* it. Every model
took irreversible actions it should have escalated. A classification gate stopped all of them, at a
cost of roughly 21 extra questions per 120 actions.

Replicated on an independent set of 571 human-labelled sessions for under $2 of API spend. Known
limitations are documented in the README, including that the labels are one person's judgement.

### [Chaos study](https://github.com/aryan597/kiri-gate/tree/main/experiments/chaos) — a duplicate-payment bug in LangChain's agent runtime

When a tool call succeeds but its response times out, the default retry logic re-ran refunds and
emails on every run, and the model re-issued them unprompted on most of the rest. Deriving an
idempotency key from the tool arguments eliminated both failure modes.

Filed upstream with the evidence:
[langchain#40688](https://github.com/langchain-ai/langchain/issues/40688) ·
[langgraph#8464](https://github.com/langchain-ai/langgraph/issues/8464)

### [site-capability-evaluator](https://github.com/aryan597/site-capability-evaluator) — a production LLM service that behaves the same every time

A stateless HTTP service that decides which capabilities a browser agent needs to test a given site.
Every deterministic decision lives in a pure module with no model dependency, so output is identical
whether it runs on a frontier model or a local 8B. Model responses are treated as untrusted input:
schema validation, defensive parsing, and a CI test that fails the build on any drift from the
OpenAPI 3.1 contract.

The README documents the design decisions and where the prototype cuts corners.

### [nestshift-os](https://github.com/aryan597/nestshift-os) — on-device energy optimisation

Cut projected household energy bills by 26.4%, about £476 a year, across 20 real homes, by pairing a
scheduling algorithm with a demand forecaster trained on real smart-meter data. Verified under
changing conditions with a Monte Carlo simulation across four tariffs. Runs on a Jetson and a
Raspberry Pi with sub-second retraining, on an event-driven MQTT and FastAPI backend.

[Paper](https://doi.org/10.5281/zenodo.20426804) ·
[Model](https://huggingface.co/aryabun/demand-forecaster-lgbm-lcl)

---

## Stack

**Python**, FastAPI, PostgreSQL, Docker, CI/CD
**Agents & LLMs** — LangChain, LangGraph, MCP, RAG, context and prompt engineering, local serving
(LM Studio, Ollama)
**Evaluation** — benchmark design, calibration, fault injection, idempotency
**ML** — LightGBM, XGBoost, scikit-learn, time-series forecasting

---

## How I work

Claims come with the evidence, the limits, and the fallback. When I replicated ask-or-act on an
external benchmark, the first number looked plausible and was wrong — a bug in my own scoring, caught
by reading the models' reasoning instead of trusting the output. That's the habit the rest of this
work rests on.

MSc Data Science & Analytics (Merit), Royal Holloway, University of London.
