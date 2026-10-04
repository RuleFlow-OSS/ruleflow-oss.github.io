---
icon: lucide/earth
---

# Overview

<video width="100%" controls autoplay loop muted playsinline>
    <source src="../assets/scaffold.webm" type="video/mp4">
</video>

**RuleFlow Studio** is an extensible terminal user interface (TUI) research environment [built with Textual](https://github.com/textualize/textual/) that operates directly on top of the core RuleFlow Python framework. It serves as an interactive researcher toolbox, providing immediate access to live script execution, stepping/regressing flows, cell-level hover inspection, sparkline metrics, and graph exports without requiring researchers to write custom visualization boilerplate.

The Studio is strictly decoupled from the core framework across several architectural boundaries:

* **Headless, UI-Agnostic Core Engine:** The primary framework (`core/`, `lang/`, and `analysis/`) operates completely independently of any user interface. It manages contiguous memory vectors, pattern matching, FlowLang grammar parsing, and NetworkX-backed causal DAG calculations. The core engine can run entirely headless inside CLI scripts, automated test pipelines, Docker containers, or remote compute servers.


* **Decoupled MVC Presentation Layer:** RuleFlow Studio (`studio/`) acts strictly as a presentation and interaction layer structured around a Model-View-Controller architecture. It imports the core engine as a backend dependency, using a lightweight signal framework to observe state transitions and bind simulation updates to reactive widgets and data tables.


* **Research-Centric Extensibility:** While the core framework strictly enforces mathematical simulation boundaries and state invariants, Studio offers an unopinionated plugin system (`studio.model.Plugin`). Researchers can drop standalone Python files into a local folder to mount custom buttons, experiment controls, and diagnostic panels into the TUI without modifying the engine's underlying codebase.