# 📄 RAG Document Question Answering System

A production-ready **Retrieval-Augmented Generation (RAG)** application that allows users to upload multiple PDF documents and ask natural language questions. The system intelligently retrieves relevant context and generates accurate, grounded answers using a HuggingFace language model.


## 🎯 Problem Statement

Reading through large PDF documents to find specific information is slow and inefficient. This system lets you upload any PDF — a textbook, research paper, medical document, or legal contract — and ask questions in plain English. The system finds the relevant context and answers instantly.

---

## 🏗 RAG Pipeline Architecture

```
User uploads PDF(s)
        │
        ▼
┌──────────────────────┐
│    PyPDFLoader       │  ← Loads each PDF page by page
└──────────────────────┘
        │
        ▼
┌──────────────────────────────────┐
│  RecursiveCharacterTextSplitter  │  ← Splits text into 500-char chunks
│  chunk_size=500, overlap=100     │     overlap=100 preserves boundary context
└──────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────┐
│    HuggingFaceEmbeddings         │  ← Converts chunks to 384-dim vectors
│    all-MiniLM-L6-v2              │     using sentence-transformers
└──────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────┐
│    FAISS Vector Store            │  ← Stores all embeddings for
│    Approximate Nearest Neighbour │     fast similarity search
└──────────────────────────────────┘
        │
   User asks question
        │
        ▼
┌──────────────────────────────────┐
│    Retriever (k=3)               │  ← Finds top 3 most relevant
│    Semantic Similarity Search    │     chunks for the question
└──────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────┐
│    google/flan-t5-base           │  ← Generates natural language
│    RetrievalQA Chain             │     answer from retrieved context
└──────────────────────────────────┘
        │
        ▼
   Answer + Source Chunks displayed to user
```

---

## ✨ Features

- 📁 Upload multiple PDF files simultaneously
- 🔍 Semantic search — finds meaning, not just keywords
- 🤖 AI-generated natural language answers
- 📌 Shows source document chunks used to generate answer
- ⚡ Fast retrieval using FAISS vector similarity search
- 🖥 Clean interactive web interface built with Streamlit

---

## 🛠 Tech Stack

| Component | Technology |
|---|---|
| Frontend / UI | Streamlit |
| Document Loading | LangChain PyPDFLoader |
| Text Chunking | RecursiveCharacterTextSplitter |
| Embedding Model | sentence-transformers/all-MiniLM-L6-v2 |
| Vector Database | FAISS (Facebook AI Similarity Search) |
| Language Model | google/flan-t5-base |
| LLM Framework | LangChain RetrievalQA |
| Language | Python 3.10+ |

---

## 📂 Project Structure

```
rag-document-qa/
│
├── app.py                      ← Main Streamlit application
├── requirements.txt            ← All dependencies
├── README.md                   ← Project documentation
├── .gitignore
├── rag_demo_document.pdf       ← Sample RAG demo document
├── ai_ml_dl_demo.pdf           ← AI/ML/DL reference document
├── data_science_nlp_demo.pdf   ← Data Science & NLP document
└── paracetamol.pdf             ← Medical document sample
```

---

## ▶️ How to Run

### Step 1 — Clone the repository
```bash
git clone https://github.com/Pruthviss/rag-document-qa.git
cd rag-document-qa
```

### Step 2 — Create virtual environment (recommended)
```bash
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Mac/Linux
```

### Step 3 — Install dependencies
```bash
pip install -r requirements.txt
```

### Step 4 — Run the application
```bash
streamlit run app.py
```

### Step 5 — Open in browser
The app will automatically open at `http://localhost:8501`

---

## 💡 How to Use

1. Click **Browse files** and upload one or more PDF documents
2. Wait for the system to process and index the documents
3. Type your question in the text box
4. The system retrieves relevant chunks and generates an answer
5. Scroll down to see the source document chunks used

---

## 🧪 Example Questions to Try

Using the included demo PDFs:

- *"What is Artificial Intelligence?"*
- *"What is Machine Learning?"*
- *"What is Natural Language Processing?"*
- *"What is Deep Learning?"*
- *"What is Retrieval-Augmented Generation?"*
- *"What are the uses of Paracetamol?"*

---

## 🔍 How RAG Works — Simple Explanation

Traditional language models answer from memory (training data). RAG is different:

1. **Retrieve** — find the most relevant parts of YOUR documents
2. **Augment** — add that context to the question
3. **Generate** — LLM answers based on YOUR documents, not general knowledge

This means the model answers from your actual documents — reducing hallucination and giving accurate, grounded responses.
