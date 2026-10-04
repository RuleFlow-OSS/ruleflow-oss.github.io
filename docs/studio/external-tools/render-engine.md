---
icon: lucide/cast
---

!!! info "Documentation Work in Progress"
    There is minimal documentation available for this page.

The Render Engine is a standalone Godot application (work in progress) designed to visually render discrete complex systems simulated by RuleFlow. It acts as a lightweight, external client that connects to the RuleFlow server via a custom TCP socket protocol to receive continuous live state updates. By decoupling the heavy mathematical computation from the graphics pipeline, it allows users to smoothly explore evolving 2D and 3D environments, as well as causal network graphs, in real time without bottlenecking the main simulation.
