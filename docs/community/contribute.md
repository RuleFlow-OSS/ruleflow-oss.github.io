---
icon: lucide/git-pull-request-create
---

# Contributing to RuleFlow

Thank you for your interest in RuleFlow! We welcome contributions from researchers and developers alike. To keep the project organized, maintainable, and rigorously tested, please follow our general contribution policies and ensure your work is submitted to the correct repository.

## 1. Where to Contribute

The RuleFlow ecosystem is modularized across dedicated repositories. Please direct your issues and pull requests to the appropriate location:

* **Core Engine & FlowLang DSL:** Submit all improvements related to the causal engine, mathematical primitives, topologies, memory vectors, and the FlowLang parser/interpreter to the [main repository](https://github.com/RuleFlow-OSS/RuleFlow).
* **RuleFlow Studio Plugins:** Submit all custom tools, graphical widgets, and plugins for the terminal IDE to the separate [RuleFlow Studio Plugins](https://github.com/RuleFlow-OSS/RuleFlow-Studio-Plugins) repository.
* **Documentation & Website:** Submit all tutorials, guides, API references, and documentation improvements to [ruleflow-oss.github.io](https://github.com/RuleFlow-OSS/ruleflow-oss.github.io), the main website and documentation hub for the RuleFlow Software.

## 2. General Contribution Policies

* **Discuss Before Building:** For major architectural changes, new FlowLang directives, or significant engine modifications, please open an issue to discuss your proposed design before writing code. This ensures alignment with the project's core research goals and prevents wasted effort.
* **Maintain Academic Rigor:** RuleFlow is a verifiable research engine. Any modifications to the core engine or DSL execution boundaries must maintain this mathematical accuracy. Ensure your changes do not break the isolated test suites (e.g., vector memory or causal snapshot verification).
* **Preserve Modularity:** Keep the functional domains strictly isolated. UI rendering and interactive logic belong exclusively in the Studio/Plugins ecosystem, while the core `ruleflow` engine must remain entirely headless and UI-agnostic.
* **Code of Conduct:** We are committed to maintaining a respectful, inclusive, and collaborative environment. All contributors are expected to review and adhere to our [Code of Conduct](./code-of-conduct.md).
* **AI Assistance:** If you utilize AI tools to generate, refactor, or debug code submitted to the project, you must comply with the verification and disclosure standards outlined in our [AI Usage Policy](./ai-policy.md).


## 3. Licensing and Attribution

When contributing to the RuleFlow Software, you must ensure that all submissions are either your original work or appropriately licensed for inclusion in the project.

* **Project License:** By submitting a contribution, you agree that your code and documentation will be licensed under the project's existing open-source license.
* **Third-Party Work:** If you incorporate or adapt code, algorithms, or assets from other open-source projects, you must verify that their license is compatible with ours and retain all original copyright notices, headers, and attributions.
