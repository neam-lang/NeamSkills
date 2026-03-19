# LogFormatter

Produces a structured log line with a live timestamp, uppercased level label, and message body.

## Params
- `level` — Severity label: `INFO`, `WARN`, or `ERROR` (case-insensitive — it will be uppercased)
- `message` — The text to log

## How It Works
Calls `time_now()` to get the current timestamp, formats it as `YYYY-MM-DD HH:MM:SS` using
`time_format()`, then assembles the final string.

## Example
Input: `level="warn"`, `message="Disk usage above 80%"`
Output: `[2026-03-19 14:05:32] [WARN] Disk usage above 80%`
