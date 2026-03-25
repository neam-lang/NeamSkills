# Claude-Neam-Programming-skill

> Comprehensive Neam language skill for Claude Code — write, debug, and understand Neam programs with full language coverage.

## Install

Import this skill into Claude Code:

```bash
/import skills/claude-neam-programming/SKILL.md
```

Or install from GitHub:

```bash
claude skill install neam-lang/NeamSkills/skills/claude-neam-programming
```

## What's Covered

This skill gives Claude complete knowledge of the Neam programming language:

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

## vs. `neam-programming` Skill

| Feature | `neam-programming` | `Claude-Neam-Programming-skill` |
|---------|--------------------|---------------------------------|
| Data types | 5 basic | All 13 (incl. Option, Result, TypedArray, Record, Table) |
| OOP | Not covered | Structs, traits, sealed types, match, impl |
| Error handling | Basic try/catch | Full: try/catch/throw, panic, Option, Result, context |
| Providers | 3 | 7 (+ custom endpoints, failover) |
| Multi-agent | Pipeline only | Runners, handoffs, spawn, dag_execute, 10 patterns |
| NeamClaw | Not covered | Full claw + forge agents |
| RAG strategies | 7 listed | 8 with detailed config |
| Guards | Basic | 6 handler types + policies |
| Deployment | CLI commands | Full cloud config (neam.toml, backends, telemetry) |
| Built-in functions | ~40 | 100+ with signatures |
| Real-world examples | 4 patterns | 8 production patterns |

## Upcoming Skills

- **Claude-Neam-special_agents-skill** — Deep coverage of specialized agent patterns, cognitive agents, voice agents, A2A protocol, and advanced orchestration

## Links

- [Neam Language](https://github.com/neam-lang/Neam)
- [Documentation](https://neam-lang.github.io/Neam-The-AI-Native-Programming-Language/)
- [Smart Support Claw (Example)](https://github.com/neam-lang/smart_support_claw)
- [NeamForge Site Generator (Example)](https://github.com/samsuljahith/neamforge-site-generator)
