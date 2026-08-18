# Sembi IQ — Claude plugins

[Claude plugins](https://claude.com/plugins) for the [TestRail](https://www.testrail.com/), [Testmo](https://www.testmo.com/), and [Xray](https://www.getxray.app/) test-management platforms. They install in **[Claude Code](https://claude.com/product/claude-code)** and in the **Claude apps** (web chat, the Chat tab in Claude Desktop, and Claude Cowork) — with some differences noted under [usage](#usage).

Each plugin adds test-driven workflows, backed by the TestRail, Testmo, or Xray MCP server:

- **`spec-implementer`** — implement a feature whose acceptance criteria already exist as test cases. Reads the live cases and writes code that satisfies every one.
- **`regression-preventer`** — guard new or in-progress code against breaking behavior that existing test cases already protect. Works out what the change can reach, reads the cases guarding it, and presents a guard rail brief for your confirmation before writing or repairing any code.
- **`change-evaluator`** — predict whether recent code changes will make test cases pass or fail, before running the suite.
- **`import`** — import test cases from a spreadsheet, CSV, Markdown, XML, plaintext, or test code into the platform. Presents what it found for review and writes nothing until you confirm.

## Non-Claude agents

These plugins target Claude (Claude Code and the Claude apps — Desktop, web chat, Cowork). For other agents and tools, the same workflows are published as [agentskills.io](https://agentskills.io/specification) skills in [`sembi-iq-skills`](https://github.com/SembiIQ/sembi-iq-skills).

## Prerequisite — connect the Sembi MCP server

The plugins provide the skills and subagent but **do not bundle the required MCP server connection.** You connect to the remote Sembi MCP server yourself, and the connection **must be named `testmo`, `testrail`, or `xray`**.

**For TestRail**, follow the MCP connection steps at: [https://testrail.sembi.com/](https://testrail.sembi.com/)

**For Testmo**, follow the MCP connection steps at: [https://testmo.sembi.com/](https://testmo.sembi.com/)

**For Xray**, follow the MCP connection steps at: [https://xray.sembi.com/](https://xray.sembi.com/)

> [!IMPORTANT]
> If the MCP server isn't connected (or is connected under a different name), the skills will reference tools that aren't available.

The `spec-implementer`, `regression-preventer`, and `change-evaluator` skills only read, so a read-only connection is enough for them. The **`import` skill creates cases and folders**, so it needs a connection with write access.

## Prerequisite — Python, for Excel and CSV imports only

The `import` skill needs a Python interpreter on the `PATH` **when importing Excel or CSV**, because it runs bundled scripts to parse those formats. Markdown, XML, plaintext, and test-code sources are read directly, and need no Python. Where it is needed, the skill probes for it (`python3`, then `python`, then `py`); if none is found it offers to install Python for your platform, with your permission.

No packages and no virtual environment are needed — `.xlsx` and `.csv` are read with the standard library alone. Legacy `.xls` is the one exception, needing the pure-Python `xlrd` package, which the skill installs only if and when an `.xls` source actually appears.

The `spec-implementer`, `regression-preventer`, and `change-evaluator` skills need no Python at all.

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

#### Xray

**For Xray**, from within a Claude Code session, run these three commands:

```
/plugin marketplace add SembiIQ/sembi-iq-plugins
```

```
/plugin install xray@sembi-iq
```

```
/reload-plugins
```

### Claude Desktop (Chat, Cowork, Code)

In **Claude Desktop**:

1. Open the **Customize** menu and go to the **Plugins** tab.
2. In the **Personal plugins** section, click the **+** button, then choose **Add marketplace**.
3. Paste the marketplace source — `SembiIQ/sembi-iq-plugins` — and **Sync**.
4. Once it syncs, browse the catalog and click **Install** on `testrail`, `testmo`, or `xray`. Installed plugins activate automatically.

## Usage

Skills can be invoked by their slash command or they can be auto-activated by the agent when a task matches their description. The `import` skills are the exception — they are marked user-invoked only, so they run when you use the slash command and never activate on their own.

### TestRail

| Trigger                          | What it does                                               |
|----------------------------------|------------------------------------------------------------|
| `/testrail:spec-implementer`     | Implement a feature from TestRail test cases               |
| `/testrail:regression-preventer` | Guard changes against breaking existing TestRail cases     |
| `/testrail:change-evaluator`     | Predict pass/fail of TestRail cases for recent changes     |
| `/testrail:import`               | Import test cases from a source file into TestRail         |

`/testrail:import` takes optional hints — `/testrail:import [source-file] [project] [section] [template]`. Most imports pass none; the skill finds the source and asks for the project. Hints are matched loosely (a partial or slightly misspelled name is enough) and confirmed before use, so their order does not matter.

In addition, the `testrail:change-evaluator-isolated` subagent is automatically invoked by the agent (or invoked by you, by name) when you want the evaluation sandboxed, in the background, in its own context, with a guaranteed read-only toolset. This isolated, background subagent is Claude Code and Claude Cowork specific and does not work in Claude chat on the web or desktop.

### Testmo

| Trigger                        | What it does                                           |
|--------------------------------|--------------------------------------------------------|
| `/testmo:spec-implementer`     | Implement a feature from Testmo test cases             |
| `/testmo:regression-preventer` | Guard changes against breaking existing Testmo cases   |
| `/testmo:change-evaluator`     | Predict pass/fail of Testmo cases for recent changes   |
| `/testmo:import`               | Import test cases from a source file into Testmo       |

`/testmo:import` takes optional hints — `/testmo:import [source-file] [project] [folder] [template]`. Most imports pass none; the skill finds the source and asks for the project. Hints are matched loosely (a partial or slightly misspelled name is enough) and confirmed before use, so their order does not matter.

In addition, the `testmo:change-evaluator-isolated` subagent is automatically invoked by the agent (or invoked by you, by name) when you want the evaluation sandboxed, in the background, in its own context, with a guaranteed read-only toolset. This isolated, background subagent is Claude Code and Claude Cowork specific and does not work in Claude chat on the web or desktop.

### Xray

| Trigger                      | What it does                                       |
|------------------------------|----------------------------------------------------|
| `/xray:spec-implementer`     | Implement a feature from Xray Tests                |
| `/xray:regression-preventer` | Guard changes against breaking existing Xray Tests |
| `/xray:change-evaluator`     | Predict pass/fail of Xray Tests for recent changes |
| `/xray:import`               | Import test cases from a source file into Xray     |

`/xray:import` takes optional hints — `/xray:import [source-file] [project] [folder] [test-type]`. Most imports pass none; the skill finds the source and asks for the project. Hints are matched loosely (a partial or slightly misspelled name is enough) and confirmed before use, so their order does not matter.

In addition, the `xray:change-evaluator-isolated` subagent is automatically invoked by the agent (or invoked by you, by name) when you want the evaluation sandboxed, in the background, in its own context, with a guaranteed read-only toolset. This isolated, background subagent is Claude Code and Claude Cowork specific and does not work in Claude chat on the web or desktop.
