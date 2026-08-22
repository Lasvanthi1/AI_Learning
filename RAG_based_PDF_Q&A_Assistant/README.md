📄 Chat with Your PDF — RAG Application

A simple Retrieval-Augmented Generation (RAG) application that allows users to upload a PDF and ask questions about its content using natural language.

The application retrieves the most relevant sections from the uploaded PDF and provides answers using an LLM, helping reduce hallucinations by grounding responses in the document.

🚀 Features
📄 Upload a PDF through a web interface
🔍 Automatically extract text from the PDF
✂️ Split large documents into smaller chunks
🧠 Generate vector embeddings using Hugging Face
🗄️ Store document vectors using Chroma
🔎 Perform semantic similarity search
🤖 Generate answers using Groq LLM
📚 Include source page numbers in responses
💬 Interactive chat interface using Gradio
🔄 Re-index automatically whenever a new PDF is uploaded
🏗️ Architecture
                    PDF
                     │
                     ▼
              PyPDFLoader
                     │
                     ▼
          Document Pages / Text
                     │
                     ▼
      RecursiveCharacterTextSplitter
           chunk_size = 800
           overlap = 100
                     │
                     ▼
        Hugging Face Embeddings
                     │
                     ▼
                  Chroma
              Vector Store
                     │
                     │
              User Question
                     │
                     ▼
          Similarity Search (k=4)
                     │
                     ▼
           Relevant PDF Chunks
                     │
                     ▼
              Prompt Template
                     │
                     ▼
             Groq LLM
           openai/gpt-oss-20b
                     │
                     ▼
                Answer
                     │
                     ▼
               Gradio UI
🛠️ Technologies Used
Technology	Purpose
Python	Application development
LangChain	RAG orchestration
PyPDF	PDF text extraction
RecursiveCharacterTextSplitter	Document chunking
Hugging Face	Text embeddings
Sentence Transformers	Embedding model
Chroma	Vector database
Groq	LLM inference
Gradio	Web UI
python-dotenv	Environment variable management
📁 Project Structure
AI_Scaler/
│
├── class_3/
│   └── rag.py
│
├── data/
│   └── sample.pdf
│
├── .env
├── .gitignore
└── README.md
⚙️ Installation
1. Create a virtual environment
python -m venv .venv
2. Activate the virtual environment

Windows PowerShell:

.venv\Scripts\Activate.ps1
3. Install dependencies
python -m pip install -U langchain langchain-openai langchain-chroma langchain-huggingface langchain-community langchain-groq langchain-text-splitters pypdf sentence-transformers gradio python-dotenv
🔑 Configure Groq API Key

Create a .env file in the project root:

GROQ_API_KEY=your_groq_api_key

The application loads the API key using:

from dotenv import load_dotenv


load_dotenv()
⚠️ Security

Never commit your API key to GitHub.

Add this to .gitignore:

.env
.venv/
__pycache__/
🧠 Embedding Model

The application uses:

HuggingFaceEmbeddings(
    model_name="all-MiniLM-L6-v2"
)

The embedding model converts text chunks into numerical vectors.

For example:

"What is the return policy?"
             │
             ▼
       Embedding Model
             │
             ▼
[0.12, -0.34, 0.87, ...]

These vectors allow Chroma to find text that is semantically similar to a user's question.

✂️ Document Chunking

The PDF is divided into smaller chunks using:

RecursiveCharacterTextSplitter(
    chunk_size=800,
    chunk_overlap=100
)
Chunk size
800 characters approximately
Chunk overlap
100 characters

The overlap helps preserve context between neighboring chunks.

Chunk 1
────────────────────────
        800 chars
             │
             │ 100 overlap
             ▼
        ┌───────────┐
        │           │
        └───────────┘
             Chunk 2
🗄️ Vector Store

The chunks are stored in Chroma:

db = Chroma.from_documents(
    chunks,
    embedder
)

In this implementation, the Chroma store is created in memory.

The vector store contains information that allows the application to perform similarity searches against the document.

🔍 Retrieval

When the user asks a question:

chunks = db.similarity_search(
    question,
    k=4
)

The application retrieves the 4 most relevant chunks.

For example:

Question:
"How can I return a product?"


        ↓


Chroma similarity search


        ↓


Chunk 1 → Return policy
Chunk 2 → Refund conditions
Chunk 3 → Return window
Chunk 4 → Return procedure

Only these relevant chunks are passed to the LLM.

🤖 Generation

The retrieved content is inserted into the prompt:

prompt = ChatPromptTemplate.from_template("""
You are a helpful PDF assistant.
Answer the question using ONLY the context below.


If the context doesn't contain the answer,
say "I couldn't find that in the document."


After your answer, list the page numbers
you used as:


Sources: page X, page Y.


Context:
{context}


Question:
{question}
""")

The LLM then generates an answer based on the retrieved context.

📚 Source Tracking

Each retrieved chunk contains PDF page metadata.

The code converts the zero-based page number into a human-readable page number:

c.metadata['page'] + 1

The context is formatted as:

[page 3] The product can be returned within 30 days...


[page 5] Refunds are processed after inspection...

This allows the LLM to provide responses such as:

You can return the product within 30 days.




The user workflow is:

Upload PDF
    ↓
PDF indexed
    ↓
Ask question
    ↓
Retrieve relevant chunks
    ↓
Generate answer
▶️ Running the Application



The Gradio application will start locally.

Because the code uses:

demo.launch(share=True)

Gradio also creates a temporary public sharing URL.

🔄 Complete RAG Flow
Document ingestion
PDF
 ↓
PyPDFLoader
 ↓
Pages
 ↓
Text Splitter
 ↓
Chunks
 ↓
Hugging Face Embeddings
 ↓
Chroma
Question answering
User Question
 ↓
Embedding / Similarity Search
 ↓
Top 4 Relevant Chunks
 ↓
Prompt + Context
 ↓
Groq LLM
 ↓
Answer + Sources
🎯 Example Questions

After uploading a suitable PDF, users can ask:

What is the return policy?
What are the prerequisites?
How do I install the application?
What are the main features?
What troubleshooting steps are recommended?

The application will answer using the uploaded document.

🧩 Important RAG Concepts Demonstrated

This project demonstrates the major stages of a basic RAG pipeline:

1. Document Loading
PyPDFLoader

Extracts content from the PDF.

2. Chunking
RecursiveCharacterTextSplitter

Breaks large documents into manageable pieces.

3. Embeddings
HuggingFaceEmbeddings

Converts text into vector representations.

4. Vector Database
Chroma

Stores and searches the embeddings.

5. Retrieval
similarity_search()

Finds relevant document chunks.

6. Augmentation

Retrieved chunks are added to the LLM prompt as context.

7. Generation
ChatGroq

Generates the final answer.

⚠️ Current Limitations

This is a learning/portfolio implementation and has some limitations:

Chroma is currently stored in memory.
Only one uploaded PDF is maintained at a time.
Uploading a new PDF replaces the previous index.
No authentication is implemented.
No persistent document history.
Retrieval is limited to k=4 chunks.
Large PDFs may take time to embed.
The quality of answers depends on PDF extraction, chunking, embeddings, retrieval, and the LLM.
🚀 Possible Improvements

Future improvements could include:

💾 Persistent Chroma database
📚 Support for multiple PDFs
🏷️ Document and page metadata filtering
🔎 Hybrid search
🧠 Reranking retrieved chunks
💬 Conversation memory
📊 Retrieval evaluation
🔐 User authentication
☁️ Cloud deployment
📈 LangSmith tracing and evaluation
📄 Better table/image extraction
🧩 Semantic chunking instead of fixed-size chunking
📌 Key Learning

The core idea behind this project is:

LLM
+
Relevant external knowledge
=
RAG

Instead of asking the LLM to answer from its general knowledge, RAG retrieves relevant information from the user's document and provides that information as context.

This helps create applications that can answer questions about private, domain-specific, or frequently changing information.

📄 License

This project is intended for educational and portfolio purposes.
