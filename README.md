# SDD Orchestrator

A **PySide6 desktop application for Spec-Driven Development (SDD)**. It brings
workspace setup, a local knowledge base, AI CLI discovery, chat, and codebase
analysis into one interface, with an OpenSpec-oriented workspace structure.

> **Status:** active prototype. This README describes the implementation in
> this repository today; planned OpenSpec and graph workflows are not described
> as finished features.

## Features

- **Workspace management:** open, validate, and initialize an SDD workspace
  with the expected knowledge-base and OpenSpec folders.
- **Knowledge base:** browse Markdown pages and import PDF, Markdown, TXT, RST,
  and DOCX files into `knowledge-base/raw/`.
- **AI CLI discovery:** scan for Claude Code, Codex, Gemini CLI, Antigravity,
  Aider, GitHub Copilot CLI, Ollama, and Continue. On macOS, discovery can probe
  the login shell to find tools missing from a desktop app's `PATH`.
- **Chat:** send prompts to a selected local CLI and stream responses in the
  app. Structured tool-stage parsing is implemented for Claude; other CLI
  integrations use best-effort arguments and plain-text output.
- **Codebase analysis:** inspect files and summarize recognized languages,
  source lines, and largest files.
- **Desktop UI:** MVC-style separation, background workers for long-running
  tasks, and light/dark themes.

## Architecture

The application lives in [`sdd-orchestrator/`](sdd-orchestrator/):

| Area | Responsibility |
| --- | --- |
| `models/` | Workspace, wiki, CLI, chat, graph-data, and codebase logic |
| `views/` | PySide6 windows, navigation, and feature panels |
| `controllers/` | Connect models and views using Qt signals |
| `workers/` | Run CLI scans, chat, imports, and codebase analysis off the UI thread |
| `utils/` | Workspace paths, Markdown parsing, file helpers, and shell `PATH` resolution |
| `styles/` | Global light and dark QSS themes |

The application entry point is
[`sdd-orchestrator/main.py`](sdd-orchestrator/main.py).

## Requirements and setup

- Python 3.11 or newer
- PySide6
- A graphical desktop session
- Optional: supported AI CLI tools installed and configured locally

From the repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install PySide6
cd sdd-orchestrator
python main.py
```

On Windows, activate the environment with `.venv\Scripts\activate` instead.
There is currently no `pyproject.toml` or requirements file, so the GUI
dependency is installed directly.

On launch, open an existing SDD workspace or choose a folder to initialize.
Initialization creates the app's expected knowledge-base and OpenSpec
directories and starter files. Workspace-dependent features become available
after a workspace is opened.

## Current scope and limitations

- OpenSpec workspace folders are created, but the guided
  propose/review/apply/validate workflow, editor, validation, history, and
  export are not implemented.
- Graph JSON loading and HTML generation exist in the model, but there is no
  graph view wired into the running application.
- CLI argument conventions vary between tools and have not been verified
  against every CLI version. Authentication and provider setup remain local
  prerequisites.
- `sdd-orchestrator/tests/` currently contains no automated tests.
- No screenshots or demo assets are currently included.

## Contribution

This repository contains the PySide6 application foundation and the workspace,
knowledge-base, CLI discovery, chat, and codebase-analysis features described
above. This description is limited to work visible in this repository; it does
not imply employer or third-party sponsorship, ownership, or contributions.
