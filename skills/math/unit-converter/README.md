# UnitConverter

Converts a numeric value between common real-world units.

## Supported Conversions
| From        | To          |
|-------------|-------------|
| km          | miles       |
| miles       | km          |
| kg          | lbs         |
| lbs         | kg          |
| celsius     | fahrenheit  |
| fahrenheit  | celsius     |
| meters      | feet        |
| feet        | meters      |

## Params
- `value` — The number to convert
- `from_unit` — Source unit string (see table above)
- `to_unit` — Target unit string (see table above)

## Example
Input: `value=100`, `from_unit="km"`, `to_unit="miles"`
Output: `62.14 miles`
