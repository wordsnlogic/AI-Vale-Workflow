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

The short demo below shows the **exact terminal experience**, including:
- Vale findings
- AI rewrite suggestions
- Human approval prompts
- Line-specific edits applied to disk

<video controls width="100%">
  <source src="vale_ai_script_demo_cut_no_audio.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

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
