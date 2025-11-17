# Agentic RAG using LlamaIndex

This project demonstrates how to build advanced Retrieval-Augmented Generation (RAG) systems, including agent-based RAG, using [LlamaIndex](https://www.llamaindex.ai/). The notebook guides users from setting up a basic RAG pipeline on a custom PDF document to creating an agent that can answer questions using specialized tools (e.g., a document query engine, ArXiV, and Brave Search), combining enterprise AI insights, recent research, and live web data.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Setup & Installation](#setup--installation)
- [Workflow Sections](#workflow-sections)
  - [Part 0: Loading Libraries](#part-0-loading-libraries)
  - [Part 1: Simple RAG Systems](#part-1-simple-rag-systems)
  - [Part 2: Agentic RAG](#part-2-agentic-rag)
- [Key Concepts](#key-concepts)
- [References](#references)

---

## Overview

**Agentic RAG using LlamaIndex** provides a comprehensive introduction and template for state-of-the-art enterprise search and question-answering using unstructured sources (PDFs) and large language models (LLMs). It covers both simple RAG (vector store search over a document) and more sophisticated agentic RAG (AI agent routing queries to the best available search tools).

This notebook uses a real-world report (`state.pdf`), providing valuable context for organizational AI adoption, risks, and trends.

## Features

- Loads and chunks custom PDF documents for semantic search.
- Builds a local vector store and summary index.
- Integrates OpenAI LLMs and embeddings.
- Explores vector store operations and source node inspection.
- Queries the document for direct and contextual answers.
- Defines agent routing logic for summary vs. detailed queries.
- Adds observability with Arize Phoenix for agent trace monitoring.
- Extends agent with multiple tools: ArXiv paper search and Brave (web) search.
- Demonstrates multi-tool answers that combine document context, research, and live web findings.

## Setup & Installation

Clone this repository (or download the notebook) and install necessary dependencies. The notebook uses pip installation cells to ensure all required packages are present:

```python
!pip install llama-index llama-index-vector-stores-chroma llama-index-llms-huggingface-api llama-index-embeddings-huggingface -U -q
!pip install --upgrade datasets
!pip install --upgrade huggingface-hub
!pip install llama-index-callbacks-arize-phoenix arize-phoenix
!pip install llama-index-tools-arxiv llama-index-tools-wikipedia duckduckgo-search
!pip install llama-index-tools-brave-search
```

**API Keys Required:**
- [HuggingFace Token](https://huggingface.co/settings/tokens) (for some models/tools)
- OpenAI API Key (for LLM and embedding model)
- Brave Search API Key (for live web search, optional but recommended)
- Arize Phoenix API Key (for observability)

**Document:**  
You must provide the `state.pdf` file (the demo McKinsey State of AI report) in the notebook's working directory.

## Workflow Sections

### Part 0: Loading Libraries

- Installs required packages (LlamaIndex, transformers, vector stores, arxiv, etc.).
- Sets up API keys and authenticates to Hugging Face.

### Part 1: Simple RAG Systems

- Reads and chunks `state.pdf` using `SimpleDirectoryReader` and `SentenceSplitter`.
- Builds a vector index and query engine.
- Demonstrates vector search for factual Q&A (e.g. "Who is Lareina Yee?").
- Inspects embeddings, node mappings and direct source documentation.

### Part 2: Agentic RAG

- Adds `SummaryIndex` for document summarization.
- Defines multiple query engines and wraps them as tools.
- Creates a router query engine using an LLM selector:
  - Routes summary/overview to summary index.
  - Routes stats/facts to vector search.
- Sets up `AgentWorkflow` with system prompts to enforce tool selection and answer style.
- Integrates Arize Phoenix for tracing and analytics.
- Adds Brave Search and Arxiv tools for web and research Q&A.
- Creates an enhanced agent able to route and combine answers from document, web, and academic sources.
- Shows prompt examples and answers combining all resources.

## Key Concepts

- **RAG (Retrieval-Augmented Generation):**  
  LLM-driven Q&A with real-time context from a vector database of documents.
- **Agentic RAG:**  
  An agent that intelligently chooses tools (summary, details, research, web) to answer, rather than relying on a single retrieval method.
- **Vector Store:**  
  Chunks of document text embedded and indexed for fast semantic search.
- **Source Nodes:**  
  Chunks/sources connected to each answer, improving transparency.
- **Tool Integration:**  
  Extending beyond static documents—integrating academic search (ArXiV), web articles, Wikipedia for more comprehensive, up-to-date answers.

## References

- [LlamaIndex Documentation](https://docs.llamaindex.ai/)
- [McKinsey State of AI Report (demo document)](https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/the-state-of-ai)
- [Arize Phoenix](https://phoenix.arize.com/)
- [OpenAI API](https://platform.openai.com/account/api-keys)
- [Brave Search API](https://api.search.brave.com/)
- [ArXiv API](https://arxiv.org/help/api/user-manual)
- [DuckDuckGo Search](https://github.com/deedy5/duckduckgo-search)

---

## Example Usage

Query your enterprise PDF for summary or details:
```python
response = query_engine.query("Who is Lareina Yee?")
print(response)
```
Use an agent for deeper analysis, multi-source research, and current events:
```python
question = "What are the key risks organizations address with gen AI? Search recent academic research for AI risk mitigation and compare!"
response = await enhanced_agent.run(question)
print(response)
```

---

## Notes

- This notebook is intended for research and demo purposes. For production, ensure proper key/environment management and data privacy.
- Many tools require valid API keys.
- Modify system/tool prompts to tailor agent behavior for your use case.

---

## License

MIT License.  
See [LICENSE](./LICENSE) for details.
