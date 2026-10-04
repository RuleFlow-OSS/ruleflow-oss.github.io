---
icon: lucide/file-braces-corner
---
# Developing Plugins

RuleFlow Studio provides an unopinionated, MVC-based plugin architecture that allows you to mount custom tools, visualizers, and data exporters directly into the terminal IDE. Because the Studio is built entirely on the [Textual framework](https://textual.textualize.io/), all UI components are constructed using standard Textual widgets.

## The Plugin Interface

To create a custom plugin, you must define a class that inherits from the abstract `Plugin` base class found in `ruleflow.studio.model`. The application creates exactly one instance of this class per flow file.

Your plugin must declare specific class-level attributes to dictate how and when it is loaded:

* **`name`**: A string representing the display name of the plugin.
* **`file_types`**: A list of supported file extensions (e.g., `['.flow', '.pflow']`).
* **`exclude_file_types`**: An optional list of extensions that the plugin should explicitly ignore.

Every plugin must also implement three primary methods to construct its logic and interface: `on_initialized`, `panel`, and `controls`.

## Minimal Implementation

Here is a minimal block of code demonstrating how to inherit from the `Plugin` class and implement the required methods.

```python
from typing import Iterator
from textual.widgets import TabPane, Label, Button
from textual.widget import Widget
from ruleflow.studio.model import Plugin

class P(Plugin):
    name = "My Minimal Plugin"
    file_types = [".flow", ".pflow"]

    def on_initialized(self) -> None:
        # Called once the plugin is fully loaded by the model.
        # This is the ideal place to instantiate internal tools, fetch cached 
        # model data, or connect your plugin's methods to global UI signals.
        pass

    def controls(self) -> Iterator[Widget]:
        # Yields the interactive Textual widgets (like Buttons or Inputs) 
        # that will populate the plugin's right-hand control sidebar.
        yield Button("Run Process", id="btn-run-process")

    def panel(self) -> TabPane | None:
        # Returns the central widget to be displayed in the main workspace.
        # It must be wrapped in a TabPane, or return None if no panel is needed.
        return TabPane(self.name, Label("Awaiting input..."))

```

> **Important Render Order:** The view strictly calls `self.panel()` and then `self.controls()` in that sequence. If your panel references a widget instantiated in your controls, you must strategically place your instantiation logic to avoid `NoneType` errors.

## Application State and Thread Safety

Plugins maintain direct access to the broader application context through two built-in references:

* **`self.model`**: The source of truth for the application state, providing access to the current project path, file path, and active FlowLang data.
* **`self.view`**: Gives the plugin access to the Textual application instance.

When executing computationally heavy tasks via background threads, you cannot modify Textual UI widgets directly from that thread. You must wrap any UI-updating calls in **`self.cft`** (call from thread), which safely queues the update on the main application thread.

## Asset Management

If your plugin requires bundled external data or compressed archives for its operations, you can place them alongside your plugin script in the project directory. For example, if you have a file named studio.zip, you can dynamically resolve its absolute path at runtime using `self.model.project_path.joinpath("studio.zip")`.

## Finding Inspiration

The most effective way to learn the plugin architecture and Textual widget styling is by referencing existing code.

* **Core Plugins:** Review the standard built-in plugins located in the `ruleflow.studio.stdplgns` directory (`p_analysis.py`, `p_execute.py`, and `p_explore.py`). These files demonstrate best practices for rendering sparklines, updating data tables, and managing UI signals.
* **Official External Plugins:** You can browse community-built analyzers, custom interactive visualizers, and tailored data exporters in the official [RuleFlow-Studio-Plugins repository](https://github.com/RuleFlow-OSS/RuleFlow-Studio-Plugins).
