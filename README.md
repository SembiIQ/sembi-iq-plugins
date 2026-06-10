# Sembi IQ — Claude plugins

[Claude plugins](https://claude.com/plugins) for the [Testmo](https://www.testmo.com/) and [TestRail](https://www.testrail.com/) test-management platforms. They install in **[Claude Code](https://claude.com/product/claude-code)** and in the **Claude apps** (web chat, the Chat tab in Claude Desktop, and Claude Cowork) — with some differences noted under [usage](#usage).

Each plugin adds two test-driven workflows, backed by the Testmo or TestRail MCP server:

- **`spec-implementer`** — implement a feature whose acceptance criteria already exist as test cases. Reads the live cases and writes code that satisfies every one.
- **`change-evaluator`** — predict whether recent code changes will make test cases pass or fail, before running the suite.

## Non-Claude agents

These plugins target Claude (Claude Code and the Claude apps — Desktop, web chat, Cowork). For other agents and tools, the same two workflows are published as [agentskills.io](https://agentskills.io/specification) skills in [`sembi-iq-skills`](https://github.com/SembiIQ/sembi-iq-skills).

## Prerequisite — connect the Sembi MCP server

The plugins provide the skills and subagent but **do not bundle the required MCP server connection.** You connect the Sembi MCP server yourself, and the connection **must be named `testmo` or `testrail`**.

For TestRail, follow the MCP connection steps at: [https://testrail.sembi.com/](https://testrail.sembi.com/)

For Testmo, follow the MCP connection steps at: [https://testmo.sembi.com/](https://testmo.sembi.com/)

If the MCP server isn't connected (or is connected under a different name), the skills will reference tools that aren't available.

## Installation

### Claude Code

This repo is its own plugin marketplace (named `sembi-iq`). Add it, then install whichever test management product you use:

For TestRail, from within a Claude Code session:

```
/plugin marketplace add SembiIQ/sembi-iq-plugins
/plugin install testrail@sembi-iq
/reload-plugins
```

For Testmo, from within a Claude Code session:

```
/plugin marketplace add SembiIQ/sembi-iq-plugins
/plugin install testmo@sembi-iq
/reload-plugins
```

### Claude Apps

In the **Claude apps** (web chat, Claude Desktop, Cowork), add the `SembiIQ/sembi-iq-plugins` marketplace and install `testmo` / `testrail` from the **Customize → Plugins** menu (or claude.ai settings) instead of the CLI.

## Usage

Skills can be invoked by their slash command or they can be auto-activated by the agent when a task matches their description. The `change-evaluator-isolated` subagent is auto-delegated (or invoked by name) when you want the evaluation sandboxed in its own context with a guaranteed read-only toolset.

**Across Claude surfaces:** the **skills** work everywhere — slash-invocable in Claude Code, auto-invoked by relevance in the Claude apps (Desktop/web don't support custom slash triggers). The **`change-evaluator-isolated` subagent** runs in **Claude Code and Cowork**; it's unavailable (grayed out) in plain web/Desktop chat, which simply fall back to the in-context `change-evaluator` skill.

### TestRail

| Trigger                              | Type     | What it does                                                       |
|--------------------------------------|----------|--------------------------------------------------------------------|
| `/testrail:spec-implementer`         | skill    | Implement a feature from TestRail test cases                       |
| `/testrail:change-evaluator`         | skill    | Predict pass/fail of TestRail cases for recent changes             |
| `testrail:change-evaluator-isolated` | subagent | The same evaluation, but run in an isolated, **read-only** context |

### Testmo

| Trigger                            | Type     | What it does                                                       |
|------------------------------------|----------|--------------------------------------------------------------------|
| `/testmo:spec-implementer`         | skill    | Implement a feature from Testmo test cases                         |
| `/testmo:change-evaluator`         | skill    | Predict pass/fail of Testmo cases for recent changes               |
| `testmo:change-evaluator-isolated` | subagent | The same evaluation, but run in an isolated, **read-only** context |
