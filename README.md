# diff-to-prompt

A CLI tool that turns your `git diff` into a structured prompt for LLM-based code review.

## What it does

`diff_to_prompt.py` collects your current git changes (staged, unstaged, or against a specific ref), pulls in recent commit history for context, and assembles everything into a single formatted prompt you can paste into any LLM (ChatGPT, Claude, etc.) to get a code review.

## Requirements

- Python 3.10+
- Git installed and available on your `PATH`

## Usage

Run it from inside any git repository:-

```bash
python diff_to_prompt.py [OPTIONS]
```

### Options

| Flag | Default | Description |
|------|---------|-------------|
| `--staged` | `False` | Use staged changes instead of unstaged working tree changes |
| `--compare REF` | — | Diff against a branch, tag, or commit (e.g. `main`, `v1.0`, `abc1234`) |
| `--context N` | `5` | Number of surrounding context lines to include in the diff |
| `--commits N` | `10` | Number of recent commit summaries to include for context |

### Examples

Review your unstaged changes:
```bash
python diff_to_prompt.py
```

Review only staged changes:
```bash
python diff_to_prompt.py --staged
```

Compare your current branch against `main`:
```bash
python diff_to_prompt.py --compare main
```

Pipe the output directly to your clipboard (macOS):
```bash
python diff_to_prompt.py --staged | pbcopy
```

## Output

The tool prints a structured prompt to stdout containing:

1. A system instruction for the LLM reviewer
2. Recent commit history
3. A summary of changed files and their change types
4. The full raw diff
5. A closing instruction asking for a review covering bugs, readability, and improvements
