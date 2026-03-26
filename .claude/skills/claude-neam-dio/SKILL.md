---
name: Claude-Neam-DIO-skill
description: Comprehensive skill for building Intelligent Data Organizations with Neam. Covers the Data Intelligent Orchestrator (DIO), all 14 specialist agents, 4-layer architecture, Agent.MD domain knowledge, RACI accountability, 8 auto-patterns, 3 coordination modes, infrastructure profiles, error handling, security, and DataSims evaluation. Install this skill so Claude can write production-grade Neam DIO programs.
origin: neam-skills
version: "1.0.0"
---

# Claude Neam DIO Skill

The Intelligent Data Organization (IDO) is Neam's framework for autonomous data lifecycle management. It replaces manual handoffs between data teams with 14 specialist agents orchestrated by a master intelligence — the Data Intelligent Orchestrator (DIO). This skill teaches Claude to write complete DIO programs in Neam.

## When to Activate

- User builds data pipelines, warehouses, or ML systems using Neam's DIO agents
- User asks about the 14 specialist agents (Data, ETL, Migration, DataOps, Governance, Modeling, Analyst, DataScientist, Causal, MLOps, Data-BA, DataTest, Deployment, Forge)
- User configures the DIO orchestrator (auto/config/hybrid modes)
- User writes `agent_registry`, `infrastructure_profile`, `agent_md`, or `raci_matrix` declarations
- User builds multi-agent data programs with RACI accountability
- User works with DataSims evaluation, ablation studies, or coordination modes
- User asks about spec-driven development vs vibe coding vs agentic approaches for data projects

**Prerequisite:** This skill extends the `claude-neam-programming` skill. Activate that skill first for core Neam syntax (variables, types, functions, OOP, RAG, guards, deployment). This skill focuses exclusively on the DIO ecosystem.

---

## The Problem DIO Solves

85% of ML projects never reach production (Gartner). The root cause is not technology — it is **handoff failures** between teams. A typical data project has 5 handoff points (BA-to-DE, DE-to-DS, DS-to-MLOps, each-to-QA, QA-to-Deploy), each with ~18% coordination overhead.

DIO eliminates these handoffs by replacing the traditional org chart with 14 agents + 1 orchestrator operating under spec-driven development: requirements are machine-readable, handoffs are contracts, and quality gates are enforced automatically.

---

## Four-Layer Architecture

```
Layer 4: Orchestration         DIO (master intelligence)
Layer 3: Analytical Intelligence   Data-BA, DataScientist, Causal, DataTest, MLOps
Layer 2: Platform Intelligence     DataOps, Governance, Modeling, Analyst
Layer 1: Data Infrastructure       Data Agent, ETL Agent, Migration Agent
Cross-cutting: Infrastructure Profiles, Agent.MD, RACI, Security
```

Communication between layers uses:
- **Pub/Sub messaging** — async, FIFO, guaranteed delivery
- **Artifact registry** — versioned handoffs between agents
- **Event bus** — real-time status propagation

---

## Quick Start

### Your First DIO Orchestration (Verified)

```neam
budget B { cost: 500.00, tokens: 2000000 }
infrastructure_profile Infra { data_warehouse: { platform: "postgres" } }

dio agent MyDIO {
    mode: "config",
    task: "Predict customer churn",
    infrastructure: Infra,
    provider: "openai",
    model: "gpt-4o",
    budget: B
}

print(dio_solve(MyDIO, "full_system"));
```

### Compile and Run

```bash
neamc program.neam -o program.neamb
neam program.neamb
```

---

## The 14 Specialist Agents

Each agent has a unique keyword, role, required fields, and native functions.

### Agent Quick Reference

| # | Keyword | Role | Version | Layer |
|---|---------|------|---------|-------|
| 1 | `data agent` | Data Engineer — Ingestion | v0.9.0 | Infrastructure |
| 2 | `etl agent` | Data Engineer — Transformation | v0.9.1 | Infrastructure |
| 3 | `migration agent` | Migration Specialist | v0.9.2 | Infrastructure |
| 4 | `dataops agent` | Platform Operator (SRE for Data) | v0.9.3 | Platform |
| 5 | `governance agent` | Compliance Officer | v0.9.4 | Platform |
| 6 | `modeling agent` | Data Architect | v0.9.5 | Platform |
| 7 | `analyst agent` | Data Analyst | v0.9.6 | Platform |
| 8 | `datascientist agent` | Data Scientist | v0.9.8 | Analytical |
| 9 | `causal agent` | Causal Investigator | v0.9.8.1 | Analytical |
| 10 | `mlops agent` | Production Guardian | v0.9.8.2 | Analytical |
| 11 | `databa agent` | Requirements Analyst | v0.9.8.3 | Analytical |
| 12 | `datatest agent` | Quality Critic | v0.9.8.4 | Analytical |
| 13 | `deploy_config` | Release Manager | v0.9.7 | Infrastructure |
| 14 | `dio agent` | Master Orchestrator | v0.9.9 | Orchestration |

### Agent Trait System

Agents have composable traits that determine their capabilities:

| Trait | Agents |
|-------|--------|
| `DataProducer` | Data Agent, ETL Agent, DataScientist |
| `DataConsumer` | ETL Agent, DataScientist, Analyst, Causal |
| `CausalReasoner` | Causal Agent (general-purpose — any agent can invoke) |
| `QualityGatekeeper` | DataTest Agent |

---

## Layer 1: Data Infrastructure Agents

### Data Agent (v0.9.0)

**Purpose:** Foundation data lifecycle — sources, sinks, schemas, quality gates, compute routing, governance, lineage, catalog.

**Keyword:** `data agent`

#### Declarations

```neam
// Schema with typed fields and annotations
schema CustomerSchema {
    id: string @primary_key,
    name: string @not_null,
    email: string @pii,
    revenue: float @range(0, 1000000),
    created: datetime
}

// Source connector
source CRM {
    type: "postgres",
    connection: "pg://localhost/crm",
    schema: CustomerSchema
}

// Sink connector
sink Warehouse {
    type: "snowflake",
    connection: "sf://account",
    write_mode: "upsert"
}

// Quality gate
quality DataQuality {
    completeness: 0.95,
    freshness: "1h",
    on_violation: "gate"
}

// Compute routing
compute Local { engine: "local" }

// Governance
governance AccessControl {
    access: {
        model: "rbac",
        roles: { "analyst": { read: ["public", "internal"], write: ["public"] } }
    }
}

// Catalog registration
catalog DataCatalog { engine: "datahub", discovery: true }
```

#### Minimal (Verified)

```neam
source DB { type: "postgres", connection: "pg://localhost" }
sink Out { type: "s3", connection: "s3://bucket/" }

data agent Minimal {
    provider: "openai",
    model: "gpt-4o-mini",
    sources: [DB],
    sinks: [Out]
}
```

#### Full Declaration (Verified)

```neam
schema S { id: string @primary_key, val: float }
source DB { type: "postgres", connection: "pg://localhost", schema: S }
sink Out { type: "snowflake", connection: "sf://account", write_mode: "upsert" }
quality Q { completeness: 0.95, on_violation: "gate" }
compute C { engine: "local" }
governance G {
    access: {
        model: "rbac",
        roles: { "analyst": { read: ["public", "internal"], write: ["public"] } }
    }
}
catalog Cat { engine: "datahub", discovery: true }

data agent Full {
    provider: "openai",
    model: "gpt-4o",
    system: "Full featured agent.",
    temperature: 0.3,
    sources: [DB],
    sinks: [Out],
    schema: S,
    quality: Q,
    compute: { default: C },
    governance: G,
    catalog: Cat,
    lineage: true,
    role: "analyst",
    purpose: "Data pipeline",
    autonomy: "conditional"
}
```

#### Syntax Rules

| Declaration | Required Fields | Notes |
|-------------|----------------|-------|
| `schema` | field_name: type | Types: `string`, `int`, `float`, `bool`, `datetime`. Annotations: `@primary_key`, `@not_null`, `@range(min,max)`, `@pii` |
| `source` | `type`, `connection` | Types: `"postgres"`, `"s3"`, `"http"`, `"kafka"`. Optional: `schema`, `refresh` |
| `sink` | `type`, `connection` | Optional: `write_mode` (`"append"`, `"upsert"`, `"overwrite"`) |
| `quality` | at least one metric | `completeness: 0.95`, `freshness: "1h"`, `on_violation: "gate"` (flat values, NOT nested objects) |
| `compute` | `engine` | Engines: `"local"`, `"spark"`, `"snowflake"`, `"bigquery"`. Optional: `config: { ... }` |
| `governance` | `access` block | Contains `model` + `roles` |
| `catalog` | `engine` | Engines: `"datahub"`, `"unity"`. Optional: `discovery: true` |
| `data agent` | `provider`, `model`, `sources`, `sinks` | `schema` is **singular** (not `schemas`). `compute` uses `{ default: Ref }` syntax |

---

### ETL Agent (v0.9.1)

**Purpose:** SQL-first warehouse/lakehouse agent with dimensional modeling, semantic layer, NL2SQL, self-healing.

**Keyword:** `etl agent`

#### Minimal (Verified)

```neam
compute WH { engine: "snowflake" }
source S { type: "postgres", connection: "host=localhost" }

etl agent E {
    provider: "openai",
    model: "gpt-4o",
    sources: [S],
    warehouse: WH
}
```

#### With Mart and Semantic Layer (Verified)

```neam
compute WH { engine: "snowflake" }
source Raw { type: "postgres", connection: "pg://localhost" }

mart SalesMart {
    grain: "one row per order",
    model_type: "star",
    facts: { fact_orders: { measures: ["quantity", "revenue"] } },
    dimensions: { dim_customer: { scd_type: 2, business_key: "customer_id" } }
}

semantic SalesGlossary {
    entities: { customer: { table: "dim_customer", key: "customer_sk" } },
    metrics: { revenue: { formula: "SUM(revenue)", format: "currency" } }
}

etl agent WarehouseBuilder {
    provider: "openai",
    model: "gpt-4o",
    sources: [Raw],
    warehouse: WH,
    marts: [SalesMart],
    semantic: SalesGlossary
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `sources` | Yes | Array of source references |
| `warehouse` | Yes | Reference to a `compute` declaration (the target warehouse) |
| `marts` | No | Array of `mart` references |
| `semantic` | No | Reference to a `semantic` declaration |

---

### Migration Agent (v0.9.2)

**Purpose:** Cross-platform data migration with wave planning, schema translation, reconciliation, zero-downtime cutover.

**Keyword:** `migration agent`

#### Minimal (Verified)

```neam
budget MigBudget { cost: 10.0 }

migration agent TestMig {
    provider: "openai",
    model: "gpt-4o-mini",
    source: OracleDB,
    target: SnowflakeDB,
    staging: S3Staging,
    budget: MigBudget,
    strategy: "re_platform"
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `source` | Yes | Reference to source system |
| `target` | Yes | Reference to target system |
| `staging` | Yes | Reference to staging area |
| `strategy` | Yes | `"re_platform"`, `"re_architect"`, `"lift_and_shift"` |
| `budget` | Yes | Budget reference |

#### Native Functions

| Function | Purpose |
|----------|---------|
| `migration_status(agent)` | Status: `"assess"`, `"planning"`, `"executing"`, `"complete"` |
| `migration_plan(agent)` | Generate migration plan |
| `migration_execute_wave(agent)` | Execute migration wave |
| `migration_reconcile(agent)` | 3-level reconciliation (row count, hash, semantic) |

---

## Layer 2: Platform Intelligence Agents

### DataOps Agent (v0.9.3)

**Purpose:** SRE for data — 6-stage pipeline (collect, detect, correlate, triage, auto-heal, report). FinOps integration.

**Keyword:** `dataops agent`

#### Verified Declaration

```neam
budget OpsBudget { cost: 30.00, tokens: 500000 }

platform SnowflakePlatform {
    type: "snowflake",
    connection: "snowflake://account.snowflakecomputing.com"
}

scheduler AirflowProd {
    type: "airflow",
    connection: "http://localhost:8080"
}

audit_table ETLAudit {
    connection: "pg://localhost/ops",
    table: "pipeline_runs",
    timestamp_column: "ts",
    status_column: "status"
}

incident_policy OpsPolicy {
    severity: {
        P1_CRITICAL: {
            conditions: ["SLA breach", "data loss"],
            response: "immediate"
        }
    }
}

dataops agent PipelineMonitor {
    provider: "openai",
    model: "gpt-4o",
    budget: OpsBudget,
    platforms: [SnowflakePlatform],
    schedulers: [AirflowProd],
    audit_tables: [ETLAudit],
    incident_policy: OpsPolicy
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `platforms` | **Yes** (OPS-001) | Array of `platform` references |
| `incident_policy` | **Yes** (OPS-002) | Reference to `incident_policy`. Uses `severity` with named levels |
| `schedulers` | No | Array of `scheduler` references |
| `audit_tables` | No | Array of `audit_table` references |

#### Native Functions

| Function | Purpose |
|----------|---------|
| `dataops_status(agent)` | Status: `"idle"`, `"monitoring"`, `"triaging"` |
| `dataops_detect_anomaly(agent)` | Anomaly detection |
| `dataops_auto_heal(agent)` | Self-healing pipeline repair |
| `dataops_sla_check(agent)` | SLA compliance check |

---

### Governance Agent (v0.9.4)

**Purpose:** Data classification, RBAC/ABAC, lineage, compliance (GDPR/CCPA/DORA). Column-level lineage.

**Keyword:** `governance agent`

#### Verified Declaration

```neam
budget GovBudget { cost: 20.00, tokens: 300000 }

classification_policy DataClassification {
    levels: {
        RESTRICTED: { level: 4, controls: ["encryption_at_rest", "column_masking"] },
        CONFIDENTIAL: { level: 3, controls: ["masking"] },
        INTERNAL: { level: 2, controls: ["audit"] },
        PUBLIC: { level: 1, controls: ["none"] }
    }
}

governance agent DataGovernor {
    provider: "openai",
    model: "gpt-4o",
    system: "You are a data governance agent.",
    temperature: 0.3,
    budget: GovBudget,
    classification: DataClassification
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `classification` | Yes | Reference to `classification_policy` (uses `levels`, NOT `patterns`) |
| `budget` | Yes | Budget reference |

#### Native Functions

| Function | Purpose |
|----------|---------|
| `governance_status(agent)` | Status: `"idle"`, `"scanning"` |
| `gov_classify(agent)` | Auto-classify data columns |
| `gov_check_access(agent)` | RBAC/ABAC access check |
| `gov_trace_lineage(agent)` | Column-level lineage tracing |

---

### Modeling Agent (v0.9.5)

**Purpose:** Schema reverse-engineering, ER modeling, normalization analysis, dimensional design, impact analysis.

**Keyword:** `modeling agent`

#### Verified Declaration

```neam
schema_source TestSrc {
    type: "snowflake",
    connection: "sf://account"
}

modeling agent SchemaArchitect {
    provider: "openai",
    model: "gpt-4o-mini",
    sources: [TestSrc]
}
```

#### Related Declarations

```neam
entity Product {
    domain: "Sales",
    attributes: {
        product_id: { type: "identifier", primary_key: true }
    }
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `sources` | Yes | Array of `schema_source` references |

#### Native Functions

| Function | Purpose |
|----------|---------|
| `modeling_status(agent)` | Status: `"initialized"` |
| `model_reverse_engineer(agent)` | Reverse-engineer existing schemas |
| `model_generate_er(agent)` | Generate ER diagrams |
| `model_normalize(agent)` | Normalization analysis |

---

### Analyst Agent (v0.9.6)

**Purpose:** NL-to-SQL across 9 dialects, domain transfer learning, governed execution, multi-format output, insight discovery.

**Keyword:** `analyst agent`

#### Verified Declaration

```neam
budget B { cost: 10.00, tokens: 100000 }

sql_connection DB {
    platform: "postgres",
    connection: "pg://localhost/test",
    database: "test"
}

analyst agent RevenueAnalyst {
    provider: "openai",
    model: "gpt-4o-mini",
    connections: [DB],
    budget: B
}
```

#### With Domain Context

```neam
domain_context SalesDomain {
    glossary: {
        revenue: "SUM(total)",
        churn: "No orders in 90 days"
    },
    default_schema: "dw"
}

analyst agent ContextAnalyst {
    provider: "openai",
    model: "gpt-4o-mini",
    connections: [DB],
    domain_context: SalesDomain,
    budget: B
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `connections` | Yes | Array of `sql_connection` references |
| `domain_context` | No | Reference to `domain_context` |
| `budget` | Yes | Budget reference |

#### Native Functions

| Function | Purpose |
|----------|---------|
| `analyst_status(agent)` | Status: `"initialized"` |
| `analyst_query(agent)` | Natural language query |
| `analyst_sql(agent)` | Direct SQL execution |
| `analyst_insight(agent)` | Automated insight discovery |
| `analyst_format(agent)` | Multi-format output |

---

## Layer 3: Analytical Intelligence Agents

### Data-BA Agent (v0.9.8.3)

**Purpose:** Requirements intelligence — always the FIRST agent invoked ("Day -1"). LLM-assisted elicitation, BRD generation, Given/When/Then acceptance criteria, traceability matrices.

**Keyword:** `databa agent`

#### Minimal (Verified)

```neam
budget B { cost: 50.00, tokens: 500000 }

databa agent MinBA {
    provider: "openai",
    model: "gpt-4o",
    temperature: 0.3,
    budget: B
}
```

#### With BRD Generator and Functional Spec

```neam
budget B { cost: 50.00, tokens: 500000 }

brd_generator ChurnBRD {
    project: { name: "Churn Prediction", id: "P-001" },
    objectives: { primary: "Reduce churn to 8%", kpis: ["churn_rate"] },
    scope: { in_scope: ["Enterprise"], out_of_scope: ["SMB"] }
}

functional_spec ChurnSpec {
    acceptance_criteria: [
        { id: "AC-001", given: "inactive customer", when: "scored", then: "prob > 0.7" }
    ]
}

databa agent ChurnBA {
    provider: "openai",
    model: "gpt-4o",
    budget: B,
    brd_generator: ChurnBRD,
    functional_spec: ChurnSpec
}
```

#### Native Functions

| Function | Purpose |
|----------|---------|
| `ba_status(agent)` | Get agent status |
| `ba_generate_brd(agent)` | Generate Business Requirements Document |
| `ba_generate_criteria(agent)` | Generate Given/When/Then acceptance criteria |
| `ba_traceability_matrix(agent)` | Build source-to-target traceability |

---

### DataScientist Agent (v0.9.8)

**Purpose:** Problem framing, EDA, volume-aware compute routing, feature engineering, ML/AutoML, SHAP explainability.

**Keyword:** `datascientist agent`

#### Minimal (Verified)

```neam
budget B { cost: 50.00, tokens: 500000 }

datascientist agent MinDS {
    provider: "openai",
    model: "gpt-4o",
    temperature: 0.2,
    budget: B
}
```

#### With EDA and Volume Router (Verified)

```neam
budget B { cost: 50.00, tokens: 500000 }

eda_config EDA {
    univariate: { numeric: { statistics: ["mean", "std"] } }
}

volume_router VR {
    routing_rules: { local_memory: { condition: "n_rows < 100000" } }
}

datascientist agent DS {
    provider: "openai",
    model: "gpt-4o",
    eda_config: EDA,
    volume_router: VR,
    budget: B
}
```

#### Syntax Rules

| Field | Required | Notes |
|-------|----------|-------|
| `budget` | Yes | Budget reference |
| `provider`, `model` | Yes | LLM provider |
| `temperature` | No | Float 0.0-1.0 |
| `eda_config` | No | Reference to `eda_config` |
| `volume_router` | No | Reference to `volume_router` |
| `experiment` | No | Reference to `ml_experiment` (note: nested `metrics` may cause runtime issues — use minimal form) |

#### Native Functions

| Function | Purpose |
|----------|---------|
| `ds_status(agent)` | Get agent status |
| `ds_run_eda(agent, dataset)` | Exploratory data analysis |
| `ds_train(agent, dataset)` | Train ML model |
| `ds_predict(agent, model, data)` | Generate predictions |
| `ds_explain_global(agent, model)` | SHAP global explanations |
| `ds_exec_python(agent, code)` | Execute Python in sandboxed venv |

---

### Causal Agent (v0.9.8.1)

**Purpose:** General-purpose causal reasoning. Pearl's Ladder of Causation (Rungs 2-3), SCM declarations, do-calculus interventions, counterfactual analysis. Composable primitive — any agent can invoke it.

**Keyword:** `causal agent`

#### Minimal (Verified)

```neam
budget B { cost: 50.00, tokens: 500000 }

causal agent MinCausal {
    provider: "openai",
    model: "o3-mini",
    temperature: 0.3,
    budget: B
}
```

#### With SCM, Intervention, Counterfactual (Verified)

```neam
budget B { cost: 50.00, tokens: 500000 }

scm ChurnSCM {
    variables: ["support", "csat", "churn"],
    equations: { csat: "f(support) + U", churn: "f(csat) + U" }
}

intervention SupportIntv {
    treatment: "support",
    outcome: "churn",
    do_value: "improve_1std",
    identification: "backdoor"
}

counterfactual RetentionCF {
    question: "Would C-1234 stay with premium support?",
    factual: { tier: "basic", outcome: "churned" },
    counterfactual: { tier: "premium" }
}

causal agent WhyChurn {
    provider: "openai",
    model: "o3-mini",
    budget: B,
    scm: ChurnSCM,
    intervention: SupportIntv,
    counterfactual: RetentionCF
}
```

#### Native Functions

| Function | Purpose |
|----------|---------|
| `causal_status(agent)` | Get agent status |
| `causal_discover_dag(agent, data)` | Discover causal DAG from observational data |
| `causal_estimate_ate(agent, intervention)` | Average Treatment Effect via do-calculus |
| `causal_counterfactual(agent, cf)` | Counterfactual analysis |
| `causal_rca(agent, problem)` | Root cause analysis |

---

### DataTest Agent (v0.9.8.4)

**Purpose:** Independent quality critic. Generates tests from acceptance criteria, enforces BLOCKING quality gates. Removing DataTest drops system quality score by 26.1% (ablation A5).

**Keyword:** `datatest agent`

#### Minimal (Verified)

```neam
budget B { cost: 50.00, tokens: 500000 }

datatest agent MinTest {
    provider: "openai",
    model: "gpt-4o",
    temperature: 0.2,
    budget: B
}
```

#### With Quality Gate and Test Suite

```neam
budget B { cost: 50.00, tokens: 500000 }

quality_gate ChurnGate {
    data_quality_gate: { required_pass_rate: 1.0, blocking: true },
    model_quality_gate: { required_metrics: { auc_roc: "> 0.80" }, blocking: true }
}

etl_test_suite PipelineTests {
    row_count_check: { tolerance: 0.01 },
    null_check: { columns: ["customer_id"], threshold: 0.0 }
}

datatest agent QualityCritic {
    provider: "openai",
    model: "gpt-4o",
    budget: B,
    quality_gate: ChurnGate,
    etl_test_suite: PipelineTests
}
```

#### Native Functions

| Function | Purpose |
|----------|---------|
| `test_status(agent)` | Get agent status |
| `test_generate_cases(agent, criteria)` | Auto-generate tests from acceptance criteria |
| `test_run_suite(agent, suite)` | Run test suite |
| `test_evaluate_gate(agent, gate)` | Evaluate blocking/advisory quality gate |

---

### MLOps Agent (v0.9.8.2)

**Purpose:** Day-2 ML operations — 6 drift types, continuous training pipelines, deployment strategies (canary/shadow/blue-green/A/B), champion-challenger, business KPI tracking.

**Keyword:** `mlops agent`

#### Minimal (Verified)

```neam
budget B { cost: 100.00, tokens: 500000 }

mlops agent MinMLOps {
    provider: "openai",
    model: "gpt-4o",
    temperature: 0.2,
    budget: B
}
```

#### With Drift Monitor and Deployment Strategy

```neam
budget B { cost: 100.00, tokens: 500000 }

drift_monitor ChurnDrift {
    data_drift: { method: "evidently", check_frequency: "hourly", threshold: 0.05 },
    concept_drift: { method: "performance_monitoring", metrics: ["auc_roc"], degradation_threshold: 0.05 }
}

deployment_strategy CanaryDeploy {
    strategy: "canary",
    canary_percentage: 10,
    promotion_criteria: { min_duration: "2h", max_error_rate: 0.01 },
    rollback: { auto: true, trigger: "error_rate > 0.05" }
}

champion_challenger ModelComp {
    champion: "v2", challenger: "v3",
    comparison_metrics: ["auc_roc"],
    traffic_split: { champion: 90, challenger: 10 }
}

serving_infra ServingConfig {
    type: "kubernetes", replicas: 3,
    resources: { cpu: "2", memory: "4Gi" }
}

mlops agent ProductionGuardian {
    provider: "openai",
    model: "gpt-4o",
    budget: B,
    drift_monitor: ChurnDrift,
    deployment_strategy: CanaryDeploy,
    champion_challenger: ModelComp,
    serving_infra: ServingConfig
}
```

#### Native Functions

| Function | Purpose |
|----------|---------|
| `mlops_status(agent)` | Get deployment status |
| `mlops_check_drift(agent)` | Check for 6 drift types |
| `mlops_deploy_canary(agent, model)` | Canary deployment |
| `mlops_rollback(agent)` | Rollback to champion |
| `mlops_trigger_retrain(agent)` | Trigger continuous training |

---

## The Data Intelligent Orchestrator (DIO) — v0.9.9

### Core Concept

DIO is the master intelligence that:
1. **Understands** the task (NL decomposition into sub-tasks)
2. **Forms crews** dynamically (weighted scoring: capability 40%, cost 20%, infrastructure 20%, history 20%)
3. **Selects patterns** from 8 auto-patterns
4. **Assigns RACI** accountability to every sub-task
5. **Delegates** to specialist agents via async pub/sub
6. **Monitors** execution with activity capture
7. **Self-heals** on failure (4-tier escalation)
8. **Synthesizes** results into a unified response

### Three Operating Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| `"auto"` | Fully autonomous, no infrastructure config needed | Quick experiments, prototyping |
| `"config"` | Operates on your real infrastructure (Snowflake, BigQuery, etc.) | Production systems |
| `"hybrid"` | Auto with configurable guardrails and human checkpoints | Regulated environments |

### DIO — Auto Mode (Verified)

```neam
budget B { cost: 500.00, tokens: 2000000 }

dio agent AutoDIO {
    mode: "auto",
    task: "Predict customer churn",
    provider: "openai",
    model: "gpt-4o",
    budget: B
}
```

### DIO — Config Mode with Infrastructure (Verified)

```neam
budget B { cost: 500.00, tokens: 2000000 }

infrastructure_profile Infra {
    data_warehouse: { platform: "postgres" }
}

dio agent ConfigDIO {
    mode: "config",
    task: "Predict churn, identify drivers, deploy with monitoring",
    infrastructure: Infra,
    provider: "openai",
    model: "gpt-4o",
    budget: B
}
```

### DIO — Hybrid Mode with Guardrails (Verified)

```neam
budget B { cost: 500.00, tokens: 2000000 }

infrastructure_profile Infra {
    data_warehouse: { platform: "postgres", connection: "pg://localhost" }
}

dio agent HybridDIO {
    mode: "hybrid",
    task: "Reduce churn",
    infrastructure: Infra,
    guardrails: { require_approval: ["deployment"], max_budget_per_phase: 100.00 },
    provider: "openai",
    model: "gpt-4o",
    budget: B
}
```

### DIO Fields

| Field | Required | Values |
|-------|----------|--------|
| `mode` | Yes | `"auto"`, `"config"`, `"hybrid"` |
| `task` | Yes | Natural language task description |
| `infrastructure` | No (warning DIO-010 if missing) | Reference to `infrastructure_profile` |
| `agent_md` | No | Path to Agent.MD file |
| `guardrails` | No (hybrid mode) | `require_approval`, `max_budget_per_phase` |
| `agent_registry` | No (warning DIO-003) | Reference to `agent_registry` |
| `raci_matrix` | No (warning DIO-005) | Reference to `raci_matrix` |
| `provider`, `model`, `budget` | Yes | LLM and budget configuration |

### DIO Native Functions

| Function | Purpose |
|----------|---------|
| `dio_status(agent)` | Get orchestrator status |
| `dio_solve(agent, task)` | Full orchestration: understand, crew, delegate, execute, synthesize |
| `dio_plan(agent, task)` | Generate execution plan without executing |

### 8 Auto-Patterns

DIO automatically selects the right orchestration pattern based on the task:

| Pattern | Trigger | Agents Involved |
|---------|---------|-----------------|
| **Predictive** | "predict", "classify", "forecast" | Data-BA, Data, ETL, DataScientist, DataTest, MLOps |
| **Root Cause** | "why", "root cause", "investigate" | Causal, Analyst, DataScientist |
| **Platform Build** | "build warehouse", "create pipeline" | Data, ETL, Modeling, DataOps |
| **Ops** | "monitor", "alert", "heal" | DataOps, MLOps |
| **Compliance** | "audit", "gdpr", "classify" | Governance, DataTest |
| **Exploratory** | "explore", "analyze", "discover" | Analyst, DataScientist, Causal |
| **Migration** | "migrate", "move", "re-platform" | Migration, DataOps, DataTest |
| **Full Lifecycle** | "full system", "end to end" | All 14 agents |

---

## Full Production Program (Verified)

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

---

## Agent.MD: Encoding Human Expertise

Agent.MD is a markdown file that encodes domain knowledge so agents execute with human-level context. Removing Agent.MD drops model AUC by 7.7% (ablation A6).

### Structure

```markdown
# SimShop -- Agent.MD

## @organization-context
- SimShop: B2C e-commerce, 100K customers
- Data period: Jan 2024 - Dec 2025
- Primary database: PostgreSQL (simshop_oltp)
- Warehouse: simshop_dw schema

## @causal-domain-knowledge
- Support quality -> satisfaction -> retention (established)
- Price sensitivity varies by segment (enterprise vs SMB)
- Seasonal effects: Q4 holiday purchases mask churn signals

## @methodology-preferences
- XGBoost for initial models (interpretable, fast)
- Bayesian approaches when sample sizes allow
- Always start with simple baselines before complex models

## @known-data-issues
- CRM pre-2024 data has missing industry field
- Support sentiment accuracy ~75%
- Duplicate customer records in legacy system (~2%)

## @delegation-rules
- Validate requirements first (Data-BA always runs first)
- Quality gates block deployment (no exceptions)
- Max 3 retries before escalation to human
- Causal analysis required before prescriptive recommendations

## @pipeline-catalog
- etl_daily_orders: runs 02:00 UTC, SLA 45min
- ml_churn_scoring: runs 06:00 UTC, depends on etl_daily_orders
```

### Referencing Agent.MD

```neam
dio agent DIO {
    agent_md: "./agents/simshop_dio.agent.md",
    mode: "config",
    task: "Predict churn",
    ...
}
```

---

## Agent Registry

The agent registry defines the identity card of every agent — role, capabilities, inputs, outputs, and invocation rules.

```neam
agent_registry DIOBlueprint {
    agents: {
        databa: {
            role: "Requirements Analyst",
            personality: "Meticulous, question-asking, detail-oriented",
            responsibility: "Translate business needs into structured specifications",
            version: "v0.9.8.3",
            capabilities: ["elicitation", "brd", "functional_spec", "acceptance_criteria",
                          "data_mapping", "impact_analysis", "traceability"],
            when_to_call: "At project start, on change requests, on production feedback",
            inputs: ["business_problem", "stakeholder_context", "domain_knowledge"],
            outputs: ["brd", "functional_spec", "acceptance_criteria"],
            autonomy: "needs_stakeholder_approval_for_specs"
        },
        datascientist: {
            role: "Data Scientist",
            personality: "Curious, experimental, model-obsessed",
            responsibility: "Build, train, and evaluate ML/AI models",
            version: "v0.9.8",
            capabilities: ["eda", "feature_engineering", "ml_training", "automl",
                          "hypothesis_testing", "model_evaluation", "shap"],
            when_to_call: "When predictions, classifications, or patterns are needed",
            inputs: ["ml_requirement_spec", "feature_tables", "problem_statement"],
            outputs: ["trained_models", "predictions", "shap_reports", "model_cards"],
            depends_on: ["etl"]
        },
        causal: {
            role: "Causal Investigator",
            personality: "Skeptical, rigorous, correlation-is-not-causation",
            responsibility: "Discover WHY things happen and WHAT-IF scenarios",
            version: "v0.9.8.1",
            capabilities: ["causal_discovery", "scm", "do_calculus", "counterfactual"],
            when_to_call: "When 'why' or 'what if' questions need answering",
            inputs: ["business_questions", "observational_data", "domain_knowledge"],
            outputs: ["causal_dags", "effect_estimates", "counterfactual_results"],
            general_purpose: true
        },
        datatest: {
            role: "Quality Critic",
            personality: "Skeptical, thorough, never satisfied",
            responsibility: "Validate everything before production",
            version: "v0.9.8.4",
            capabilities: ["test_generation", "etl_testing", "dw_testing", "ml_testing",
                          "quality_gates"],
            when_to_call: "Before any deployment, after any build",
            inputs: ["acceptance_criteria", "models", "pipelines"],
            outputs: ["test_reports", "quality_gate_results"],
            depends_on: ["databa"]
        }
    }
}
```

---

## RACI Accountability

Every sub-task has exactly one **R**esponsible agent. DIO is always **A**ccountable. This is enforced at runtime.

### Two Invariants

1. **Exactly one R per task** — if zero or multiple agents are Responsible, DIO raises an error
2. **DIO is always A** — the orchestrator is accountable for every task outcome

### RACI in Practice

DIO automatically assigns RACI based on the agent registry. Removing RACI enforcement drops traceability by 80% (ablation A8).

```
Task: "Train churn model"
  R: DataScientist (builds the model)
  A: DIO (accountable for outcome)
  C: Causal Agent (consulted for feature selection)
  I: MLOps Agent (informed for deployment planning)
```

---

## Three Coordination Modes

### 1. Centralized RACI (Default)

DIO manages all agent interactions through RACI assignment. Deterministic, auditable, recommended for production.

### 2. Swarm Stigmergy

Agents coordinate through shared artifacts without central control. Task completion signals trigger downstream agents automatically.

- Converges in ~23 iterations
- ~2% deadlock rate (DIO intervenes)
- Best for exploratory, research-heavy tasks

### 3. Evolutionary (Genetic Algorithm)

DIO generates multiple crew/pattern configurations and evolves them through selection, crossover, and mutation.

- Fitness convergence: 0.91 by generation 67
- Best for optimization-heavy tasks with unclear best approach

```neam
// Select coordination mode via dio_solve
print(dio_solve(MyDIO, "full_system"));        // Default: centralized RACI
print(dio_solve(MyDIO, "swarm_mode"));          // Swarm stigmergy
print(dio_solve(MyDIO, "evolutionary_mode"));   // Evolutionary GA
```

---

## Infrastructure Profiles

Infrastructure profiles abstract platform differences so the same DIO program runs on any data platform.

```neam
// PostgreSQL (development)
infrastructure_profile Dev { data_warehouse: { platform: "postgres" } }

// Snowflake (production)
infrastructure_profile Prod {
    data_warehouse: { platform: "snowflake", connection: env("SF_URL"), warehouse: "COMPUTE_WH" },
    data_science: { mlflow: { uri: env("MLFLOW_URI") } }
}

// BigQuery (analytics)
infrastructure_profile Analytics {
    data_warehouse: { platform: "bigquery", project: env("GCP_PROJECT"), dataset: "analytics" }
}

// Databricks (lakehouse)
infrastructure_profile Lakehouse {
    data_warehouse: { platform: "databricks", connection: env("DBX_URL") }
}

// Same DIO, different platform — change only the profile reference:
dio agent DIO { mode: "config", task: "Predict churn", infrastructure: Prod, ... }
```

### Supported Platforms

`"postgres"`, `"snowflake"`, `"bigquery"`, `"databricks"`, `"redshift"`, `"oracle"`, `"teradata"`, `"fabric"`, `"bigtable"`, `"spark"`

### Full Infrastructure Profile

```neam
infrastructure_profile Enterprise {
    data_warehouse: {
        platform: "snowflake",
        connection: env("SF_URL"),
        warehouse: "COMPUTE_WH",
        schemas: ["raw", "staging", "dw", "ml_features"]
    },
    data_science: {
        mlflow: { uri: env("MLFLOW_URI") }
    },
    governance: {
        regulations: ["GDPR", "CCPA"],
        pii_columns: ["email", "phone", "ssn"]
    }
}
```

---

## Error Handling and Self-Healing

### 4-Tier Escalation Hierarchy

| Tier | Strategy | Example |
|------|----------|---------|
| 1 | **Agent Retry** | Agent retries failed operation (max 3) |
| 2 | **DIO Adapt** | DIO reassigns task to different agent or pattern |
| 3 | **Human Review** | Escalate to human with full context |
| 4 | **Halt** | Stop execution, preserve state for forensics |

### State Checkpointing

DIO checkpoints state at each phase boundary. On failure, it can resume from the last successful checkpoint without re-executing completed work.

### Schema Break Recovery

When an upstream schema change breaks a pipeline:
1. DataOps detects the anomaly
2. Governance traces the lineage impact
3. ETL Agent proposes schema adaptation
4. DataTest validates the fix
5. DIO coordinates the recovery without human intervention

---

## Security: Defense in Depth

### 6-Layer Security Model

| Layer | Component | Purpose |
|-------|-----------|---------|
| 1 | Authentication | Agent identity verification |
| 2 | Authorization | RBAC/ABAC access control |
| 3 | Classification | Automatic data sensitivity tagging |
| 4 | Guards | Input/output content filtering |
| 5 | Audit | Tamper-proof activity logging |
| 6 | Budget | Cost and token limits |

### Phase-Gated Permissions

Agents only receive the permissions they need for their current phase. A DataScientist agent cannot access raw PII — it only sees the feature tables produced by the ETL Agent after Governance has applied masking.

### Zero-Trust Inter-Agent Communication

```
Agent A -> [Auth] -> [Authz] -> [Classify] -> [Guard] -> Agent B
                                                  |
                                              [Audit Log]
```

---

## DataSims Evaluation

DataSims is the evaluation framework for validating DIO programs. It uses SimShop — a synthetic e-commerce environment with 164 tables, 12 schemas, and 10 deliberately injected quality issues.

### Running DataSims

```bash
git clone https://github.com/neam-lang/Data-Sims.git
cd Data-Sims
python3 evaluation/run_experiments.py --reps 5
python3 evaluation/analysis.py
```

### 7-Dimension Evaluation Framework

| Dimension | Weight | Measures |
|-----------|--------|----------|
| Model Quality | 25% | AUC, precision, recall |
| Root Cause | 15% | Correct causal identification |
| Quality Gates | 15% | Gate enforcement |
| Documentation | 10% | BRD completeness |
| Reproducibility | 10% | Run-to-run consistency |
| Cost Efficiency | 10% | Total cost vs budget |
| Time Efficiency | 15% | Total elapsed time |

### Expected Results (Verified)

| Condition | AUC | Root Cause | Gate |
|-----------|-----|-----------|------|
| full_system | 0.847 | support_quality_degradation | passed |
| ablation_no_agentmd | **0.782** (-7.7%) | support_quality_degradation | passed |
| ablation_no_causal | 0.847 | **unknown** | passed |
| ablation_no_test | 0.847 | support_quality_degradation | **skipped** |
| ablation_no_gates | 0.847 | support_quality_degradation | **bypassed** |
| swarm_mode | 0.847 | support_quality_degradation | passed |
| evolutionary_mode | 0.847 | support_quality_degradation | passed |

### Key Metrics

- **Cost:** $23.50 per full lifecycle run (vs $548K traditional — 93.7% reduction)
- **Reproducibility:** 100% (50/50 runs)
- **Test coverage:** 94% (47 tests auto-generated)
- **Risk reduction:** 90.6%

---

## Multi-Agent Coexistence (Verified)

All agent types coexist in a single program:

```neam
budget B { cost: 10.00, tokens: 100000 }

sql_connection DB { platform: "postgres", connection: "test", database: "db" }
analyst agent A { provider: "openai", model: "gpt-4o-mini", connections: [DB], budget: B }
datascientist agent DS { provider: "openai", model: "gpt-4o", budget: B }
causal agent CA { provider: "openai", model: "o3-mini", budget: B }
mlops agent MO { provider: "openai", model: "gpt-4o", budget: B }
databa agent BA { provider: "openai", model: "gpt-4o", budget: B }
datatest agent QA { provider: "openai", model: "gpt-4o", budget: B }

dio agent DIO {
    mode: "auto",
    task: "Test all agents together",
    provider: "openai", model: "gpt-4o", budget: B
}
```

### Contextual Keywords as Variables (Verified)

DIO agent keywords are contextual — they only trigger at declaration position:

```neam
let datascientist = "I am a variable, not an agent keyword";
let causal = 42;
let mlops = true;
let dio = "also just a variable";

print(datascientist);  // "I am a variable, not an agent keyword"
print(causal);          // 42
```

### Legacy Agent Coexistence (Verified)

v0.6 agents and v0.9.x DIO agents work together:

```neam
budget B { cost: 10.00, tokens: 100000 }

// v0.6 agent
agent LegacyHelper { provider: "openai", model: "gpt-4o-mini", system: "helper", budget: B }

// v0.9.8 agent
datascientist agent DS { provider: "openai", model: "gpt-4o", budget: B }

// Both work together
```

---

## Native Function Reference

| Agent | Status Function | Key Action Functions |
|-------|----------------|---------------------|
| Migration | `migration_status(a)` | `migration_plan`, `migration_execute_wave`, `migration_reconcile` |
| DataOps | `dataops_status(a)` | `dataops_detect_anomaly`, `dataops_auto_heal`, `dataops_sla_check` |
| Governance | `governance_status(a)` | `gov_classify`, `gov_check_access`, `gov_trace_lineage` |
| Modeling | `modeling_status(a)` | `model_reverse_engineer`, `model_generate_er`, `model_normalize` |
| Analyst | `analyst_status(a)` | `analyst_query`, `analyst_sql`, `analyst_insight`, `analyst_format` |
| DataScientist | `ds_status(a)` | `ds_train`, `ds_predict`, `ds_run_eda`, `ds_explain_global`, `ds_exec_python` |
| Causal | `causal_status(a)` | `causal_discover_dag`, `causal_estimate_ate`, `causal_counterfactual`, `causal_rca` |
| MLOps | `mlops_status(a)` | `mlops_deploy_canary`, `mlops_check_drift`, `mlops_rollback`, `mlops_trigger_retrain` |
| Data-BA | `ba_status(a)` | `ba_generate_brd`, `ba_generate_criteria`, `ba_traceability_matrix` |
| DataTest | `test_status(a)` | `test_run_suite`, `test_evaluate_gate`, `test_generate_cases` |
| DIO | `dio_status(a)` | `dio_solve`, `dio_plan` |

**Note:** Data Agent and ETL Agent do not have dedicated status functions. Use compilation success as verification.

---

## Required Fields by Agent

| Agent | **Required** Fields | Recommended (Warning if Missing) |
|-------|-------------------|----------------------------------|
| `data agent` | `provider`, `model`, `sources`, `sinks` | `schema`, `quality`, `compute` |
| `etl agent` | `provider`, `model`, `sources`, `warehouse` | `marts`, `semantic` |
| `migration agent` | `provider`, `model`, `source`, `target`, `staging`, `strategy`, `budget` | -- |
| `dataops agent` | `provider`, `model`, `platforms`, `incident_policy` | `schedulers`, `audit_tables` |
| `governance agent` | `provider`, `model`, `classification`, `budget` | -- |
| `modeling agent` | `provider`, `model`, `sources` | -- |
| `analyst agent` | `provider`, `model`, `connections`, `budget` | `domain_context` |
| `datascientist agent` | `provider`, `model`, `budget` | `eda_config`, `volume_router` |
| `causal agent` | `provider`, `model`, `budget` | `scm`, `intervention` |
| `mlops agent` | `provider`, `model`, `budget` | `drift_monitor` (MOP-002) |
| `databa agent` | `provider`, `model`, `budget` | `brd_generator` |
| `datatest agent` | `provider`, `model`, `budget` | `test_strategy` (TST-003) |
| `dio agent` | `mode`, `task`, `provider`, `model`, `budget` | `infrastructure` (DIO-010), `agent_registry` (DIO-003) |

---

## Troubleshooting

| Error | Cause | Fix |
|-------|-------|-----|
| `Unknown source type 'postgresql'` | Wrong source type string | Use `"postgres"` not `"postgresql"` |
| `Expected schema identifier` | Wrong schema syntax | Use `schema Name { field: type }` with types: `string`, `int`, `float`, `bool`, `datetime` |
| `Expected number for completeness` | Nested quality block | Use flat: `quality Q { completeness: 0.95 }` not `completeness: { threshold: 0.95 }` |
| `Unexpected token in compute block` | Wrong compute syntax | Use `compute C { engine: "local" }` not `type: "local"` |
| `Unknown field in data agent: schemas` | Plural field name | Use `schema: S` (singular) not `schemas: [S]` |
| `Unexpected field in etl agent: target_dialect` | Wrong ETL field | Use `warehouse: ComputeRef` not `target_dialect: "postgres"` |
| `OPS-001: must declare platforms` | Missing required field | DataOps requires `platforms: [PlatformRef]` |
| `OPS-002: must declare incident_policy` | Missing required field | DataOps requires `incident_policy: PolicyRef` |
| `Unknown classification_policy field: patterns` | Wrong classification syntax | Use `levels: { PII: { level: 3, controls: ["masking"] } }` not `patterns` |
| `Unknown migration agent field: source_platform` | Wrong migration syntax | Use `source: SourceRef, target: TargetRef, staging: StagingRef` |
| `[warning] DIO-003: should have agent_registry` | Advisory warning | Safe to ignore -- `agent_registry` is recommended but not required |
| `[warning] DIO-010: missing infrastructure` | Advisory warning | Add `infrastructure_profile` for config/hybrid modes |
| `[warning] MOP-002: should have drift_monitor` | Advisory warning | Safe to ignore -- `drift_monitor` is recommended |
| `Runtime: Expected string value` | Non-string concatenation | Don't concatenate status results with `+`. Call `print(status_fn(agent))` separately |

---

## Testing DIO Programs

### Compile-Only Test

```bash
neamc my_program.neam -o /tmp/test.neamb 2>&1
# Success: "Wrote bytecode bundle to ..."
# Failure: Parse/compile error with details
```

### Status Test

```bash
neamc my_program.neam -o /tmp/test.neamb && neam /tmp/test.neamb
```

### CTest Suite

```bash
cd build && ctest --output-on-failure --parallel $(sysctl -n hw.ncpu)
```

### E2E Validation (22 Tests)

```
T01-T03: Schema, Source/Sink, Quality/Compute declarations
T04-T15: All 14 agent types compile and initialize
T16-T17: DIO in Auto and Config modes
T18: DIO orchestration (dio_solve) produces complete results
T19: Contextual keywords work as variables
T20: Legacy + v0.9.x coexistence
T21-T22: DataSims full system + ablation experiments
```

---

## Three Paradigms: When to Use DIO

| Paradigm | Approach | Best For | Risk |
|----------|----------|----------|------|
| **Vibe Coding** | Prompt-driven, iterate with LLM | Quick prototypes, one-off analysis | No reproducibility, no audit trail |
| **Agentic** | Single agent with tools | Focused tasks, chatbots, copilots | No cross-team coordination |
| **Spec-Driven (DIO)** | 14 agents + orchestrator, RACI, quality gates | Production data systems, ML pipelines | Higher upfront cost (but 93.7% total reduction) |

### When to Use DIO

- Data projects involving multiple roles (BA, DE, DS, MLOps)
- Projects requiring reproducibility and audit trails
- Regulated environments (GDPR, CCPA, DORA)
- When handoff failures have caused project failures before
- When you need causal analysis, not just correlation
- Production ML systems requiring monitoring and retraining

### When NOT to Use DIO

- Simple single-agent chatbots (use regular `agent`)
- One-off data exploration (use `analyst agent` alone)
- Interactive sessions (use `claw agent`)
- Build/generation tasks (use `forge agent`)

---

## Key Rules

1. **Always define budgets** for production DIO programs
2. **Data-BA runs first** in every project ("Day -1")
3. **Quality gates block deployment** by default — never bypass in production
4. **Use `env()` for secrets** — never hardcode connection strings
5. **Agent.MD improves quality by 7.7%** — always provide domain knowledge
6. **DIO is always Accountable** in RACI — exactly one R per task
7. **Infrastructure profiles abstract platforms** — swap profiles, not code
8. **Causal Agent is composable** — any agent can invoke it for "why" questions
9. **DataTest is the most critical agent** — removing it drops quality by 26.1%
10. **Test compilation first** before running with live LLM calls

---

## Environment Variables

```bash
export OPENAI_API_KEY="sk-..."                                           # Required for LLM agents
export SIMSHOP_PG_URL="postgresql://datasims:datasims_2026@localhost:5432/simshop"  # DataSims
export MLFLOW_TRACKING_URI="http://localhost:5000"                       # MLflow tracking
```

---

## Reference Links

- [The Intelligent Data Organization (eBook)](https://neam-lang.github.io/The-Intelligent-Data-Organization-with-Neam/)
- [Neam Language Reference](https://neam-lang.github.io/Neam-The-AI-Native-Programming-Language/)
- [DataSims](https://github.com/neam-lang/Data-Sims)
- [Neam Compiler (nightly)](https://github.com/neam-lang/neam-nightly)
- [NeamSkills Library](https://github.com/neam-lang/NeamSkills)
