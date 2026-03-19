# DataCounter

Counts the number of items in a JSON array string without needing a built-in `.length` method.

## Params
- `json_array` — A valid JSON array as a string, e.g. `"[1,2,3]"` or `"[\"a\",\"b\"]"`

## How It Works
Strips the outer `[` and `]`, splits the inner content by `","`, and counts the resulting
segments (commas + 1). Returns a human-readable label like `"Items: 3"`.

## Example
Input: `json_array="[10,20,30,40]"`
Output: `Items: 4`
