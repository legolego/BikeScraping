# BikeScraping: hybrid RAG search over mountain bike articles

An early-stage personal project that scrapes mountain bike news articles and finds the ones most relevant to a question, using hybrid retrieval and an LLM.

## Pipeline

1. **Collect links** (`1.1`): pulls recent Pinkbike article links from its RSS feed.
2. **Scrape and clean** (`2.1`): downloads each article with Requests or Selenium and parses the title, author, date, and body text with BeautifulSoup into JSON (58 articles in `data/`).
3. **Index** (`3.1`): stores the articles in a Chroma vector database using `intfloat/multilingual-e5-large-instruct` sentence-transformer embeddings.
4. **Search** (`4.1`–`4.3`): runs vector search (Chroma) and keyword search (BM25 via `bm25s`, with stemming), then merges the two rankings with reciprocal rank fusion (k = 60).
5. **Generate** (`5.1`): sends the top results to an open-source LLM (Qwen QwQ-32B) through the OpenRouter API.

## Files

- `Utils.py`: scraping and parsing helpers
- `Chroma_func.py` and `BM25_func.py`: search helpers
- `data/`: collected links, article text, and fused search results

## Setup

```bash
pip install -r requirements.txt
```

ChromaDB needs Microsoft C++ Build Tools on Windows. Set an `OPENROUTER_API_KEY` environment variable before running step 5.
