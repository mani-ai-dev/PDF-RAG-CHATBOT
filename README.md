# 🤖 PDF RAG Chatbot

> Ask questions about any PDF using natural language — powered by LangChain, FAISS, Groq (LLaMA 3), and Gradio.

![Chatbot Demo](https://github.com/mani-ai-dev/pdf-RAG-chatbot/blob/abcdef1234567890/Notebook/images/demo.png?raw=true)

---

## 📌 What is this?

This project is a **Retrieval-Augmented Generation (RAG)** chatbot that:

- Takes **any PDF** as input
- Processes and indexes its content using vector embeddings
- Lets you **ask questions in natural language**
- Returns accurate, context-grounded answers via **LLaMA 3.1** (Groq API)
- Serves a live chat UI via **Gradio** — shareable with a public link

No GPU needed. Runs entirely in **Google Colab** for free.

---

## 🖼️ Architecture

```
PDF Upload
    │
    ▼
PyMuPDFLoader ──► Raw text (page by page)
    │
    ▼
clean_text() ──► Strips LaTeX, broken lines, symbols
    │
    ▼
RecursiveCharacterTextSplitter ──► 400-char chunks, 50 overlap
    │
    ▼
HuggingFace Embeddings ──► all-MiniLM-L6-v2 vectors
    │
    ▼
FAISS VectorStore ──► Indexed & searchable
    │
    ▼
User Question ──► Retriever (top 3 chunks)
    │
    ▼
Groq API (LLaMA 3.1 8B) ──► Answer generation
    │
    ▼
Gradio ChatInterface ──► Live chat UI
```

---

## 🛠️ Tech Stack

| Layer          | Tool                                       |
|----------------|--------------------------------------------|
| PDF Loader     | PyMuPDF via LangChain                      |
| Text Cleaning  | Python `re` (regex)                        |
| Text Splitting | LangChain `RecursiveCharacterTextSplitter` |
| Embeddings     | `sentence-transformers/all-MiniLM-L6-v2`   |
| Vector Store   | FAISS (CPU)                                |
| LLM            | LLaMA 3.1 8B via Groq API                  |
| Chat UI        | Gradio `ChatInterface`                     |
| Runtime        | Google Colab (free tier)                   |

---

## ⚡ Quick Start

### 1. Get a free Groq API key
Go to [console.groq.com](https://console.groq.com) → sign up (free) → copy your API key.

### 2. Open the notebook in Colab
Click the badge at the top of this README, or go to:

```
File → Open notebook → GitHub → paste this repo URL → open RAG_Chatbot.ipynb
```

### 3. Add your API key
In the notebook, find and update this line:
```python
client = Groq(api_key="YOUR_GROQ_API_KEY_HERE")
```

### 4. Run all cells
```
Runtime → Run all
```
Upload your PDF when prompted by the file upload widget.

### 5. Chat!
A Gradio public URL will appear. Open it and ask anything about your PDF.

---

## 📁 Repository Structure

```
pdf-rag-chatbot/
│
├── RAG_Chatbot.py        # Main notebook — run this in Colab
├── requirements.txt      # All Python dependencies
├── README.md             # This file
└── .gitignore            # Files excluded from version control
```

---

## 📦 Dependencies

Install locally (optional — not needed for Colab):
```bash
pip install -r requirements.txt
```

Key packages:
```
langchain / langchain-community / langchain-huggingface
pymupdf / pypdf
faiss-cpu
sentence-transformers
groq
gradio
```

---

## 🔑 Keeping Your API Key Safe

Never hardcode your API key in committed code. For local use, create a `.env` file (already in `.gitignore`):

```env
GROQ_API_KEY=your_actual_key_here
```

For Colab, use Colab Secrets (recommended):
```python
from google.colab import userdata
client = Groq(api_key=userdata.get("GROQ_API_KEY"))
```

---

## 🧠 Design Decisions

**Why clean before splitting?**
PDFs often contain LaTeX math symbols, broken mid-word line breaks, and extra whitespace. Cleaning first ensures all chunks contain readable, meaningful text.

**Why chunk size 400 with k=3?**
Smaller focused chunks with only the top 3 retrieved keeps the total context concise, preventing the LLM from getting confused by irrelevant text.

**Why Groq over local flan-t5?**
`flan-t5-large` cannot reason over long academic documents — it echoes fragments instead of generating answers. Groq's LLaMA 3.1 8B handles open-ended QA correctly and is free for personal use.

---

## ⚠️ Limitations

- Gradio share links expire after **1 week** (free Colab tier)
- Colab sessions reset — notebook must be re-run each time
- Very large PDFs (100+ pages) may take 1–2 minutes to embed
- Groq free tier has rate limits (generous for personal use)

---

## 🔮 Future Improvements

- [ ] Multi-PDF support in one session
- [ ] Google Drive persistence for the FAISS index
- [ ] Multi-turn conversation memory
- [ ] Deploy to Hugging Face Spaces (permanent URL)
- [ ] Support DOCX and TXT file formats
- [ ] Streaming responses in Gradio

---

## 🙏 Acknowledgements

- [LangChain](https://langchain.com) — RAG pipeline framework
- [Groq](https://groq.com) — Free, ultra-fast LLaMA 3 inference
- [Gradio](https://gradio.app) — Instant chat UI
- [HuggingFace](https://huggingface.co) — Embedding model hosting
- [Meta AI](https://ai.meta.com) — LLaMA 3 model
