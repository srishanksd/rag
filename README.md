# Guidelines
> `git clone "https://github.com/srishanksd/rag"`
> `cd rag`
> `uv venv`
> `.venv\Scripts\Activate.ps1` (for windows)
> `uv sync`



------------------------------------------------------------------------

# Retrieval-Augmented Generation (RAG) Notes

> Beginner to Intermediate notes for building a RAG application.

------------------------------------------------------------------------

# What is RAG?

**RAG = Retrieval + Augmentation + Generation**

Instead of asking an LLM to answer only from its training data, RAG
first searches your own documents and then gives the relevant
information to the LLM.

    User Question
          │
          ▼
    Retrieve Relevant Information
          │
          ▼
    Augment Prompt
          │
          ▼
    LLM Generates Answer

------------------------------------------------------------------------

# RAG Pipeline

## Phase 1 - Indexing (Done Once)

    PDF
     │
     ▼
    Extract Text
     │
     ▼
    Clean Text
     │
     ▼
    Split into Chunks
     │
     ▼
    Generate Embeddings
     │
     ▼
    Store in Vector Database

## Phase 2 - Retrieval (Every Question)

    User Question
     │
     ▼
    Generate Query Embedding
     │
     ▼
    Search Vector Database
     │
     ▼
    Retrieve Top-K Chunks
     │
     ▼
    Augment Prompt
     │
     ▼
    LLM
     │
     ▼
    Final Answer + Citations

------------------------------------------------------------------------

# Main Components

## 1. Document Processor

### Responsibility

-   Load PDF
-   Extract text
-   Clean text
-   Chunk text

### Example Methods

``` python
load_pdf()
clean_text()
chunk_text()
```

Input

    PDF

Output

    Chunks

------------------------------------------------------------------------

## 2. Embedding Manager

### Responsibility

Convert text into vectors.

Example

    Text
     ↓
    Embedding Model
     ↓
    [0.12, -0.53, 0.81, ...]

Methods

``` python
embed_documents()
embed_query()
```

Remember:

-   Creates embeddings.
-   Never stores them.

------------------------------------------------------------------------

## 3. Vector Store

Example: Chroma

### Responsibility

-   Store embeddings
-   Store text chunks
-   Store metadata
-   Perform semantic search

Methods

``` python
add_documents()
search()
delete()
```

Think of Chroma as a semantic search engine.

Google searches by keywords.

Chroma searches by meaning.

------------------------------------------------------------------------

## 4. Retriever

### Responsibility

Retrieve the most relevant chunks.

    Question
        │
        ▼
    Embedding
        │
        ▼
    Vector Store
        │
        ▼
    Top 5 Chunks

------------------------------------------------------------------------

## 5. Augmentation

Augmentation means adding the retrieved chunks to the prompt.

Without augmentation

    Question
     ↓
    LLM

With augmentation

    Question

    +

    Retrieved Context

    ↓

    LLM

------------------------------------------------------------------------

## 6. LLM

The LLM does not search.

It only generates the final answer.

Input

-   User Question
-   Retrieved Context

Output

-   Human-readable answer

------------------------------------------------------------------------

# Embeddings

Embeddings are numerical representations of text.

Example

    Machine Learning

    ↓

    [0.11, -0.43, 0.82, ...]

Similar sentences produce similar vectors.

------------------------------------------------------------------------

# Why Embeddings?

Traditional Search

    learn

will not match

    trained

Semantic Search understands that both have similar meanings.

------------------------------------------------------------------------

# Cosine Similarity

Cosine similarity compares two embeddings.

Higher value means more similar.

Example

  Similarity   Meaning
  ------------ --------------------
  1.0          Identical
  0.9          Very Similar
  0.5          Moderately Similar
  0.0          Unrelated

------------------------------------------------------------------------

# Cosine Distance

Some vector databases return distance instead.

  Distance   Meaning
  ---------- ------------
  0.0        Best Match
  0.2        Good Match
  0.8        Poor Match

Many projects convert it using

    score = 1 - distance

------------------------------------------------------------------------

# Chunking

Never embed an entire document.

Split it into smaller chunks.

Example

    Chunk 1

    Introduction...

    -------------------

    Chunk 2

    Neural Networks...

    -------------------

    Chunk 3

    Backpropagation...

Benefits

-   Better retrieval
-   Faster search
-   Fits embedding model limits

------------------------------------------------------------------------

# Metadata

Metadata is extra information stored with each chunk.

Example

``` python
{
    "source": "ML.pdf",
    "page": 42,
    "chapter": "Backpropagation"
}
```

Metadata helps generate citations.

------------------------------------------------------------------------

# Citations

Citations tell the user where the answer came from.

Example

    Source:

    ML.pdf

    Page 42

------------------------------------------------------------------------

# Chroma

Chroma stores

-   Embeddings
-   Text Chunks
-   Metadata

It also performs similarity search.

------------------------------------------------------------------------

# Complete Flow

## Indexing

    PDF
     ↓
    Document Processor
     ↓
    Chunks
     ↓
    Embedding Manager
     ↓
    Embeddings
     ↓
    Chroma

------------------------------------------------------------------------

## Retrieval

    Question
     ↓
    Embedding Manager
     ↓
    Query Embedding
     ↓
    Chroma
     ↓
    Relevant Chunks
     ↓
    Prompt Augmentation
     ↓
    LLM
     ↓
    Answer

------------------------------------------------------------------------

# Responsibilities

  Component            Responsibility
  -------------------- ---------------------
  Document Processor   Load, Clean, Chunk
  Embedding Manager    Generate Embeddings
  Chroma               Store & Search
  Retriever            Fetch Top-K Chunks
  LLM                  Generate Answer
  Pipeline             Connect Everything

------------------------------------------------------------------------

# Folder Structure

    project/

    ├── data/
    │   ├── pdfs/
    │   ├── text/
    │   └── vectors/
    │
    ├── embeddings.py
    ├── vector_store.py
    ├── document_processor.py
    ├── retriever.py
    ├── rag_pipeline.py
    ├── app.py
    └── README.md

------------------------------------------------------------------------

# Interview Questions

### Why do we chunk documents?

-   Better retrieval
-   Better embeddings
-   Fits model limits

### Why embeddings?

To represent meaning instead of exact words.

### Why Chroma?

To store embeddings and search similar vectors.

### Why cosine similarity?

To measure semantic similarity.

### What is augmentation?

Adding retrieved context to the prompt.

### Does the LLM search documents?

No.

The vector store searches.

The LLM generates.

------------------------------------------------------------------------

# Cheat Sheet

    PDF
     ↓
    Extract
     ↓
    Clean
     ↓
    Chunk
     ↓
    Embed
     ↓
    Store
    -------------------------
    Question
     ↓
    Embed
     ↓
    Search
     ↓
    Retrieve
     ↓
    Augment
     ↓
    LLM
     ↓
    Answer

------------------------------------------------------------------------

# One-Line Summary

> Document Processor prepares the data, Embedding Manager converts text
> into vectors, Chroma stores and searches vectors, Retriever fetches
> relevant chunks, Augmentation builds the prompt, and the LLM generates
> the final answer.
