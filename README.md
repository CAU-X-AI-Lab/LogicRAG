# LogicRAG

**A Logic-Driven Framework for Document Generation with Retrieval-Augmented Generation**

---

## Overview

Retrieval-Augmented Generation (RAG) has become a standard paradigm for knowledge-intensive generation. However, conventional RAG systems usually focus on retrieving semantically relevant content, while paying less attention to the reusable writing logic that governs how professional documents are organized.

LogicRAG addresses this limitation by introducing the **Logic-Driven Document Generation Problem (LDP)**. In LDP, a system must generate a complete document that is not only factually grounded, but also consistent with the writing structure, section order, and data usage patterns observed in sample documents.

To solve LDP, LogicRAG models reusable writing logic as a deterministic finite automaton (DFA). Each state corresponds to a discourse segment with a stable semantic function, and state transitions describe the organizational order among these segments. Given a user query, LogicRAG extracts a task-specific sub-automaton, retrieves state-level data, and generates the final report state by state.

---

## Motivation

Professional financial reports are not simple collections of retrieved facts. They usually follow domain-specific writing conventions, such as:

- reviewing market movements before discussing sector-level drivers;
- analyzing different products or assets in a stable order;
- grounding each section in specific numerical indicators;
- maintaining local coherence across adjacent sections.

Similarity-based retrieval alone often fails to capture these structural regularities. LogicRAG therefore treats document generation as a logic-guided process, where retrieval and generation are both organized around reusable document states.

---

## Method

LogicRAG consists of three main stages:

1. **Document Logic Extraction**  
   The system reads sample reports, extracts state sequences, identifies shared states across reports, and constructs a global DFA representing reusable writing logic.

2. **Query Processing**  
   The user query is embedded and matched against the global state index. LogicRAG selects relevant states and extracts a connected query-specific sub-automaton that covers the matched states.

3. **State-Transition Generation**  
   For each state in the sub-automaton, LogicRAG retrieves the required data, generates the corresponding text segment, summarizes the generated segment, and passes only the brief summary to the next state as local context.

This design transforms RAG from content-centric retrieval into writing-logic-based matching and execution.

---

## Implementation

This repository contains a runnable prototype of the LogicRAG pipeline:

- `document_learner.py`: extracts state sequences from sample reports and builds the global state template.
- `query_processing.py`: matches a user query to the global state index and extracts a query-specific subtree.
- `ifind_data_plugin.py`: retrieves market data from iFinD and binds it to state-level `required_materials`.
- `report_generator.py`: generates the final report along the query-specific state order.
- `logicrag_client.py`: provides a user-friendly one-command client for the full pipeline.

The implementation supports OpenAI-compatible chat APIs, DashScope Bailian embeddings, and iFinD market-data retrieval.

---

## Benchmark Context

The accompanying paper introduces **FinLDP-Bench**, a benchmark for evaluating the Logic-Driven Document Generation Problem in financial domains. It covers multiple financial verticals and evaluates generated reports from both structural and factual perspectives, including fluency, consistency, data accuracy, and text-matching metrics.

---

## Citation

If you find this project useful, please cite:

```bibtex
@article{logicrag2026,
  title={LogicRAG: A Logic-Driven Framework for Document Generation with Retrieval-Augmented Generation},
  author={Anonymous},
  year={2026}
}
```

---

## Experiment Guide

Detailed experiment instructions, including API-key configuration, hyperparameters, dataset paths, and full execution commands, are provided in:

[docs/EXPERIMENT_GUIDE.md](docs/EXPERIMENT_GUIDE.md)
