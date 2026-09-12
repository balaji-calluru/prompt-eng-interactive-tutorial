# Anthropic 1P — Prompt Engineering Interactive Tutorial

The **Anthropic first-party (1P) API** track of the tutorial: 9 chapters plus 3 appendices of
Jupyter notebooks that call Claude directly through the [Anthropic Python SDK](https://docs.anthropic.com/claude/reference/client-sdks)
and the [Messages API](https://docs.anthropic.com/en/api/messages).

If you'd rather go through AWS, use the parallel [AmazonBedrock](../AmazonBedrock/) track instead.

---

## Quick start

**1. Get an API key** from the [Anthropic Console](https://console.anthropic.com/).

**2. Put it in a `.env` file** at the repo root (one level above this folder):

```
ANTHROPIC_API_KEY=sk-ant-...
```

`.env` is git-ignored, so the key never gets committed. If no `.env` is found, the setup cell in
`00_Tutorial_How-To.ipynb` prompts you for the key securely instead (session only).

**3. Activate the environment and launch Jupyter** from the repo root:

```powershell
.\prompt-eng\Scripts\Activate.ps1
jupyter notebook "Anthropic 1P/"
```

**4. Select the `Prompt Eng` kernel** in the notebook, then run
**`00_Tutorial_How-To.ipynb` first** — it loads your key and stashes `API_KEY` and `MODEL_NAME`
into the IPython store, which every other notebook reads back with `%store -r`. Skip it and the
chapter setup cells will fail with a `NameError`.

**5. Work through the chapters in order**, top to bottom within each notebook.

> Building the environment from scratch instead: `python -m venv prompt-eng`,
> `.\prompt-eng\Scripts\Activate.ps1`, `pip install -r requirements.txt`, then
> `python -m ipykernel install --user --name prompt-eng --display-name "Prompt Eng"`.

---

## Chapter map

Each chapter notebook has the same four sections: **Setup** → **Lesson** → **Exercises** →
**Example Playground** (a free-form scratch area at the bottom for experimenting).

### Beginner

| Notebook | What it covers |
| --- | --- |
| [00_Tutorial_How-To.ipynb](00_Tutorial_How-To.ipynb) | Key setup, the `get_completion` helper, how to run the course |
| [01_Basic_Prompt_Structure.ipynb](01_Basic_Prompt_Structure.ipynb) | Messages API shape: `model`, `max_tokens`, alternating roles, `system`, `temperature` |
| [02_Being_Clear_and_Direct.ipynb](02_Being_Clear_and_Direct.ipynb) | Say exactly what you want; the most common failure mode is vagueness |
| [03_Assigning_Roles_Role_Prompting.ipynb](03_Assigning_Roles_Role_Prompting.ipynb) | Role prompting to shift tone and reasoning quality |

### Intermediate

| Notebook | What it covers |
| --- | --- |
| [04_Separating_Data_and_Instructions.ipynb](04_Separating_Data_and_Instructions.ipynb) | Prompt templates, f-string substitution, XML tags to fence off data |
| [05_Formatting_Output_and_Speaking_for_Claude.ipynb](05_Formatting_Output_and_Speaking_for_Claude.ipynb) | Controlling output shape; prefilling the assistant turn |
| [06_Precognition_Thinking_Step_by_Step.ipynb](06_Precognition_Thinking_Step_by_Step.ipynb) | Give Claude room to reason before answering |
| [07_Using_Examples_Few-Shot_Prompting.ipynb](07_Using_Examples_Few-Shot_Prompting.ipynb) | Few-shot examples as the highest-leverage steering tool |

### Advanced

| Notebook | What it covers |
| --- | --- |
| [08_Avoiding_Hallucinations.ipynb](08_Avoiding_Hallucinations.ipynb) | Escape hatches, grounding in quoted evidence, verification |
| [09_Complex_Prompts_from_Scratch.ipynb](09_Complex_Prompts_from_Scratch.ipynb) | Full prompt builds: career-coach chatbot, legal services; exercises for financial services and a codebot |

### Appendix — beyond standard prompting

| Notebook | What it covers |
| --- | --- |
| [10.1_Appendix_Chaining Prompts.ipynb](10.1_Appendix_Chaining%20Prompts.ipynb) | Splitting one hard task across several chained calls |
| [10.2_Appendix_Tool Use.ipynb](10.2_Appendix_Tool%20Use.ipynb) | Letting Claude call your functions |
| [10.3_Appendix_Search & Retrieval.ipynb](10.3_Appendix_Search%20&%20Retrieval.ipynb) | Pointers to RAG / retrieval material |

---

## Exercises, hints, and answers

- Most exercises in Chapters 1–8 are auto-graded by a small local `grade_exercise` function —
  usually a substring or regex check on Claude's output. Iterate on the prompt until the cell
  prints a pass. Chapter 9 and the Tool Use appendix are open-ended instead: you judge the output.
- Stuck? Every exercise has a hint. Run its hint cell, or read
  [hints.py](hints.py) directly (e.g. `exercise_4_1_hint`).
- Full answers live in the
  [answer key spreadsheet](https://docs.google.com/spreadsheets/d/1jIxjzUWG-6xBVIa2ay6yDpLyeuOh_hR_ZB75a47KX_E/edit?usp=sharing).

---

## Model and SDK notes

This track is pinned to **Claude Haiku 4.5** (`claude-haiku-4-5-20251001`) at **temperature 0** —
fast, cheap, and deterministic enough that the graded exercises behave consistently. The techniques
themselves transfer to every Claude model (Haiku 4.5, Sonnet 5, Opus 5, Fable 5.1); to try another
one, change `MODEL_NAME` in `00_Tutorial_How-To.ipynb` and re-run it so the new value lands in the
store.

Two things differ from the original upstream tutorial, both because it now runs on **`anthropic` 1.5.x**:

- **`temperature` moved into `extra_body`.** SDK 1.x dropped `temperature` / `top_p` / `top_k` from
  the typed `messages.create()` signature, so the helpers pass
  `extra_body={"temperature": 0.0}` instead of a plain keyword argument. Newer models such as
  Opus 5 and Sonnet 5 reject the parameter outright — if you switch `MODEL_NAME` to one of those,
  drop the `extra_body` line.
- **The key comes from `.env`** via `python-dotenv` rather than being pasted into a notebook cell.
  Hardcoded keys end up in plaintext inside the `.ipynb` and in git history.

---

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| `NameError: name 'API_KEY' is not defined` | Run `00_Tutorial_How-To.ipynb` first — it populates the `%store`. |
| `%store -r` returns nothing / stale values | `pickleshare` must be installed (it's in `requirements.txt`); re-run the how-to notebook to refresh. |
| `ModuleNotFoundError: anthropic` | Wrong kernel. Switch to **Prompt Eng** (Kernel → Change kernel). |
| `401 authentication_error` | Key missing or wrong — check `.env` at the **repo root**, not in this folder, and restart the kernel so `load_dotenv()` re-reads it. |
| `TypeError: ... unexpected keyword argument 'temperature'` | You're passing `temperature=` directly; use `extra_body={"temperature": 0.0}`. |
| `400 ... temperature` on Opus/Sonnet 5 | That model doesn't accept `temperature` — remove `extra_body` entirely. |
| `429` / overloaded | Rate limit or credit exhaustion; wait and retry, or check usage in the Console. |

---

## Files in this folder

- `00`–`09`, `10.x` — the tutorial notebooks (above)
- [hints.py](hints.py) — hint strings for every exercise
- `.ipynb_checkpoints/`, `__pycache__/` — Jupyter/Python scratch, git-ignored

Repo-level overview and the Bedrock alternative: [../README.md](../README.md).
