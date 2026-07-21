# Agent Foundry

> Autonomous agent patterns, agent platform specifications, multi-agent orchestration frameworks, and operational governance policies for enterprise AI systems.

[![Layer: L1](https://img.shields.io/badge/Layer-L1%20—%20Reference%20Catalogs-8b5cf6?style=flat-square)](https://github.com/technehub-labs/dea-metamodel)
[![Status: Alpha](https://img.shields.io/badge/Status-Alpha-FF6B35?style=flat-square)](./)
[![License: MIT](https://img.shields.io/badge/License-MIT-44CC11?style=flat-square)](LICENSE)

---

## Overview

Agent Foundry is the L1 reference catalog for all things related to autonomous and semi-autonomous AI agents in the enterprise context. It provides enterprise architects and AI platform teams with:

- **Agent pattern library** — proven patterns for agentic architectures: ReAct, Plan-and-Execute, Supervisor-Worker, Debate, Reflect
- **Platform specifications** — runtime requirements, tool-use contracts, memory architectures, and security boundaries for enterprise agent platforms
- **Multi-agent orchestration** — patterns for coordinating multiple agents: hierarchical, market-based, consensus-based, event-driven
- **Operational governance** — policies for agent auditability, rollback, guardrails, and compliance in regulated environments

---

## Framework Position

```
L0 — Metamodel (dea-metamodel)
  └─ defines: SolutionComponent, Technology, Relationship
L1 — Agent Foundry (this repo)
  └─ seeds and governs agent patterns, platforms, and orchestration frameworks
L2 — Tooling (dea-cli, dea-scripts, dea-code-gen)
  └─ generate agent scaffolds, validate tool-use contracts, simulate multi-agent interactions
L3 — Governance
  └─ Agent Foundry governance contracts: audit trails, rollback policies, guardrail enforcement
```

---

## Structure

```
dea-catalog-agent-foundry/
├── README.md
├── CATALOG/
│   ├── v1-alpha/
│   │   ├── index.yaml
│   │   ├── agent-patterns/          ← one YAML per agent pattern
│   │   │   ├── react.yaml
│   │   │   ├── plan-and-execute.yaml
│   │   │   ├── supervisor-worker.yaml
│   │   │   ├── debate.yaml
│   │   │   └── reflect.yaml
│   │   ├── agent-platforms/          ← agent runtime specifications
│   │   │   ├── agent-platform-standard.yaml
│   │   │   └── agent-security-boundary.yaml
│   │   └── orchestration-frameworks/
│   │       ├── hierarchical.yaml
│   │       ├── market-based.yaml
│   │       └── event-driven.yaml
│   └── index.yaml
├── governance/
│   ├── agent-audit-policy.yaml
│   ├── agent-rollback-policy.yaml
│   └── agent-guardrails.yaml
├── viewpoints/
│   └── stakeholder-views.yaml
└── LICENSE
```

---

## Agent Pattern Entity

Each entry is a `SolutionComponent` entity typed against the DEA metamodel:

```yaml
id: dea:agent-pattern-react
type: SolutionComponent
name: ReAct Agent Pattern
abbreviation: ReAct
version: "1.0.0"
status: active
description: >
  Reasoning + Acting pattern where the agent interleaves thought steps (Reasoning)
  with actions (Acting) against an external environment, observing results to inform
  the next reasoning cycle. Suitable for tool-use and RAG tasks.
pattern_type: single-agent
cognitive_architecture:
  reasoning: ReAct (Reasoning + Acting)
  memory: Short-term (turn context) + Long-term (vector store)
  planning: None (reactive to observation)
  tools: Required (function calling interface)
strengths:
  - Transparent reasoning trace
  - Effective for tool-use tasks
  - Simple to implement
  - Well-suited for RAG
weaknesses:
  - Limited long-horizon planning
  - Reasoning token overhead
  - Brittle on ambiguous tasks
use_cases:
  - Document Q&A with tool access
  - Database query synthesis
  - Multi-step API orchestration
antipatterns:
  - Long-horizon planning tasks
  - Tasks requiring global context
  - High-cost multi-step reasoning
technology_stack:
  - LLM: GPT-4 or Claude 3+ with function calling
  - Vector store: Pinecone / Weaviate / Chroma
  - Framework: LangChain / LlamaIndex
relationships:
  - source_id: dea:agent-pattern-react
    target_id: dea:principle-observable
    relationship_type: implements
  - source_id: dea:agent-pattern-react
    target_id: dea:pattern-cqrs
    relationship_type: complements
tags:
  - agent-pattern
  - single-agent
  - reasoning
  - tool-use
```

---

## Agent Platform Specification

```yaml
id: dea:agent-platform-standard
name: Enterprise Agent Platform Specification
version: "1.0.0"
status: active

runtime_requirements:
  isolation: Each agent runs in an isolated process/container with resource limits
  memory_per_agent: Minimum 512MB RAM, recommended 2GB
  tool_access: Agents may only call tools in the approved tool registry
  network: Egress restricted to approved endpoints; no direct internet for reasoning agents
  storage: Agent state persisted to encrypted store between invocations

security_boundaries:
  - Agents cannot modify their own runtime configuration
  - Agents cannot spawn sub-processes or shell commands
  - Agents cannot access secrets directly — must use secret-vault integration
  - Agent actions are logged to immutable audit trail
  - All agent outputs to users are filtered through output guardrails

memory_architecture:
  short_term: Turn context window (managed by LLM provider)
  long_term: Vector store with semantic search (approved vendors)
  working_memory: Ephemeral scratch space, cleared between sessions

tool_use_contract:
  - All tools declared in agent system prompt at initialisation
  - Tool schemas published to agent tool registry
  - Tool execution results returned as structured observations
  - No dynamic tool discovery at runtime

audit_and_compliance:
  - Every agent action logged: timestamp, agent_id, action, target, result
  - Audit logs retained for minimum 7 years (regulated environments)
  - Rollback: full session state serialisable and restorable within 5 minutes
```

---

## Operational Governance

### Agent Audit Policy
Every production agent must:
- Emit an immutable audit event for every action taken
- Include: agent ID, session ID, timestamp, action type, target resource, outcome
- Audit events must not contain PII in plain text — use reference IDs

### Agent Rollback Policy
- Full agent session state (memory, context, pending actions) must be serialisable
- Restore time from snapshot must not exceed 5 minutes for any agent
- Automatic rollback triggered on: LLM provider outage, memory corruption, tool registry inconsistency

### Agent Guardrails
- Input validation: user prompts scanned for injection patterns before processing
- Output filtering: agent outputs pass through a content classifier before user delivery
- Resource limits: per-agent token budget, tool call budget, session duration limit
- Escalation: agents must escalate to human review when confidence < threshold or action is irreversible

---

## How to Use

**Browse agent patterns:**
```bash
dea query --type SolutionComponent --tag agent-pattern
```

**Generate a CTO viewpoint on agent governance:**
```bash
dea viewpoint --stakeholder CTO --tag agent-governance
```

**Generate an agent scaffold from a pattern:**
```bash
dea generate --template agent-scaffold --id dea:agent-pattern-react
```

---

## Contributing

1. Propose a new agent pattern or platform spec via issue
2. Align pattern with the DEA metamodel `SolutionComponent` entity
3. Declare all known relationships to other patterns, principles, and technologies
4. Include strengths, weaknesses, use cases, and anti-patterns
5. Submit PR — CI validates against metamodel schema

---

## License

MIT License — see [LICENSE](./LICENSE)
