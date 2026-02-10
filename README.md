# AI Vale Workflow

A human-in-the-loop workflow that combines **Vale** linting with **AI rewrite suggestions** to help technical writing teams resolve style issues faster—without surrendering editorial control.

This tool:

- Runs **Vale** with `--output=JSON`
- Sends only **flagged lines + Vale messages** to an OpenAI model
- Prints a **Before / After / Vale Comment** table for review
- Requires explicit **human approval**
- Applies **line-specific edits only**
- Writes approved changes to disk
- Asks the writer to manually **re-run Vale**

> This is an assistive tool. Nothing changes unless a human approves it.

---

## Why this exists

Vale is excellent at *identifying* documentation issues, but it doesn’t help writers fix them.

AI is excellent at *rewriting*, but unsafe when given too much autonomy.

This workflow deliberately combines the two:

- Vale remains the **source of truth**
- AI provides **targeted, scoped suggestions**
- Humans remain **in control of every change**

---

## How it works (high level)

1. You run this script against a Markdown file or folder.
2. Vale flags issues (rule message + line number).
3. The script sends the **exact flagged line** and **Vale message** to the AI.
4. The script prints a **Before / After / Vale Comment** table.
5. You approve (`y`) or skip (`n`) each suggestion.
6. Approved edits are written to disk on the exact line number.
7. You manually re-run Vale to verify results.

---

## Example output (exact terminal output)

The examples below show **exactly what the script prints in the terminal**.

Tables may appear vertically expanded due to fixed-width wrapping.  
This is intentional and mirrors real CLI behavior.

Each example includes:
- A short readable summary
- The raw terminal output for full transparency

---

### Example 1: Politeness (“please”)

**Summary**

- Vale flags polite language
- AI removes “please” without changing meaning
- Writer approves the change

**Exact terminal output**

```text
+--------------------+-------------------------+------------------------------------------------------------+
| Before AI          | After AI                | Vale Comment                                               |
+====================+=========================+============================================================+
| Please ensure that | Ensure that the user is | Avoid using “please” in technical documentation.           |
| the user is        | notified when the       |                                                            |
| notified when the  | backup is completed.    |                                                            |
| backup is          |                         |                                                            |
| completed.         |                         |                                                            |
+--------------------+-------------------------+------------------------------------------------------------+

Apply this change? (y/n):
```

---

### Example 2: UI interaction language (“click”)

**Summary**

- Vale enforces device-neutral language
- AI rewrites UI instructions
- Markdown formatting is preserved

**Exact terminal output**

```text
+--------------------+-------------------------+------------------------------------------------------------+
| Before AI          | After AI                | Vale Comment                                               |
+====================+=========================+============================================================+
| Click the Create   | Select **Create         | Avoid “click.” Use a more general verb.                    |
| Project button to  | Project** to start a    |                                                            |
| start a new ACME   | new ACME Cloud project. |                                                            |
| Cloud project.     |                         |                                                            |
+--------------------+-------------------------+------------------------------------------------------------+

Apply this change? (y/n):
```

---

### Example 3: Complex wording → simpler language

**Summary**

- Vale flags unnecessarily complex phrasing
- AI simplifies wording without semantic drift
- Writer remains the final decision-maker

**Exact terminal output**

```text
+--------------------+-------------------------+------------------------------------------------------------+
| Before AI          | After AI                | Vale Comment                                               |
+====================+=========================+============================================================+
| In order to        | To authenticate, the    | Use simpler words when possible.                           |
| successfully       | client must use a valid |                                                            |
| authenticate, the  | token.                  |                                                            |
| client must        |                         |                                                            |
| utilize a valid    |                         |                                                            |
| token.             |                         |                                                            |
+--------------------+-------------------------+------------------------------------------------------------+

Apply this change? (y/n):
```

---

## What gets sent to the AI

For each Vale issue, the script sends:

- The **exact flagged line**
- The **Vale message**

It does **not** send:

- The entire file
- Surrounding paragraphs
- The full documentation set

This design keeps the workflow safer, predictable, and auditable.

---

## Requirements

### Tools

- **Python 3.9+**
- **Vale CLI** installed and available on your `PATH`
- A documentation repo containing:
  - Markdown files (`.md`)
  - A `.vale.ini`
  - Vale rule styles (for example, Microsoft)

### Python dependency

- `openai`

---

## Install

### 1) Clone this repo

```bash
git clone https://github.com/<your-org-or-username>/ai-vale-workflow.git
cd ai-vale-workflow
```

### 2) Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3) Install dependencies

```bash
pip install openai
```

---

## Configure OpenAI API key

Set your API key as an environment variable:

```bash
export OPENAI_API_KEY="YOUR_KEY_HERE"
```

If the key is missing, the script exits with:

```text
ERROR: OPENAI_API_KEY not found in environment.
```

---

## Ensure Vale works first

Before using AI, confirm Vale runs successfully in your documentation repo:

```bash
vale path/to/docs
```

This workflow assumes:

- Vale is installed
- `.vale.ini` is discoverable
- Rules resolve correctly

---

## Usage

The script supports:

- `--file` to process a single Markdown file
- `--folder` to process a folder recursively
- `--dry-run` to preview changes without writing to disk

### Single file

```bash
python ai_vale_workflow.py --file path/to/docs/index.md
```

### Folder (recursive)

```bash
python ai_vale_workflow.py --folder path/to/docs/
```

### Dry run (recommended first)

```bash
python ai_vale_workflow.py --folder path/to/docs/ --dry-run
```

For each issue, you’ll be prompted:

```text
Apply this change? (y/n):
```

---

## Safety and editorial control

This tool is intentionally conservative.

It **does**:

- Require human approval
- Edit only flagged lines
- Print file and line number confirmations
- Encourage manual re-validation

It **does not**:

- Auto-rewrite files
- Apply changes silently
- Replace editorial judgment
- Re-run Vale automatically

---

## Known limitations

- Line-based replacement assumes the issue fits on one line
- Multi-line or structural issues may require manual edits
- Vale rule output formats may vary by rule pack

These constraints are deliberate to preserve trust.

---

## Recommended team workflow

1. Run with `--dry-run` first
2. Apply changes in small batches
3. Re-run Vale and review diffs
4. Merge through standard PR review
