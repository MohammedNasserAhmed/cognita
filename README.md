<div align="center">
  <br />
  <h1>🧠 Cognita: The Agentic RAG Engine</h1>
  <br />
  <p>
    <strong>
      Chat with your documents using a cutting-edge, production-ready, and open-source Agentic RAG engine.
    </strong>
  </p>
  <p>Built with LlamaIndex, FastAPI, and Weaviate.</p>

  <p>
    <a href="/LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"></a>
    <a href="https://www.python.org/"><img src="https://img.shields.io/badge/python-3.11+-blue.svg" alt="Python Version"></a>
    <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Container-Docker-blue.svg" alt="Containerization: Docker"></a>
    <a href="https://github.com/features/actions"><img src="https://img.shields.io/badge/CI/CD-GitHub_Actions-green.svg" alt="CI/CD: GitHub Actions"></a>
    <a href="https://github.com/psf/black"><img src="https://img.shields.io/badge/code%20style-black-000000.svg" alt="Code style: black"></a>
    <a href="https://github.com/astral-sh/ruff"><img src="https://img.shields.io/badge/linter-ruff-red" alt="Linter: Ruff"></a>
  </p>
</div>

---

**Cognita** is a complete, agentic RAG application designed for developers and teams. It provides a robust backend that enables users to upload documents and receive accurate, source-cited answers through a conversational interface. By packaging a state-of-the-art AI stack into a containerized, easy-to-deploy application, Cognita serves as the perfect foundation for building custom knowledge-based systems.

This project was developed to meet and exceed the specifications of a senior-level AI engineering task, showcasing modern, production-grade practices.

## 📖 Table of Contents

- [🌟 Key Features](#-key-features)
- [🏗️ Architecture Deep Dive](#️-architecture-deep-dive)
- [🛠️ Tech Stack](#️-tech-stack)
- [🚀 Getting Started](#-getting-started)
- [📁 Project Structure](#-project-structure)
- [⚙️ Configuration](#️-configuration)
- [🤝 How to Contribute](#-how-to-contribute)
- [🗺️ Future Roadmap](#️-future-roadmap)
- [📝 License](#-license)

## 🌟 Key Features

* **💬 Interactive Chat API**: A powerful FastAPI backend ready to power any frontend application.
* **📄 Multi-Format Document Ingestion**: Upload PDFs, DOCs, and more through a simple API endpoint. LlamaParse integration ensures high-fidelity text extraction even from complex layouts.
* **📚 Accurate, Cited Answers**: Every response is generated directly from the provided documents and includes precise citations with filename and page number, virtually eliminating hallucinations.
* **🧠 Agentic Reasoning Core**: Moves beyond simple RAG by using an agentic loop. This allows for conversation memory and lays the groundwork for multi-step reasoning to tackle complex queries.
* **🚫 Graceful "No Answer" Handling**: If the answer isn't in the documents, Cognita will clearly state that, preventing guesswork and maintaining user trust.
* **🧩 Modular & Extensible Design**: Built with clean, decoupled components, making it easy to swap vector databases, LLMs, or add new tools.
* **🐳 Fully Containerized**: With Docker and Docker Compose, the entire stack (API + Vector DB) can be spun up with a single command, ensuring a smooth, reproducible setup.

## 🏗️ Architecture Deep Dive

Cognita operates on a two-stage process: **Ingestion** and **Querying**.

```mermaid
graph TD
    subgraph Ingestion Flow
        A[User uploads file via API] --> B{Document Parser};
        B -- LlamaParse for complex PDFs --> C[Text & Metadata Extraction];
        B -- Standard Parsers for others --> C;
        C --> D[Chunking Engine];
        D --> E[Embedding Model];
        E --> F[Weaviate Vector DB];
    end

    subgraph Query & Agentic Flow
        G[User asks question via API] --> H{Chat Agent};
        H -- maintains --> I[Conversation Memory];
        H -- uses --> J[Query Engine Tool];
        J -- creates query --> K[Embedding Model];
        K -- searches --> F;
        F -- returns relevant chunks --> J;
        J -- synthesizes context --> H;
        H -- generates final response with LLM --> L[Answer with Citations];
        L --> M[User Receives Response];
    end

    style F fill:#f9f,stroke:#333,stroke-width:2px
````

1.  [cite\_start]**Ingestion Flow**: When a document is uploaded, it's processed by a pipeline that extracts text, splits it into manageable chunks, generates vector embeddings, and stores them in the Weaviate database[cite: 28, 29].
2.  **Query Flow**: A user's question initiates the agentic loop. [cite\_start]The agent retrieves relevant text chunks from Weaviate, combines them with the conversation history, and uses the LLM to generate a coherent, accurate, and cited answer[cite: 3, 24, 25].

## 🛠️ Tech Stack

Cognita integrates best-in-class tools to deliver a robust and modern application.

| Category                | Tool                                                                                                   | Description                                                                                                   |
| ----------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| **Core AI Framework** | [**LlamaIndex**](https://www.llamaindex.ai/)                                                             | The central nervous system for indexing, retrieval, and agentic logic.                           |
| **LLM & Embeddings** | [**OpenAI**](https://openai.com/)                                                                        | Provides state-of-the-art models for generation and embedding. Easily swappable for other providers.        |
| **Vector Database** | [**Weaviate**](https://weaviate.io/)                                                                     | A powerful, scalable open-source vector database for storing and searching embeddings.               |
| **Document Parsing** | [**LlamaParse**](https://www.google.com/search?q=https://docs.llamaindex.ai/en/stable/module_guides/loading/document_parsers/llama_parse/) | https://www.google.com/search?q=Optional high-fidelity parser for complex documents like PDFs with tables and figures[cite: 32].           |
| **API Framework** | [**FastAPI**](https://fastapi.tiangolo.com/)                                                             | Delivers a high-performance, asynchronous API for all interactions.                                           |
| **Containerization** | [**Docker & Docker Compose**](https://www.docker.com/)                                                   | For creating a reproducible, isolated, and easy-to-deploy environment.                                 |
| **Dev & Quality Tools** | **Ruff, Black, pre-commit** | For automated linting, formatting, and ensuring high code quality before every commit.                        |

## 🚀 Getting Started

Get your own instance of Cognita running in just a few minutes.

### Prerequisites

  - [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
  - An **OpenAI API Key**.
  - (https://www.google.com/search?q=Optional) A **LlamaParse API Key** for superior document parsing.

### Installation Steps

**1. Clone the Repository**

```bash
git clone <repository_url>
cd cognita
```

**2. Configure Your Environment**
Create a `.env` file from the example template and add your API keys.

```bash
cp .env.example .env
```

Now, edit the `.env` file:

```dotenv
# .env
OPENAI_API_KEY="sk-..."
LLM_MODEL_NAME="gpt-4-turbo"
EMBEDDING_MODEL_NAME="text-embedding-3-small"
LLAMA_CLOUD_API_KEY="..." # Optional, for LlamaParse
```

**3. Launch the Application**
This single command builds the containers, starts the API, and launches the Weaviate database.

```bash
docker-compose up --build -d
```

**4. Interact with the API**
The API is now live\! You can access the interactive Swagger documentation at:
**[http://localhost:8000/docs](https://www.google.com/search?q=http://localhost:8000/docs)**

From there, you can use the UI to upload files and ask questions.

Or, use `curl`:

  - **Upload a document:**

    ```bash
    curl -X POST "http://localhost:8000/api/v1/upload" \
         -H "accept: application/json" \
         -H "Content-Type: multipart/form-data" \
         -F "files=@/path/to/your_document.pdf"
    ```

  - **Ask a question:**

    ```bash
    curl -X POST "http://localhost:8000/api/v1/chat" \
         -H "accept: application/json" \
         -H "Content-Type: application/json" \
         -d '{"query": "What is the main objective of this project?", "session_id": "user123"}'
    ```

## 📁 Project Structure

The project is organized into logical modules for clarity and scalability.

```
cognita/
├── api/                # FastAPI application, routers, and schemas
├── core/               # Core application logic: agent, engine, config
├── data/               # Local directory for uploaded documents
├── tests/              # Unit and integration tests
├── .github/            # CI/CD workflows
├── .env.example        # Environment variable template
├── docker-compose.yml  # Orchestrates all services
├── Dockerfile          # Defines the API service container
└── README.md           # You are here!
```

## ⚙️ Configuration

All major settings are controlled via environment variables for security and flexibility.

| Variable               | Description                                           | Default            |
| ---------------------- | ----------------------------------------------------- | ------------------ |
| `OPENAI_API_KEY`       | **Required.** Your API key for OpenAI services.       | `N/A`              |
| `LLM_MODEL_NAME`       | The generation model to use.                          | `gpt-4-turbo`      |
| `EMBEDDING_MODEL_NAME` | The embedding model to use.                           | `text-embedding-3-small` |
| `LLAMA_CLOUD_API_KEY`  | https://www.google.com/search?q=Optional key for LlamaParse.                          | `None`             |
| `WEAVIATE_URL`         | The URL for the Weaviate vector database.             | `http://weaviate:8080` |
| `CHUNK_SIZE`           | Size of text chunks for the RAG pipeline.             | `1024`             |
| `SIMILARITY_TOP_K`     | Number of relevant chunks to retrieve for each query. | `4`                |

## 🤝 How to Contribute

We welcome contributions\! Cognita is an open-source project, and we believe in the power of community collaboration.

1.  **Fork** the repository.
2.  Create a new **branch** (`git checkout -b feature/your-awesome-feature`).
3.  **Code** your changes.
4.  **Run tests and linters** (`pre-commit run --all-files`).
5.  Submit a **Pull Request** with a clear description of your changes.

## 🗺️ Future Roadmap

Cognita is a strong foundation. Here are some features we're excited about for the future:

  - [ ] **Simple Frontend**: A Streamlit or React-based UI for a complete user experience.
  - [ ] **Expanded Document Support**: Natively support more formats like `.txt`, `.md`, and `.csv`.
  - [ ] **Advanced Agentic Tools**: Equip the agent with more tools (e.g., web search, code interpreter).
  - [ ] **More Vector DBs**: Add support for other databases like ChromaDB and Neo4j.
  - [ ] **Fine-Tuning Scripts**: Add example scripts for fine-tuning embedding or re-ranking models.

## 📝 License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

-----
<div align="center"\>
<em\>This project was completed to fulfill a technical assignment. Total time spent: <strong\>8 hours</strong\>.<em\>
</div\>

```
```
