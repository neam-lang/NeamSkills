# Claude Neam DIO Skill

Comprehensive skill for building **Intelligent Data Organizations** with Neam's DIO (Data Intelligent Orchestrator) framework.

## What It Covers

- **14 Specialist Agents** — Data, ETL, Migration, DataOps, Governance, Modeling, Analyst, DataScientist, Causal, MLOps, Data-BA, DataTest, Deployment, Forge
- **DIO Orchestrator** — 3 operating modes (auto/config/hybrid), 8 auto-patterns, dynamic crew formation
- **4-Layer Architecture** — Infrastructure, Platform Intelligence, Analytical Intelligence, Orchestration
- **Agent.MD** — Domain knowledge encoding for agents
- **RACI Accountability** — Runtime-enforced responsibility assignment
- **3 Coordination Modes** — Centralized RACI, Swarm Stigmergy, Evolutionary GA
- **Infrastructure Profiles** — 10 platform abstractions (Snowflake, BigQuery, Databricks, etc.)
- **Error Handling** — 4-tier self-healing escalation
- **Security** — 6-layer defense-in-depth model
- **DataSims** — Evaluation framework with ablation studies

## Prerequisites

This skill extends `claude-neam-programming`. Install that skill first for core Neam syntax.

## Installation

### As a Plugin Skill

Available as `neam-skills:claude-neam-dio` after installing the neam-skills plugin.

### As a Personal Skill

```bash
mkdir -p ~/.claude/skills/claude-neam-dio
curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-dio/SKILL.md \
  -o ~/.claude/skills/claude-neam-dio/SKILL.md
```

### As a Project-Level Skill

```bash
mkdir -p .claude/skills/claude-neam-dio
curl -sL https://raw.githubusercontent.com/neam-lang/NeamSkills/main/skills/claude-neam-dio/SKILL.md \
  -o .claude/skills/claude-neam-dio/SKILL.md
```

## Quick Example

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

## Based On

- [The Intelligent Data Organization with Neam](https://neam-lang.github.io/The-Intelligent-Data-Organization-with-Neam/) (30-chapter eBook)
- [Neam v0.9.9 Usage Guidelines](https://github.com/neam-lang/neam-nightly/blob/main/docs/v0.9_The_Intelligent_Data_Organization_Usage_Guideline_Readme.md)
- Verified against 22/22 passing E2E tests
