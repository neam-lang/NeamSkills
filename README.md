<p align="center">
  <img src="assets/banner.svg" alt="NeamSkills Banner" width="100%"/>
</p>

<p align="center">
  <a href="https://github.com/neam-lang/NeamSkills/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square" alt="License: MIT"/></a>
  <a href="https://github.com/neam-lang/Neam"><img src="https://img.shields.io/badge/language-Neam-7b2ff7.svg?style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0id2hpdGUiPjx0ZXh0IHg9IjQiIHk9IjE4IiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iYm9sZCI+TjwvdGV4dD48L3N2Zz4=" alt="Language: Neam"/></a>
  <a href="https://github.com/neam-lang/NeamSkills"><img src="https://img.shields.io/badge/skills-31+-00d2ff.svg?style=flat-square" alt="Skills: 31+"/></a>
  <a href="https://github.com/neam-lang/NeamSkills"><img src="https://img.shields.io/badge/built--in_functions-100+-a78bfa.svg?style=flat-square" alt="Built-in Functions: 100+"/></a>
  <a href="https://neam-lang.github.io/Neam-The-AI-Native-Programming-Language/"><img src="https://img.shields.io/badge/docs-28_chapters-green.svg?style=flat-square" alt="Docs: 28 chapters"/></a>
  <a href="#install-from-github"><img src="https://img.shields.io/badge/Claude_Code-plugin-ff6b35.svg?style=flat-square" alt="Claude Code Plugin"/></a>
  <a href="#install-from-github"><img src="https://img.shields.io/badge/install-verified-brightgreen.svg?style=flat-square" alt="Install Verified"/></a>
</p>

<p align="center">
  <b>Official skill library for the <a href="https://github.com/neam-lang/Neam">Neam programming language</a></b><br/>
  <sub>The compiled, AI-native language for building agent systems</sub>
</p>

---

Neam is a compiled AI agent programming language. This repo gives you four things:

1. **`Claude-Neam-Programming-skill`** — comprehensive Claude Code skill covering the full Neam language (recommended)
2. **`Claude-Neam-DIO-skill`** — comprehensive skill for building Intelligent Data Organizations with Neam's 14 agents + DIO orchestrator
3. **`neam-programming` skill** — lightweight quick-reference skill for Claude Code
4. **31 ready-made `.neam` skills** — copy into your agents to add abilities instantly

---

## New to Neam? Start Here

Pick the skills you need and install them. Both skills auto-activate — Claude uses them automatically when you work with `.neam` files or DIO agents.

| Skill | What it covers | Size |
|-------|---------------|------|
| **`claude-neam-programming`** | Core Neam language — 13 types, agents, claw/forge agents, RAG, guards, OOP, deployment, 100+ built-ins | 1,394 lines |
| **`claude-neam-dio`** | DIO ecosystem — 14 specialist agents, orchestrator, RACI, infrastructure profiles, DataSims | 1,561 lines |
| **`neam-programming`** | Lightweight quick-reference for basic Neam | 623 lines |

---

## Setup Instructions

### Option 1: Project-Level Skill (recommended for teams)

Copy the skills into your project's `.claude/skills/` directory. They auto-activate when Claude Code opens the project.

**Install both skills:**

```bash
# From your project root:
mkdir -p .claude/skills/claude-neam-programming .claude/skills/claude-neam-dio

curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-programming/SKILL.md \
  -o .claude/skills/claude-neam-programming/SKILL.md

curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-dio/SKILL.md \
  -o .claude/skills/claude-neam-dio/SKILL.md
```

Commit `.claude/skills/` to version control — the skills are available to anyone who clones your repo.

**Verify:** Run `/claude-neam-programming` or `/claude-neam-dio` in Claude Code. The full skill content should load.

**Install just one:**

```bash
# Neam language only:
mkdir -p .claude/skills/claude-neam-programming
curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-programming/SKILL.md \
  -o .claude/skills/claude-neam-programming/SKILL.md

# DIO only:
mkdir -p .claude/skills/claude-neam-dio
curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-dio/SKILL.md \
  -o .claude/skills/claude-neam-dio/SKILL.md
```

### Option 2: Personal Skill (works across all projects)

Install to `~/.claude/skills/` so the skills are available in every Claude Code session, regardless of project.

```bash
# Neam Language skill:
mkdir -p ~/.claude/skills/claude-neam-programming
curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-programming/SKILL.md \
  -o ~/.claude/skills/claude-neam-programming/SKILL.md

# DIO Data Intelligence skill:
mkdir -p ~/.claude/skills/claude-neam-dio
curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-dio/SKILL.md \
  -o ~/.claude/skills/claude-neam-dio/SKILL.md
```

**Verify:** Start a new Claude Code session and run `/claude-neam-programming` or `/claude-neam-dio`.

### Option 3: Direct Import (one-time use)

If you just want the skill for the current session without installing:

```bash
/import skills/claude-neam-programming/SKILL.md
/import skills/claude-neam-dio/SKILL.md
```

> **Note:** Direct imports only last for the current session. Use Option 1 or 2 for persistent access.

---

## Quick Examples

### DIO: Autonomous Data Lifecycle

```neam
budget B { cost: 500.00, tokens: 2000000 }
infrastructure_profile Infra { data_warehouse: { platform: "postgres" } }

dio agent MyDIO {
    mode: "config",
    task: "Predict customer churn, identify drivers, deploy with monitoring",
    infrastructure: Infra,
    provider: "openai",
    model: "gpt-4o",
    budget: B
}

print(dio_solve(MyDIO, "full_system"));
```

### Skills: Add Abilities to Agents

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

## Using Claude-Neam-DIO

The **Claude-Neam-DIO** skill teaches Claude to build autonomous data lifecycle systems using Neam's 14 specialist agents and the DIO orchestrator. It is based on [The Intelligent Data Organization with Neam](https://neam-lang.github.io/The-Intelligent-Data-Organization-with-Neam/) (30-chapter eBook) and verified against 22/22 passing E2E tests.

### When to use it

- Building data pipelines, warehouses, or ML systems with multiple agent roles
- Orchestrating end-to-end workflows: requirements -> ingestion -> transformation -> modeling -> deployment -> monitoring
- Working with infrastructure profiles (Snowflake, BigQuery, Databricks, Redshift, etc.)
- Needing RACI accountability, quality gates, or compliance automation (GDPR/CCPA)
- Writing `dio agent`, `datascientist agent`, `causal agent`, `datatest agent`, or any of the 14 DIO agent types

### What it covers

| Topic | Details |
|-------|---------|
| **14 Specialist Agents** | Full syntax, verified code examples, required fields, native functions |
| **DIO Orchestrator** | 3 modes (`auto`/`config`/`hybrid`), 8 auto-patterns, `dio_solve`/`dio_plan` |
| **Agent.MD** | 6 sections for encoding domain knowledge (improves model AUC by 7.7%) |
| **RACI Accountability** | Runtime-enforced responsibility assignment (2 invariants) |
| **3 Coordination Modes** | Centralized RACI, Swarm Stigmergy, Evolutionary GA |
| **Infrastructure Profiles** | 10 platforms abstracted so same code runs anywhere |
| **Error Handling** | 4-tier self-healing: retry -> adapt -> human review -> halt |
| **Security** | 6-layer defense-in-depth, zero-trust, phase-gated permissions |
| **DataSims** | Evaluation framework: 7-dimension scoring, ablation studies |
| **Troubleshooting** | 14 common errors with exact fixes |

### Example: Full production DIO program

```neam
budget DIOBudget { cost: 500.00, tokens: 2000000 }
budget AgentBudget { cost: 50.00, tokens: 500000 }

infrastructure_profile SimShopInfra {
    data_warehouse: {
        platform: "postgres",
        connection: env("SIMSHOP_PG_URL"),
        schemas: ["simshop_oltp", "simshop_dw", "ml_features"]
    },
    data_science: { mlflow: { uri: env("MLFLOW_TRACKING_URI") } },
    governance: { regulations: ["GDPR"], pii_columns: ["email", "phone"] }
}

databa agent ChurnBA { provider: "openai", model: "gpt-4o", budget: AgentBudget }
sql_connection SimShopDB { platform: "postgres", connection: env("SIMSHOP_PG_URL"), database: "simshop" }
analyst agent SimShopAnalyst { provider: "openai", model: "gpt-4o-mini", connections: [SimShopDB], budget: AgentBudget }
datascientist agent ChurnDS { provider: "openai", model: "gpt-4o", budget: AgentBudget }
causal agent ChurnCausal { provider: "openai", model: "o3-mini", budget: AgentBudget }
datatest agent ChurnTester { provider: "openai", model: "gpt-4o", budget: AgentBudget }
mlops agent ChurnMLOps { provider: "openai", model: "gpt-4o", budget: AgentBudget }

dio agent SimShopDIO {
    mode: "config",
    task: "Predict which customers will churn in 90 days, identify drivers, deploy with monitoring",
    infrastructure: SimShopInfra,
    agent_md: "./agents/simshop_dio.agent.md",
    provider: "openai",
    model: "gpt-4o",
    budget: DIOBudget
}

print(dio_solve(SimShopDIO, "full_system"));
```

### Relationship with Claude-Neam-Programming

| Skill | Scope | Use when |
|-------|-------|----------|
| `Claude-Neam-Programming` | Core language (types, OOP, RAG, guards, deployment) | Writing any `.neam` program |
| `Claude-Neam-DIO` | Data intelligence (14 agents, DIO, RACI, infrastructure) | Building data/ML systems with the DIO ecosystem |

Both skills are complementary. The DIO skill assumes knowledge of core Neam syntax covered by the Programming skill.

---

## Claude Code Skills

### `Claude-Neam-Programming-skill` (Recommended)

> **1,394 lines** — Complete Neam language reference for Claude Code

| Area | Coverage |
|------|----------|
| **Core Language** | 13 data types, variables, functions, control flow, comprehensions, pipe operator, destructuring |
| **OOP** | Structs, traits, impl blocks, sealed types, match expressions, generics |
| **Error Handling** | try/catch/throw, panic, Option (Some/None), Result (Ok/Err), context chaining |
| **Agents** | Stateless agents, 7 LLM providers, vision/multimodal, structured output |
| **Multi-Agent** | Runners, handoffs, spawn, dag_execute, 10 orchestration patterns |
| **NeamClaw** | Claw agents (sessions, channels, lanes, semantic memory), Forge agents (build-verify loops, checkpoints) |
| **RAG** | Knowledge bases, 8 retrieval strategies, vector stores, chunking |
| **Skills & Tools** | Skills, tools, extern skills (HTTP/MCP/Claude), MCP servers, adopt |
| **Security** | Guards (6 handler types), guard chains, policies, budgets |
| **Modules** | Module system, imports, visibility, package manager (neam-pkg) |
| **Deployment** | Docker, Kubernetes, Lambda, Cloud Run, ECS, Terraform |
| **Cloud Config** | neam.toml, state backends, LLM gateway, telemetry, secrets management |
| **Built-in Functions** | 100+ functions: math, string, list, map, file, HTTP, crypto, time, regex, async |
| **Testing** | Test blocks, 9 assertion types |

### `Claude-Neam-DIO-skill` (Data Intelligence)

> **Complete DIO reference** — 14 specialist agents + DIO orchestrator for data lifecycle management

| Area | Coverage |
|------|----------|
| **14 Agents** | Data, ETL, Migration, DataOps, Governance, Modeling, Analyst, DataScientist, Causal, MLOps, Data-BA, DataTest, Deployment, Forge |
| **DIO Orchestrator** | 3 operating modes (auto/config/hybrid), 8 auto-patterns, dynamic crew formation |
| **Architecture** | 4-layer architecture, Agent.MD domain knowledge, RACI accountability |
| **Coordination** | Centralized RACI, Swarm Stigmergy, Evolutionary GA |
| **Infrastructure** | 10 platform profiles (Snowflake, BigQuery, Databricks, Redshift, etc.) |
| **Quality** | Error handling (4-tier self-healing), security (6-layer defense-in-depth) |
| **Evaluation** | DataSims framework, ablation studies, 7-dimension scoring |

### `neam-programming` (Lightweight)

> **623 lines** — Quick-reference skill for basic Neam development

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
├── .claude-plugin/
│   └── plugin.json           ← Plugin manifest (for /plugin install)
├── .claude/
│   └── skills/               ← Project-level skills (auto-discovered)
│       ├── claude-neam-programming/
│       │   └── SKILL.md
│       └── claude-neam-dio/
│           └── SKILL.md
├── assets/
│   └── banner.svg
├── skills/                      ← Claude Code skills (plugin auto-discovered)
│   ├── claude-neam-programming/ ← Full Claude Code skill (recommended)
│   │   ├── SKILL.md
│   │   └── README.md
│   ├── claude-neam-dio/         ← DIO/Data Intelligence skill
│   │   ├── SKILL.md
│   │   └── README.md
│   └── neam-programming/        ← Lightweight Claude Code skill
│       └── SKILL.md
├── neam-skills-library/         ← Ready-made .neam skills (copy into agents)
│   ├── utility/
│   ├── web/
│   ├── data/
│   ├── file/
│   ├── math/
│   ├── security/
│   ├── development/
│   └── productivity/
└── README.md
```

---

## Upcoming Skills

| Skill | Status | Description |
|-------|--------|-------------|
| `Claude-Neam-Programming-skill` | Available | Full Neam language reference (1,394 lines) |
| `Claude-Neam-DIO-skill` | Available | DIO orchestrator, 14 specialist agents, data intelligence |
| `Claude-Neam-special_agents-skill` | Planned | Cognitive agents, voice agents, A2A protocol, advanced orchestration |

---

## Links

- [Neam Language](https://github.com/neam-lang/Neam) — compiler, runtime, REPL
- [Neam Documentation](https://neam-lang.github.io/Neam-The-AI-Native-Programming-Language/) — full language book (28 chapters)
- [The Intelligent Data Organization](https://neam-lang.github.io/The-Intelligent-Data-Organization-with-Neam/) — DIO eBook (30 chapters)
- [DataSims](https://github.com/neam-lang/Data-Sims) — evaluation framework for DIO programs
- [Smart Support Claw](https://github.com/neam-lang/smart_support_claw) — production claw agent example
- [NeamForge Site Generator](https://github.com/samsuljahith/neamforge-site-generator) — forge agent example

---

## License

MIT
