# RAG-Chatbot

A document-based Retrieval-Augmented Generation (RAG) application built with Streamlit. It enables users to upload their own documents and interact with their content through natural-language questions.

This project explores the development of a complete RAG workflow, covering document processing, text chunking, embedding generation, semantic search, vector storage, LLM integration and context-aware response generation. It supports both externally hosted and locally running language models.

## Features

- Upload and read PDF and DOCX files.
- Extract document content and divide it into smaller text segments.
- Generate embeddings and store them in a local ChromaDB vector database.
- Perform semantic similarity search to identify relevant document passages.
- Generate answers using the retrieved information as contextual input.
- Display supporting text passages and source excerpts alongside responses.
- Measure retrieval performance through a Recall@K evaluation script.
- Provide an interactive chatbot interface using Streamlit.
- Support optional integration with external LLM APIs.
- Offer a local pretrained model alternative when an external API key is not configured.

## Technologies Used

- Python
- Streamlit
- ChromaDB
- Text Embeddings
- Semantic Search
- Retrieval-Augmented Generation (RAG)
- Large Language Model (LLM) Integration
- Locally Hosted Pretrained Models

## How It Works

1. Users upload or place their documents in the designated data directory.
2. The ingestion pipeline extracts the text, splits it into chunks and generates embeddings.
3. The resulting vector representations are indexed and saved in a local ChromaDB collection.
4. When a question is submitted, the retrieval component searches the database for the most relevant text segments.
5. The retrieved passages are supplied to the language model, which generates an answer grounded in the available document content.
6. Relevant context and source excerpts are presented to users, making the generated responses easier to inspect.

## Quickstart

### 1. Set up a virtual environment

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it:

**Windows**
```bash
.venv\Scripts\activate
```

**macOS / Linux**
```bash
source .venv/bin/activate
```

### 2. Install the required packages

```bash
pip install -r requirements.txt
```

### 3. Add your documents

Create the following directory:

```text
data/docs/
```

Place the PDF or DOCX files you want the chatbot to process inside this folder.

### 4. Process and index the documents

Run the ingestion script:

```bash
python ingest.py --docs_dir data/docs --db_dir data/chroma
```

### 5. Launch the application

Start the Streamlit interface:

```bash
streamlit run app.py -- --db_dir data/chroma
```

## API Configuration

The application can connect to external language model providers when the appropriate API credentials are available.

API keys should be stored securely and must not be included in committed repository files. Depending on the configuration, credentials can be supplied through environment variables or the application interface.

If an external API key is not provided, the application can use its local pretrained model fallback.

## Retrieval Evaluation

A dedicated evaluation script is included to assess the quality of document retrieval using the Recall@K metric.

First, create an evaluation directory and question file:

```text
data/eval/questions.json
```

Example:

```json
[
  {
    "question": "How many absences are allowed?",
    "must_contain": ["absence", "allowed"]
  }
]
```

Run the evaluation:

```bash
python evaluate.py --db_dir data/chroma --eval_file data/eval/questions.json --k 5
```

## Project Structure

```text
app.py                 Streamlit application interface
config.py              Application configuration
ingest.py              Document extraction, chunking, embedding and indexing
rag.py                 Retrieval logic, prompt construction and LLM integration
evaluate.py            Retrieval evaluation using Recall@K
requirements.txt       Project dependencies
SOUTENANCE_SCRIPT.md   Project presentation and discussion notes
```

## Local Data and Storage

The `data/` directory is excluded from the repository because it may contain user-provided documents, generated embeddings, local vector databases and other runtime files.

Users can supply their own documents locally and create the corresponding vector database by running the ingestion pipeline.

## Project Status

This project serves as a practical exploration of document-based RAG systems and their main components, including semantic retrieval, embedding-based indexing, LLM integration, context-grounded generation and retrieval evaluation.
