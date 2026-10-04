---
icon: lucide/shopping-bag
---

# External Plugins

[:lucide-shopping-bag: **View Official Plugin Hub**](https://github.com/RuleFlow-OSS/RuleFlow-Studio-Plugins){ .md-button .md-button--primary }

RuleFlow Studio is designed to adapt to custom experimental workflows. While the core installation provides essential tools for execution, causal metrics, and cell exploration, **External Plugins** allow you to extend the Studio's interface with specialized analyzers, custom visualizations, and tailored data exporters.

---

## Installing External Plugins

Adding an external plugin to your project requires no compilation, registry updates, or environment modifications. RuleFlow Studio dynamically detects, imports, and renders plugins at runtime.

### Step-by-Step Instructions

1. Navigate to the root directory of your active RuleFlow project (where your `.flow` files reside).
2. Locate or create a directory named `plugins`:
```text
my-ruleflow-project/
├── plugins/
│   └── custom_analyzer.py    <-- Place plugin files here
├── main.flow
└── system.pflow

```
3. Drop the plugin's single Python file (e.g., `custom_analyzer.py`) or package into the `plugins/` directory.
4. Launch or reload RuleFlow Studio in your project. The plugin will automatically register its controls in the right sidebar and mount its central display panel in the workspace.

!!! note "Plugin Specific Instructions"
    Each plugin may have unique configuration requirements or dependencies. Always check the plugin's README.md (or other documentation) for any additional setup steps.

??? tip "Dynamic Loading"
    RuleFlow Studio inspects files in the `plugins/` directory using standard Python reflection. Any class inheriting from the Studio's base `Plugin` class is mounted automatically upon launch.

??? info "Contributing Your Own Plugins"
    If you develop a custom plugin that could benefit other researchers, consider submitting it to the [community hub](https://github.com/RuleFlow-OSS/RuleFlow-Studio-Plugins)! Check out the [Contribution Guidelines](../community/contribute.md) to get started.
