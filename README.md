# Simple RAG System with Gemini

A simple Retrieval-Augmented Generation (RAG) system built from scratch in Python to understand how RAG works internally without using LangChain or a vector database.

## What I Built

The system answers questions using a small knowledge base of backend engineering documents:

* Redis
* FastAPI
* PostgreSQL
* Docker

### RAG Pipeline

```text
Documents
   ↓
Topic-based Chunking
   ↓
Gemini Embeddings
   ↓
In-memory Storage
   ↓
Question Embedding
   ↓
Cosine Similarity
   ↓
Top-K Relevant Chunks
   ↓
Gemini LLM
   ↓
Answer
```

## Chunking Strategy

I split each Markdown document based on **meaningful topics/sections** rather than using a fixed number of words.

For example:

```text
Redis
 ├── Caching
 ├── Session Management
 ├── Performance
 └── Use Cases
```

Each section becomes a separate chunk, while keeping the section heading with the content.

This helps retrieval return a focused piece of information instead of an entire document containing unrelated topics.

## Retrieval

Each chunk is converted into an embedding using:

```text
gemini-embedding-001
```

When a user asks a question, the question is also converted into an embedding.

I then calculate **cosine similarity** between the question embedding and each chunk embedding and retrieve the top-K most similar chunks.

For this project, the embeddings are stored and searched **in memory using Python** rather than using a vector database. This was intentional so I could understand the underlying retrieval process first.

## Generation

The retrieved chunks are provided as context to:

```text
gemini-3.8-flash
```

The LLM uses the retrieved context to generate the final answer and is instructed not to make up information that is not present in the provided documents.

## Technologies

* Python
* Google Gemini API
* `google-genai`
* Google Colab
* Markdown
* Cosine Similarity

## Why I Built This

The goal was to understand the core RAG concepts hands-on:

**chunking → embeddings → retrieval → context → generation**

before moving to tools such as vector databases and RAG frameworks.
