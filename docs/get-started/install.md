---
icon: lucide/download
---

# Installation

There are several ways to install RuleFlow, each of which is covered below.

## Simple One-Click Script (*Recommended*)

The fastest way to get up and going is using our automated bootstrapping scripts. These scripts completely automate environment preparation:

- Check for an existing installation of `uv` and install it automatically if it is missing.
- Create an isolated, persistent global tool environment for `ruleflow`.
- Check PyPI for any new releases and upgrade `ruleflow` in-place automatically.
- Launch **RuleFlow Studio** directly in your terminal.

You do not need to configure virtual environments, activate shells, or manage Python versions manually. You may find the location of these scripts in the source here: [`bin/auto`](https://github.com/RuleFlow-OSS/RuleFlow/tree/main/bin/auto).

=== "Windows"

    #### Download & Run
    [:material-download: **Download `ruleflow.bat`**](https://rawcdn.githack.com/RuleFlow-OSS/RuleFlow/18fba6270e6c79d3c15941fa652e5ae21006835d/bin/auto/ruleflow.bat){ .md-button .md-button--primary }

    1. Run the file:
       - **Via File Explorer:** Double-click on the `ruleflow.bat` file.
       - **OR Via PowerShell:**
         ```bat
         .\ruleflow.bat
         ```

    ??? tip "Desktop Shortcut & Custom Icon"
        Because `.bat` files cannot embed custom application icons natively, you can create a desktop shortcut:
        
        1. Put the `ruleflow.bat` file in a folder and [download this icon](https://raw.githubusercontent.com/RuleFlow-OSS/RuleFlow/refs/heads/main/logo.ico) into the same folder.
        2. Right-click `ruleflow.bat` :lucide-move-right: click **Show more options** :lucide-move-right: select **Create shortcut**.
        3. Right-click the newly generated shortcut :lucide-move-right: select **Properties**.
        4. Under the **Shortcut** tab, click **Change Icon...** and select `logo.ico` *(use the location of `logo.ico`)*.
        5. Place the newly created shortcut file into `C:\Users\<user name>\Desktop` and name it as you wish.

    ??? bug "Troubleshooting on Windows"
        - You may see Windows Defender warnings due to RuleFlow being an unsigned software. You may safely ignore these; RuleFlow is safe to run and will not cause vulnarabilities.
        - **PowerShell Execution Policy Restrictions:**
          If your system blocks the automated `uv` download script, run the following command in PowerShell (in the same directory as `ruleflow.bat`) to allow user-level script execution:
          ```powershell
          Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
          Unblock-File -Path .\ruleflow.bat
          ```
          This only needs to be done once.
=== "macOS & Linux"
    #### Download & Run

    [:material-download: **Download `ruleflow.bash`**](https://rawcdn.githack.com/RuleFlow-OSS/RuleFlow/18fba6270e6c79d3c15941fa652e5ae21006835d/bin/auto/ruleflow.bash){ .md-button .md-button--primary }

    1. Grant execution permissions to the script:
       ```bash
       chmod +x ruleflow.bash
       ```

    2. Execute the script:
       ```bash
       ./ruleflow.bash
       ```

       *(Alternatively, run it directly with `bash ruleflow.bash` without modifying permissions).*

    ??? bug "Troubleshooting on Linux / macOS"
        - **Permission Denied (`chmod +x`):**
          If you download the script via a browser or `curl`, POSIX executable permissions are stripped by default. Run `chmod +x ruleflow.bash` before running `./ruleflow.bash`.
        - **Missing `curl`:**
          Minimal Linux distributions and container images may omit `curl`. Install it via your distribution package manager (e.g., `sudo apt install curl` or `sudo pacman -S curl`).

---

## Using the `uv` Package Manager

[`uv`](https://docs.astral.sh/uv/) is an extremely fast Python package manager and the default package manager for RuleFlow. It enables running RuleFlow directly in an isolated tool environment or managing standard local virtual environments while automatically managing all the dependencies. For more details on how to use `uv`, read their official documentation [here](https://docs.astral.sh/uv/).

Follow [these instructions](https://docs.astral.sh/uv/getting-started/installation/) to download `uv` before completing any further steps.

#### Option 1: Standalone Studio (`uv tool`)

If your primary goal is to use **RuleFlow Studio** as an interactive terminal interface, install and run it globally as an isolated tool:

1. **Install the Studio tool**:
```bash
uv tool install "ruleflow"
```

2. **Launch RuleFlow Studio**:
```bash
uv tool run --from ruleflow studio
```

3. **Updating RuleFlow**:
To fetch the latest release from PyPI, upgrade the tool directly:
```bash
uv tool upgrade ruleflow
```


#### Option 2: Core Library in an Environment (`uv add`)

??? note "Cache Refresh"
    If a new version was recently pushed to PyPI and uv reports that the version cannot be found, force a fresh query to bypass local cache:
    ```bash
    uv cache clean ruleflow
    ```

If you are developing custom models, running simulations from scripts, etc., simply install the package into an active virtual environment:

```bash
uv add "ruleflow"
```

---

## Source clone with `git`

Cloning the repository directly from GitHub gives you full access to the source code, tests, and development tree. This setup is recommended if you plan to inspect internal mechanisms, build custom Studio plugins, or contribute to RuleFlow.

To modify and edit code, we recommend using both [VS Code](https://code.visualstudio.com/) for quick and easy file editing, and [PyCharm](https://www.jetbrains.com/pycharm/) for a full Python IDE experiance.


#### 1. Clone the Repository

Clone the project repository to a directory on your local machine using `git`:

```bash
git clone https://github.com/RuleFlow-OSS/RuleFlow.git
cd RuleFlow
```

Alternatively, you may use [GitHub Desktop](https://github.com/apps/desktop) for a user freindly `git` interface.


#### 2. Environment Setup & Editable Installation

Install the package in **editable mode** (`-e`) so that any modifications made to the Python source files under `src/` take effect immediately without requiring a reinstallation.

```bash
# Create and activate a local virtual environment
uv venv
source .venv/bin/activate

# Install in editable mode
uv pip install -e .
```


#### 3. Verify the Installation
Ensure the environment is configured correctly by launching the Studio interface:

```bash
# Launch RuleFlow Studio directly from the source tree
python -m ruleflow.studio.view
```
