# Cognita 🧠 | Agentic RAG Engine

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Framework: LlamaIndex](https://img.shields.io/badge/Framework-LlamaIndex-blueviolet)](https://www.llamaindex.ai/)
[![API: FastAPI](https://img.shields.io/badge/API-FastAPI-green.svg)](https://fastapi.tiangolo.com/)
[![Containerization: Docker](https://img.shields.io/badge/Container-Docker-blue.svg)](https://www.docker.com/)

Cognita is a production-grade, agentic RAG application. It enables users to chat with their documents, receiving accurate, cited answers directly from the source material. Built with a modern tech stack, it serves as a robust foundation for developing complex question-answering systems.

[cite_start]This project directly implements the requirements outlined in the technical assignment document[cite: 1].

---

### 🌟 Features

- [cite_start]**📄 Document Upload:** Supports uploading PDF and DOC files through a simple API endpoint[cite: 6, 21].
- [cite_start]**💬 Interactive Chat Interface:** A backend API ready to power any simple and functional frontend[cite: 15, 18, 19].
- [cite_start]**📚 Source-Cited Answers:** Every response is backed by citations, including the filename and page number, to ensure accuracy and avoid hallucinations[cite: 25, 26].
- [cite_start]**🧠 Agentic Core:** Utilizes an agentic loop to handle queries, allowing for potential multi-step reasoning[cite: 36].
- [cite_start]**🤔 Conversation Memory:** Remembers the context of the conversation for coherent follow-up questions[cite: 38].
- [cite_start]**🚫 "No Answer" Handling:** Gracefully informs the user when no relevant information can be found in the documents[cite: 26].
- **🧩 Modular & Scalable:** Designed with a clean, scalable architecture to support new features and integrations.
- [cite_start]**🐳 Containerized:** Fully containerized with Docker and Docker Compose for easy setup and deployment[cite: 42].

---

### 🛠️ Tech Stack & Architecture

- [cite_start]**Core Framework:** **LlamaIndex** is used for the entire RAG pipeline, from data ingestion and indexing to retrieval and synthesis[cite: 28, 41].
- **LLM & Embedding Models:** Configurable to use OpenAI models (e.g., GPT-4, text-embedding-ada-002) but easily adaptable to Hugging Face or other providers.
- [cite_start]**Vector Database:** **Weaviate** is integrated as the vector store for storing document embeddings, running in its own Docker container[cite: 29, 30].
- [cite_start]**Document Parsing:** **LlamaParse** is included as an optional, high-accuracy parser for complex PDFs[cite: 32].
- **API Server:** **FastAPI** provides a high-performance, async API for handling chat and document uploads.
- **Configuration:** **Pydantic** manages all environment variables and settings for type-safe configuration.
- [cite_start]**Containerization:** **Docker & Docker Compose** for reproducible environments and simplified deployment[cite: 42, 48].
- [cite_start]**Language:** **Python 3.11+**[cite: 40].

#### [cite_start]Architecture Overview [cite: 52]

1.  **API Layer (FastAPI):** Exposes two main endpoints:
    * `/api/v1/upload`: Handles multipart file uploads.
    * `/api/v1/chat`: Manages the conversational flow.
2.  **Ingestion Pipeline:**
    * When a document is uploaded, it's processed by the `IngestionEngine`.
    * [cite_start]**LlamaParse** (or a standard parser) extracts text and metadata[cite: 32].
    * The text is chunked and converted into vector embeddings.
    * Embeddings and text are stored in the Weaviate vector database.
3.  **Agentic Chat Engine:**
    * The `/chat` endpoint invokes the `ChatAgent`.
    * The agent uses a `QueryEngineTool` to search the Weaviate index for relevant context.
    * [cite_start]It maintains **Conversation Memory** to understand follow-up questions[cite: 38].
    * The retrieved context and chat history are passed to the LLM to generate a response.
    * [cite_start]Source nodes from the retrieval step are formatted into citations (filename and page number)[cite: 25].

---

### 🚀 Setup and Usage

Follow these instructions to get Cognita running locally using Docker.

#### Prerequisites

- Docker and Docker Compose
- Python 3.11+ (for local development without Docker)
- An OpenAI API Key (or another supported LLM provider)

#### 1. Clone the Repository

```bash
git clone <repository_url>
cd cognita
