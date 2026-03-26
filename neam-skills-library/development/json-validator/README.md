# JSONValidator

Performs a lightweight structural check on a string to determine whether it looks like valid JSON.

## Params
- `text` — The string to inspect

## How It Works
Trims whitespace, checks that the string is non-empty, then inspects the first character
using `string_slice()`. A leading `{` indicates an object; `[` indicates an array.
Anything else is flagged as invalid.

## Example
Input: `text="{\"name\": \"Alice\"}"`
Output: `Valid JSON`

Input: `text="name=Alice"`
Output: `Invalid JSON: must start with { or [`
