# Sembi IQ — Claude plugins

[Claude plugins](https://claude.com/plugins) for the [TestRail](https://www.testrail.com/) and [Testmo](https://www.testmo.com/) test-management platforms. They install in **[Claude Code](https://claude.com/product/claude-code)** and in the **Claude apps** (web chat, the Chat tab in Claude Desktop, and Claude Cowork) — with some differences noted under [usage](#usage).

Each plugin adds two test-driven workflows, backed by the TestRail or Testmo MCP server:

- **`spec-implementer`** — implement a feature whose acceptance criteria already exist as test cases. Reads the live cases and writes code that satisfies every one.
- **`change-evaluator`** — predict whether recent code changes will make test cases pass or fail, before running the suite.

## Non-Claude agents

These plugins target Claude (Claude Code and the Claude apps — Desktop, web chat, Cowork). For other agents and tools, the same two workflows are published as [agentskills.io](https://agentskills.io/specification) skills in [`sembi-iq-skills`](https://github.com/SembiIQ/sembi-iq-skills).

## Prerequisite — connect the Sembi MCP server

The plugins provide the skills and subagent but **do not bundle the required MCP server connection.** You connect to the remote Sembi MCP server yourself, and the connection **must be named `testmo` or `testrail`**.

**For TestRail**, follow the MCP connection steps at: [https://testrail.sembi.com/](https://testrail.sembi.com/)

**For Testmo**, follow the MCP connection steps at: [https://testmo.sembi.com/](https://testmo.sembi.com/)

> [!IMPORTANT]
> If the MCP server isn't connected (or is connected under a different name), the skills will reference tools that aren't available.

## Installation

### Claude Code

This repo is its own plugin marketplace (named `sembi-iq`). Add it, then install whichever test management product you use.

#### TestRail

**For TestRail**, from within a Claude Code session, run these three commands:

```
/plugin marketplace add SembiIQ/sembi-iq-plugins
```

```
/plugin install testrail@sembi-iq
```

```
/reload-plugins
```

#### Testmo

**For Testmo**, from within a Claude Code session, run these three commands:

```
/plugin marketplace add SembiIQ/sembi-iq-plugins
```

```
/plugin install testmo@sembi-iq
```

```
/reload-plugins
```

### Claude Desktop (Chat, Cowork, Code)

In **Claude Desktop**:

1. Open the **Customize** menu and go to the **Plugins** tab.
2. In the **Personal plugins** section, click the **+** button, then choose **Add marketplace**.
3. Paste the marketplace source — `SembiIQ/sembi-iq-plugins` — and **Sync**.
4. Once it syncs, browse the catalog and click **Install** on `testrail` or `testmo`. Installed plugins activate automatically.

## Usage

Skills can be invoked by their slash command or they can be auto-activated by the agent when a task matches their description.

### TestRail

| Trigger                      | What it does                                           |
|------------------------------|--------------------------------------------------------|
| `/testrail:spec-implementer` | Implement a feature from TestRail test cases           |
| `/testrail:change-evaluator` | Predict pass/fail of TestRail cases for recent changes |

In addition, the `testrail:change-evaluator-isolated` subagent is automatically invoked by the agent (or invoked by you, by name) when you want the evaluation sandboxed, in the background, in its own context, with a guaranteed read-only toolset. This isolated, background subagent is Claude Code and Claude Cowork specific and does not work in Claude chat on the web or desktop.

### Testmo

| Trigger                    | What it does                                         |
|----------------------------|------------------------------------------------------|
| `/testmo:spec-implementer` | Implement a feature from Testmo test cases           |
| `/testmo:change-evaluator` | Predict pass/fail of Testmo cases for recent changes |

In addition, the `testmo:change-evaluator-isolated` subagent is automatically invoked by the agent (or invoked by you, by name) when you want the evaluation sandboxed, in the background, in its own context, with a guaranteed read-only toolset. This isolated, background subagent is Claude Code and Claude Cowork specific and does not work in Claude chat on the web or desktop.
