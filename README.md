<div align="center">

# l0l1

**SQL that learns. AI that validates. Privacy that protects.**

[![CI](https://github.com/skelf-research/l0l1-api/actions/workflows/ci.yml/badge.svg)](https://github.com/skelf-research/l0l1-api/actions/workflows/ci.yml)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docs](https://img.shields.io/badge/docs-skelfresearch.com-blue)](https://docs.skelfresearch.com/l0l1)

[Website](https://l0l1.skelfresearch.com) | [Documentation](https://docs.skelfresearch.com/l0l1) | [API Reference](https://docs.skelfresearch.com/l0l1/api) | [Examples](https://github.com/skelf-research/l0l1-api/tree/main/examples) | [Skelf Research](https://skelfresearch.com)

</div>

---

## What is l0l1?

l0l1 is a developer toolkit for SQL analysis that combines AI-powered validation with continuous learning. It detects PII, suggests improvements, and gets smarter from your successful queries.

```bash
# Validate with AI insights
$ l0l1 validate "SELECT * FROM users WHERE email LIKE '%@%'"

# Detect PII automatically
$ l0l1 check-pii "SELECT ssn, email FROM customers"
  Found: SSN, EMAIL_ADDRESS
  Anonymized: SELECT <SSN>, <EMAIL> FROM customers

# Learn from your patterns
$ l0l1 complete "SELECT * FROM orders WHERE"
  Suggestions based on 47 learned patterns...
```

## Installation

```bash
# Clone and install with uv (recommended)
git clone https://github.com/skelf-research/l0l1-api.git && cd l0l1-api
uv sync

# Or install from PyPI
pip install l0l1
```

**Requirements:** Python 3.11+, OpenAI or Anthropic API key

## Quick Start

```bash
# Set your API key
export OPENAI_API_KEY="sk-..."

# Validate a query
uv run l0l1 validate "SELECT * FROM users WHERE id = 1"

# Start the API server
uv run l0l1-serve
# API at http://localhost:8000, docs at /docs
```

## Core Features

| Feature | Description |
|---------|-------------|
| **AI Validation** | Multi-provider support (OpenAI, Anthropic) for query analysis |
| **PII Detection** | Presidio-powered detection and anonymization |
| **Continuous Learning** | Graph-based pattern learning from successful queries |
| **Multi-Interface** | CLI, REST API, Jupyter magic, VS Code extension |
| **Schema Aware** | Context-aware validation with schema introspection |

## Usage

### CLI

```bash
l0l1 validate "SELECT * FROM users"           # Validate syntax and semantics
l0l1 explain "SELECT COUNT(*) FROM orders"    # Get AI explanation
l0l1 check-pii "SELECT email FROM users"      # Detect sensitive data
l0l1 complete "SELECT * FROM orders WHERE"    # AI-powered completion
l0l1 serve                                    # Start API server
```

### REST API

```bash
# Start server
uv run l0l1-serve

# Validate query
curl -X POST http://localhost:8000/sql/validate \
  -H "Content-Type: application/json" \
  -d '{"query": "SELECT * FROM users", "schema_context": "users(id, name, email)"}'

# Check PII
curl -X POST http://localhost:8000/sql/check-pii \
  -H "Content-Type: application/json" \
  -d '{"query": "SELECT * FROM users WHERE email = '\''john@example.com'\''"}'
```

### Python SDK

```python
from l0l1.models.factory import ModelFactory
from l0l1.services.pii_detector import PIIDetector
from l0l1.services.learning_service import LearningService

# AI-powered validation
model = ModelFactory.get_default_model()
result = await model.validate_sql_query(
    "SELECT * FROM users WHERE id = 1",
    schema_context="users(id INT, name VARCHAR, email VARCHAR)"
)

# PII detection
detector = PIIDetector()
findings = detector.detect_pii("SELECT * FROM users WHERE ssn = '123-45-6789'")
anonymized, _ = detector.anonymize_sql(query)

# Learning from successful queries
learning = LearningService()
await learning.record_successful_query("workspace-1", query, execution_time=0.5)
suggestions = await learning.get_query_suggestions("SELECT * FROM", "workspace-1")
```

### Jupyter Integration

```python
# Load the magic extension
%load_ext l0l1.integrations.jupyter.magic

# Use magic commands
%%sql_validate
SELECT u.name, COUNT(o.id)
FROM users u JOIN orders o ON u.id = o.user_id
GROUP BY u.name
```

## Configuration

Create a `.env` file or set environment variables:

```bash
# Required: AI Provider (at least one)
OPENAI_API_KEY=sk-...
# ANTHROPIC_API_KEY=sk-ant-...

# Optional: Provider selection
L0L1_AI_PROVIDER=openai          # or "anthropic"

# Optional: Features
L0L1_ENABLE_PII_DETECTION=true
L0L1_ENABLE_LEARNING=true

# Optional: Server
L0L1_API_PORT=8000
L0L1_CORS_ORIGINS=http://localhost:3000
```

See [Configuration Guide](https://docs.skelfresearch.com/l0l1/configuration) for all options.

## Architecture

```
l0l1/
├── api/           # FastAPI REST API
├── cli/           # Typer CLI
├── core/          # Configuration
├── models/        # AI providers (OpenAI, Anthropic)
├── services/      # Core services
│   ├── pii_detector.py      # PII detection (Presidio)
│   ├── learning_service.py  # Pattern learning
│   ├── database_service.py  # DB connections
│   └── schema_service.py    # Schema management
└── integrations/
    ├── jupyter/   # Notebook integration
    └── ide/       # LSP server
```

## Development

```bash
# Setup dev environment
make setup

# Run tests
make test

# Lint and format
make lint
make format

# Start dev server with reload
make serve

# Build documentation
make docs
```

### Docker

```bash
# Build and run
docker-compose up -d

# Check health
curl http://localhost:8000/health
```

## Documentation

Full documentation at **[docs.skelfresearch.com/l0l1](https://docs.skelfresearch.com/l0l1)**

- [Getting Started](https://docs.skelfresearch.com/l0l1/getting-started)
- [Configuration](https://docs.skelfresearch.com/l0l1/configuration)
- [CLI Reference](https://docs.skelfresearch.com/l0l1/cli)
- [REST API](https://docs.skelfresearch.com/l0l1/api)
- [Jupyter Guide](https://docs.skelfresearch.com/l0l1/guides/jupyter)
- [VS Code Extension](https://docs.skelfresearch.com/l0l1/guides/vscode-extension)

## Contributing

Contributions welcome! See [Development Guide](https://docs.skelfresearch.com/l0l1/development).

```bash
# Fork, clone, then:
uv sync --all-extras
pre-commit install
make test
```

## License

MIT License - see [LICENSE](LICENSE)

---

<div align="center">

**[Documentation](https://docs.skelfresearch.com/l0l1)** | **[Issues](https://github.com/skelf-research/l0l1-api/issues)** | **[Discussions](https://github.com/skelf-research/l0l1-api/discussions)**

Built by [Skelf Research](https://skelfresearch.com)

</div>

---

## Part of Skelf Research

`l0l1` is built by **[Skelf Research](https://skelfresearch.com)** — an independent UK AI research lab publishing production-grade open-source projects.

🌐 [Website](https://l0l1.skelfresearch.com) · 📚 [Documentation](https://docs.skelfresearch.com/l0l1) · 🔬 [All projects](https://skelfresearch.com/projects) · 🤗 [Hugging Face](https://huggingface.co/skelfresearch)

**Related projects:** [compere](https://compere.skelfresearch.com) (pairwise ranking) · [mpl](https://mpl.skelfresearch.com) (agent-comms contracts) · [savanty](https://savanty.skelfresearch.com) (English→constraint solver)

<sub>Released under MIT / Apache-2.0. © Skelf Research Limited.</sub>
