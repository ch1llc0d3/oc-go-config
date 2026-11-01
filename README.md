# oc-go-config

Personal configuration for [OpenCode Go](https://opencode.ai) CLI — a multi-agent setup for full-stack and cloud engineering work.

## Overview

`opencode.json` defines a primary orchestration agent backed by eleven specialist subagents. The primary agent classifies each task by domain and delegates to the appropriate specialist before implementing anything.

## Agent Roster

| Agent | Role | Model | Cost (in/out per 1M) |
|---|---|---|---|
| **build** (primary) | Orchestrates full-stack and cloud tasks | claude-sonnet-5-5 | $2.00 / $10.00 |
| **plan** (primary) | Decomposition, risk analysis, pre-implementation review | gemini-4-argon | $2.00 / $10.00 |
| **general** | Summaries, synthesis, medium-complexity side tasks | qwen3.8-flash | $0.15 / $0.47 |
| **explore** | Read-only repo search, dependency tracing, convention lookup | qwen3.8-flash | $0.15 / $0.47 |
| **vision** | Screenshot analysis, visual QA, UI inspection | claude-haiku-5-5 | $0.10 / $0.50 |
| **frontend** | React, Next.js, CSS, accessibility, components | kimi-k2.7-code | $0.95 / $4.00 |
| **backend** | APIs, services, data models, auth flows, business logic | deepseek-v4-pro | $1.74 / $3.48 |
| **cloud** | Cloud, CDK, Terraform, Docker, CI/CD, networking | glm-5.3 | $1.40 / $4.40 |
| **security** | Secrets, IAM, auth, encryption, dependency risk | glm-5.3 | $1.40 / $4.40 |
| **architect** | Module boundaries, data flow, layering, tradeoffs | qwen3.6-plus | $0.50 / $3.00 |
| **qa** | Regression analysis, edge cases, test strategy, release risk | minimax-m3 | $0.30 / $1.20 |
| **refiner** | Small refactors, naming, formatting, cleanup | minimax-m3 | $0.30 / $1.20 |
| **documenter** | Setup notes, API refs, runbooks, changelogs | qwen3.6-plus | $0.50 / $3.00 |

## Free Fallback Models

When paid tokens run out, swap to these free alternatives:

| Agent(s) | Free Fallback | ID |
|---|---|---|
| build, general | Beam | `beam-free` |
| explore, cloud, security | GLM-5.3-Flash | `glm-5.3-flash-free` |
| frontend, backend | DeepSeek V4.1 Flash | `deepseek-v4.1-flash-free` |
| vision | Nemotron 3.5 Super VL | `nemotron-3.5-super-vl-free` |
| qa, refiner, documenter | MiMo-V2.6-Flash | `mimo-v2.6-flash-free` |
| plan, architect | Big Pickle | `big-pickle` |

## Design Principles

- **Specialist-first delegation** — the primary agent must use a domain specialist when one exists; falling back to `general` for a covered domain is not allowed.
- **Read before write** — `plan`, `explore`, `architect`, `qa`, and `security` agents are read-only by default.
- **Conservative cloud changes** — destructive cloud commands (`cdk deploy`, `terraform apply/destroy`, `cdk destroy`) are denied; mutations require confirmation.
- **Minimal changes** — all agents prefer targeted edits over speculative rewrites.

## Usage

Place `opencode.json` at the root of a project (or your home config directory) and run OpenCode normally. The build agent picks up the configuration automatically.

```sh
opencode
```

To use only planning mode:

```sh
opencode --agent plan
```

## Config file location

Place `opencode.json` in the OpenCode config directory for your OS:

| OS | Path |
|---|---|
| MacOS | `~/.config/opencode/opencode.json` |
| Linux | `~/.config/opencode/opencode.json` |
| Windows | `%APPDATA%\opencode\opencode.json` |

> `~` expands to `/Users/<your-user>` on MacOS and `/home/<your-user>` on Linux.  
> `%APPDATA%` expands to `C:\Users\<your-user>\AppData\Roaming` on Windows.

## Files

```
opencode.json   # Full agent configuration with models, prompts, and permissions
index.html      # GitHub Pages showcase site (OoT-themed, light/dark mode)
```
