# Heartbeat Checklist

## Most Important Rule
If EVERYTHING is OK: Reply ONLY with the word HEARTBEAT_OK
Do NOT send a message if there is nothing to report.
A message is ONLY allowed if a concrete problem exists.

## Quick Checks (every 2 hours, 07-22 local time)

### 1. Pending Actions
- Pending approvals about to expire?
- Reminders in memory/ that are due today?

### 2. Exchange Check (Multi-Agent)
- Pull latest from shared exchange repo
- New files from other agents? Read, respond, push
- If nothing new: continue (do NOT report)

### 3. System Health (read only)
- Last line of watchdog log — any PROBLEM or WARNING?
- Last 3 lines of gateway error log — new errors since last check?

### 4. Decision

| Situation | Action |
|-----------|--------|
| Everything OK | HEARTBEAT_OK (NO message!) |
| Approval expiring soon | Short message (max 200 chars) |
| Reminder due | Short message |
| System PROBLEM/WARNING | Short message with details |
| Exchange: New items | Read + respond + push (NO message to user) |

## Do NOT Report
- "No approvals found" — that's the normal state
- "No reminders" — that's normal
- Status updates without action needed
- The current time
