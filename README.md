# VaultMind

**VaultMind** — a fully private, local RAG system for secure document intelligence. Your data never leaves your machine.

Portfolio project by [Rushi021](https://github.com/Rushi021). Clone and run on your laptop:

```bash
git clone https://github.com/Rushi021/private-RAG-system.git
cd private-RAG-system
```

## Problem VaultMind solves

Teams and developers often need to ask questions over sensitive documents (clinical notes, SOPs, internal guidelines, policy docs) without sending that data to public SaaS tools.

Most generic AI chat tools are not designed for strict privacy boundaries, local control, or domain-specific retrieval quality. **VaultMind** is a private, controllable RAG backend that runs locally and can be exposed only where you choose.

## What VaultMind is

**VaultMind** is a Retrieval-Augmented Generation (RAG) system (built on PrivateGPT foundations) focused on secure, local document intelligence.

It lets you:
- ingest your own documents,
- create embeddings and searchable vector indexes,
- query them through an OpenAI-style API,
- use a built-in Gradio UI for interactive usage.

By default, **VaultMind** is designed for local/private operation, with optional cloud model integrations based on configuration.

## What it is good at

- **Private document Q&A:** keeps document retrieval and response generation under your control.
- **Flexible model backends:** supports local (`llamacpp`, `ollama`) and cloud-style providers through profile settings.
- **API-first architecture:** easy to integrate into internal apps and workflows via HTTP endpoints.
- **Structured ingestion flow:** document upload, indexing, retrieval, chat/completions, and summarization are separated cleanly.
- **Config-driven deployment:** switch profiles with YAML/env vars instead of rewriting code.

## How to use it

### 1) Install dependencies

Use Python 3.11 and Poetry. Install dependencies with the extras needed for your selected profile (example for local llama.cpp + HF embeddings + qdrant + UI):

```bash
poetry install --extras "ui llms-llama-cpp vector-stores-qdrant embeddings-huggingface"
```

### 2) Pick/confirm config

Start from `settings.yaml` or a profile file like:
- `settings-local.yaml`
- `settings-ollama.yaml`
- `settings-openai.yaml`
- `settings-docker.yaml`

Current default profile expects:
- `llm.mode: llamacpp`
- `embedding.mode: huggingface`
- `vectorstore.database: qdrant`
- server on port `8001`

### 3) Download models (if running local mode)

```bash
make setup
```

This fetches the configured embedding model and local GGUF LLM artifacts.

### 4) Run the server

```bash
make run
```

For local development:

```bash
make dev-windows
```

### 5) Docker (local build only)

From the repository root, build and start services from source (no published **VaultMind** image is required):

```bash
docker compose up --build
```

Dependency images (e.g. Ollama, Traefik) may still be pulled from public registries as defined in `docker-compose.yaml`.

### 6) Ingest documents and ask questions

- Use API routes under `/v1` (`/ingest/file`, `/chat/completions`, `/chunks`, `/summarize`, etc.).
- Or use the Gradio UI (enabled by `ui.enabled: true`).

## High-level architecture

- `private_gpt/components/`: LLM, embedding, ingest, vector store wiring (Python package name remains `private_gpt` — internal identifier).
- `private_gpt/server/`: API routers and services
- `private_gpt/settings/`: YAML + env profile loading
- `scripts/`: setup and ingestion helpers
- `local_data/`, `models/`: runtime storage for indexes and model files

## Notes

- **VaultMind** is an independently maintained portfolio project and is not the official upstream PrivateGPT repository.
- Derived from the open-source PrivateGPT codebase, licensed under Apache-2.0. See [`LICENSE`](LICENSE).
