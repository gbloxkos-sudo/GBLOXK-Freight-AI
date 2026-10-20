# GBLOXK OS PENTESTER AI

Unified AI-Powered Security Assessment Platform — Ollama-backed pentesting agent with full tool calling, compliance mapping, and SARIF/Markdown/HTML reporting.

## Features

- **AI Agent Loop** — Autonomous security assessment with Ollama (local LLM) tool calling
- **Tool Registry** — Shell, HTTP, File, Browser, Scanner (nuclei/nmap/semgrep/trivy/gitleaks/katana), DNS, Recon, Evidence, Findings, Compliance
- **Compliance Mapping** — OWASP Top 10, PCI DSS Lite, ISO 27001 Lite, NIST CSF Lite
- **Reporting** — SARIF, Markdown, HTML reports per workspace
- **REST API** — FastAPI server with `/assess`, `/scan`, `/tools`, `/memory`, `/prompts`, `/health`
- **CLI** — `gbloxk assess`, `gbloxk scan`, `gbloxk serve`, `gbloxk interactive`, `gbloxk wizard`
- **Safety** — Destructive command guarding, scope validation, header redaction
- **License** — GNU Affero General Public License v3.0

## Quick Start

```bash
pip install -e .
gbloxk assess https://example.com --mode deep --profile attacker
gbloxk serve --port 8080
```

## Commands

| Command | Description |
|---------|-------------|
| `health` | Check Ollama status and models |
| `assess <target>` | Full AI-powered security assessment |
| `scan <target>` | Quick security scan |
| `interactive` | Interactive REPL mode |
| `serve` | Start REST API server |
| `wizard` | Guided assessment setup |
| `report <workspace>` | View generated report |
| `findings <workspace>` | List findings |
| `compliance <workspace>` | Show compliance mappings |

## License

GNU Affero General Public License v3.0 — see `LICENSE`.
