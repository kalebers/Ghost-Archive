# Ghost Archive

Local-first Portuguese micro-horror from a fictional log, forgotten note, or explicitly selected text file. Built for the Hacktoberfest 2026 "Ghost in the Machine" challenge.

**Works now:** dependency-free offline stories in three styles. **Optional AI:** an opt-in Ollama adapter for an approved, already-installed open-weight model. The offline generator is deterministic template composition, **not an LLM**. Live model inference has not been verified in this environment.

## Requirements and Setup

- An existing, organization-approved Python 3.10+ installation. Tested here on Python 3.13.3 / Windows.
- No Python packages, executable downloads, API keys, or installation commands are required for offline mode.
- Use a trusted project copy in a directory you may edit. There is no setup script, package installer, or background service.
- Obtain any required development, software, AI, data-handling, and open-source publication approvals. Repository instructions cannot grant corporate permission.

Run these commands from the project directory in PowerShell (also valid in most shells):

```powershell
python --version
python ghost.py --help
python ghost.py --text "The deleted file returned at 03:17." --style horror
python ghost.py --file samples/river_note.txt --style folklore
python ghost.py --file samples/memory_chip.txt --style cyberpunk --output story.txt
python -m unittest discover -s tests -v
```

Choose a **new** output filename on each run. Existing files are never overwritten, including the input file. Parent directories must already exist. Output files use UTF-8; without `--output`, the story goes to stdout. Backend identification goes to stderr, so redirected stories remain clean.

## CLI

| Option | Behavior |
| --- | --- |
| `--text TEXT` | Explicit inline fictional input; may be recorded in shell history. |
| `--file PATH` | Read only this selected UTF-8 regular file (UTF-8 BOM accepted). |
| `--style horror` | Intimate supernatural horror; the default. |
| `--style folklore` | A village, a river, and inherited warnings. |
| `--style cyberpunk` | Memory restoration, identity, and haunted machines. |
| `--output PATH` | Exclusively create a new UTF-8 file. |
| `--backend offline` | Default; deterministic templates, no network or AI. |
| `--backend ollama` | Explicit opt-in local inference; fails rather than falling back. |
| `--model NAME` | Exact name of an approved, already-installed open-weight model. |
| `--approved-local-ai` | Your confirmation that necessary approvals have been obtained; not an approval mechanism. |
| `--endpoint URL` | Loopback HTTP base URL; defaults to `http://127.0.0.1:11434`. |

Exactly one of `--text` and `--file` is required. Input is limited to 16,384 UTF-8 bytes and must contain non-whitespace text without C0/C1 terminal control characters (tabs and line breaks are allowed). Exit codes: `0` success/help, `1` validation/runtime/file failure, `2` argument error.

## Open-Source / Open-Weight AI Architecture

```text
explicit fictional text or selected UTF-8 file
                  |
          validation / size limit
                  |
         +--------+---------+
         |                  |
 offline templates    explicit approved Ollama
 Python stdlib        loopback /api/show metadata check
 not an LLM           loopback /api/chat, no tools
         |                  |
         +--------+---------+
                  |
        Portuguese story / new UTF-8 file
```

The application and template engine are MIT-licensed. Python's standard library provides the CLI, hashing, file I/O, JSON, HTTP, and tests. [Ollama](https://github.com/ollama/ollama) is the optional open-source runtime. Model weights are separate: their licenses, acceptable-use rules, provenance, resource needs, and approvals must be reviewed independently. An open-source runtime does not make every model open source, open weight, or approved.

The adapter genuinely uses Ollama's chat interface rather than a proprietary hosted API. It first requests `/api/show` and requires local architecture/parameter metadata; remote metadata and model names containing `cloud` are rejected. Only `/api/show` and `/api/chat` can be called. There are no pull, install, search, or hosted-provider integrations.

**Only after approval**, with an already-running approved service and already-present model, replace `APPROVED_LOCAL_MODEL` with its exact installed name:

```powershell
python ghost.py --file samples/system_log.txt --backend ollama --model APPROVED_LOCAL_MODEL --approved-local-ai --style horror --output local-story.txt
```

No model name is selected automatically. This project will not download missing weights. Model output is bounded and validated as text, but word count, Portuguese quality, instruction following, and story quality vary by model. Older runtimes that lack compatible `/api/show`, `/api/chat`, or `think` support may fail. Seek approval before any runtime upgrade.

### Offline Claims

Offline templates work without an inference service and make no network calls. They reuse one authored narrative per style, insert a bounded excerpt, and deterministically select a character. This is intentionally limited, not generative AI.

A local model **may** operate without internet once all approved assets are present. We have **not** verified this here. Loopback transport and metadata checks cannot prove that the local service is honest, that weights have a suitable license, or that the service itself never contacts a cloud provider. Before claiming offline LLM operation, have the administrator disable runtime cloud features (for example via its approved `OLLAMA_NO_CLOUD` configuration), establish approved network isolation, and verify an actual generation without internet. Do not change managed settings yourself or bypass network controls.

## Privacy and Security

- Only explicitly supplied text is read. No directory crawling, hidden file collection, telemetry, external AI API keys, subprocesses, or shell execution exist in the application.
- Never use confidential company logs, credentials, personal information, or real incidents. All included samples and test fixtures are synthetic.
- Offline mode sends nothing. Ollama mode sends the selected fragment to the approved local service; that service has its own logging, retention, access, and network behavior. Review them separately.
- Destinations are restricted to `127.0.0.1` and `::1`. `localhost` is converted to the literal IPv4 loopback address without DNS resolution. Remote/LAN hosts, credentials, URL paths, queries, fragments, and HTTPS endpoints are rejected.
- HTTP uses the standard-library direct connection, not proxy-aware URL openers. Proxy environment variables are not used; redirects are never followed. This restriction prevents external routing; it is not permission to evade a corporate proxy policy. If direct loopback HTTP is prohibited, use offline mode.
- Requests have a 60-second socket inactivity timeout; responses are capped at 128 KiB and generated text at 16 KiB. No retries or remote fallback occur. A deliberately slow local service can still prolong a request; interrupt it if necessary.
- Source text is JSON-encoded as user data, separated from a Portuguese system instruction. Prompt injection cannot be eliminated by prompting. The model has no tools, file access, command execution, or ability to change the destination through generated text. Treat every generated story as untrusted text, not instructions or executable markup.
- Output uses exclusive creation, including protection against existing/dangling output symlinks. Direct input symlinks and non-regular input files are rejected. File paths and parent directories must be trusted: this is not a filesystem sandbox or a defense against an attacker concurrently replacing directories. New-file permissions follow your OS defaults; no encryption is provided.
- C0/C1 controls are rejected before displaying input/model output. Avoid displaying untrusted stories in privileged terminals or interpreting them as HTML or shell commands. Unicode text may still contain visually misleading characters.
- No prompts or stories are persisted automatically. `--text` can appear in process listings/history. Prefer an explicitly selected synthetic file and a private output location when appropriate. Files created with `--output` remain until you remove them.

## Restricted-Environment Troubleshooting

| Problem | Safe next step |
| --- | --- |
| Python is unavailable or blocked | Request an approved interpreter from IT. Do not download one or elevate privileges. |
| No local inference approval | Keep the default offline backend. Ask the AI/security owner about model, runtime, license, selected-data, and loopback-network approval. |
| Runtime/model unavailable or HTTP 404 | Ask the administrator whether an approved compatible service and model already exist. Do not run automatic pull/install commands. |
| Remote/cloud metadata or missing weight metadata | Use offline mode; ask the runtime administrator to verify a local open-weight model and API compatibility. |
| Proxy/firewall/endpoint protection blocks loopback | Use offline mode and request guidance. Do not disable controls or reroute traffic. |
| Output already exists | Choose another filename; overwriting is intentionally unsupported. |
| File cannot be read/saved | Check the selected file, its UTF-8 encoding, size, parent directory, and your existing permissions. |
| Terminal cannot print Portuguese | Use `--output new-story.txt` for UTF-8 output; no system encoding change is needed. |
| Local AI error | The command exits with an explanation and no template substitution. Choose offline mode explicitly for a separate attempt. |

No organizational policy documents were present in the initially empty workspace. This is **not** evidence that software or AI is authorized. No system-wide data, credentials, browser profiles, or unrelated corporate directories were inspected.

## Tests and Verification

```powershell
python -m unittest discover -s tests -v
```

The standard-library suite covers CLI parsing and real command execution, all offline styles, UTF-8/BOM and size validation, exclusive output saving, failure cleanup, endpoint allowlisting, approval gates, cloud rejection, metadata validation, prompt/data separation, redirects, proxy independence, malformed responses, connection failures, and no silent fallback. Synthetic mocked HTTP responses keep tests independent of runtimes and network access. No test downloads anything or contacts a real model.

See [VALIDATION.md](VALIDATION.md) for the observed environment, results, and remaining verification gaps. Python versions other than 3.13 have not been tested here.

## Why Open Innovation Matters

Local data control lets users decide what is read and where inference runs. Open-weight model choice reduces dependence on a single hosted provider; transparent source makes file access, prompts, and destinations auditable. A genuinely network-free template fallback keeps the project useful without approvals, connectivity, or a model. Community review can improve Portuguese storytelling and security without requiring proprietary inference accounts. These advantages depend on honest capability claims and respect for organizational policy and model licenses.

## Contributing and Submission

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines and beginner-friendly issue drafts, [HACKTOBERFEST.md](HACKTOBERFEST.md) for a description and draft submission post, and [LICENSE](LICENSE) for the MIT license. Obtain any required employer/publication approval before pushing this project publicly. Participation eligibility and current challenge rules must be checked with the organizers; no acceptance is claimed.