# Neam Skills Hub

A collection of ready-made skills for the [Neam programming language](https://github.com/neam-lang/Neam).

Browse by category, copy a skill into your `.neam` file, and attach it to any agent.

---

## How to Use Any Skill

```neam
// Step 1 — Copy the skill definition into your .neam file
skill Calculator {
  description: "Perform basic math operations",
  params: [
    { name: "operation", schema: { "type": "string", "description": "add/sub/mul/div/pow/sqrt" } },
    { name: "a", schema: { "type": "number", "description": "First number" } },
    { name: "b", schema: { "type": "number", "description": "Second number" } }
  ],
  impl: fun(operation, a, b) {
    if (operation == "add") { return a + b; }
    if (operation == "mul") { return a * b; }
    return "Unknown operation";
  }
}

// Step 2 — Attach it to your agent
agent MathBot {
  provider: "openai",
  model: "gpt-4o-mini",
  system: "You are a math assistant. Use Calculator to solve problems.",
  skills: [Calculator]
}

// Step 3 — Run
let answer = MathBot.ask("What is 25 multiplied by 4?");
emit answer;
```

---

## Skills by Category

### Utility
General-purpose tools every agent can use.

| Skill | File | Description |
|-------|------|-------------|
| `Calculator` | [utility/calculator](./skills/utility/calculator/) | Add, subtract, multiply, divide, power, square root |
| `UUIDGen` | [utility/uuid-gen](./skills/utility/uuid-gen/) | Generate a random UUID v4 |
| `GetTimestamp` `FormatTime` | [utility/timer](./skills/utility/timer/) | Current timestamp, format dates |
| `TextUpper` `TextLower` `TextTrim` | [utility/text-tools](./skills/utility/text-tools/) | Uppercase, lowercase, trim whitespace |
| `Hasher` `Base64Encode` | [utility/hasher](./skills/utility/hasher/) | SHA256/SHA1/MD5 hash, Base64 encode |

---

### Web
Make HTTP requests and build URLs.

| Skill | File | Description |
|-------|------|-------------|
| `WebFetch` | [web/web-fetch](./skills/web/web-fetch/) | Fetch a URL with HTTP GET |
| `HTTPRequest` | [web/http-request](./skills/web/http-request/) | POST/GET/PUT/DELETE with body and headers |
| `URLBuilder` | [web/url-builder](./skills/web/url-builder/) | Build a URL from base, path, and query string |

---

### Data
Parse, format, and count structured data.

| Skill | File | Description |
|-------|------|-------------|
| `JSONParser` `JSONFormatter` | [data/json-tools](./skills/data/json-tools/) | Parse JSON strings, convert objects to JSON |
| `CSVParser` | [data/csv-parser](./skills/data/csv-parser/) | Parse CSV text into rows |
| `DataCounter` | [data/data-counter](./skills/data/data-counter/) | Count items in a JSON array |

---

### File
Read, write, check, and copy files on disk.

| Skill | File | Description |
|-------|------|-------------|
| `FileReader` | [file/file-reader](./skills/file/file-reader/) | Read a file from disk |
| `FileWriter` | [file/file-writer](./skills/file/file-writer/) | Write content to a file |
| `FileExists` | [file/file-exists](./skills/file/file-exists/) | Check if a file exists |
| `FileCopy` | [file/file-copy](./skills/file/file-copy/) | Copy a file from one path to another |

---

### Math
Number crunching and unit conversions.

| Skill | File | Description |
|-------|------|-------------|
| `UnitConverter` | [math/unit-converter](./skills/math/unit-converter/) | Convert km/miles, kg/lbs, celsius/fahrenheit, meters/feet |
| `FindMax` `FindMin` | [math/statistics](./skills/math/statistics/) | Max and min value from a list of numbers |

---

### Security
Signing, hashing, and input validation.

| Skill | File | Description |
|-------|------|-------------|
| `PasswordValidator` | [security/password-validator](./skills/security/password-validator/) | Check password strength |
| `HMACSign` | [security/hmac-sign](./skills/security/hmac-sign/) | Generate HMAC signature with a secret key |

---

### Development
Tools for building and debugging agents.

| Skill | File | Description |
|-------|------|-------------|
| `LogFormatter` | [development/log-formatter](./skills/development/log-formatter/) | Format log messages with timestamp and level |
| `JSONValidator` | [development/json-validator](./skills/development/json-validator/) | Validate whether a string is valid JSON |

---

### Productivity
Text and date tools for everyday tasks.

| Skill | File | Description |
|-------|------|-------------|
| `WordCounter` `CharCounter` | [productivity/word-counter](./skills/productivity/word-counter/) | Count words and characters in text |
| `DaysFromNow` `TimestampToDate` | [productivity/date-calculator](./skills/productivity/date-calculator/) | Calculate future dates, convert timestamps |

---

## All Skills — Quick List

| # | Skill | Category |
|---|-------|----------|
| 1 | `Calculator` | Utility |
| 2 | `UUIDGen` | Utility |
| 3 | `GetTimestamp` | Utility |
| 4 | `FormatTime` | Utility |
| 5 | `TextUpper` | Utility |
| 6 | `TextLower` | Utility |
| 7 | `TextTrim` | Utility |
| 8 | `Hasher` | Utility |
| 9 | `Base64Encode` | Utility |
| 10 | `WebFetch` | Web |
| 11 | `HTTPRequest` | Web |
| 12 | `URLBuilder` | Web |
| 13 | `JSONParser` | Data |
| 14 | `JSONFormatter` | Data |
| 15 | `CSVParser` | Data |
| 16 | `DataCounter` | Data |
| 17 | `FileReader` | File |
| 18 | `FileWriter` | File |
| 19 | `FileExists` | File |
| 20 | `FileCopy` | File |
| 21 | `UnitConverter` | Math |
| 22 | `FindMax` | Math |
| 23 | `FindMin` | Math |
| 24 | `PasswordValidator` | Security |
| 25 | `HMACSign` | Security |
| 26 | `LogFormatter` | Development |
| 27 | `JSONValidator` | Development |
| 28 | `WordCounter` | Productivity |
| 29 | `CharCounter` | Productivity |
| 30 | `DaysFromNow` | Productivity |
| 31 | `TimestampToDate` | Productivity |

---

## License

MIT
