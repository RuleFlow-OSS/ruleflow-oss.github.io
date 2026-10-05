---
icon: lucide/info
---

# About

!!! quote "The knowledge of an effect depends on and involves the knowledge of a cause."
    — Baruch Spinoza


RuleFlow is both a framework and a software suite designed to facilitate the exploration and analysis of complex systems. As a framework, RuleFlow provides a structured protocol for modeling systems that evolve over time, whether cellular automata, substitution and rewriting systems, or any domain defined by state transitions governed by rules. While established tools like [Golly](https://golly.sourceforge.io/) and [Mathematica](https://www.wolfram.com/mathematica/) offer distinct strengths, they generally lack a dedicated focus on causal analysis. Simulating the evolution of a complex system is one challenge; providing a generalizable platform specifically built to trace, inspect, and evaluate causal histories across time is another. RuleFlow is built with this causality-first aim at its core.

Within the scientific Python ecosystem, researchers have often had to choose between inflexible domain-specific tools and cumbersome low-level scripts. RuleFlow bridges this gap by offering a Python-native environment that balances analytical power with accessibility. By prioritizing clean abstractions, RuleFlow aims to accommodate systems of virtually any structure while remaining intuitive to researchers, regardless of their programming background.

Architecturally, RuleFlow is organized around a [Hub-and-Spoke model](https://abstractopedia.org/archetypes/hub_and_spoke_coordination/concise/). A central hub manages global state and simulation flow, while pluggable spokes handle specialized tasks such as visualization, metric collection, and causal tracing. Implemented through [reactive signals](https://en.wikipedia.org/wiki/Observer_pattern), callbacks, extensible [plugin](../studio/external-plugins.md) APIs, and standard object-oriented inheritance, this decoupled structure ensures the engine can adapt cleanly to unforeseen use cases. RuleFlow abstracts away execution complexity so users can focus on their models, embodying David Wheeler's classic insight: "Any problem in computer science can be solved with another level of indirection."

---

## What RuleFlow Addresses

1. **A Generalizable Causal Analysis Engine**
RuleFlow tracks spatial changes down to discrete spatial quanta (`Cell` objects with immutable coordinate locations, generations, and unique IDs). When rules fire, state transitions are decomposed into `DeltaCell` units that record exact destroyed and created cells. Every step records an `Event` that preserves its ancestry, allowing downstream extraction of network metrics (e.g., degree assortativity, network density, flow hierarchy, and longest DAG paths) without re-simulating the system.

2. **A Low-Boilerplate, High-Performance DSL (FlowLang)**
Writing rewriting systems and cellular automata by hand often demands tedious array-slicing logic or regex boilerplate. FlowLang introduces a concise syntax for rewrites (`->`), overwrites (`-->`), insertions (`>`), and deletions (`><`). It includes native flags for fine-grained behavioral control, such as parallel execution limits (`-pl`), branch caps (`-bl`), conflict marking protocols (`-cmp`), conflict resolution protocols (`-crp`), and stochastic match thresholds (`-p_rule`, `-p_space`).

3. **Multiway Universe Exploration**
RuleFlow natively supports non-deterministic branching. When multiple rules match or spatial spans conflict, systems can branch off into isolated universe paths. The engine maintains historical spatial links (`parent_delta`), enabling researchers to walk backward through branch histories via coordinate lookups (`(event_idx, space_idx)`).

4. **Bootstrapped Dynamic Scripts**
FlowLang integrates directly with general-purpose programming languages. By enclosing FlowLang statements within `---` blocks, researchers can write standard Python (`.pflow`) or Wolfram Language (`.wpflow`) code to dynamically generate rulesets with loops, mathematical predicates, or automated enumeration logic.

5. **RuleFlow Studio**
A modular, terminal-based GUI built on Textual. Studio offers real-time visualization of evolving spaces, inspection of individual cells via hover events, interactive causal graph generation (via PyVis / VisJS), and an unopinionated plugin architecture for rapid experimentation.
