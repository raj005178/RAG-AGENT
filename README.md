# RAG-Agent: Multi-Agent Retrieval-Augmented Generation System

## Overview

RAG-Agent is a Retrieval-Augmented Generation (RAG) pipeline built using LangGraph, ChromaDB, Sentence Transformers, and open-source LLMs. The project is designed to process unstructured documents, retrieve relevant context using semantic search, and generate structured responses through a multi-agent workflow.

The notebook demonstrates:

* Document ingestion and preprocessing
* Text chunking strategies
* Embedding generation using Sentence Transformers
* Vector storage using ChromaDB
* Semantic retrieval pipeline
* Multi-agent orchestration using LangGraph
* Tree-of-Thought style drafting and synthesis
* Final answer generation with review and evaluation

---

# Features

## Document Support

The system supports multiple file formats:

* PDF
* DOCX
* TXT

## Retrieval Pipeline

* Semantic similarity search
* Embedding-based retrieval
* Top-k document matching
* Chunk overlap for contextual continuity

## Multi-Agent Workflow

The LangGraph workflow contains specialized agents:

| Agent         | Responsibility                      |
| ------------- | ----------------------------------- |
| Query Planner | Breaks user queries into subqueries |
| Retriever     | Retrieves relevant document chunks  |
| Writer        | Generates intermediate drafts       |
| Synthesizer   | Combines retrieved evidence         |
| Reviewer      | Reviews and improves final output   |

## Vector Database

* Persistent ChromaDB storage
* Local vector persistence
* Fast semantic querying

## Open-Source LLM Support

The notebook attempts to load multiple open-source instruction-tuned models:

* Meta Llama 3 8B Instruct
* HuggyLlama 7B
* Mistral 7B Instruct

---

# Tech Stack

| Technology                | Purpose                        |
| ------------------------- | ------------------------------ |
| Python                    | Core development language      |
| LangGraph                 | Multi-agent orchestration      |
| ChromaDB                  | Vector database                |
| Sentence Transformers     | Embedding generation           |
| FAISS                     | Similarity search acceleration |
| Hugging Face Transformers | LLM inference                  |
| PyPDF                     | PDF text extraction            |
| python-docx               | DOCX parsing                   |
| Google Colab              | Execution environment          |

---

# Project Architecture

```text
                +----------------+
                | User Query     |
                +--------+-------+
                         |
                         v
                +----------------+
                | Query Planner  |
                +--------+-------+
                         |
                         v
                +----------------+
                | Retriever      |
                +--------+-------+
                         |
                         v
                +----------------+
                | Writer Agent   |
                +--------+-------+
                         |
                         v
                +----------------+
                | Synthesizer    |
                +--------+-------+
                         |
                         v
                +----------------+
                | Reviewer       |
                +--------+-------+
                         |
                         v
                +----------------+
                | Final Answer   |
                +----------------+
```

---

# Installation

## Download the Repository

Download the raw file in the main branch  

## Install Dependencies

```bash
pip install -q langgraph chromadb sentence-transformers pypdf python-docx transformers accelerate safetensors faiss-cpu --upgrade
```

---

# Required Libraries

```python
langgraph
chromadb
sentence-transformers
pypdf
python-docx
transformers
accelerate
safetensors
faiss-cpu
numpy
pandas
scikit-learn
```

---

# File Processing Pipeline

## Supported Loaders

The notebook contains utility functions for loading:

```python
load_pdf(path)
load_docx(path)
load_txt(path)
```

## Chunking Strategies

Two chunking approaches are implemented:

### Sentence-Based Splitting

Splits text using sentence boundaries.

### Fixed-Length Chunking

Uses:

* Configurable chunk size
* Configurable overlap
* Sequential slicing

Example:

```python
chunk_fixed(text, chunk_size=1000, overlap=150)
```

---

# Embedding Model

The project uses:

```python
all-MiniLM-L6-v2
```

via Sentence Transformers.

Purpose:

* Dense vector embedding generation
* Semantic similarity comparison
* Fast retrieval

---

# Vector Database Setup

```python
client = chromadb.PersistentClient(path="./chroma_store")
```

Collection:

```python
langgraph_docs
```

The database persists embeddings locally for reuse across sessions.

---

# Retrieval System

## Retrieval Function

```python
retrieve(query, k=5)
```

### Workflow

1. Encode query into embedding vector
2. Search ChromaDB collection
3. Return top-k matching chunks
4. Provide metadata and similarity scores

---

# Multi-Agent Workflow with LangGraph

## State Definition

The workflow uses a shared graph state:

```python
class GState(TypedDict):
```

State includes:

* Query
* Subqueries
* Retrieved evidence
* Draft responses
* Final answer
* Metrics

## Nodes

### QueryPlanner

Breaks complex questions into smaller semantic tasks.

### Retriever

Retrieves relevant context from vector database.

### Writer

Generates candidate responses using retrieved evidence.

### Synthesizer

Combines multiple drafts into a coherent answer.

### Reviewer

Performs final refinement and quality checks.

---

# LangGraph Execution Flow

```python
graph = StateGraph(GState)
```

Execution sequence:

```text
START
  → QueryPlanner
  → Retriever
  → Writer
  → Synthesizer
  → Reviewer
  → END
```

---

# Running the Notebook

## Step 1: Upload Documents

Use the Google Colab upload widget:

```python
uploaded = files.upload()
```

## Step 2: Process Files

The system:

* Extracts text
* Chunks documents
* Generates embeddings
* Stores vectors in ChromaDB

## Step 3: Ask Questions

Provide a natural language query:

```python
initial_state = {
    "query": "Your question here"
}
```

## Step 4: Execute Graph

```python
result_state = graph.invoke(initial_state)
```

---

# Output Structure

The notebook returns:

```python
{
    "query": ...,
    "final_answer": ...,
    "metrics": ...,
    "evidence_summary": ...
}
```

---

# Example Use Cases

## Educational Research

Query large academic documents and summarize findings.

## Legal Document Analysis

Retrieve relevant clauses from contracts or policies.

## Insurance and Finance

Answer questions from policy documents using semantic retrieval.

## Enterprise Knowledge Base

Create conversational interfaces over internal documentation.

---

# Future Improvements

Potential enhancements:

* Hybrid retrieval (BM25 + semantic search)
* Reranking models
* Streaming response generation
* Citation-aware answers
* Web search integration
* Memory-enabled conversational agents
* Multi-modal document support
* Deployment using FastAPI or Streamlit

---

# Performance Considerations

## Current Strengths

* Modular architecture
* Persistent vector storage
* Flexible agent orchestration
* Open-source model compatibility

## Limitations

* Requires GPU for larger LLMs
* Retrieval quality depends on chunking
* Colab memory constraints for large datasets

---

# Repository Structure

```text
project/
│
├── Soumyadeep_Mondal_NSEC_Internship_25_(1).ipynb
├── chroma_store/
├── README.md
└── requirements.txt
```

---

# Example Queries

```text
Summarize the uploaded document.

What are the key findings from the report?

Explain the policy conditions mentioned in the file.

Retrieve clauses related to insurance claims.
```

---

# Author

Soumyadeep Mondal

Developed as part of an internship project focused on:

* Retrieval-Augmented Generation (RAG)
* Multi-Agent AI Systems
* LangGraph Workflows
* Semantic Search Pipelines

---




