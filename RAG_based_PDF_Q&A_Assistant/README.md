# 📄 Chat with Your PDF — RAG Application

A **Retrieval-Augmented Generation (RAG)** application that allows users to upload a PDF and ask questions about its contents using natural language.

The application extracts PDF content, splits it into smaller chunks, converts those chunks into vector embeddings, stores them in Chroma, retrieves the most relevant content for a user query, and uses a Groq-hosted LLM to generate an answer grounded in the uploaded document.

---

## 📌 Project Overview

Searching through lengthy documents manually can be time-consuming. This project provides a conversational interface for interacting with PDF documents.

A user can:

1. Upload a PDF.
2. Wait for the document to be indexed.
3. Ask a question.
4. The system searches the document for relevant content.
5. The retrieved content is provided to the LLM as context.
6. The LLM generates an answer based on the retrieved information.
7. Relevant PDF page numbers are included as sources.

This approach is useful for technical documentation, product manuals, company policies, training materials, research papers, and DevOps runbooks.

---

## ✨ Features

- 📄 PDF document upload
- 🔍 Automatic PDF text extraction
- ✂️ Recursive document chunking
- 🧠 Hugging Face sentence embeddings
- 🗄️ Chroma vector database
- 🔎 Semantic similarity search
- 🤖 Groq LLM-powered question answering
- 📚 Page-level source references
- 💬 Interactive Gradio chat interface
- 🔄 Automatic re-indexing when a new PDF is uploaded
- 🔐 API key management using environment variables

---

## 🏗️ Architecture

### Document Ingestion

PDF  
↓  
PyPDFLoader  
↓  
Text Splitting  
↓  
Hugging Face Embeddings  
↓  
Chroma Vector Store

### Question Answering

User Question  
↓  
Similarity Search  
↓  
Relevant Chunks  
↓  
Prompt Context  
↓  
Groq LLM  
↓  
Answer + Sources

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| Python | Application development |
| LangChain | RAG pipeline orchestration |
| PyPDF | PDF document processing |
| RecursiveCharacterTextSplitter | Document chunking |
| Hugging Face | Text embeddings |
| Sentence Transformers | Local embedding model |
| Chroma | Vector storage and similarity search |
| Groq | LLM inference |
| Gradio | Web-based user interface |
| python-dotenv | Environment variable management |

---
