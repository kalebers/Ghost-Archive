# Hacktoberfest 2026 Submission Draft

## Short Description

Ghost Archive turns explicitly selected fictional text into Portuguese micro-horror. A dependency-free, non-LLM offline storyteller works immediately; an opt-in, loopback-only Ollama adapter supports approved, already-installed open-weight models without proprietary hosted inference.

## Draft Post

For "Ghost in the Machine," I built Ghost Archive: a tiny local-first archive where forgotten notes, fictional system logs, and abandoned memory chips become supernatural stories in Brazilian Portuguese.

Choose horror, folklore, or cyberpunk; supply a fragment or one UTF-8 file; read the story or save it to a new file. Nothing scans your folders. Nothing downloads a model. Existing files are never overwritten.

The project is MIT-licensed and uses only Python's standard library. Its default offline engine is openly labeled deterministic template composition, not an LLM. The optional AI path uses open-source Ollama and an explicitly approved local open-weight model through loopback-only HTTP. It rejects remote endpoints and cloud-model metadata, ignores proxy routing, refuses redirects, and fails clearly rather than switching to a hosted provider.

Open innovation matters because people should control their selected data, inspect how software handles it, choose compatible models and licenses, and keep a useful offline experience without a proprietary inference account. Community contributions can improve both the language and the safeguards.

Verification so far: the automated standard-library tests pass on Python 3.13.3, including real offline CLI runs and mocked local-AI security checks. Live open-weight inference and disconnected model operation have not been verified; they require an approved runtime and model. No software or model downloads were performed.

Beginner-friendly contributions include new original folklore narratives, synthetic forgotten-commit samples, and Portuguese editorial tests. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Before Publishing

- Obtain any employer/IP and public open-source release approvals.
- Confirm the current Hacktoberfest/challenge participation rules with the organizers.
- Publish to a repository you are authorized to make public; add its actual URL to your submission.
- Review the complete diff and exclude generated stories, model files, personal paths, secrets, and company data.
- If a verified AI demonstration is required, complete the approved local-model run first and update [VALIDATION.md](VALIDATION.md) honestly. The current offline demo does not demonstrate LLM inference.
- Review licenses for any future model or third-party assets independently of the project's MIT license.