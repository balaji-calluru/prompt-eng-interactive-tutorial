# Walkthrough: Environment Setup & API Upgrade

## What Was Done

### 1. Named Python Virtual Environment — `prompt-eng`

Created a fresh named venv at the repo root and installed all dependencies:

```powershell
python -m venv prompt-eng
.\prompt-eng\Scripts\pip.exe install anthropic jupyter notebook ipykernel pickleshare
```

| Package | Version Installed |
| --- | --- |
| `anthropic` | **1.5.0** (was 0.21.3) |
| `jupyter` | latest |
| `notebook` | latest (7.6.2) |
| `ipykernel` | latest |
| `pickleshare` | latest |

### 2. Jupyter Kernel Registered

```
Kernel name: prompt-eng
Display name: "Prompt Eng"
```

Verified with `jupyter kernelspec list`:

```
prompt-eng  → C:\Users\Balaji Calluru\AppData\Roaming\jupyter\kernels\prompt-eng
```

### 3. New Top-Level `requirements.txt`

[requirements.txt](file:///c:/Users/Balaji%20Calluru/Documents/Learning/AI/prompt-eng-interactive-tutorial/requirements.txt) — unpinned for always pulling latest.

### 4. Model Upgrade — All Notebooks

| Folder | Old Model | New Model |
| --- | --- | --- |
| `Anthropic 1P/` | `claude-3-haiku-20240307` | `claude-haiku-4-5-20251001` |
| `AmazonBedrock/` | `anthropic.claude-3-haiku-20240307-v1:0` | `anthropic.claude-haiku-4-5-20251001-v1:0` |

Files changed:

- [Anthropic 1P/00_Tutorial_How-To.ipynb](file:///c:/Users/Balaji%20Calluru/Documents/Learning/AI/prompt-eng-interactive-tutorial/Anthropic%201P/00_Tutorial_How-To.ipynb) — the one source-of-truth notebook; all others use `%store -r MODEL_NAME`
- `AmazonBedrock/anthropic/00_Tutorial_How-To.ipynb`
- `AmazonBedrock/anthropic/10_3_Appendix_Empirical_Performance_Evaluations.ipynb`
- `AmazonBedrock/boto3/00_Tutorial_How-To.ipynb`
- `AmazonBedrock/boto3/10_3_Appendix_Empirical_Performance_Eval.ipynb`

### 5. README Updated

[README.md](file:///c:/Users/Balaji%20Calluru/Documents/Learning/AI/prompt-eng-interactive-tutorial/README.md) — added Environment Setup section with activation commands and updated model description.

---

## Verification Results

| Check | Result |
| --- | --- |
| `anthropic` importable in venv | ✅ `1.5.0` |
| `prompt-eng` kernel registered | ✅ |
| No stale `claude-3-haiku-20240307` refs | ✅ Zero found |
| 4 AmazonBedrock notebooks updated | ✅ |

---

## How to Start Practicing

```powershell
# From the repo root:
.\prompt-eng\Scripts\Activate.ps1
jupyter notebook "Anthropic 1P/"
```

1. Open `00_Tutorial_How-To.ipynb`
2. Select kernel **"Prompt Eng"** (top-right dropdown)
3. Set `API_KEY = "your_anthropic_api_key_here"` and run all cells
4. Proceed to `01_Basic_Prompt_Structure.ipynb`

> [!TIP]
> Get your API key at [console.anthropic.com](https://console.anthropic.com/). The free tier includes enough credits to complete the full tutorial.
