# PasswordValidator

Evaluates a password string and rates its strength based on length tiers.

## Params
- `password` — The password to evaluate

## Strength Tiers
| Length       | Rating  | Message                                          |
|--------------|---------|--------------------------------------------------|
| < 8 chars    | Weak    | Too short (min 8 characters)                     |
| 8–11 chars   | Medium  | Consider a longer password (12+ recommended)     |
| 12+ chars    | Strong  | Good password length                             |

## Example
Input: `password="hello"`
Output: `Weak: too short (min 8 characters)`

Input: `password="MySecurePass123!"`
Output: `Strong: good password length`
