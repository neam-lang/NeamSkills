---
name: Claude-Neam-Programming-skill
description: Comprehensive Neam language skill for Claude Code. Covers the full language — 13 types, agents, claw/forge agents, multi-agent orchestration, RAG, skills, tools, MCP, guards, policies, budgets, OOP, modules, deployment, and 100+ built-in functions. Install this skill so Claude can write production-grade Neam programs.
origin: neam-skills
version: "2.0.0"
---

# Claude Neam Programming Skill

Neam is a compiled, AI-native programming language for building agent systems. It provides first-class support for LLMs, RAG, multi-agent orchestration, multi-cloud deployment, and cost management. Written in C++17/20 with a tree-sitter parser.

## When to Activate

- User writes or asks about `.neam` files
- User builds AI agents, RAG pipelines, multi-agent systems in Neam
- User asks about Neam syntax, keywords, built-in functions, or deployment
- User wants to connect agents to knowledge bases, skills, tools, or MCP servers
- User works with NeamClaw (claw agents, forge agents)
- User configures `neam.toml` or deploys Neam to cloud

---

## Toolchain

```bash
# Install prerequisites (macOS)
brew install cmake curl openssl

# Clone and build
git clone https://github.com/neam-lang/Neam.git
cd Neam && mkdir -p build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --parallel $(sysctl -n hw.ncpu)

# Compile .neam to bytecode
neamc hello.neam -o hello.neamb

# Run bytecode
neam-cli hello.neamb

# Interactive REPL
neam-cli
neam> 1 + 2
3
```

### Build Outputs

| Binary | Purpose |
|--------|---------|
| `neamc` | Compiler (.neam -> .neamb bytecode) |
| `neam-cli` | Runtime / REPL |
| `neam-lsp` | Language Server Protocol |
| `neam-dap` | Debug Adapter Protocol |
| `neam-pkg` | Package manager |
| `neam-api` | HTTP API server |
| `neam-gym` | Training/benchmarking |

---

## Core Syntax

### Variables

```neam
let name = "Alice";        // mutable
const MAX = 100;           // immutable constant
let active = true;
let scores = [10, 20, 30];
```

Variables are block-scoped. `let` is reassignable, `const` is not.

### Output

```neam
print("debug message");   // debug / logging (no newline)
emit result;              // final output — use for results (newline)
```

### F-Strings

```neam
emit f"Hello, {name}!";
emit f"{age + 1} next year";
```

### Functions

```neam
fun greet(name) {
  return "Hello, " + name + "!";
}

// Anonymous / lambda
let doubler = fn(x) { return x * 2; };
let concise = fn(x) { x * 2 };  // implicit return

// Multi-return with tuples
fun divide(a, b) {
  return (a / b, a % b);
}
let (quotient, remainder) = divide(10, 3);
```

**Visibility modifiers:** `pub` (public), `crate` (package-internal), `super` (parent module), default is private.

### Control Flow

```neam
// if / else if / else
if (score > 90) {
  emit "A";
} else if (score > 70) {
  emit "B";
} else {
  emit "C";
}

// while loop
let i = 0;
while (i < 5) { print(i); i = i + 1; }

// for-in loop
for (item in ["apple", "banana"]) { emit item; }

// range() — three forms
for (i in range(5)) { ... }           // 0..4
for (i in range(1, 6)) { ... }        // 1..5
for (i in range(0, 20, 5)) { ... }    // 0,5,10,15

// enumerate
for ((i, color) in colors.enumerate()) { emit f"{i}: {color}"; }

// break / continue
for (n in numbers) {
  if (n < 0) { continue; }
  if (n > 100) { break; }
  emit str(n);
}
```

### Comprehensions

```neam
let squares = [x * x for x in range(10)];
let evens = [x for x in range(20) if x % 2 == 0];
let word_lengths = {word: len(word) for word in words};
let unique_lens = set(len(w) for w in words);
let pairs = [(x, y) for x in range(3) for y in range(3)];
```

### Pipe Operator

```neam
let result = data
  |> filter(fn(x) { x % 2 == 0; })
  |> map(fn(x) { x * x; })
  |> fold(0, fn(acc, x) { acc + x; });
```

---

## 13 Data Types

| Type | Description | Example |
|------|-------------|---------|
| Number | 64-bit IEEE 754 double | `42`, `3.14` |
| String | Immutable, double-quoted | `"hello"` |
| Boolean | `true` / `false` | `true` |
| Nil | Absence of value | `nil` |
| List | Ordered, mutable, mixed types | `[1, "a", true]` |
| Map | String-keyed key-value pairs | `{"name": "Alice"}` |
| Tuple | Immutable, fixed-size | `(3.14, 2.72)` |
| Set | Unordered unique elements | `set(1, 2, 3)` |
| Range | Lazy integer sequence | `range(10)` |
| Option | `Some(value)` or `None` | `Some(42)` |
| TypedArray | Homogeneous numeric array | `float_array([1.0, 2.0])` |
| Record | Named tuple (immutable) | `record Point { x: number, y: number }` |
| Table | Columnar data | `table({"col": [1,2]})` |

### Destructuring & Spread

```neam
let [first, second, ...rest] = [1, 2, 3, 4, 5];
let (x, y) = (3.14, 2.72);
let {name, age} = person;
let combined = [...front, ...back];
let config = {...defaults, ...overrides};
```

### Slicing

```neam
items[2:5]    // indices 2,3,4
items[::2]    // every other
items[-2:]    // last two
```

### Operators

- **Arithmetic:** `+`, `-`, `*`, `/`, `%`
- **Comparison:** `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Logical:** `&&`, `||`, `!`
- **Membership:** `in`, `not in`
- **String repeat:** `"ha" * 3` -> `"hahaha"`
- **Broadcasting:** `[1, 2, 3] + 10` -> `[11, 12, 13]`

### Type Conversion

```neam
str(42)        // "42"
num("42")      // 42
bool(0)        // true (only false/nil are falsy)
int(3.7)       // 3
typeof(42)     // "number"
```

---

## Structs, Traits, and Sealed Types

### Structs

```neam
struct Point { x: number, y: number }

let p = Point(3, 4);
let p2 = p with (x: 10);  // immutable copy

mut struct Counter { value: number }
let c = Counter(0);
c.value = c.value + 1;
```

### Impl Blocks

```neam
impl Point {
  fn distance_to(self, other) {
    let dx = self.x - other.x;
    let dy = self.y - other.y;
    return math_sqrt(dx * dx + dy * dy);
  }
  fn origin() { return Point(0, 0); }  // static
}
```

### Traits

```neam
trait Describable {
  fn describe(self) -> string;
}

impl Describable for Point {
  fn describe(self) {
    return f"Point({self.x}, {self.y})";
  }
}
```

### Sealed Types and Match

```neam
sealed Shape {
  Circle(radius: number),
  Rectangle(width: number, height: number),
  Point
}

match shape {
  Circle(r) => { return 3.14159 * r * r; },
  Rectangle(w, h) => { return w * h; },
  Point => { return 0; }
}
```

---

## Error Handling

### Try / Catch / Throw

```neam
try {
  let result = risky_operation();
  emit result;
} catch (err) {
  emit "Error: " + str(err);
}

fun validate(age) {
  if (age < 0) { throw "Age cannot be negative"; }
  return age;
}
```

### Panic (Unrecoverable)

```neam
panic("Missing required config");  // halts execution, uncatchable
```

### Option Type

```neam
let maybe = Some(42);
let nothing = None;

maybe.unwrap()            // 42
maybe.unwrap_or(0)        // 42
nothing.unwrap_or(0)      // 0
maybe.map(fn(x) { x * 2; })  // Some(84)
maybe.is_some()           // true
```

### Native Result Type

```neam
fun safe_divide(a, b) {
  if (b == 0) { return Err("division by zero"); }
  return Ok(a / b);
}

let answer = safe_divide(10, 3)
  .map(fn(x) { x * 2; })
  .unwrap_or(0);

// Chaining
parse_number("42")
  .and_then(validate_positive)
  .map(fn(n) { n * 2; });
```

### Error Context

```neam
context(err, "while loading config");
with_context(fn() { read_file(path); }, "loading config from " + path);
```

---

## Agents

Agents wrap an LLM with a system prompt. Three types: `agent` (stateless), `claw agent` (persistent sessions), `forge agent` (iterative builds).

### Stateless Agent

```neam
agent Assistant {
  provider: "openai",
  model: "gpt-4o-mini",
  system: "You are a helpful assistant.",
  temperature: 0.7
}

let answer = Assistant.ask("What is the capital of France?");
emit answer;
```

### Agent Properties

| Field | Type | Required | Default | Purpose |
|-------|------|----------|---------|---------|
| `provider` | string | Yes | -- | LLM provider |
| `model` | string | Yes | -- | Model identifier |
| `system` | string | No | `""` | System prompt |
| `temperature` | float | No | 0.7 | Randomness (0.0-2.0) |
| `api_key_env` | string | No | Provider default | API key env var |
| `endpoint` | string | No | Provider default | Custom endpoint URL |
| `skills` | list | No | `[]` | Tool capabilities |
| `connected_knowledge` | list | No | `[]` | RAG knowledge bases |
| `guardchains` / `guards` | list | No | `[]` | Security guards |
| `budget` | ref/inline | No | -- | Cost/token limits |
| `env` | ref | No | -- | Environment config |
| `memory` | ref | No | -- | Session memory |
| `handoffs` | list | No | `[]` | Agent transfer targets |
| `policy` | ref | No | -- | Security policy |
| `output_type` | map | No | -- | Structured output schema |
| `context_from` | string | No | -- | Path to AGENTS.md |

### 7 Supported Providers

| Provider | Value | Auth | Notes |
|----------|-------|------|-------|
| Ollama | `"ollama"` | None | Local, free, private |
| OpenAI | `"openai"` | `OPENAI_API_KEY` | GPT-4o, GPT-4o-mini |
| Anthropic | `"anthropic"` | `ANTHROPIC_API_KEY` | Claude Sonnet/Opus/Haiku |
| Gemini | `"gemini"` | `GEMINI_API_KEY` | 1M+ token context |
| Azure OpenAI | `"azure_openai"` | `AZURE_OPENAI_API_KEY` | Enterprise |
| AWS Bedrock | `"bedrock"` | AWS credentials (SigV4) | Native AWS |
| Vertex AI | `"openai"` adapter | GCP credentials | Via OpenAI-compatible endpoint |

### Vision / Multimodal

```neam
let desc = VisionBot.ask_with_image(
  "What is in this image?",
  "https://example.com/photo.jpg"
);
```

### Multi-Agent Pipeline

```neam
agent Researcher { provider: "openai", model: "gpt-4o", system: "Research thoroughly." }
agent Writer { provider: "openai", model: "gpt-4o-mini", system: "Write from research notes." }
agent Editor { provider: "openai", model: "gpt-4o-mini", system: "Edit for clarity." }

let research = Researcher.ask("Research: " + topic);
let draft = Writer.ask("Write from: " + research);
let final = Editor.ask("Edit: " + draft);
emit final;
```

### Runners (Orchestration)

```neam
runner CustomerService {
  entry_agent: TriageAgent
  max_turns: 5
  tracing: enabled
  input_guardrails: [InputChain]
  output_guardrails: [OutputChain]
}

let result = CustomerService.run("user query");
// result.final_output, result.total_turns, result.completed
```

### Handoffs

```neam
agent Triage {
  provider: "openai", model: "gpt-4o-mini",
  handoffs: [RefundAgent, BillingAgent, TechAgent]
}

// Advanced handoff config
handoff_to(UrgentAgent) {
  tool_name: "urgent_support"
  description: "Escalate urgent issues"
  input_filter: sanitize_input
  on_handoff: log_handoff
  is_enabled: is_business_hours()
}
```

### spawn and dag_execute

```neam
let result = spawn researcher("Analyze this topic");

let results = dag_execute([
  { "id": "research", "agent": "researcher", "task": "...", "depends_on": [] },
  { "id": "analysis", "agent": "analyst", "task": "...", "depends_on": ["research"] }
]);
```

### Orchestration Patterns

1. **Triage Routing** — classifier dispatches to specialists
2. **Sequential Pipeline** — chain agent outputs
3. **Supervisor/Worker** — worker + evaluator loop
4. **Debate/Adversarial** — opposing agents + judge
5. **Planning Agent** — decompose goals into steps
6. **Deep Search** — sub-queries + synthesis
7. **Chain-of-Thought** — explicit reasoning steps
8. **ReAct** — Thought/Action/Observation loop
9. **Self-Reflection** — writer + critic iterations
10. **Red Team/Blue Team** — security analysis

### Provider Failover

```neam
fun ask_with_fallback(prompt) {
  try {
    return PrimaryBot.ask(prompt);
  } catch (err) {
    try { return FallbackBot.ask(prompt); }
    catch (err2) { return "All providers unavailable"; }
  }
}
```

---

## NeamClaw: Claw Agents (Persistent Sessions)

Claw agents maintain conversation history, auto-compaction, channels, lanes, and semantic memory.

```neam
channel support_cli { type: "cli", prompt: "you> " }
channel support_http { type: "http", port: 8080, path: "/chat" }

claw agent SupportBot {
  provider: "openai"
  model: "gpt-4o-mini"
  channels: [support_cli, support_http]
  skills: [lookup_order, escalate]
  guards: [safety_chain]
  system: "You are helpful support staff."

  session: {
    idle_reset_minutes: 30
    daily_reset_hour: 4
    max_history_turns: 100
    compaction: "auto"       // "auto", "manual", "disabled"
  }

  lanes: {
    default: { concurrency: 4, priority: "normal" }
    vip: { concurrency: 2, priority: "high" }
  }

  semantic_memory: {
    backend: "sqlite"
    embedding_model: "nomic-embed-text"
    search: "hybrid"         // "keyword", "vector", "hybrid"
    top_k: 5
  }
}

let r = SupportBot.ask("Where is my order?");
let history = SupportBot.history();
SupportBot.reset();
```

### .ask() Pipeline (5 steps)

1. **Security checks** — guards, budget, kill switch
2. **Session load** — get/create session, check idle timeout
3. **Context build** — history + semantic memory + RAG, auto-compact at 80% tokens
4. **Tool loop** — LLM call + tool execution (up to 25 iterations)
5. **Response** — output guards, persist, record metrics

---

## NeamClaw: Forge Agents (Build-Verify Loop)

Forge agents iterate: fresh context per iteration, persistent state in filesystem.

```neam
fun check_output(ctx) {
  let file = file_read("output.txt");
  if (file != nil) { return VerifyResult.Done("Created."); }
  return VerifyResult.Retry("File missing. Create output.txt.");
}

forge agent Builder {
  provider: "openai"
  model: "gpt-4o"
  verify: check_output
  system: "You are a code builder."
  skills: [write_file, read_file, run_command]
  workspace: "./project"

  loop {
    max_iterations: 30
    max_cost: 12.0
    max_tokens: 600000
    prompt_file: "prompt.md"
    plan_file: "plan.txt"
    progress_file: "progress.jsonl"
    learnings_file: "learnings.jsonl"
  }

  checkpoint: "git"   // "git", "snapshot", "none"
}

let outcome = Builder.run();
match outcome {
  Completed(msg) => { emit "Done: " + msg; },
  MaxIterations => { emit "Hit iteration limit"; },
  Aborted(reason) => { emit "Aborted: " + reason; },
  BudgetExhausted => { emit "Budget exceeded"; }
}
```

### VerifyResult (Sealed Type)

```neam
VerifyResult.Done(message)    // advance to next task
VerifyResult.Retry(feedback)  // retry with feedback
VerifyResult.Abort(reason)    // stop loop
```

### Verify Context Fields

- `ctx.iteration` — current iteration (1-based)
- `ctx.current_task` — task description
- `ctx.feedback` — previous verify feedback (or nil)
- `ctx.total_cost` — cumulative USD
- `ctx.total_tokens` — cumulative tokens
- `ctx.workspace` — workspace path

---

## Knowledge Bases (RAG)

```neam
knowledge ProductDocs {
  vector_store: "usearch",
  embedding_model: "nomic-embed-text",
  chunk_size: 200,
  chunk_overlap: 50,
  sources: [
    { type: "file", path: "./docs/guide.md" },
    { type: "file", path: "./docs/faq.md" },
    { type: "text", content: "Inline documentation..." }
  ],
  retrieval_strategy: "hybrid",
  top_k: 5
}

agent DocBot {
  provider: "openai", model: "gpt-4o-mini",
  system: "Answer from documentation only.",
  connected_knowledge: [ProductDocs]
}
```

### 8 Retrieval Strategies

| Strategy | LLM Calls | Latency | Use Case |
|----------|-----------|---------|----------|
| `"basic"` | 0 | Low | Simple factual lookups |
| `"mmr"` | 0 | Low | Diverse, non-repetitive results |
| `"hybrid"` | 0 | Low | Technical terms + semantic |
| `"hyde"` | 1 | Medium | Abstract/conceptual queries |
| `"self_rag"` | 1 | Medium | High-accuracy (medical, legal) |
| `"crag"` | 1-3 | Medium | Complex multi-part questions |
| `"agentic"` | 2-5+ | High | Deep research, iterative |
| `"graph_rag"` | 1-2 | Medium | Entity relationships |

---

## Skills and Tools

### Skill (Preferred Syntax)

```neam
skill get_weather {
  description: "Get weather for a city"
  params: { city: string }
  impl(city) {
    let url = f"https://wttr.in/{city}?format=j1";
    try {
      return http_get(url);
    } catch (err) {
      return f"Unavailable: {err}";
    }
  }
}
```

### Tool (Classic Syntax with JSON Schema)

```neam
tool Calculator {
  description: "Perform math",
  params: [
    { name: "operation", schema: { "type": "string" } },
    { name: "a", schema: { "type": "number" } },
    { name: "b", schema: { "type": "number" } }
  ],
  impl(operation, a, b) {
    if (operation == "add") { return a + b; }
    if (operation == "sub") { return a - b; }
    if (operation == "mul") { return a * b; }
    if (operation == "div") {
      if (b == 0) { return "Error: division by zero"; }
      return a / b;
    }
  }
}
```

### Extern Skill — HTTP Binding

```neam
extern skill get_weather {
  description: "Get weather"
  params: { city: string }
  binding: http {
    method: "GET"
    url: "https://wttr.in/{city}?format=j1"
    headers: ["Accept: application/json"]
    response_path: "/current_condition/0/weatherDesc/0/value"
    timeout: 5000
  }
}
```

### Extern Skill — MCP Binding

```neam
mcp_server filesystem {
  command: "npx"
  args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user"]
}

extern skill read_file {
  description: "Read a file"
  params: { path: string }
  binding: mcp {
    server: "filesystem"
    tool: "read_file"
  }
}

// Bulk import MCP tools
adopt filesystem.*;
adopt filesystem.{read_file, write_file} as fs_;
```

### MCP Server Transports

```neam
// Stdio
mcp_server GitHub {
  transport: "stdio"
  command: "npx"
  args: ["-y", "@modelcontextprotocol/server-github"]
  env: { GITHUB_TOKEN: env("GITHUB_TOKEN") }
}

// SSE
mcp_server Postgres {
  transport: "sse"
  url: "http://localhost:3001/sse"
}

agent DevBot {
  provider: "openai", model: "gpt-4o",
  mcp_servers: [GitHub, Postgres]
}
```

### Sensitive Skills

```neam
skill delete_record {
  description: "Delete a record"
  params: { id: string }
  sensitive: true           // requires user approval
  impl(id) { return db_delete(id); }
}
```

### Structured Output

```neam
agent SentimentBot {
  provider: "openai", model: "gpt-4o-mini",
  output_type: {
    "sentiment": "string",
    "confidence": "number",
    "explanation": "string"
  }
}
```

---

## Guards, Policies, and Security

### Guards

```neam
guard InputValidator {
  description: "Validates input"
  on_tool_input(input) {
    if (string_length(input) == 0) { return "block"; }
    if (string_length(input) > 10000) { return "block"; }
    return input;
  }
}

guard OutputSanitizer {
  description: "Removes secrets from output"
  on_tool_output(output) {
    if (output.contains("sk-")) { return "[REDACTED]"; }
    return output;
  }
}

guardchain SecurityChain = [InputValidator, OutputSanitizer];
```

### 6 Guard Handler Types

| Handler | Trigger | Purpose |
|---------|---------|---------|
| `on_observation` | User input received | Filter prompts |
| `on_action` | Agent produces output | Filter responses |
| `on_tool_input` | Before tool execution | Validate args |
| `on_tool_output` | After tool executes | Validate results |
| `on_tool_call` | Tool invocation | Control which tools run |
| `on_result` | Final output | Last-chance filter |

Return `"block"` to reject, modified string to transform, original to pass through.

### Policy Declarations

```neam
policy StrictSecurity {
  prompt_injection: "deny"
  pii_detection: "redact"
  max_input_length: 10000
  max_output_length: 50000
  allowed_domains: ["api.example.com", "wttr.in"]
  blocked_patterns: ["ignore previous", "system:"]
}

agent SecureBot {
  provider: "openai", model: "gpt-4o-mini",
  policy: StrictSecurity,
  guards: [SecurityChain],
  budget: ProdBudget
}
```

---

## Budgets

```neam
budget ProductionBudget {
  api_calls: 1000
  tokens: 5000000
  cost_usd: 50.0
  reset: "daily"        // daily, hourly, weekly, monthly
}

// Inline budget
agent BudgetBot {
  provider: "openai", model: "gpt-4o-mini",
  budget: { max_daily_calls: 100, max_daily_cost: 5.0, max_daily_tokens: 50000 }
}
```

---

## Environment Configuration

```neam
env Production {
  API_URL: "https://api.prod.com",
  DEBUG: "false",
  API_KEY: env("PROD_API_KEY")
}

agent ProdAgent {
  provider: "openai", model: "gpt-4o",
  env: Production
}
```

---

## Memory, World Model, Planning

```neam
memory ConversationMemory {
  backend: "redis",
  retention: "session",
  max_events: 10000
}

world_model TaskWorld {
  tier: 1,
  state_schema: "task_state_v1",
  update_frequency: 1000
}

plan HierarchicalPlanner {
  pattern: "hierarchical",
  max_depth: 5,
  backtrack: true
}

agent StrategicAgent {
  provider: "openai", model: "gpt-4o",
  memory: ConversationMemory,
  world_model: TaskWorld,
  plan: HierarchicalPlanner
}
```

---

## Checkpoint and Rewind

```neam
checkpoint "safe_point";
let result = risky_operation();
if (!result) { rewind "safe_point"; }
emit result;
```

---

## Module System

```neam
module my.app.agents;

import std.list;
import std.math::{sqrt, abs};
import my.app.config as cfg;

pub fun public_helper() { }  // exported
fun internal_helper() { }    // private
crate fun package_only() { } // package-internal

// Re-export
pub use my.lib.agents;
```

### neam.toml (Project Manifest)

```toml
neam_version = "1.0"

[project]
name = "my-agent"
version = "0.1.0"
type = "binary"

[project.entry_points]
main = "src/main.neam"

[dependencies]
utils = "1.0.0"
ai-tools = { git = "https://github.com/neam/ai-tools" }

[agent]
provider = "openai"
model = "gpt-4o-mini"
tracing = true

[agent.limits]
max-tokens-per-request = 4096
timeout-seconds = 300
max-retries = 3

[security]
prompt_injection = "deny"
pii_detection = "redact"
max_input_length = 10000

[deploy]
target = "kubernetes"

[deploy.kubernetes]
namespace = "production"
replicas = 3
```

### Package Manager

```bash
neam-pkg init my-project
neam-pkg install <pkg>
neam-pkg install --git <url>
neam-pkg check
neam-pkg publish
neam-pkg update
```

---

## Testing

```neam
test "addition works" {
  assert_eq(2 + 3, 5);
}

test "string concat" {
  assert_eq("Hello" + " World", "Hello World");
}
```

| Assertion | Description |
|-----------|-------------|
| `assert_eq(a, b)` | `a == b` |
| `assert_ne(a, b)` | `a != b` |
| `assert_true(cond)` | Truthy |
| `assert_false(cond)` | Falsy |
| `assert_throws(fn)` | Throws error |
| `assert_some(val)` | Not nil |
| `assert_none(val)` | Is nil |
| `assert_ok(result)` | Is Ok |
| `assert_err(result)` | Is Err |

---

## Deployment

```bash
# Docker
neamc deploy --target docker
docker build -t my-agent -f build/deploy/docker/Dockerfile .
docker run -e OPENAI_API_KEY=$OPENAI_API_KEY my-agent

# Kubernetes
neamc deploy --target kubernetes --output ./deploy/

# AWS Lambda
neamc deploy --target aws-lambda --memory 512 --timeout 15 --arch arm64

# GCP Cloud Run
neamc deploy --target gcp-cloudrun --region us-central1

# ECS Fargate
neamc deploy --target ecs-fargate --output ./deploy/

# Terraform
neamc deploy --target terraform

# Dry run (preview manifests)
neamc deploy --target kubernetes --dry-run
```

### Cloud Configuration (neam.toml)

```toml
[state]
backend = "postgres"
connection-string = "secret://DATABASE_URL"

[llm]
default-provider = "openai"

[llm.rate-limits.openai]
requests-per-minute = 500

[llm.circuit-breaker]
failure-threshold = 5
reset-timeout-seconds = 60

[llm.cache]
enabled = true
max-entries = 5000
ttl-seconds = 300

[llm.cost]
daily-budget-usd = 500.0

[llm.fallback-chain]
providers = ["openai", "anthropic"]

[telemetry]
enabled = true
endpoint = "http://otel-collector:4318"
service-name = "my-agent"

[secrets]
provider = "aws-secrets-manager"
```

### State Backends

| Backend | Use Case |
|---------|----------|
| SQLite | Local development |
| PostgreSQL | Production multi-node |
| Redis | High-throughput |
| DynamoDB | AWS-native |
| CosmosDB | Azure-native |
| Firestore | GCP-native |

---

## Built-in Functions

### Core

```neam
len(value)                   // length of string/list/map
str(value)                   // any -> string
num(text)                    // string -> number
int(x)                       // truncate to integer
bool(x)                      // to boolean
typeof(value)                // type name string
print(value)                 // debug output
emit(value)                  // final output
input(prompt)                // read user input
json_parse(text)             // parse JSON
json_stringify(value)        // to JSON string
```

### Math

```neam
math_abs(x)                  math_floor(x)
math_ceil(x)                 math_round(x)
math_min(a, b)               math_max(a, b)
math_clamp(x, min, max)      math_sqrt(x)
math_pow(base, exp)          math_sin(x)
math_cos(x)                  math_tan(x)
math_asin(x)                 math_acos(x)
math_atan(x)                 math_atan2(y, x)
math_exp(x)                  math_log(x)
math_log10(x)                math_cbrt(x)
math_random()                math_random_int(min, max)
```

### String

```neam
contains(haystack, needle)   starts_with(text, prefix)
ends_with(text, suffix)      index_of(haystack, needle)
substring(text, start, end)  replace(text, old, new)
split(text, delim)           join(list, sep)
trim(text)                   str_lower(text)
str_upper(text)              str_repeat(text, n)
str_pad_left(text, w, ch)    str_pad_right(text, w, ch)
```

### List

```neam
list_push(list, val)         list_pop(list)
list_slice(list, start, end) list_contains(list, val)
list_index_of(list, val)     list_reverse(list)
list_sort(list)              list_map(list, fn)
list_filter(list, fn)        list_reduce(list, init, fn)
list_flat_map(list, fn)      list_unique(list)
list_zip(a, b)
```

### Map

```neam
map_keys(m)                  map_values(m)
map_has(m, key)              map_remove(m, key)
map_merge(base, overlay)     map_entries(m)
```

### File I/O

```neam
file_read_string(path)       file_write_string(path, content)
file_read_bytes(path)        file_write_bytes(path, bytes)
file_exists(path)            file_remove(path)
file_copy(src, dst)          file_rename(old, new)
file_open(path, mode)        // mode: "r"/"w"/"a"
```

### HTTP

```neam
http_get(url)
http_request(options)        // { method, url, headers, body }
```

### Crypto

```neam
crypto_hash(algo, data)      // "sha256"/"sha384"/"sha512"/"md5"
crypto_hmac(algo, key, data)
crypto_random_bytes(count)
crypto_uuid_v4()
crypto_base64_encode(data)   crypto_base64_decode(encoded)
crypto_hex_encode(data)      crypto_hex_decode(hex)
```

### Time

```neam
clock()                      // seconds since VM start
time_now()                   // UTC ISO 8601
time_now_millis()            // Unix ms
time_now_micros()            // Unix us
time_sleep(ms)
time_parse(text, fmt)        // -> Unix seconds
time_format(ts, fmt)         // -> formatted string
```

### Regex

```neam
regex_match(pattern, text)        // -> bool
regex_find(pattern, text)         // -> first match
regex_find_all(pattern, text)     // -> all matches
regex_replace(pattern, text, rep) // -> replaced text
```

### Environment

```neam
env_get(name)                env_get_or(name, default)
env_has(name)
```

### Async / Futures

```neam
future_resolve(value)        future_reject(error)
future_all(futures)          future_race(futures)
await_all(futures)           future_delay(ms)
```

### Workspace & Memory (NeamClaw)

```neam
workspace_read(path)         workspace_write(path, content)
workspace_append(path, content)
memory_search(query, top_k)  // -> [{file_path, chunk, score}]
session_history(key, limit)  // -> [{role, content}]
```

### Higher-Order Functions

```neam
map(list, fn)                filter(list, fn)
fold(list, initial, fn)      find(list, fn)
sort_by(list, fn)            group_by(list, fn)
```

---

## Standard Library Modules

```neam
import std.math::{sqrt, abs, pi};
import std.crypto::{sha256, uuid_v4};
import std.time::{now, sleep};
import std.io::{read_file, write_file};
import std.collections::{list, map, set};
import std.core::{Option, Result};
import std.data::{json, csv};
import std.net::{http};
import std.text::{regex, format};
import std.agents::{prompts, redteam};
import std.rag::{pipeline, error};
import std.testing::{assert, runner};
```

---

## Common Patterns

### Simple Q&A Bot

```neam
agent QABot {
  provider: "openai", model: "gpt-4o-mini",
  system: "Answer questions clearly."
}
emit QABot.ask(input());
```

### RAG Document Bot

```neam
knowledge Docs {
  vector_store: "usearch", embedding_model: "nomic-embed-text",
  chunk_size: 200, chunk_overlap: 50,
  sources: [{ type: "file", path: "./docs/" }]
}
agent DocBot {
  provider: "openai", model: "gpt-4o-mini",
  system: "Answer from documentation only.",
  connected_knowledge: [Docs]
}
emit DocBot.ask(input());
```

### Production Agent with Safety

```neam
guard SafetyGuard {
  on_observation(input) {
    if (input.contains("ignore previous")) { return "block"; }
    return input;
  }
  on_tool_output(output) {
    if (output.contains("sk-")) { return "[REDACTED]"; }
    return output;
  }
}
guardchain Safety = [SafetyGuard];

policy Strict {
  prompt_injection: "deny"
  pii_detection: "redact"
}

budget Prod { api_calls: 1000, tokens: 5000000, cost_usd: 50.0, reset: "daily" }

agent ProdBot {
  provider: "openai", model: "gpt-4o-mini",
  system: "Production assistant.",
  guards: [Safety], policy: Strict, budget: Prod
}
```

### Customer Support (Claw Agent)

```neam
channel cli { type: "cli" }

skill lookup_order {
  description: "Look up order status"
  params: { order_id: string }
  impl(order_id) { return {"status": "shipped", "eta": "2026-02-20"}; }
}

claw agent Support {
  provider: "openai", model: "gpt-4o-mini",
  channels: [cli], skills: [lookup_order],
  session: { idle_reset_minutes: 30, compaction: "auto" },
  semantic_memory: { backend: "sqlite", search: "hybrid", top_k: 5 }
}
```

### Code Builder (Forge Agent)

```neam
fun verify_tests(ctx) {
  let result = exec("npm test 2>&1");
  if (result.exit_code == 0) {
    return VerifyResult.Done(f"Task '{ctx.current_task}' passed.");
  }
  if (ctx.iteration > 5) {
    return VerifyResult.Abort("Too many retries.");
  }
  return VerifyResult.Retry(f"Tests failed:\n{result.stdout}");
}

forge agent CodeBuilder {
  provider: "anthropic", model: "claude-sonnet-4",
  verify: verify_tests,
  skills: [write_file, read_file, run_command],
  workspace: "./project",
  loop { max_iterations: 30, max_cost: 12.0, plan_file: "plan.txt" },
  checkpoint: "git"
}
```

### Multi-Provider Routing

```neam
agent Triage {
  provider: "openai", model: "gpt-4o-mini", temperature: 0.1,
  system: "Classify: TECHNICAL, BILLING, or GENERAL."
}
agent TechBot {
  provider: "openai", model: "gpt-4o",
  system: "Senior technical support."
}
agent BillingBot {
  provider: "gemini", model: "gemini-2.0-flash",
  system: "Billing specialist."
}

fun route(query) {
  let cat = Triage.ask(query);
  if (cat.contains("TECHNICAL")) { return TechBot.ask(query); }
  if (cat.contains("BILLING")) { return BillingBot.ask(query); }
  return "General: " + query;
}
```

---

## Key Rules

1. **Never hardcode API keys** — use `api_key_env: "ENV_VAR"` or `env("VAR")`
2. **Always add a budget** to production agents
3. **Use `emit`** for output, `print` for debug
4. **Use `const`** for values that never change
5. **One agent, one job** — keep system prompts focused
6. **Add guards** for any agent receiving user input
7. **Layer defenses** — guards + policy + budget
8. **Use claw agents** for conversational/session-based systems
9. **Use forge agents** for iterative build/generation tasks
10. **Test agents independently** before composing multi-agent systems

---

## Reference Links

- [Neam Language](https://github.com/neam-lang/Neam)
- [Documentation](https://neam-lang.github.io/Neam-The-AI-Native-Programming-Language/)
- [NeamSkills Library](https://github.com/neam-lang/NeamSkills)
- [Smart Support Claw Example](https://github.com/neam-lang/smart_support_claw)
- [NeamForge Site Generator Example](https://github.com/samsuljahith/neamforge-site-generator)
