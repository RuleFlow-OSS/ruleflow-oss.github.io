---
icon: lucide/book-open-check
---

# Key Concepts

!!! abstract "The RuleFlow Paradigm"
    RuleFlow is a Python research framework designed to model, evolve, and perform rigorous causal analysis on discrete complex systems. Instead of treating state transitions as simple array transformations, RuleFlow tracks systems at the level of **atomic computational events**.

By treating updates as discrete events that destroy and generate spatial quanta, RuleFlow preserves the operational lineages and collision dynamics of your system. When rules fire, state transitions are decomposed into `DeltaCell` units that record exactly which cells were destroyed and created. Every step records an Event that preserves its ancestry. This atomic logging enables the downstream extraction of explicit **Causal Directed Acyclic Graphs (DAGs)**, allowing researchers to measure causal distances to creation events and trace multiway universe branch histories.

---

## 1. FlowLang Domain-Specific Language
Writing rewriting systems by hand often demands tedious array-slicing logic or regex boilerplate. **FlowLang** is RuleFlow's concise, expressive domain-specific language designed to eliminate this friction.

Every rule specifies a sequence of selectors, an operator, and targets. 

### Core Operators
* **Substitution (`->`):** Replaces matched selector spans with target values.
* **Overwrite (`-->`):** Overwrites values at index spans without altering overall vector length (supports `-1` wildcards).
* **Insertion (`>`):** Inserts target values before the matched selector.
* **Deletion (`><`):** Drops matched elements entirely, contracting the universe space.

### Directives & Selectors
* **Directives:** Instructions like `@init(...)` set the starting physical space, while `@evolve(n)` evolves the loaded ruleset across _n_ sequential ticks. You can also use `@macro(...)` to import external presets.
* **Selectors:** Selectors and targets can utilize quoted strings for regex searches, bare words for literal character tokens, explicit numerical tuples, bounding spatial ranges, or dynamically evaluated function terms.

### Example System

    @init("ABA");

    # Standard substitution
    "ABA" -> "AAB";

    # Overwrite a specific spatial range
    [0, 4] --> AB;

    # Deletion (notice there is no right-hand target)
    "BB" >< ;

---

## 2. Multiway Branching and Conflict Resolution
RuleFlow natively supports non-deterministic branching. When multiple rules match or spatial spans conflict, systems can branch off into isolated universe paths.

!!! tip "Exploring the Multiway Universe"
    The engine maintains historical spatial links (`parent_delta`), enabling researchers to walk backward through branch histories via coordinate lookups.

Flags provide fine-grained behavioral control:

* **`-pl[n]` (Parallel Execution Limit):** Limits how many non-overlapping matches execute per step before branching.
* **`-bl[n]` (Branch Limit):** The hard limit on how many multiway branches can spawn from a rule.
* **`-cmp` (Conflict Marking Protocol):** Specifies which match gets marked as conflicting when spans overlap (`"ignore"`, `"this"`, `"og"`, or `"both"`).
* **`-crp` (Conflict Resolution Protocol):** Dictates engine response to conflicts (e.g., `"branch"` spawns multiway universes, while `"skip"` or `"break"` prunes them).

---

## 3. Bootstrapped Scripting
FlowLang integrates directly with general-purpose programming languages. By enclosing FlowLang statements within `---` blocks, researchers can write standard Python (`.pflow`) or Wolfram Language (`.wpflow`) code to dynamically generate rulesets. 

**Example: Generating rules dynamically in `.pflow`**

    for i in range(2):
        ---
        # FlowLang is injected dynamically
        "{i}" -> "X";
        ---

---

## 4. Extensible Architecture & RuleFlow Studio
RuleFlow Studio is a modular, terminal-based research environment built on the Textual framework. 

* **Interactive State Inspection:** Inspect individual cells via hover events to view underlying Quanta, Generation, and Identity values.
* **Live Execution & Causal Graphs:** Step forward, regress, and render live topological and causal network distributions using terminal sparklines or interactive browser views via VisJS.
* **Extensibility:** Studio encourages research-oriented tooling via an unopinionated plugin architecture. Adding a custom tool simply requires subclassing the `Plugin` base class.
