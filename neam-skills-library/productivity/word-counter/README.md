# WordCounter & CharCounter

Two productivity skills for measuring text length.

## Skills

### WordCounter
Counts the approximate number of space-separated words in a string.

### CharCounter
Counts the exact number of characters (including spaces and punctuation) in a string.

## Params
Both skills take a single param:
- `text` — The input string to measure

## How It Works
- **WordCounter**: trims the text, splits by `" "`, then counts segments via comma-counting in the JSON array.
- **CharCounter**: calls `string_length(text)` directly.

## Example
Input: `text="Hello world foo"`
WordCounter output: `Word count: 3`
CharCounter output: `Character count: 15`
