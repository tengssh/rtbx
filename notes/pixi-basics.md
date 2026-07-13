# Pixi basics

[Pixi](https://pixi.prefix.dev/) is a cross-platform, multi-language package manager. Derived from [Conda](https://conda.org/) and [uv](https://docs.astral.sh/uv/), Pixi can be used to install and manage packages in various languages, as well as to create virtual environments for specific software development needs. Additionally, Pixi provides lockfiles and a simple configuration interface, enabling consistent environments and predefined tasks for reproducible workflows.

## Install Pixi
- Install in a Linux terminal
    ```bash
    curl -fsSL https://pixi.sh/install.sh | PIXI_HOME=/opt/pixi PIXI_NO_PATH_UPDATE=1 sh
    ```
    - Custom installation with [installer options](https://pixi.prefix.dev/latest/installation/#installer-script-options)
- Add PATH
    ```bash=
    export PIXI_HOME=/opt/pixi
    export PATH="/opt/pixi/bin:$PATH"
    ```

## Use Pixi
- Initialization
    ```bash
    pixi init [OPTIONS] PROJECT_FOLDER
    ```
    - Created `pixi.toml` and `pixi.lock`
    - [OPTIONS]
        - `--import requirements.txt`: add dependencies to `pixi.toml`
- Install packages
    ```bash
    pixi add python==3.11
    ```
- Rebuild environment
    ```bash
    pixi install --frozen
    ```
    - [Update options](https://pixi.prefix.dev/latest/reference/cli/pixi/install/#update-options)
- Activate Python virtual environment
    ```bash
    pixi shell
    ```
- [Pixi tasks](https://pixi.prefix.dev/latest/workspace/advanced_tasks/)
    - Add tasks to the `pixi.toml`
        ```bash=
        [workspace]
        channels = ["conda-forge"]
        name = "example"
        platforms = ["linux-64", "osx-64", "osx-arm64", "win-64"]
        version = "0.1.0"

        [dependencies]
        python = "3.11.*"

        [feature.test.dependencies]
        pytest = "*"

        [environments]
        test = ["test"]

        [tasks]
        test = { cmd="PYTHONPATH=scripts python -m pytest tests", default-environment="test" }
        ```
    - List available tasks
        ```bash
        pixi tasks list
        ```
    - Run the task
        ```bash
        pixi run <TASK_NAME>
        ```
- [Multi environment](https://pixi.prefix.dev/latest/tutorials/multi_environment/)
    - Add a feature
        ```bash
        pixi add --feature debug python=3.9
        pixi add --feature debug sisl=0.15
        ```
    - Add an environment
        ```bash
        pixi workspace environment add debug --feature debug
        ```
    - In `pixi.toml`
        ```bash=
        ...
        [feature.prod.dependencies]
        python = "3.11.*"
        sisl = ">=0.16.2,<0.17"

        [feature.debug.dependencies]
        python = "3.9.*"
        sisl = "0.15.*"

        [environments]
        default = ["prod"]
        debug = ["debug"]
        ```
    - Add packages from Github repositories
        ```bash
        pixi add  --pypi --feature prod "sisl @ git+https://github.com/zerothi/sisl.git"
        ```
        - Include dependencies for build
            ```bash=
            # pixi.toml
            [dependencies]
            gcc_linux-64 = "*"
            gxx_linux-64 = "*"
            gfortran_linux-64 = "*"
            cmake = ">=3.20"
            make = "*"
            ```
        - Add extras for optional [PyPI-dependencies](https://pixi.prefix.dev/v0.23.0/reference/project_configuration/#pypi-dependencies)
            ```bash
            # pixi.toml
            [feature.prod.pypi-dependencies]
            sisl = { git = "https://github.com/zerothi/sisl.git", extras = ["viz"] }
            ```
    - Run command in a specified environment
        ```bash
        pixi run --environment debug python -c "import sisl; print(sisl.__version__)"
        ```
- Clean environments
    ```bash
    pixi clean [OPTIONS] [COMMAND]
    ```
    - [COMMAND]
        - `cache`: remove cached packages