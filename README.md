# MCP-Powered PDF Retrieval-Augmented Generation (RAG) Assistant

## Project Overview

This project implements a Retrieval-Augmented Generation (RAG) assistant powered by the FastMCP framework. It enables users to upload and index various document formats (PDF, DOCX, PPTX, CSV, TXT, Markdown) and then ask questions based on the content of these documents. The assistant leverages Groq's powerful LLM for conversational AI and spaCy embeddings with FAISS for efficient document retrieval.

Key features include robust text extraction from diverse file types, intelligent text chunking, and a conversational interface for querying your indexed knowledge base.

## Features

*   **Multi-format Document Support:** Index and retrieve information from:
    *   **PDF (`.pdf`):** Advanced text and table extraction.
    *   **Word Documents (`.docx`):** Extracts text from paragraphs, tables, headers, and footers.
    *   **PowerPoint Presentations (`.pptx`):** Extracts text from slides and shapes.
    *   **CSV Files (`.csv`):** Parses data with automatic encoding detection and multiple delimiter support, providing structured text output and basic summary statistics.
    *   **Plain Text (`.txt`):** Simple text extraction.
    *   **Markdown Files (`.md`, `.markdown`):** Intelligent parsing to preserve structure (headers, lists, code blocks) and convert to clean text.
*   **Intelligent Text Preprocessing:**
    *   **Text Cleaning & Normalization:** Removes excessive whitespace, special characters, PDF artifacts, page numbers, and normalizes punctuation and quotes.
    *   **Automatic Encoding Detection:** Uses `chardet` to automatically detect the encoding of text and CSV files, preventing `UnicodeDecodeError` issues.
    *   **Recursive Character Text Splitter:** Chunks documents into manageable sizes for efficient embedding and retrieval.
*   **Groq-Powered LLM:** Utilizes Groq's fast and efficient Language Models (LLMs) for generating conversational responses.
*   **FAISS Vector Store:** Stores document embeddings for rapid semantic search and retrieval.
*   **Conversational Memory:** Maintains chat history to provide context-aware responses.
*   **FastMCP Server:** Provides a robust and scalable server framework for exposing the RAG capabilities as API endpoints.

## Prerequisites

Before you begin, ensure you have the following installed:

*   **Python 3.8+:** Download from [python.org](https://www.python.org/downloads/).
*   **`pip`:** Python package installer (usually comes with Python).
*   **`venv` module:** For creating virtual environments (also typically included with Python).
*   **Groq API Key:** Obtain a free API key from the [Groq Console](https://console.groq.com/).

## Setup Instructions

Follow these steps to set up and run the application:

### 1. Navigate to the Project Directory

Open your terminal or command prompt and navigate to the `MCP-Powered-PDF-Retrieval-Augmented-Generation-Assistant` directory:

```bash
cd C:\Users\Dell\OneDrive\Desktop\PROJECT\MCP-Powered-PDF-Retrieval-Augmented-Generation-Assistant
```

### 2. Create and Activate a Virtual Environment

It's highly recommended to use a virtual environment to manage project dependencies.

```bash
python -m venv .venv
```

Activate the virtual environment:

*   **On Windows (PowerShell):**
    ```bash
    .venv\Scripts\Activate.ps1
    ```
*   **On Windows (Command Prompt):**
    ```bash
    .venv\Scripts\activate.bat
    ```
*   **On macOS/Linux:**
    ```bash
    source .venv/bin/activate
    ```

### 3. Install Dependencies

Once the virtual environment is active, install the required Python packages:

```bash
pip install -r requirements.txt
```

### 4. Download spaCy Model

The `SpacyEmbeddings` component requires a spaCy model. Download the `en_core_web_sm` model:

```bash
python -m spacy download en_core_web_sm
```

### 5. Configure Environment Variables

Create a `.env` file in the root of your project directory (`MCP-Powered-PDF-Retrieval-Augmented-Generation-Assistant/.env`) and add the following content. **Make sure to replace `your_groq_api_key_here` with your actual Groq API key.**

```ini
# MCP Server Configuration
HOST=127.0.0.1
PORT=8000

# Groq API Configuration
GROQ_API_KEY=your_groq_api_key_here
GROQ_MODEL=llama-3.3-70b-versatile

# Optional: Set to debug mode
DEBUG=True
```

**To create/edit the `.env` file using the terminal (Windows PowerShell):**

```bash
# Create the .env file
New-Item -Path ".env" -ItemType File -Force

# Add content to the .env file
Add-Content -Path ".env" -Value "PORT=8000" -Encoding UTF8
Add-Content -Path ".env" -Value "HOST=127.0.0.1" -Encoding UTF8
Add-Content -Path ".env" -Value "GROQ_API_KEY=your_groq_api_key_here" -Encoding UTF8
Add-Content -Path ".env" -Value "GROQ_MODEL=llama-3.3-70b-versatile" -Encoding UTF8
Add-Content -Path ".env" -Value "DEBUG=True" -Encoding UTF8
```

You can verify the content of your `.env` file with:

```bash
Get-Content .env
```

## Running the Application

After completing the setup, you can start the FastMCP server.

```bash
python mcp_app.py
```

The server will start and display a URL (e.g., `http://127.0.0.1:8000/sse/`). This is the endpoint you will use to interact with your RAG assistant.

## Usage

Once the MCP server is running, you can interact with it using the provided tools. The application exposes two main tools: `index_file` and `rag_query`.

### `index_file(file_path: str) -> str`

This tool allows you to index a document. Provide the absolute or relative path to the file you want to index.

**Example (using a hypothetical client or FastMCP UI):**

```python
# Assuming you have a FastMCP client or similar way to call tools
response = client.call_tool("index_file", file_path="./sample_files/my_document.pdf")
print(response)
# Expected output: "Successfully indexed my_document.pdf"
```

### `rag_query(question: str) -> str`

This tool allows you to ask questions based on the content of the indexed documents.

**Example (using a hypothetical client or FastMCP UI):**

```python
response = client.call_tool("rag_query", question="What is the main topic of the indexed documents?")
print(response)
# Expected output: "The main topic is related to..."
```

