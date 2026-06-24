# CLAUDE.md

## Project Overview

This is **Generative AI for Beginners** — a 21-lesson educational curriculum by Microsoft teaching Generative AI fundamentals and application development. It is a content-heavy tutorial repository, not a production application. The primary artifacts are Markdown lesson files, Jupyter notebooks, and standalone code examples in Python, TypeScript, JavaScript, and .NET.

## Repository Structure

```
├── 00-course-setup/ through 21-meta/   # 22 numbered lesson directories
│   ├── README.md                       # Lesson content (primary artifact)
│   ├── images/                         # Lesson-specific images
│   ├── python/                         # Python examples (.py and .ipynb)
│   ├── typescript/                     # TypeScript examples (self-contained apps)
│   ├── js-githubmodels/                # JavaScript GitHub Models examples
│   └── dotnet/                         # .NET interactive notebooks (.dib)
├── translations/                       # 54 language translations (automated)
├── translated_images/                  # Translated image assets
├── shared/python/                      # Shared Python utilities (api_utils, env_utils, input_validation)
├── docs/                               # Documentation site assets
├── images/                             # Root-level images
├── presentations/                      # Slide decks
├── .devcontainer/                      # GitHub Codespaces / Dev Container config
└── .github/workflows/                  # CI: Markdown validation only
```

### Lesson Directory Conventions

- **"Learn" lessons** (e.g., 01, 02, 03, 10, 12-14): README.md only, no runnable code
- **"Build" lessons** (e.g., 04-09, 11, 15, 18-21): Include working code examples
- Code examples exist for multiple providers: `aoai-` (Azure OpenAI), `oai-` (OpenAI), `githubmodels-` (GitHub Models)
- Each lesson is self-contained and independently runnable

## Tech Stack

**Python** (primary): 3.10+ (pinned to 3.12.10 in `.python-version`)
- `openai`, `azure-ai-inference`, `python-dotenv`, `tiktoken`, `pandas`, `numpy`, `matplotlib`, `scikit-learn`
- Jupyter notebooks for interactive examples

**TypeScript/JavaScript** (secondary): Node.js
- `openai`, `@azure-rest/ai-inference`, `@azure/core-auth`
- Each TypeScript example is a self-contained app with its own `package.json` and `tsconfig.json`

**.NET**: Interactive notebooks (`.dib` format), not standalone projects

## Setup

```bash
# Environment variables: copy template and fill in API keys
cp .env.copy .env

# Python
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# TypeScript (per-lesson, e.g.)
cd 06-text-generation-apps/typescript/recipe-app && npm install

# Dev Container (recommended): Open in GitHub Codespaces or VS Code Dev Containers
```

### Required Environment Variables

Defined in `.env` (see `.env.copy` for template):
- `OPENAI_API_KEY` — OpenAI API
- `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_DEPLOYMENT` — Azure OpenAI
- `AZURE_OPENAI_EMBEDDINGS_DEPLOYMENT`, `AZURE_OPENAI_API_VERSION` — Azure embeddings
- `HUGGING_FACE_API_KEY` — Hugging Face
- `GITHUB_TOKEN` — GitHub Models

## CI / Validation

There are **no unit tests or integration tests**. The only CI is Markdown validation on PRs to `main`:

**Workflow**: `.github/workflows/validate-markdown.yml` — runs on PR when `.md` or `.ipynb` files change (excluding `translations/`). Five checks:
1. **Broken relative paths** — all relative links must resolve
2. **Paths have tracking** — relative links must end with `?WT.mc_id=academic-105485-koreyst`
3. **URLs have tracking** — Microsoft domain URLs must include the tracking ID
4. **URLs don't have locale** — no `/en-us/` or `/en/` in URLs
5. **Broken URLs** — external URLs must be reachable

## Code Style

### Python
- **Formatter**: Black (line length 100, target py310+)
- **Linter**: Ruff (E, W, F, I, B, C4, UP, S rules; E501 ignored)
- **Import sorting**: isort (black profile, line length 100)
- **Type checking**: mypy (Python 3.10, lenient — no strict untyped enforcement)
- Use `python-dotenv` for env vars; never hardcode credentials

### TypeScript/JavaScript
- **Formatter**: Prettier (single quotes, trailing commas es5, 100 char width, 2-space indent, LF)
- **Linter**: ESLint (`eslint:recommended` + `@typescript-eslint/recommended` for .ts)
- Key rules: `no-var`, `eqeqeq`, `prefer-const`, `no-eval`, explicit return types (warn)

### Markdown
- URLs wrapped in `[text](url)` — no bare URLs
- Relative links start with `./` or `../`
- All Microsoft domain URLs and relative paths include tracking: `?WT.mc_id=academic-105485-koreyst`
- No country-specific locales in URLs (no `/en-us/`)
- Images in `./images/` with descriptive English filenames using dashes

## Key Conventions

- **Educational focus**: Code examples prioritize clarity over production patterns. Keep them simple.
- **Multi-provider support**: When adding code examples, provide variants for Azure OpenAI, OpenAI API, and GitHub Models where applicable.
- **Shared utilities**: Reusable Python code lives in `shared/python/` (API helpers, env validation, input sanitization).
- **File naming**: `{provider}-{example-name}.{ext}` — e.g., `aoai-app.py`, `oai-assignment.ipynb`, `githubmodels-assignment.ipynb`.
- **Translations**: Managed by automated GitHub Actions. Do not edit `translations/` or `translated_images/` manually. Do not submit partial translations.
- **No secrets in code**: All credentials come from `.env` via `python-dotenv` (Python) or `dotenv` (Node.js).

## PR Guidelines

- Fork first, one PR per logical change
- Test code in both Python and TypeScript when applicable
- Ensure tracking IDs on all Microsoft URLs and relative paths
- PR titles: `[Lesson XX] Description` or `Update README for lesson XX`
- Sign Microsoft CLA (automated on first PR)
- Translation PRs must be complete (all files for a language)

## Common Tasks

**Run a Python example**: `cd XX-lesson/python && python aoai-app.py`
**Run a notebook**: Open in VS Code with Jupyter extension, or `jupyter notebook`
**Run a TypeScript example**: `cd XX-lesson/typescript/app-name && npm install && npm run build && npm start`
**Build docs PDF**: `npm run convert` (uses docsify-to-pdf)
