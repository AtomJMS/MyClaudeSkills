---
name: emails-with-attachments
description: Find Outlook emails that have attachments, optionally filtered by sender, date range, or topic. Use when the user asks for emails with attachments, attached files, or "emails that have a file/PDF/document".
argument-hint: [sender, date range, or topic]
---

Find emails with attachments using the Microsoft 365 `outlook_email_search` tool.

## Search

- Always put `hasattachments:true` in `query`. Add any topic words after it, e.g. `hasattachments:true invoice`.
- If the user names a sender, pass it in `sender`. If they give a time window, pass `afterDateTime` / `beforeDateTime`. Never invent a date range.
- Do not set `folderName` or `order` together with `query` — the search covers all folders; tell the user that.
- Use `limit: 25`. If the response ends with `nextOffset`, keep paging when the user asked for everything in a period; otherwise say more results remain and offer to continue.
- Keep only results where `hasAttachments` is `true` (the search can occasionally return others).
- Results are not guaranteed to be in date order — sort them newest first yourself.

If the Microsoft 365 tools are not available, tell the user the Microsoft 365 connector needs to be connected in their claude.ai connector settings.

## Output

Show a table, newest first:

| Received | From | Subject | Link |
|---|---|---|---|
| 2026-06-24 | jsonza@fultonmay.com | New Onboarding and Offboarding Form | [Open](webLink) |

- Format `receivedDateTime` as YYYY-MM-DD.
- Link to each email with its `webLink`.
- End with the count shown, and whether more results remain.

If the user wants the attachment names or contents, read that email with `read_resource` on its `uri`.
