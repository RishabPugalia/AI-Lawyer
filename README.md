# ⚖️ AI Lawyer — RAG-Powered Legal Document Assistant

An AI-powered legal chatbot that uses Retrieval-Augmented Generation (RAG) to answer questions grounded in the specific legal documents you upload — not general knowledge, not guesses. Upload a PDF, and every answer is retrieved from and traceable back to that document, which keeps the chatbot reliable for the kind of accuracy legal use cases demand.

## Features

- 📂 Upload and analyze legal documents (PDF)
- 🔍 Retrieve relevant passages using a FAISS vector database
- 🤖 Ask questions and get answers grounded in the uploaded document, powered by Groq
- 📜 Summarize legal documents
- 📄 Generate and download an AI-generated report of your Q&A session

## Project Structure

```
├── frontend.py          # Streamlit UI for AI Lawyer
├── rag_pipeline.py       # Retrieval-Augmented Generation pipeline (LLM calls, summarization, report generation)
├── vector_database.py    # Document loading, chunking, embeddings, and FAISS indexing
├── requirements.txt      # Python dependencies
└── README.md             # Project documentation
```

`main.py` is a legacy, unused alternate entrypoint kept for reference — the app's entrypoint is `frontend.py`.

## Technologies Used

- **[Groq](https://groq.com/)** — fast LLM inference (currently `openai/gpt-oss-120b`)
- **[LangChain](https://www.langchain.com/)** — RAG orchestration
- **[Streamlit](https://streamlit.io/)** — chatbot UI
- **[FAISS](https://github.com/facebookresearch/faiss)** — vector similarity search
- **[Sentence-Transformers](https://www.sbert.net/)** (`all-MiniLM-L6-v2`, via HuggingFace) — document embeddings
- **[pdfplumber](https://github.com/jsvine/pdfplumber)** — PDF text extraction
- **[ReportLab](https://www.reportlab.com/)** — downloadable PDF report generation

> Groq periodically retires older model IDs. If you see a `groq.BadRequestError` at runtime, check [Groq's deprecation page](https://console.groq.com/docs/deprecations) and update the model name in `rag_pipeline.py`.

## Installation & Setup

### 1️⃣ Clone the repository

```bash
git clone <your-repo-url>
cd ai-lawyer
```

### 2️⃣ Set up a virtual environment

```bash
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows
```

### 3️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

### 4️⃣ Add your Groq API key

Create a `.env` file in the project root:

```
GROQ_API_KEY=your-groq-api-key-here
```

Get a free key at [console.groq.com/keys](https://console.groq.com/keys).

## Usage

```bash
streamlit run frontend.py
```

1. Upload a legal document (PDF)
2. Ask a question, or click **Summarize Document**
3. Download a PDF report of your session with **Download Report**

## How It Works

1. **Upload PDF** — the document is saved and loaded with `pdfplumber`
2. **Chunking & embedding** — text is split into overlapping chunks and embedded with `all-MiniLM-L6-v2`
3. **Vector indexing** — chunks are indexed in a FAISS store, filterable by source file
4. **Retrieval** — on each question, the most relevant chunks from *that* document are retrieved
5. **LLM response** — Groq generates an answer grounded only in the retrieved context
6. **Report generation** — the full Q&A session can be exported as a PDF via ReportLab

## Deployment on Streamlit Community Cloud

### 1️⃣ Push code to GitHub

```bash
git add .
git commit -m "Deploy AI Lawyer"
git push origin main
```

### 2️⃣ Deploy

- Go to [Streamlit Community Cloud](https://share.streamlit.io/) → **Deploy a new app**
- Select your repo, branch `main`, and entrypoint `frontend.py`
- Under **Advanced settings**, set the Python version to **3.11** or **3.12** (some dependencies, like `faiss-cpu` and `torch`, don't yet publish wheels for the very latest Python release)
- In the **Secrets** field, add:
  ```
  GROQ_API_KEY = "your-groq-api-key-here"
  ```
- Click **Deploy**

## 🌐 Deployed Version

Try the live app: **[AI Lawyer](https://ai-lawyer-by-rishab.streamlit.app/)**

## 🎯 Future Improvements

- 📝 Support additional document formats (DOCX, TXT)
- ⚡ Improve response speed and retrieval accuracy
- 🔗 Integrate external legal databases for richer context

## License

MIT — see [LICENSE](LICENSE).
