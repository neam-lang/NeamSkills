# DateCalculator

Two skills for working with dates and timestamps.

## Skills

### DaysFromNow
Adds a number of days to today's date and returns the resulting date string.
Pass a negative number to get a past date.

### TimestampToDate
Converts a Unix timestamp (in milliseconds) to a human-readable datetime string.

## Params

**DaysFromNow**
- `days` — Integer number of days to offset from today

**TimestampToDate**
- `timestamp` — Unix timestamp in milliseconds

## Example
DaysFromNow — Input: `days=7`
Output: `2026-03-26`

TimestampToDate — Input: `timestamp=1711843200000`
Output: `2024-03-31 00:00:00`
