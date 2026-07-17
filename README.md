# model-sync for NVIDIA 4090

This repository keeps a curated set of Ollama models fresh on a weekly schedule using GitHub Actions.

## Models Parsed from nvidia_4090.md

- deepseek-r1:32b
- qwen2.5:32b
- qwen2.5-coder:32b
- mistral-small:24b
- llama3.1:8b

## Files

- `.github/workflows/refresh-ollama-models.yml`: Weekly workflow that installs Ollama, enables the service, and pulls models.
- `nvidia_4090_models.conf`: Source of truth for model tags to pull.
- `NVIDIA-4090.md`: Human-readable recommendations and rationale.

## Workflow behavior

The workflow runs weekly and can also be started manually.

It executes:

1. `sudo dnf install -y ollama`
2. `sudo systemctl enable --now ollama`
3. `ollama pull $model` for each line in `nvidia_4090_models.conf`

## Runner requirements

This workflow is configured for a Fedora-based self-hosted runner:

- Labels: `self-hosted`, `linux`, `x64`
- `sudo` privileges for `dnf` and `systemctl`
- Ollama-compatible GPU host (NVIDIA 4090)

If your runner labels differ, update `runs-on` in the workflow file.

## Local self-hosted runner (from this repo folder)

Use the Makefile to install and run a GitHub Actions runner in `./actions-runner`.

1. Download, verify, and extract the runner package:

	`make runner-setup`

2. Generate a fresh runner token in GitHub and configure the runner:

	`make runner-config RUNNER_TOKEN=<paste-token-here>`

3. Run the runner process:

	`make runner-run`

4. Install the runner as a service:

	`make runner-service-install`

Useful options:

- Override version: `make runner-setup RUNNER_VERSION=2.335.1`
- Override repo URL: `make runner-config RUNNER_URL=https://github.com/christophermarklee/model-sync RUNNER_TOKEN=<token>`
- Install service: `make runner-service-install`
- Remove registration: `make runner-remove RUNNER_TOKEN=<token>`

Security note: Do not commit runner tokens. Always use a short-lived token from GitHub UI and pass it via `RUNNER_TOKEN` at command time.
