# Jaydari RAG · API Foundation

A Python API foundation for a retrieval-augmented generation project. The current default branch establishes a FastAPI application, a health endpoint, and typed configuration for a future retrieval and language-model pipeline.

## Implemented in `main`

- FastAPI application with an application lifespan handler.
- Pydantic Settings configuration loaded from environment variables and an optional `.env` file.
- `GET /health` endpoint returning `{"status": "OK"}`.
- Automatically generated API documentation through FastAPI.

## Planned pipeline

```text
Documents → chunking → embeddings → Qdrant retrieval
Question + retrieved context → language model → answer
```

Configuration includes Qdrant collection settings, `sentence-transformers/all-MiniLM-L6-v2` embeddings, TinyLlama, optional adapter settings, retrieval thresholds, and chunk sizes. These settings describe the intended pipeline: **document ingestion, vector search, and answer generation are not implemented in the current default branch**. Installing model dependencies does not activate a working RAG workflow.

## Technology

Implemented foundation: Python, FastAPI, Uvicorn, Pydantic, and pydantic-settings.

Declared dependencies for further development: qdrant-client, sentence-transformers, Transformers, PyTorch, and PEFT.

## Run locally

Use Python 3.11, as in the project's original environment setup.

```bash
git clone https://github.com/shohruhinomjonov691-hub/jaydari_rag.git
cd jaydari_rag
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
uvicorn app.main:app --reload
```

On Windows, activate with `.venv\Scripts\activate`.

- API documentation: `http://127.0.0.1:8000/docs`
- Health endpoint: `http://127.0.0.1:8000/health`

```bash
curl http://127.0.0.1:8000/health
```

Expected response:

```json
{"status": "OK"}
```

The health endpoint does not test Qdrant connectivity or model readiness. No Qdrant server or model download is required by the current application startup code, although the requirements install the libraries intended for those components.

## Configuration

All settings have defaults and are defined in [`app/core/config.py`](app/core/config.py). Optional environment names include:

| Setting | Purpose |
| --- | --- |
| `QDRANT_URL`, `QDRANT_API_KEY` | Future vector-store connection |
| `QDRANT_COLLECTION` | Collection name; default `documents` |
| `EMBEDDING_MODEL` | Intended embedding model |
| `LLM_MODEL`, `LLM_ADAPTER_PATH`, `LLM_DEVICE` | Intended generator/adapter configuration |
| `TOP_K`, `MIN_SCORE`, `MAX_CONTEXT_CHARS` | Intended retrieval/context controls |
| `CHUNK_SIZE_CHARS`, `CHUNK_OVERLAP_CHARS` | Intended document chunking controls |

Never commit real API keys. The current route does not consume these RAG settings.

## Structure & status

```text
app/main.py          Application and lifespan
app/api/routes.py    Health route
app/core/config.py   Typed settings
requirements.txt     Declared dependencies
```

This project is at the API scaffolding stage. The default `main` branch is the basis of this README; development branches should be reviewed separately before presenting additional functionality. A working document-to-answer demonstration, retrieval evaluation, authentication, and automated tests remain future work.

## Author

[Shokhrukhbek Inomjonov](https://github.com/shohruhinomjonov691-hub)
