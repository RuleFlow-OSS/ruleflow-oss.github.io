---
icon: lucide/earth
---

# Overview

FlowLang is a native Domain-Specific Language (DSL) designed on top of the core RuleFlow framework. It provides a concise, expressive syntax for defining rule-based transformations, cellular automata, and complex multi-way systems over vector topologies.

[Here is a good example](../get-started/quick-guide/first-steps.md) of a simple script that defines a basic set of transformation rules.


## Core Capabilities

* **Expressive Transformations:** Define spatial operations using distinct operators: Substitution (`->`), Overwrite (`-->`), Insertion (`>`), and Deletion (`><`).
* **Advanced Rule Control:** Fine-tune execution with inline flags for stochastic probabilities (`-p_rule`, `-p_space`), parallel execution limits (`-pl`), and branch limits (`-bl`).
* **Directives:** Modify simulation behavior and ruleset properties with directives (e.g. `@init(...)`, `@evolve(...)`, `@compress(...)`, `@merge(...)`, `@macro(...)`).
* **Bootstrapping:** Embed FlowLang directly into Python (`.pflow`) or Wolfram Language (`.wpflow`) scripts. Use `---` blocks to seamlessly inject dynamic variables and host-language logic into your rulesets.



## FlowLang Philosophy

FlowLang operates on a simple premise: an initial state (a universe) is transformed over discrete time steps by a sequence of pattern-matching rules. Rather than writing traditional imperative loops, you define the topological structures you want to find and the transformations that should occur when they are matched. 

### The Universe and the State

In the RuleFlow Software engine, the "universe" is represented by a `SpaceState`, typically a 1-dimensional vector of integers or characters. You initialize this space using the `@init` directive. All rules evaluate against the current state of this space, looking for specific sequences to act upon.

### The Evolution Cycle

Execution in FlowLang is event-driven and step-based. When you trigger an evolution (e.g., via the `@evolve` directive or programmatically), the interpreter performs a distinct execution cycle:

1. **Matching:** Every active rule scans the current space to find matching patterns (selectors).
2. **Conflict Resolution:** If multiple matches overlap, the engine consults the rule's Conflict Resolution Protocol (`-crp`) and Conflict Marking Protocol (`-cmp`) to determine if the matches should be ignored, skipped, or branched.
3. **Application:** The rules apply their chosen operations (Substitute, Overwrite, Insert, or Delete) to the matched regions. By default, **each individual rule must generate and return an entirely new space upon application** (generating spatial modifications known internally as a `DeltaSpace`). However, the `@merge` directive can be used to make a group of rules atomic. Once merged into a chain, these rules apply sequentially to the same state, meaning they no longer need to create a new space for each individual rule's application.
4. **Commit:** The modified spaces become the "current" spaces for the next evolutionary step, and the system's time increments by one.

### Multi-Way Branching

FlowLang natively supports non-deterministic, multi-way systems. Unlike standard cellular automata that resolve to a single next state, FlowLang can branch a single universe into multiple parallel universes. This occurs under two primary conditions:

* **Parallel Execution Limits (`-pl`):** If a rule finds more matches than it is allowed to process concurrently in a single space, it branches the space, applying the remaining transformations in a newly spawned universe.
* **Conflict Resolution (`-crp`):** If two rule matches conflict (e.g., they try to modify the exact same sequence), the engine can be configured to spawn a parallel branch so both outcomes occur in entirely separate timelines.

### Rule Evaluation Precedence

By default, rules are evaluated sequentially (in the order they are defined) until a rule successfully applies a transformation, upon which the remaining rules in its group will be ignored for the remainder of the current step (a Sequential System). However, rules can be grouped together. If a rule successfully modifies the space and possesses a Group Break (`-gb`) flag set to `False`, the engine will continue evaluating any subsequent rules in that same group for the remainder of the current step. This allows you to choose between fallback rules and mutually exclusive transformations, or to create a more complex, multi-rule evaluation system that can apply multiple transformations in a single step (a Parallel System).

### Python Execution Example

While FlowLang scripts can be entirely self-contained, they are natively embedded, interpreted, and managed via the Python `FlowLang` class. The execution history is stored as a series of events, allowing you to easily inspect or render the state of all multi-way branches at any point in time.

```python
from ruleflow.lang.interpreter import FlowLang

# 1. Define the FlowLang script
code = """
@init("ABA");  # Initialize the starting space
ABA -> AAB;
A   -> ABA;
"""

# 2. Instantiate the interpreter
flow = FlowLang()

# 3. Interpret the AST and execute Phase 1 & Phase 2 directives
flow.interpret(code)

# 4. Programmatically evolve the system 10 steps
flow.evolve(10)

# 5. Inspect the output multi-way events
for event in flow.events:
    for space in event.spaces:
        print(space, end=', ')
    print()
```
