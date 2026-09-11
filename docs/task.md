# Prompt Engineering Tutorial – Setup & Upgrade

## Part 1: Python Environment Setup

- [x] Create a named Python virtual environment (`prompt-eng`)
- [x] Install `anthropic` (latest), `jupyter`, `notebook`, `ipykernel`, `pickleshare`
- [x] Register the venv as a Jupyter kernel named `prompt-eng`
- [x] Create a top-level `requirements.txt` with pinned latest versions

## Part 2: API Upgrade (Anthropic 1P notebooks)

- [x] Update `MODEL_NAME` from `claude-3-haiku-20240307` → `claude-haiku-4-5-20251001`
- [ ] Update `00_Tutorial_How-To.ipynb` – model name + any SDK changes
- [ ] Update `01_Basic_Prompt_Structure.ipynb` to `05_...` – setup cells/model
- [ ] Update remaining notebooks 06–10
- [ ] Update README.md model references

## Part 3: Verification

- [ ] Verify venv activation works
- [ ] Verify `import anthropic` works in kernel
- [ ] Confirm notebook setup cell runs without errors (manual)
