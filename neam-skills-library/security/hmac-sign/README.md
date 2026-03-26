# HMACSign

Generates a keyed-hash message authentication code (HMAC) for a given message.

## Params
- `secret` — The private key used to sign (keep this safe)
- `message` — The data to sign
- `algorithm` — Either `"sha256"` (recommended) or `"sha1"`

## How It Works
Delegates directly to the built-in `crypto_hmac(algorithm, secret, message)` function and
returns the hex-encoded signature string.

## Example
Input: `secret="mysecret"`, `message="hello world"`, `algorithm="sha256"`
Output: `b94a8fe5ccb19ba61c4c0873d391e987982fbbd3` *(example — actual value depends on inputs)*
