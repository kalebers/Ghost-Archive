# Contributing to Ghost Archive

Keep the project small, auditable, and useful without a model.

## Working Locally

1. Use an existing approved Python 3.10+ environment. There are no third-party packages to install.
2. Read the privacy, file-handling, and local-inference boundaries in [README.md](README.md).
3. Make a focused change with synthetic fixtures only. Preserve existing behavior unless the change is explicitly discussed.
4. Run `python -m unittest discover -s tests -v`. No runtime or model is needed for this suite.
5. Review your diff for company data, identifying paths, secrets, output stories, binaries, model files, and unrelated changes.
6. Describe the behavior change, tests, limitations, and whether any inference was actually verified in your pull request.

Do not install packages, download weights, change managed settings, or call external inference without the relevant approvals. A successful local test does not grant publication permission. Get any necessary employer authorization before contributing.

## Design Boundaries

- The offline backend must remain dependency-free, deterministic, and clearly labeled as not an LLM.
- No automatic downloads, external inference, telemetry, secrets, hidden scans, or shell execution from input.
- Local inference stays opt-in, literal-loopback-only, bounded, and fail-closed. Never substitute another backend silently.
- Preserve exclusive output creation and explicit file selection. Never overwrite user files for convenience.
- Templates and prompts should favor idiomatic Brazilian Portuguese, specific sensory details, restrained supernatural tension, and an ending that changes the meaning of an earlier detail.
- Treat fragments and model output as untrusted data. Prompt wording is not a security boundary.
- Add regression tests to the existing test module. Do not add a framework for a narrow improvement.

## Beginner-Friendly Issue Drafts

### 1. Add a Second Folklore Narrative

Suggested labels: `good first issue`, `enhancement`, `writing`.

Add an original Portuguese folklore template selected deterministically from the fragment. Keep a title and three paragraphs, preserve the bounded excerpt, and avoid copying published stories. Acceptance: tests prove deterministic selection, valid Portuguese UTF-8, narrative structure, and zero network use.

### 2. Add Synthetic Forgotten-Commit Examples

Suggested labels: `good first issue`, `documentation`.

Contribute two short invented commit messages or fictional archival notes and one README usage example. Acceptance: no real repository/person/company information; each sample is valid UTF-8 below the input limit and works with the documented offline command.

### 3. Improve Portuguese Editorial Tests

Suggested labels: `good first issue`, `tests`, `writing`.

Review repetition and pacing in existing templates. Add focused regression cases for multiline excerpts and punctuation without asserting a single model-generated response. Acceptance: the existing suite passes; no packages or model are needed.

### 4. Document an Approved Disconnected Model Run

Suggested labels: `help wanted`, `documentation`, `local-ai`.

Only after explicit approval, document runtime version, exact model identifier and license, local metadata compatibility, and a synthetic generation under administrator-approved network isolation. Acceptance: distinguish direct observations from assumptions, include no personal paths or corporate policy documents, and do not claim that a loopback URL alone proves disconnected inference. This issue is not authorization to install or download anything.

## Reporting Security Concerns

Do not publish sensitive fragments or exploit output containing private data. Contact the repository maintainer through an available private security-reporting channel before disclosing a vulnerability. If the eventual host offers private vulnerability reporting, the maintainer should enable it before publication. No contact address is fabricated in this starter repository.

Contributions are provided under the project's MIT license. Model licenses remain separate.