---
icon: lucide/earth
---

# Getting Started

RuleFlow operates natively on the [**Python**](https://www.python.org/) programming language. Unlike standard simulation tools that treat state updates as simple array transformations, RuleFlow tracks systems at the level of atomic computational events. Every rewrite records spatial quantum creation and destruction, preserving causal histories, lineages, and collision records.

The project separates execution from interface:

* **The Core Library & Engine:** The foundational Python [library](../core/overview.md) that handles memory topologies, pattern matching, state evolution, and the [FlowLang](../lang/overview.md) DSL.
* **RuleFlow [Studio](../studio/core-plugins.md) (TUI):** A standalone terminal user interface (TUI) built on Textual. Conceptually separate from the core library, Studio is built *on top* of the core engine to provide an interactive, terminal-based research environment with data inspection, visual playback, and an extensible plugin system. Additionall functionality (Graph Rendering, Analysis Tools, etc.) is provided through optional plugin packages following the Hub-and-Spoke design philosophy.

---


## Installation Pathways

RuleFlow can be installed and set up in three different ways:

1. **Automated Setup Scripts (Recommended)**: The provided `ruleflow.bash` (Linux/macOS) or `ruleflow.bat` (Windows) scripts in `bin/auto/` handle the entire bootstrapping sequence automatically. These simple, one-click scripts, detect and install `uv`, set up the environment, check for updates, and launch the RuleFlow Studio application without manual intervention. They will install the `ruleflow` library globally and are ideal if the workflow is centered around *RuleFlow Studio* and the optional plugin toolchain (plugins simply import `ruleflow` from anaywhere).
2. **Manual `uv` Installation**: Installing RuleFlow and its optional toolchain directly via [`uv`](https://docs.astral.sh/uv/), the recommended package manager for managing dependencies and isolated tool environments.
3. **Manual Source Clone from GitHub**: Cloning the repository directly from GitHub to inspect/edit the source code, develop custom components, or [contribute](../community/get-involved.md) to the project.

---

## Where to Go Next

* [Installation](./install.md) — Installation links, detailed setup instructions, and dependency notes.
* [First Steps](./quick-guide/next-steps.md) — Set up your first model and step through an evolving system.
* [Key Concepts](./quick-guide/key-concepts.md) — Learn how cells, deltas, and causal events work under the hood in the context of FlowLang, our domain specific language.
