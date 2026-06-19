# SearchService - Setup Guide

Complete cross-platform instructions to get the SearchService development environment running.

## Prerequisites

- Python 3.11 or newer
- Git
- (Optional but recommended) A terminal with good Unicode support

## 1. Clone the Repository

```bash
git clone https://github.com/digitaldesignerjazz/SearchService.git
cd SearchService
```

## 2. Create a Virtual Environment

Choose the instructions for your operating system and shell.

### Linux / macOS (bash or zsh)

```bash
python -m venv .venv
source .venv/bin/activate
```

### Windows - Command Prompt (cmd.exe)

```cmd
python -m venv .venv
.\ .venv\Scripts\activate.bat
```

### Windows - PowerShell (Recommended)

```powershell
python -m venv .venv
.\ .venv\Scripts\Activate.ps1
```

> **Important PowerShell Note:**
> If you see an error like `running scripts is disabled on this system`, run the following command **once** (you may need to run PowerShell as Administrator for the first time):
>
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
>
> After that, the activation command above will work normally.

### Windows - Git Bash

```bash
python -m venv .venv
source .venv/Scripts/activate
```

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

> **Tip:** You can uncomment lines in `requirements.txt` later (e.g. `whoosh`, `sentence-transformers`, `chromadb`) when you want to enable full hybrid/vector search.

## 4. Run the Development Server

```bash
uvicorn app.main:app --reload
```

You should see output similar to:

```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

## 5. Verify Everything Works

### Option A: Interactive API Documentation (Recommended)

Open your browser and visit:

**http://127.0.0.1:8000/docs**

This loads Swagger UI where you can try all endpoints visually.

### Option B: Command Line Tests

Open a **new terminal** (keep the server running) and run:

```bash
# Health check
curl http://127.0.0.1:8000/health

# Search example
curl -X POST http://127.0.0.1:8000/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{"query": "Hannover mesh nodes QNET Soilnova", "limit": 5, "explain": true}'

# Ingest example
curl -X POST http://127.0.0.1:8000/api/v1/ingest \
  -H "Content-Type: application/json" \
  -d '{
    "source": "prototype",
    "component": "soilnova",
    "data": {"ph": 7.4, "temp_c": 18.2},
    "metadata": {"location": "Hannover", "timestamp": "2026-06-20T01:00:00Z"}
  }'
```

## Troubleshooting

| Problem                              | Solution                                                                 |
|--------------------------------------|--------------------------------------------------------------------------|
| PowerShell script execution error    | Run `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` |
| `ModuleNotFoundError: No module named 'fastapi'` | Make sure you activated the venv and ran `pip install -r requirements.txt` |
| Port 8000 already in use             | Use `uvicorn app.main:app --reload --port 8001`                          |
| Activation command does nothing      | Check that you are in the correct directory and the venv was created     |
| Want to deactivate the venv          | Type `deactivate`                                                        |

## Next Steps

Once the server is running:

- Explore the full design in [ARCHITECTURE.md](ARCHITECTURE.md)
- Implement real adapters (mesh, blockchain, prototypes, agent swarms)
- Add Docker support for easy deployment on your Tenda Nova / xMesh nodes
- Enable vector search by uncommenting the relevant lines in `requirements.txt`

---

**Part of the Nexus ecosystem** — Hannover, Germany • 2026
