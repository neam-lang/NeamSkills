# NeamSkills

> Official skill library for the [Neam programming language](https://github.com/neam-lang/Neam)

Neam is a compiled AI agent programming language. This repo gives you three things:

1. **`Claude-Neam-Programming-skill`** — comprehensive Claude Code skill covering the full Neam language (recommended)
2. **`neam-programming` skill** — lightweight quick-reference skill for Claude Code
3. **31 ready-made `.neam` skills** — copy into your agents to add abilities instantly

---

## New to Neam? Start Here

Import the comprehensive skill into Claude Code:

```bash
/import skills/claude-neam-programming/SKILL.md
```

This gives Claude full knowledge of Neam — all 13 data types, agents, claw/forge agents, multi-agent orchestration, RAG (8 strategies), skills, MCP, guards, policies, OOP (structs/traits/sealed types), modules, cloud deployment, and 100+ built-in functions.

**Lightweight alternative** (quick reference only):

```bash
/import skills/neam-programming/SKILL.md
```

---

## Quick Example

```neam
// 1. Copy a skill from this repo into your .neam file
skill Calculator {
  description: "Perform math operations",
  params: [
    { name: "operation", schema: { "type": "string", "description": "add/sub/mul/div" } },
    { name: "a", schema: { "type": "number", "description": "First number" } },
    { name: "b", schema: { "type": "number", "description": "Second number" } }
  ],
  impl: fun(operation, a, b) {
    if (operation == "add") { return a + b; }
    if (operation == "sub") { return a - b; }
    if (operation == "mul") { return a * b; }
    if (operation == "div") { return a / b; }
  }
}

// 2. Attach it to your agent
agent MathBot {
  provider: "openai",
  model: "gpt-4o-mini",
  system: "You are a math assistant. Use Calculator to solve problems.",
  skills: [Calculator]
}

// 3. Run it
emit MathBot.ask("What is 25 multiplied by 4?");
```

---

## Skills Library

### Utility
| Skill | Description |
|-------|-------------|
| `Calculator` | Add, subtract, multiply, divide, power, square root |
| `UUIDGen` | Generate a random UUID v4 |
| `GetTimestamp` `FormatTime` | Current time and date formatting |
| `TextUpper` `TextLower` `TextTrim` | Text transformations |
| `Hasher` `Base64Encode` | SHA256/SHA1/MD5 hashing, Base64 encoding |

### Web
| Skill | Description |
|-------|-------------|
| `WebFetch` | Fetch a URL with HTTP GET |
| `HTTPRequest` | Full HTTP requests — POST, GET, PUT, DELETE |
| `URLBuilder` | Build URLs from base, path, and query params |

### Data
| Skill | Description |
|-------|-------------|
| `JSONParser` `JSONFormatter` | Parse and format JSON |
| `CSVParser` | Parse CSV text into rows |
| `DataCounter` | Count items in a JSON array |

### File
| Skill | Description |
|-------|-------------|
| `FileReader` | Read a file from disk |
| `FileWriter` | Write content to a file |
| `FileExists` | Check if a file exists |
| `FileCopy` | Copy a file from one path to another |

### Math
| Skill | Description |
|-------|-------------|
| `UnitConverter` | Convert km/miles, kg/lbs, celsius/fahrenheit, meters/feet |
| `FindMax` `FindMin` | Max and min from a list of numbers |

### Security
| Skill | Description |
|-------|-------------|
| `PasswordValidator` | Check password strength |
| `HMACSign` | Generate an HMAC signature with a secret key |

### Development
| Skill | Description |
|-------|-------------|
| `LogFormatter` | Format log messages with timestamp and level |
| `JSONValidator` | Validate whether a string is valid JSON |

### Productivity
| Skill | Description |
|-------|-------------|
| `WordCounter` `CharCounter` | Count words and characters in text |
| `DaysFromNow` `TimestampToDate` | Future dates and timestamp conversion |

---

## Repo Structure

```
NeamSkills/
├── skills/
│   ├── claude-neam-programming/ ← Full Claude Code skill (recommended)
│   │   ├── SKILL.md
│   │   └── README.md
│   ├── neam-programming/        ← Lightweight Claude Code skill
│   │   └── SKILL.md
│   ├── utility/              ← Ready-made .neam skills
│   │   ├── calculator/
│   │   ├── uuid-gen/
│   │   ├── timer/
│   │   ├── text-tools/
│   │   └── hasher/
│   ├── web/
│   │   ├── web-fetch/
│   │   ├── http-request/
│   │   └── url-builder/
│   ├── data/
│   │   ├── json-tools/
│   │   ├── csv-parser/
│   │   └── data-counter/
│   ├── file/
│   │   ├── file-reader/
│   │   ├── file-writer/
│   │   ├── file-exists/
│   │   └── file-copy/
│   ├── math/
│   │   ├── unit-converter/
│   │   └── statistics/
│   ├── security/
│   │   ├── password-validator/
│   │   └── hmac-sign/
│   ├── development/
│   │   ├── log-formatter/
│   │   └── json-validator/
│   └── productivity/
│       ├── word-counter/
│       └── date-calculator/
└── README.md
```

---

## Upcoming Skills

- **Claude-Neam-special_agents-skill** — Deep coverage of specialized agent patterns: cognitive agents, voice agents, A2A protocol, advanced orchestration, and production deployment patterns

---

## Links

- [Neam Language](https://github.com/neam-lang/Neam) — compiler, runtime, REPL
- [Neam Documentation](https://neam-lang.github.io/Neam-The-AI-Native-Programming-Language/) — full language book (28 chapters)
- [Smart Support Claw](https://github.com/neam-lang/smart_support_claw) — production claw agent example
- [NeamForge Site Generator](https://github.com/samsuljahith/neamforge-site-generator) — forge agent example

---

## License

MIT
