---
icon: lucide/network
---

# Gephi External Tools

[:octicons-globe-24: Visit the Official Gephi Website (gephi.org)](https://gephi.org){ .md-button .md-button--primary }

[Gephi](https://gephi.org) is an open-source network analysis and visualization software package designed for exploring and manipulating complex graphs. Often described as the "Photoshop for network data," it combines an interactive OpenGL-powered canvas with a robust suite of graph-layout algorithms, statistical analysis tools, and dynamic filtering engines.

While RuleFlow Studio provides immediate terminal metrics and lightweight browser visualizers (via PyVis / VisJS), Gephi serves as the recommended external tool for rendering and deeply inspecting large-scale causal networks.

---

## Exporting Graphs from RuleFlow Studio

??? note "Manual Graph Export"
    If you are using a pure Python environment, you may export using the standard NetworkX tooling. 


RuleFlow Studio's built-in **Analysis Plugin** natively supports exporting causal graph structures directly to Gephi's native `.gexf` (Graph Exchange XML Format) file format:

1. Open your `.flow` or `.pflow` system in RuleFlow Studio and evolve the system to the desired generation.
2. In the right-hand sidebar, navigate to the **Analysis** panel.
3. Under the **Causal Network** section:
    * Specify the slice of interest in the **Event Range** input (e.g., `:200` to capture events up to step 200).
    * Choose whether to check **Collapse Edges** to combine redundant multiway transitions.
    * Click **Build Graph** to compute the underlying NetworkX DAG.
4. Under **Persistence**, select **Gephi** from the export dropdown menu.
5. Click **Export as Format**.

The studio will export the network to your project root directory with the naming scheme:

```text
<file_name>_at_<event_range>.gexf
```
