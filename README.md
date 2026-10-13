# RealEstate_RAG_Translator
# Real Estate RAG Translator

OCR + RAG + FAISS + Sentence Transformers + Ollama + Multilingual Translation

A Natural Language Processing mini project (T.E. Sem VI, Vidyalankar Institute of Technology, Department of Computer Engineering) that lets users upload real-estate documents, ask questions about them, look up terminology, and translate content into **English, Marathi, and Hindi**, while preserving names, dates, numbers, identifiers, addresses, and legal information.

> Full project report: [`docs/NLP_Mini_Project_Report_Final.docx`](docs/NLP_Mini_Project_Report_Final.docx)

---

## Features

- **Document ingestion**: PDF text extraction (PyMuPDF) with OCR fallback (Tesseract) for scanned pages
- **Semantic search**: chunking + Sentence Transformers embeddings + FAISS vector index
- **RAG question answering**: retrieved evidence is passed to a local Ollama LLM with strict grounding and language instructions
- **Multilingual output**: English, standard Marathi, standard Hindi
- **Glossary**: real-estate terms with English / Marathi / Hindi meanings, fuzzy search, and LLM fallback for unknown terms
- **Controlled translation**: critical values (numbers, dates, IDs, addresses) are preserved
- **Local LLM**: no remote commercial API needed
- **Web UI + REST API** served by FastAPI

## Tech Stack

| Category | Tools |
| --- | --- |
| Backend | Python, FastAPI, Pydantic |
| Document processing | PyMuPDF, Tesseract OCR |
| Embeddings | Sentence Transformers |
| Vector search | FAISS |
| Database | SQLite |
| Language model | Ollama (local LLM) |
| Frontend | HTML / CSS / JavaScript served via FastAPI |

## Pipeline

1. Upload a document through the web interface
2. Extract text page-wise (OCR where required)
3. Split text into chunks with page references
4. Store document metadata and chunks in SQLite
5. Embed chunks with Sentence Transformers
6. Index vectors in FAISS
7. Embed the user question and retrieve the most relevant chunks
8. Send the evidence to Ollama with language and grounding rules
9. Return the answer in the requested language

## Project Structure

> Adjust this to match your actual folders (run `tree /F` in the project root).

```
RealEstate_RAG_Translator/
├── main.py            # FastAPI app and API routes
├── database.py        # SQLite operations
├── rag.py             # Retrieval + generation logic
├── ocr.py             # PDF / OCR extraction
├── glossary.py        # Glossary seeding
├── config.py          # Ollama endpoint/model, upload dir, DB path
├── frontend/          # HTML, CSS, JS
├── docs/              # Project report
├── requirements.txt
└── README.md
```

## Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com/download) installed and running
- [Tesseract OCR](https://github.com/UB-Mannheim/tesseract/wiki) installed and added to PATH (needed for scanned documents)
- Git

## Setup and Run (Windows / PowerShell)

```powershell
# 1. Clone the repository
git clone https://github.com/<your-username>/RealEstate_RAG_Translator.git
cd RealEstate_RAG_Translator

# 2. Create and activate a virtual environment
python -m venv venv
.\venv\Scripts\Activate.ps1
# If you get an execution-policy error:
# Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

# 3. Install dependencies
pip install -r requirements.txt

# 4. Pull the Ollama model configured in config.py
ollama pull <your-model-name>

# 5. Start the backend
uvicorn main:app --reload
```

Open **http://127.0.0.1:8000** in your browser.

> If `main.py` lives inside a subfolder (for example `backend/`), use `uvicorn backend.main:app --reload` instead.

## Usage

1. Make sure Ollama is running.
2. Open the web app and **upload** a real-estate PDF.
3. Wait for extraction, chunking, embedding, and indexing to finish.
4. **Ask** a question and choose English, Marathi, or Hindi.
5. Use **Glossary search** for terms (unknown terms are explained by the LLM).
6. Use **Translate** to convert text while preserving critical values.

## API Endpoints

| Endpoint | Purpose |
| --- | --- |
| `GET /` | Serves the frontend |
| `GET /api/health` | Health check |
| `GET /api/documents` | Lists indexed documents |
| `POST /api/upload` | Upload, extract, chunk, store, and index a document |
| `POST /api/ask` | RAG question answering in the requested language |
| `POST /api/translate` | Translate text to the requested language |
| `GET /api/glossary` | Returns the full glossary |
| `GET /api/glossary/search` | Glossary search with LLM fallback |
| `POST /api/reindex` | Rebuild the vector index |

Interactive API docs: **http://127.0.0.1:8000/docs**

## Limitations

- Answer quality depends on the chosen local Ollama model and available hardware
- OCR quality on low-quality scans affects retrieval
- Translations and explanations should be verified by a human before any legal use
- This tool does not replace professional legal or real-estate advice

## Future Scope

- Multilingual OCR and better handling of complex scanned layouts
- Page-level citations for each answer
- More Indian languages (Gujarati, Bengali, Tamil, Telugu)
- Reranking models for better retrieval
- Automated evaluation with a Q&A benchmark
- Authentication and secure deployment

## Team

| Roll No. | Name |
| --- | --- |
| 23102C0085 | Chirayu Pritmani |
| 23102C0084 | Samruddhi Gawade |
| 23104B0023 | Swar Sawalkar |

**Subject:** Natural Language Processing | **Guide:** Prof. Suja Jayachandran | **Institute:** Vidyalankar Institute of Technology, Mumbai

## License

For academic purposes.
