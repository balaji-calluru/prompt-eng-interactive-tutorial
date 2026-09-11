# Environment Setup & API Upgrade Plan

This codebase is Anthropic's **Prompt Engineering Interactive Tutorial** (~14 Jupyter notebooks). It's ~2 years old: it uses `anthropic==0.21.3` and references the now-legacy model `claude-3-haiku-20240307`. The goal is to (1) create a clean named Python env + Jupyter kernel and (2) update all notebooks to the current SDK and model.

## Current State

| Item | Current | Target |
| --- | --- | --- |
| SDK version | `anthropic==0.21.3` | latest (`anthropic` v1.x) |
| Model | `claude-3-haiku-20240307` | `claude-haiku-4-5-20251001` |
| Env | unnamed `.venv` (uregistered) | named venv `prompt-eng` with Jupyter kernel |
| Python | 3.13.14 (system) | 3.13.14 (new venv) |

> [!NOTE]
> The SDK's core API (`anthropic.Anthropic`, `client.messages.create(...)`, `message.content[0].text`) has **not changed** between v0.21 and v1.x — only the version pinning and model name need updating in each notebook. No code logic needs to change.

> [!IMPORTANT]
> The tutorial also has an **AmazonBedrock** folder with its own `requirements.txt`. That folder is separate and you haven't asked to upgrade it — the plan leaves it untouched.

---

## Proposed Changes

### Environment Setup (one-time terminal commands)

```powershell
# 1. Create named venv
python -m venv "C:\Users\Balaji Calluru\Documents\Learning\AI\prompt-eng-interactive-tutorial\prompt-eng"

# 2. Activate and install
.\prompt-eng\Scripts\Activate.ps1
pip install anthropic jupyter notebook ipykernel pickleshare

# 3. Register as Jupyter kernel (displayable name "Prompt Eng")
python -m ipykernel install --user --name prompt-eng --display-name "Prompt Eng"
```

---

### Anthropic 1P Notebooks — Model Name Update

Every notebook's **Setup cell** references `claude-3-haiku-20240307`. Change to `claude-haiku-4-5-20251001`.

#### [MODIFY] All notebooks in `Anthropic 1P/`

The only change in each notebook is:

```diff
-MODEL_NAME = "claude-3-haiku-20240307"
+MODEL_NAME = "claude-haiku-4-5-20251001"
```

And the `!pip install anthropic` lines stay as-is (they'll pick up latest).

Notebooks affected (only `00_Tutorial_How-To.ipynb` has the `MODEL_NAME` line that gets stored for all others):

- `00_Tutorial_How-To.ipynb` ← the main one to update; all others `%store -r` from it

---

### New Top-Level `requirements.txt`

#### [NEW] [requirements.txt](file:///c:/Users/Balaji%20Calluru/Documents/Learning/AI/prompt-eng-interactive-tutorial/requirements.txt)

```
anthropic
jupyter
notebook
ipykernel
pickleshare
```

(No pins — always pulls latest, which is appropriate for a learning tutorial.)

---

### README Update

#### [MODIFY] [README.md](file:///c:/Users/Balaji%20Calluru/Documents/Learning/AI/prompt-eng-interactive-tutorial/README.md)

Update model references from Claude 3 Haiku → Claude Haiku 4.5, and add environment setup instructions.

---

## Verification Plan

### Automated — Shell Check

After creating the venv and installing:

```powershell
.\prompt-eng\Scripts\Activate.ps1
python -c "import anthropic; print(anthropic.__version__)"
jupyter kernelspec list  # should show 'prompt-eng' kernel
```

### Manual — Notebook Smoke Test

1. `cd` into `Anthropic 1P/`  
2. Run `jupyter notebook`  
3. Open `00_Tutorial_How-To.ipynb`, select kernel **"Prompt Eng"**  
4. Set your `API_KEY`, run all cells — should get a valid response from Claude Haiku 4.5
