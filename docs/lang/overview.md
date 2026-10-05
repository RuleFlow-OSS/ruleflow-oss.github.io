---
icon: lucide/earth
---

# Overview

FlowLang is a native Domain-Specific Language (DSL) designed on top of the core RuleFlow framework. It provides a concise, expressive syntax for defining rule-based transformations, cellular automata, and complex multi-way systems over vector topologies.

## Core Capabilities

* **Expressive Transformations:** Define spatial operations using distinct operators: Substitution (`->`), Overwrite (`-->`), Insertion (`>`), and Deletion (`><`).
* **Advanced Rule Control:** Fine-tune execution with inline flags for stochastic probabilities (`-p_rule`, `-p_space`), parallel execution limits (`-pl`), and branch limits (`-bl`).
* **Directives:** Modify simulation behavior and ruleset properties with directives (e.g. `@init(...)`, `@evolve(...)`, `@compress(...)`, `@merge(...)`, `@macro(...)`).
* **Bootstrapping:** Embed FlowLang directly into Python (`.pflow`) or Wolfram Language (`.wpflow`) scripts. Use `---` blocks to seamlessly inject dynamic variables and host-language logic into your rulesets.
