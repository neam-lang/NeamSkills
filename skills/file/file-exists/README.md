# FileExists

Checks whether a file is present at the specified path.

## Params
- `path` — The file path to check (absolute or relative)

## How It Works
Calls the built-in `file_exists()` function and returns the string `"true"` if the file
is found, or `"false"` if it is not.

## Example
Input: `path="/tmp/report.txt"`
Output: `"true"` (if the file exists) or `"false"` (if it does not)
