<div align="center">

# Medical Chatbot (RAG-based Clinical Reference Assistant)

Context-aware Q&A over a local medical knowledge base using FAISS vector search + HuggingFace or Groq-hosted LLMs.

</div>

## Overview
This project implements a Retrieval-Augmented Generation (RAG) pipeline that lets you ask medical questions grounded in a curated PDF knowledge base (e.g. an encyclopedia of medicine). Instead of hallucinating, the LLM is constrained by retrieved passages from a FAISS vector store built from your documents.

Two primary entry points:
- `connect_memory_with_llm.py` – CLI prototype using a HuggingFace Inference endpoint (e.g. Mistral 7B Instruct).
- `medibot.py` – Streamlit chat UI using a Groq-hosted model (Llama 4 Maverick) with retrieval.

## Key Features
- FAISS vector store for fast semantic retrieval
- SentenceTransformer embeddings (`all-MiniLM-L6-v2`) – switchable to remote API mode
- Modular prompt template injection
- Groq or HuggingFace LLM backends
- Source document traceability (shows which chunks supported the answer)
- Caching of vector store + embeddings via Streamlit resource cache

## Architecture
```
PDF(s) --> Text Splitter --> Embeddings --> FAISS Index (vectorstore/db_faiss)
								│
User Query --> Retriever (top-k) ---------------┘
			    │
		    Prompt Assembly
			    │
		    LLM Generation (HF or Groq)
			    │
		    Answer + Source Chunks
```

### Main Components
| File | Role |
|------|------|
| `create_memory_for_llm.py` | Builds FAISS index from PDFs (embedding + persist) |
| `connect_memory_with_llm.py` | CLI RAG query using HuggingFaceEndpoint |
| `medibot.py` | Streamlit chat interface using Groq Chat model + FAISS retrieval |
| `vectorstore/db_faiss` | Persisted FAISS index (created beforehand) |
| `data/` | PDF source documents |
