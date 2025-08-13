# Llama 3 Docker Compose Package (Windows)

This package provides a ready-to-run Docker Compose setup for running **Llama 3** via **Ollama**, plus a lightweight **Parser API** and a **Scanner CLI** that:
- parses C# and XAML (WPF) files,
- builds a simple graph and UI mapping,
- generates a markdown report from a template,
- builds embeddings (sentence-transformers) and a FAISS index for RAG.

Designed for Windows machines running Docker Desktop. The default LLM backend is **Ollama** serving a local Llama 3 model (model name configurable).

## Contents
- `docker-compose.yml` — services: `ollama`, `parser-api`, `scanner` (scanner is optional CLI service)
- `parser-api/` — FastAPI app that exposes `/parse/xaml` and `/parse/file` endpoints
- `scanner/` — Python CLI to scan a mounted workspace and produce `/data/output/report.md` and `embeddings.index`
- `docs/` — usage and template example
- `.env.example` — copy to `.env` and edit

## Quick start (Windows)
1. Unzip the package to a folder, e.g., `C:\tools\arch-scanner`
2. Edit `.env` (copy `.env.example`) and set `HOST_WORKSPACE` to your repo path.
3. Open PowerShell and run:
   ```powershell
   cd C:\tools\arch-scanner
   docker compose up -d
   ```
   Ollama will start and attempt to serve; the first run may pull the model `LLM_MODEL` (internet required).
4. Run scanner CLI (from a new PowerShell):
   ```powershell
   docker exec -it scanner_api python /app/cli.py all --workspace /workspace --template /workspace/doc-template/template.md
   ```
   (The scanner container mounts your workspace to `/workspace` and outputs to `/data/output` inside the container.)

## Notes
- Ollama: this setup expects you to have Ollama image available and will use `ollama serve`. If you prefer `gpt4all` or other runtimes, update `docker-compose.yml` and the scanner `config`.
- Models: set `LLM_MODEL` in `.env` to the model you have (e.g., `llama-3-8b`), and ensure the model is available under `./models` (pre-seeding recommended for offline use).
- Resources: Llama 3-like models need significant RAM; pick a quantized model appropriate to your machine.

## License
MIT
