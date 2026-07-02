# Summarization App

A Streamlit app for uploading a PDF and getting both **semantic search** and an **automated summary** of its contents, powered by OpenAI via [LlamaIndex](https://www.llamaindex.ai/) (formerly GPT Index).

## How It Works

1. Upload a PDF through the web UI.
2. The file is saved to the local `data/` folder and rendered inline in the browser.
3. **Semantic search:** the PDF is indexed with `GPTSimpleVectorIndex`, and your typed query is answered using that index.
4. **Summarization:** the PDF is indexed with `GPTListIndex` and queried with a "Summarize the document" prompt using tree-summarization mode.

## Project Structure

```
.
├── app.py               # Streamlit app (main entry point)
├── data/
│   └── ca3121en.pdf     # Sample PDF included in the repo
└── requirements.txt
```

## Getting Started

### Prerequisites

- Python 3.8+
- An [OpenAI API key](https://platform.openai.com/api-keys)

### Installation

```bash
git clone <your-repo-url>
cd summarization-app
pip install -r requirements.txt
```

### Configure your API key

Create a `.env` file in the project root:

```
OPENAI_API_KEY=your-openai-api-key-here
```

The app loads this automatically via `python-dotenv`. Never commit your `.env` file — add it to `.gitignore`.

### Run the app

```bash
streamlit run app.py
```

Then open the local URL Streamlit prints (usually `http://localhost:8501`).

## Usage

1. Upload a PDF.
2. The document appears in the left column.
3. In the middle column, type a question and check "search" to run semantic search over the document.
4. The right column automatically generates and displays a summary of the document.

## ⚠️ Important: Update Required Before Running

This project pins to a very old LlamaIndex API (`GPTSimpleVectorIndex`, `GPTListIndex`, imported directly from `llama_index`). These classes were renamed/removed in modern versions of the `llama-index` package (v0.10+, split into `llama-index-core` and provider packages). To get this running today, either:

- Pin an old version in `requirements.txt`, e.g. `llama-index==0.5.x` (matching whatever version those classes existed in), **or**
- Migrate `app.py` to the current LlamaIndex API — roughly:
  ```python
  from llama_index.core import SimpleDirectoryReader, VectorStoreIndex, SummaryIndex
  ```
  with `VectorStoreIndex` replacing `GPTSimpleVectorIndex` and `SummaryIndex` replacing `GPTListIndex`.

## Tech Stack

- [Streamlit](https://streamlit.io/) — web UI
- [LlamaIndex](https://www.llamaindex.ai/) — document indexing, semantic search, and summarization
- [PyPDF2](https://pypdf2.readthedocs.io/) — PDF handling
- [python-dotenv](https://pypi.org/project/python-dotenv/) — environment variable loading
- OpenAI API — underlying LLM for search and summarization

