# Welcome to Anthropic's Prompt Engineering Interactive Tutorial

## Course introduction and goals

This course is intended to provide you with a comprehensive step-by-step understanding of how to engineer optimal prompts within Claude.

**After completing this course, you will be able to**:

- Master the basic structure of a good prompt
- Recognize common failure modes and learn the '80/20' techniques to address them
- Understand Claude's strengths and weaknesses
- Build strong prompts from scratch for common use cases

## Environment Setup

### Option A – Anthropic Direct API (recommended for this tutorial)

```powershell
# Activate the included virtual environment
.\prompt-eng\Scripts\Activate.ps1

# Then launch Jupyter
jupyter notebook "Anthropic 1P/"
```

Select the **"Prompt Eng"** kernel when opening any notebook.

> The venv already has `anthropic` (latest), `jupyter`, `notebook`, `ipykernel`, and `pickleshare` installed.  
> To install from scratch: `pip install -r requirements.txt`

### Option B – Amazon Bedrock

Refer to `AmazonBedrock/README.md` for AWS credential setup.

## Course structure and content

This course is structured to allow you many chances to practice writing and troubleshooting prompts yourself. The course is broken up into **9 chapters with accompanying exercises**, as well as an appendix of even more advanced methods. It is intended for you to **work through the course in chapter order**.

**Each lesson has an "Example Playground" area** at the bottom where you are free to experiment with the examples in the lesson and see for yourself how changing prompts can change Claude's responses. There is also an [answer key](https://docs.google.com/spreadsheets/d/1jIxjzUWG-6xBVIa2ay6yDpLyeuOh_hR_ZB75a47KX_E/edit?usp=sharing).

Note: This tutorial uses **Claude Haiku 4.5** (`claude-haiku-4-5-20251001`) — the current fastest and most cost-effective Claude model, and a direct successor to the original Claude 3 Haiku. Anthropic's full model lineup includes Haiku 4.5, Sonnet 5, Opus 5, and Fable 5.1.

*This tutorial also exists on [Google Sheets using Anthropic's Claude for Sheets extension](https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8/edit?usp=sharing). We recommend using that version as it is more user friendly.*

When you are ready to begin, go to `01_Basic Prompt Structure` to proceed.

## Table of Contents

Each chapter consists of a lesson and a set of exercises.

### Beginner

- **Chapter 1:** Basic Prompt Structure

- **Chapter 2:** Being Clear and Direct  

- **Chapter 3:** Assigning Roles

### Intermediate

- **Chapter 4:** Separating Data from Instructions

- **Chapter 5:** Formatting Output & Speaking for Claude

- **Chapter 6:** Precognition (Thinking Step by Step)

- **Chapter 7:** Using Examples

### Advanced

- **Chapter 8:** Avoiding Hallucinations

- **Chapter 9:** Building Complex Prompts (Industry Use Cases)
  - Complex Prompts from Scratch - Chatbot
  - Complex Prompts for Legal Services
  - **Exercise:** Complex Prompts for Financial Services
  - **Exercise:** Complex Prompts for Coding
  - Congratulations & Next Steps

- **Appendix:** Beyond Standard Prompting
  - Chaining Prompts
  - Tool Use
  - Search & Retrieval
