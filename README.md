# Multi-Modal RAG Pipeline with LangChain

This project implements a Retrieval-Augmented Generation (RAG) pipeline that can process multi-modal documents containing text, tables, and images. It uses LangChain to extract, summarize, and index different types of content from PDF documents, enabling semantic search and question answering across all modalities.

## Table of Contents
- [Architecture Overview](#architecture-overview)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Troubleshooting](#troubleshooting)
- [References](#references)

## Architecture Overview

The multi-modal RAG pipeline follows these steps:

1. **PDF Processing**: Extract text, tables, and images from PDF documents
2. **Content Summarization**: Generate concise summaries of each content type using appropriate models
3. **Vector Storage**: Store summaries in a vector database for efficient retrieval
4. **Multi-Modal Retrieval**: Retrieve relevant content based on user queries
5. **Response Generation**: Generate answers using both textual and visual context

## Project Structure

```
rag-project/
├── content/
│   └── attention.pdf                 # Sample PDF document
├── organized_notebook.ipynb           # Main implementation notebook
├── requirements.txt                   # Python dependencies
├── .env                              # API keys (not in repo)
└── README.md                         # This file
```

## Installation

### Prerequisites
- Python 3.8+
- pip package manager

### System Dependencies

**For Ubuntu/Debian:**
```bash
sudo apt-get install poppler-utils tesseract-ocr libmagic-dev
```

**For macOS:**
```bash
brew install poppler tesseract libmagic
```

### Python Dependencies
```bash
pip install -r requirements.txt
```

## Configuration

1. Create a `.env` file in the project root with your API keys:
```
OPENROUTER_API_KEY=your_groq_api
OPENAI_API_KEY=your_openai_api_key
GROQ_API_KEY=your_groq_api_key
LANGCHAIN_API_KEY=your_langchain_api_key
LANGCHAIN_TRACING_V2=true
```

2. Place your PDF documents in the `content/` directory

## Usage

1. Open `langchain_multimodal_rag` in Jupyter Notebook
2. Run the cells sequentially
3. Modify the query in the final cell to ask questions about your document

Example:
```python
response = chain.invoke("What is the attention mechanism?")
print(response)
```

## How It Works

### 1. PDF Processing
The pipeline uses `unstructured` library to extract different elements from PDF documents:
- Text content
- Tables with structure preservation
- Images in base64 format

### 2. Content Summarization
Different models are used for different content types:
- **Text & Tables**: Summarized using Groq's Llama 3.1 8B model
- **Images**: Summarized using OpenAI's GPT-4o-mini model

### 3. Vector Storage
A multi-vector retriever approach is used:
- Summaries are stored in ChromaDB for efficient similarity search
- Original content is stored in an in-memory document store
- UUIDs link summaries to their original content

### 4. Retrieval & Generation
When a query is made:
1. The query is embedded and used to find similar summaries
2. Original content linked to relevant summaries is retrieved
3. Both text and images are included in the context for response generation
4. GPT-4o-mini generates a response using all modalities

## Troubleshooting

### Common Issues

1. **Missing system dependencies**
   - Ensure poppler, tesseract, and libmagic are installed
   - Check installation instructions for your OS

2. **API key errors**
   - Verify all keys in `.env` are correct and active
   - Check that you have access to required models

3. **PDF processing issues**
   - Try different `strategy` parameters in `partition_pdf`
   - For complex documents, try `strategy="ocr_only"`

### Error Handling

The notebook includes error handling for:
- Invalid base64 image data
- Model access errors
- PDF parsing failures

## References
- [LangChain Multi-Vector Storage](https://python.langchain.com/docs/how_to/multi_vector/)
- [Unstructured Library Documentation](https://docs.unstructured.io/)
