# Statistics

Two skills for finding extreme values in a list of numbers.

## Skills

### FindMax
Returns the largest number from a comma-separated list.

### FindMin
Returns the smallest number from a comma-separated list.

## Params (both skills)
- `numbers` — A comma-separated string of numbers, e.g. `"3,1,4,1,5,9"`

## How It Works
Splits the input string by `","`, then iterates with a `for-in` loop, tracking the running
maximum or minimum using `math_max()` / `math_min()`.

## Example
Input: `numbers="8,3,15,2,7"`
FindMax output: `15`
FindMin output: `2`
