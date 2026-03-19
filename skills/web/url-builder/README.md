# URLBuilder

Builds a complete URL by joining a base URL, a path, and an optional query string.

## Params
- `base` — Base URL (e.g. `https://api.example.com`)
- `path` — Path segment (e.g. `/users/search`)
- `query` — Query string in `key=value&key2=value2` format, or `""` to omit

## How It Works
Concatenates `base + path`. If `query` is non-empty, appends `?query` to the result.

## Example
Input: `base="https://api.example.com"`, `path="/search"`, `query="q=hello&limit=10"`
Output: `https://api.example.com/search?q=hello&limit=10`
