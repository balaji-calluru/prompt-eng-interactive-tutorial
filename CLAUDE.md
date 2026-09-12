# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

Anthropic's Prompt Engineering Interactive Tutorial — a set of Jupyter notebooks, not an
application. There is no build, no test suite, and no importable package. "Running" the project
means executing notebook cells against the live Claude API.

Two parallel tracks cover the same 9 chapters + appendices:

- **`Anthropic 1P/`** — the active track. Calls Claude directly via the `anthropic` Python SDK.
- **`AmazonBedrock/`** — the same material through AWS Bedrock (`boto3`). Untouched by recent work.

`docs/` holds the notes from the Haiku 4.5 / SDK 1.x upgrade (`task.md`, `implementation_plan.md`,
`walkthrough.md`). `Anthropic 1P/README.md` is the learner-facing guide — quick start, chapter map,
troubleshooting table. Keep it in sync when notebook setup conventions change.

## Environment

- Venv: **`prompt-eng/`** at the repo root, registered as Jupyter kernel `prompt-eng`
  (display name **"Prompt Eng"**). Activate with `.\prompt-eng\Scripts\Activate.ps1`.
- A stray `.venv/` also exists at the root and is **not** the tutorial environment — ignore it.
- `requirements.txt` pins `anthropic==1.5.0` and `python-dotenv==1.2.3` deliberately; the notebook
  helpers depend on those exact APIs. Jupyter packages are floors, not pins.
- `pickleshare` is a hard requirement, not incidental — `%store` breaks without it.

## How state flows between notebooks

`00_Tutorial_How-To.ipynb` is the only notebook that reads `.env` (repo root, git-ignored). It sets
`API_KEY` and `MODEL_NAME` and pushes both into the IPython store via `%store`. Every other notebook
opens with `%store -r API_KEY` / `%store -r MODEL_NAME`.

Consequences:

- Run `00` first in any fresh session, or chapters fail with `NameError: name 'API_KEY' is not defined`.
- Changing the model means editing `MODEL_NAME` in `00` and re-running it — not editing chapters.
- Never hardcode a key in a cell; it would persist in the `.ipynb` JSON and in git history.

## Notebook conventions

Each chapter follows the same four sections: **Setup → Lesson → Exercises → Example Playground**.

- **`get_completion` is redefined in every notebook's setup cell**, and its signature evolves by
  chapter. Don't assume one shared helper:
  | Notebooks | Signature |
  | --- | --- |
  | `00` | `get_completion(prompt)` |
  | `01`–`04` | `get_completion(prompt, system_prompt="")` |
  | `05`–`09` | `get_completion(prompt, system_prompt="", prefill="")` |
  | `10.1` | `get_completion(messages, system_prompt="")` — takes a message list, not a string |
  | `10.2` | `get_completion(messages, system_prompt="", prefill="", stop_sequences=None)` |
- **Temperature goes in `extra_body`**: `extra_body={"temperature": 0.0}`. SDK 1.x dropped
  `temperature`/`top_p`/`top_k` from the typed `messages.create()` signature. Passing
  `temperature=` directly raises `TypeError`. Newer models (Opus 5, Sonnet 5) reject the parameter
  on the wire entirely — switching `MODEL_NAME` to one of those requires deleting the `extra_body`
  line from every setup cell.
- **The Example Playground at the bottom duplicates the lesson's cells** verbatim. Editing a lesson
  example generally means editing its playground twin too.
- **Some cells are broken on purpose.** In `01`, the cell using a set literal
  (`messages=[{"Hi Claude, how are you?"}]`) and the two consecutive `user` turns are teaching
  examples for Messages API errors; the surrounding markdown says so. Do not "fix" them.

## Exercises and grading

- Chapters 1–5 and 8 grade inline with a small `grade_exercise(text)` defined in the same cell —
  usually a substring or regex check on Claude's output. Chapters 6–7 instead loop over an `EMAILS`
  list and match against a `REGEX_CATEGORIES` dict. Chapter 9 and the Tool Use appendix are
  open-ended with no grader.
- Exercise cells carry a comment marking the one field the learner should change
  (`# Prompt - this is the only field you should change`). Respect that boundary.
- `Anthropic 1P/hints.py` holds every `exercise_N_M_hint` string plus a few
  `exercise_N_M_solution` strings (6.1, 7.1, 9.1, 9.2, 10.2.1). Chapters pull them with
  `from hints import exercise_1_1_hint`.

## Known rough edges

- `10.2_Appendix_Tool Use.ipynb` **hardcodes `claude-3-sonnet-20240229`** in its setup cell instead
  of reading `MODEL_NAME`, and teaches the legacy XML `<function_calls>` prompting style rather than
  the modern `tools=` parameter with `tool_use` content blocks. It has not been migrated.
- The upgrade tracked in `docs/task.md` is incomplete — Parts 2 and 3 have unchecked items.
- Notebooks in git HEAD store `"source"` as a single string; Jupyter rewrites it as a line array on
  save. Opening and saving a notebook therefore produces a large whole-file diff even with no real
  change. Check `git diff` before committing to confirm the change is substantive.
- Model references live in three places — `00`'s `MODEL_NAME`, `README.md`, and
  `Anthropic 1P/README.md`. Update all three together.

## Git

`.gitignore` covers `.env`, `.ipynb_checkpoints/`, `__pycache__/`, `*.pyc`, `.DS_Store`. Notebook
outputs are **not** stripped and are committed — avoid committing cells whose output contains
anything sensitive.
