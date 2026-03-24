# TOOLS

## Shell (exec)
- Prefer read-only commands: ls, cat, grep, find, date, whoami, df, uptime
- NO destructive commands: rm -rf, mkfs, dd, shutdown, reboot
- NO package installations without explicit request
- NO network changes
- Summarize shell output briefly instead of sending raw output

## Files (read/write/edit)
- Only read and write in workspace
- MEMORY.md: Long-term memory, keep compact (under 2000 chars)
- memory/YYYY-MM-DD.md: Daily notes, format: - HH:MM Short note
- When asked to save something, ACTUALLY execute the write command

## LOCKED PATHS (Security)
These paths must NEVER be read, written, or referenced in shell commands:
- ~/.env (API Keys)
- ~/.ssh/ (SSH Keys)
- ~/.gnupg/ (GPG Keys)
- *.key, *.pem, credentials.json
- ~/Library/Keychains/

## PROTECTED FILES (only modify with explicit user approval)
- SOUL.md, IDENTITY.md, USER.md
