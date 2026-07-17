# uv basics

[uv](https://docs.astral.sh/uv/) is a Python package manager written in Rust, aiming for fast installation, comprehensive project management with lockfiles, and a streamlined workflow for building and publishing packages.

## Install uv
- On Linux
    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | UV_INSTALL_DIR=/opt/uv/ UV_NO_MODIFY_PATH=1 sh
    ```

## Use uv
- Virtual environment
    - Create a virtual environment
        ```bash
        uv venv --python 3.12 my-venv
        ```
    - Activate a virtual environment
        ```bash
        source my-venv/bin/activate
        ```
    - Install packages via [the `pip` interface](https://docs.astral.sh/uv/pip/)
        ```bash
        uv pip install numpy matplotlib
        ```
    - Deactivate a virtual environment
        ```bash
        deactivate
        ```
- [Projects](https://docs.astral.sh/uv/concepts/projects/)
    - Initialize a project
        ```bash
        uv init [OPTIONS] [PATH]
        ```
        - By default, `.python-version`, `README.md`, `main.py`, `pyproject.toml` will be created.
        - [OPTIONS]
            - `--bare`: only create a `pyproject.toml`
    - Generate `uv.lock` and `.venv/`
        ```bash
        uv run main.py
        ```
        - `uv.lock`: the cross-platform lockfile for package dependencies
        - `.venv/`: the virtual environment runtime folder
    - Add packages
        - Via command line
            ```bash
            uv add pandas
            ```
        - From `requirements.txt`
            ```bash
            uv add -r requirements.txt
            ```
        - Via `pyproject.toml` (e.g., "pandas>=3.0.3")
            ```python=
            [project]
            name = "my-project"
            version = "0.1.0"
            description = "Add your description here"
            readme = "README.md"
            requires-python = ">=3.14"
            dependencies = [
                "pandas>=3.0.3",
            ]
            ```
            - After modification, install packages by running `uv run main.py` or `uv sync`
    - Remove packages
        - Via command line
            ```bash
            uv remove numpy
            ```
        - Via `pyproject.toml`
            - Edit the dependencies field, and then run `uv run main.py` or `uv sync`
    - [Building & publishing a package](https://docs.astral.sh/uv/guides/package/)
        - Build distributions
            ```bash
            uv build
            ```
        - Update version
            ```bash
            uv version 1.0.0
            ```
        - Publish packages
            ```bash
            uv publish --token TOKEN --publish-url URL
            ```
- Run scripts
    - With installed dependencies
        ```bash
        uv run my-script.py
        ```
    - As [standalone script with inline metadata](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies)
        ```bash
        uv add --script my-script.py 'numpy' 'matplotlib' 
        ```
        - Add dependencies at the top of the script
        ```bash
        uv run my-script.py
        ```
        - An ephemeral virtual environment with the necessary packages will be created to run the script.
    - [Tools](https://docs.astral.sh/uv/concepts/tools/)
        - Run tools
            ```bash
            uv tool run pycowsay hello
            uvx pycowsay hello
            ```
        - Install tools
            ```bash
            uv tool install pycowsay
            ```
- Clean cache
    ```bash
    uv cache clean
    ```