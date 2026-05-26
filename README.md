# layered-design skill

Agent Skill for designing complex features and refactoring tasks with AI coding agents. Works with any [Agent Skills](https://agentskills.io)-compatible tool.

## The problem

AI agents tend to jump straight into writing code without thinking ahead. For complex tasks (such as porting code between projects, multi-file feature additions, or architectural refactorings), this leads to:

- **Spaghetti changes** -- chaotic, unstructured modifications that are extremely difficult to review and debug.
- **Broken dependencies** -- files referencing modules or symbols that haven't been defined or imported correctly.
- **Integration failures** -- components that look fine in isolation but fail to connect or compile when put together.
- **Untraceable regressions** -- modifying multiple existing files simultaneously makes it hard to isolate when and why a bug was introduced.

Existing methodologies often focus on simple coding loops and do not address the architectural discipline required for large, multi-file changes.

## What this skill does differently

This skill teaches the agent a disciplined, multi-phase design and review workflow that prevents defects at the source:

- **Four-Phase Workflow** -- Research, Write Designs, Review, and Implement.
- **Structured Design Documents** -- Produces detailed specifications (stored in an untracked `designs/` folder) including architecture, dependency order, public APIs, integration points, and acceptance criteria.
- **Additive Layers Rule** -- Early layers (L1 through L(N-1)) only add new files. Only the final layer (LN) integrates them by modifying existing files. This allows early layers to ship independently and safely.
- **Pre-Implementation Review** -- A thorough consistency check of imports, API calls, compatibility, and references before writing any code.
- **Layer-by-Layer Verification** -- Implementing and validating each layer (with tests, type-checking, and linting) completely before proceeding to the next layer.

## Core Philosophy & Why It Works

Traditional software design templates are written for human readers. This skill is optimized specifically for **agentic workflows** and the cognitive patterns (and failure modes) of LLMs:

- **Additive Phasing (L1 to LN-1)**: LLMs easily get lost in cascading refactoring errors. By forcing early layers to be strictly additive (creating new files only) and leaving modifications of existing code for the final layer, the codebase is kept compiling and testable at every commit.
- **Red-Teaming via Multi-Agent Review**: AI agents are poor at self-correction within the same context loop. Moving the review phase to a separate "explorer" agent prevents bias and uncovers hidden API mismatches.
- **Concrete Code over Pseudocode**: High-level designs lead to low-level hallucinations. Requiring exact import paths and code signatures forces the agent to solve typing and dependency details *before* writing execution logic.
- **Context-Bound Local Memory**: By storing designs in an untracked local `designs/` directory, the agent retains a persistent local memory that survives context resets, without polluting the repository's git commit history.

## What this skill does not cover

This skill is focused on the **process of architectural design and implementation**. For other capabilities, consider pairing it with:

| Need | Recommendation |
|------|---------------|
| Production-grade engineering workflows (test pyramids, quality gates) | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) |
| Context engineering and token budget optimization | [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering) |
| Official and general-purpose agent workflows | [anthropic/skills](https://github.com/anthropics/skills) |

## Install

### The easy way: ask your AI

Most AI coding agents can install skills for you. Clone or download this repo, then open your agent and say:

> Add the skill in the `skills/layered-design/` folder of this repo.

Your agent knows where its own skills directory lives and will copy it to the right place. This works for opencode, Claude Code, Google Antigravity CLI, Mistral Vibe, and any other [Agent Skills](https://agentskills.io)-compatible tool.

### The manual way: per-tool paths

If you prefer to install manually, copy the `skills/layered-design/` folder into your tool's skills directory:

| Tool | Project-level | Global |
|------|--------------|--------|
| **opencode** | `.opencode/skills/` | `~/.config/opencode/skills/` |
| **Claude Code** | `.claude/skills/` | `~/.claude/skills/` |
| **Google Antigravity CLI (`agy`)** | `.gemini/plugins/` | `~/.gemini/antigravity-cli/plugins/` |
| **Mistral Vibe** | `.vibe/skills/` | `~/.vibe/skills/` |

For any other tool following the [Agent Skills specification](https://agentskills.io/specification), the pattern is the same:

```
<skills-dir>/layered-design/SKILL.md
```

### Install from Git (Gemini CLI)

```bash
gemini skills install https://github.com/antquinonez/layered-design-skill.git --path skills/layered-design
```

### Install from Git (Antigravity CLI / `agy`)

Since `ff-design-skill` is structured as a valid Antigravity plugin, you can install it globally by cloning the repository directly into your plugins directory:

```bash
git clone https://github.com/antquinonez/layered-design-skill.git ~/.gemini/antigravity-cli/plugins/ff-design-skill
```

Or you can use `agy`'s built-in plugin subcommands if preferred.

## Skill structure

```
ff-design-skill/
├── plugin.json
├── LICENSE
├── README.md
└── skills/
    └── layered-design/
        └── SKILL.md
```

## License

MIT
