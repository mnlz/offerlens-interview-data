# Install OfferLens Interview Data

## Automatic install

The universal command installs the Skill into every supported Agent detected on the machine:

```bash
npx skills add mnlz/offerlens-interview-data --all
```

This covers Codex, Claude Code, Cursor, OpenCode, OpenClaw, Gemini CLI, and GitHub Copilot when their standard Skill directories are available. Restart the Agent after installation if it does not reload Skills automatically.

To add current official campus JDs as a separate capability:

```bash
npx skills add mnlz/offerlens-jobs --all
```

The interview-data Skill also accepts a JD pasted directly by the user, so the jobs Skill is optional.

## Free access

No configuration is needed. The client falls back to the trial key `offerlens`, with 10 successful non-empty requests per IP and at most 10 interview records per request.

## Purchased access

Set the key supplied after purchase. Do not paste it into an Agent prompt.

macOS and Linux (`zsh` or `bash`):

```bash
export OFFERLENS_API_KEY="your-key"
```

Fish:

```fish
set -Ux OFFERLENS_API_KEY "your-key"
```

PowerShell:

```powershell
$env:OFFERLENS_API_KEY = "your-key"
```

Persist the variable using the normal secure environment configuration for your Agent or shell. Purchased access allows pagination and up to 100 records per request.

## Verify

From the installed Skill directory:

```bash
python3 scripts/query_interviews.py --company 字节跳动 --days 90 --limit 1
```

A successful response includes `data`, `total`, `access`, and `trial_remaining`. Resume text is never part of this request.
