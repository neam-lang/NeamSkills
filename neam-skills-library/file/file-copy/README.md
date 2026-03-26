# FileCopy

Copies a file from a source path to a destination path with a pre-flight existence check.

## Params
- `source` — Path to the file you want to copy
- `destination` — Path where the copy should be written

## How It Works
First checks that the source file exists using `file_exists()`. If it does not, returns an
error message. If it does, calls `file_copy()` and returns a confirmation string.

## Example
Input: `source="/data/input.txt"`, `destination="/backup/input.txt"`
Output: `Copied: /data/input.txt -> /backup/input.txt`
