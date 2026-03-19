# CSVParser

Splits a CSV string into individual rows and returns them as a JSON array.

## Params
- `csv_text` — The full CSV content as a single string (rows separated by newlines)
- `delimiter` — The column separator character, typically `","`

## How It Works
Splits the input on `\n` to get each row, then returns the array as a JSON string.
Note: each element in the result is a raw row string; further splitting on `delimiter` can be done downstream.

## Example
Input: `csv_text="name,age\nAlice,30\nBob,25"`, `delimiter=","`
Output: `["name,age","Alice,30","Bob,25"]`
