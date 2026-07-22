# Guix basics

[GNU Guix](https://guix.gnu.org/) is a functional package manager based on [Nix](https://nixos.org/), which can be installed on top of an existing Linux distribution or used as a standalone system. 

Each package installed by Guix is treated as a unique installation, stored in a hashed folder and accessed via symlinks. This allows multiple versions of the same package to coexist within the same system and makes rolling back to previous versions ("time travel") very easy. 

As deterministic processes and outputs are essential for robust computation, the level of bit-for-bit reproducibility offered by Guix is pragmatically valuable for scientific computing workflows.

## Installation
- [Binary installation](https://guix.gnu.org//manual/stable/en/html_node/Binary-Installation.html)
    - On Linux (e.g., Ubuntu)
        ```bash
        apt install guix
        ```
    - Use the `guix-install.sh` script
        ```bash
        cd /tmp
        wget https://guix.gnu.org/guix-install.sh
        chmod +x guix-install.sh
        ./guix-install.sh [FLAG]
        ```
        - [FLAG]
            - `--uninstall`: uninstall Guix
    - Set up profile (e.g., in `~/.bashrc`)
        ```bash=
        GUIX_PROFILE="$HOME/.guix-profile"
        source "$GUIX_PROFILE/etc/profile"
        ```
    - Install [locales](https://guix.gnu.org/manual/stable/en/html_node/Application-Setup.html#Locales-1) (for languages other than English)
        ```bash
        guix install glibc-locales
        export GUIX_LOCPATH=$HOME/.guix-profile/lib/locale # add to ~/.bashrc
        ```
- [Standalone Guix system](https://guix.gnu.org//manual/stable/en/html_node/System-Installation.html)

## Usage
- Package management
    - Search packages
        ```bash
        guix search PACKAGES
        ```
        or
        ```bash
        guix package -s PACKAGES
        ```
    - Install packages
        ```bash
        guix install PACKAGES
        ```
        or
        ```bash
        guix package -i PACKAGES
        ```
        - [NOTE]
            - Installed packages are stored in `/gnu/store`.
    - Remove packages
        ```bash
        guix remove PACKAGES
        ```
        or
        ```bash
        guix package -r PACKAGES
        ```
    - Upgrade packages
        ```bash
        guix upgrade PACKAGES
        ```
        or
        ```bash
        guix package -u PACKAGES
        ```
    - Show package information
        ```bash
        guix show PACKAGE
        ```
        - [NOTE]
            - `gcc-toolchain`, `gfortran-toolchain` instead of `gcc`, `gfortran`
        - [Examples]
            - Show version: `guix show cowsay | recsel -p version`
            - Show package counts: `guix show gcc-toolchain | recsel -c`
    - Show installed packages
        ```bash
        guix package --list-installed # -I
        ```
        - See [manual](https://guix.gnu.org/manual/stable/en/html_node/Invoking-guix-package.html) for other options.
    - Show Guix information
        ```bash
        guix describe [OPTIONS]
        ```
        - [OPTIONS]
            - `--format=FORMAT` (or `-f FORMAT`): `channels`, `recutils`, etc.
        - [Examples]
            - Show commit ID: `guix describe -f recutils | recsel -p commit`
            - Export channels definition: `guix describe -f channels > channels.scm`
    - Edit package definition
        ```bash
        guix edit PACKAGE
        ```
    - Build package & show store path
        ```bash
        guix build PACKAGE
        # ls -l $(guix build PACKAGE)
        ```
    - Update definitions & upgrade packages
        ```bash
        guix pull [OPTIONS]
        ```
        - [NOTE]
            - Run without option for the first time.
        - [OPTIONS]
            - `–-list-generations`: show history
            - `–-roll-back`: revert an update
            - `--commit=HASH`: use a specific commit
            - `--delete-generations`: clean up history
    - Show package dependencies
        ```bash
        guix graph [OPTIONS] PACKAGE
        ```
        - [OPTIONS]
            - `--type=TYPE`: `bag`, `reverse-package`, `references`, `referrers`, etc.
            - `--max-depth=n` (or `-M n`): the maximum depth `n` of graph
        - [Examples]
            - Draw dependency DAG: `guix graph --type=bag PACKAGE | dot -Tpdf > DAG.pdf`
            - Find path between two packages: `guix graph --path PACKAGE1 PACKAGE2`
            - Show reverse dependencies (with a max-depth of 1): `guix graph --type=reverse-package -M 1 PACKAGE`
            - Show runtime dependencies: `guix graph --type=references PACKAGE`
            - Show reverse runtime dependencies: `guix graph --type=referrers PACKAGE`
- One-off environment
    ```bash
    guix shell [OPTIONS] PACKAGES -- COMMAND
    ```
    - [OPTIONS]
        - `--container` (or `-C`): create an isolated environment
        - `--expose=HOST_PATH[=IMG_PATH]`: mount a host path (to an image path)
        - `--export-manifest`: export a manifest file for a given shell command
        - `--manifest=FILE` (or `-m FILE`): load a manifest file
    - [Examples]
        - Export manifest file: `guix shell -C PACKAGES --export-manifest > manifest.scm`
        - Load manifest file in an isolated container: `guix shell -C -m manifest.scm`
- Container images
    ```bash
    guix pack [OPTIONS] [PACKAGES|-m MANIFEST.scm]
    ```
    - [OPTIONS]
        - `--format=FORMAT`: `docker` (Docker), `squashfs` (Singularity/Apptainer)
        - `--save-provenance`: save provenance information
    - [NOTE]
        - The image will be created under `/gnu/store/`.
- [Time travel](https://guix.gnu.org/blog/2026/time-travel-without-borders/)
    ```bash
    guix time-machine [OPTIONS] -- COMMANDS 
    ```
    - [OPTIONS]
        - `--commit=HASH` (or `-c HASH`): access to a certain commit ID of revision
        - `--channels=FILE.scm` (or `-C FILE.scm`): specify channels with channel file
    - [Examples]
        - time-machine + shell: `guix time-machine -c HASH -- shell -C -m manifest.scm -- hello`
        - time-machine + pack: `guix time-machine -C channels.scm -- pack -m manifest.scm --format=docker --save-provenance`
- Garbage collection
    ```bash
    guix gc
    ```

## References
- https://guix.gnu.org/manual/stable/en/html_node/index.html
- https://www.fun-mooc.fr/fr/cours/reproducible-research-ii-practices-and-tools-for-managing-comput/