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

## Demo video



https://github.com/user-attachments/assets/3a657c15-89ab-4654-a6dc-21e17e6c2a2c




▶️ **Watch / download the demo video (recommended)**  
https://raw.githubusercontent.com/wordsnlogic/AI-Vale-Workflow/main/vale_ai_script_demo_cut_no_audio.mp4

📄 **View the video file on GitHub**  
https://github.com/wordsnlogic/AI-Vale-Workflow/blob/main/vale_ai_script_demo_cut_no_audio.mp4

**About the demo**

- Runtime: ~35 seconds
- No audio
- Real terminal output
- Shows Vale findings, AI suggestions, and human approvals

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

---

### Example: Politeness (“please”)

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

## Requirements

### Tools

- **Python 3.9+**
- **Vale CLI** installed and available on your `PATH`
- A documentation repo containing:
  - Markdown files (`.md`)
  - A `.vale.ini`
  - Vale rule styles (for example, Microsoft)

---

## Install

### 1) Clone this repo

```bash
git clone https://github.com/wordsnlogic/AI-Vale-Workflow.git
cd AI-Vale-Workflow
```

### 2) Install Vale

**macOS (Homebrew)**

```bash
brew install vale
```

**Windows (Chocolatey)**

```powershell
choco install vale
```

**Linux (Snap)**

```bash
sudo snap install vale
```

Verify:

```bash
vale --version
```

---

## Usage

```bash
python ai_vale_workflow.py --folder path/to/docs/ --dry-run
```

---

## Safety and editorial control

- Requires human approval
- Edits only flagged lines
- Never rewrites files automatically
