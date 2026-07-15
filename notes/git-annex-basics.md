# Git-Annex

[Git-Annex](https://git-annex.branchable.com/) is a data management tool designed for handling large files with [Git](https://git-scm.com/). Under the hood, git-annex creates a "git-annex" branch and uses hashes, symlinks, and checksums to manage these files (metadata and file locations). This allows the content of the files to be version-controlled in normal git repositories while the actual files can be stored elsewhere.

## Installation
- Linux (Debian)
    ```bash
    apt update
    apt install git git-annex
    ```

## Usage
- Initialization
    ```bash
    git init
    git annex init ["REPO_NAME"] [-c annex.commitmessage="MESSAGE"]
    ```
    - `git annex init` (or `git-annex init`) creates a "git-annex" branch.
- Configuration
    - For git
        ```bash
        git config --local user.name "NAME"
        git config --local user.email "EMAIL"
        ```
    - For git-annex (optional)
        - Define rules for file tracking (e.g., only track files larger than 100KB in `data/` folder)
            ```bash
            git config --local annex.largefiles 'largerthan=100kb and include=data/*'
            ```
        - Not to push local metadata to remote
            ```bash
            git config annex.private true
            ```
        - Protect remote from modification (e.g., `git-annex sync`)
            ```bash
            git config remote.origin.annex-readonly true
            ```
        - Not to store annexed files from remote
            ```bash
            git config remote.origin.annex-ignore true
            ```
        - Set minimum number of copies
            ```bash
            git annex numcopies N # default: 1
            ```
    - Check configuration
        ```bash
        git config --list
        ```
- [Special remotes](https://git-annex.branchable.com/special_remotes/)
    - Add special remotes
        ```bash
        git-annex initremote REMOTE_NAME type=TYPE PARAM=VALUE [OPTIONS] [-c annex.commitmessage="MESSAGE"]
        git-annex describe REMOTE_NAME "DESCRIPTION"
        ```
        - [OPTIONS]
            - `exporttree=yes`: use readable names and paths
        - Examples
            - `type=rsync rsyncurl=URL encryption=none`
            - `type=webdav url=URL encryption=shared`
            - `type=directory directory=PATH encryption=none`
            - `type=httpalso url=URL`
            - `type=rclone ...`
    - Enable remote (before getting data)
        ```bash
        git-annex enableremote # check available remotes
        git-annex enableremote REMOTE_NAME
        ```
    - Remove special remotes
        ```bash
        git remote rm REMOTE_NAME
        git-annex dead REMOTE_NAME
        git-annex forget --drop-dead --force
        ```
    - Configure sync-branch
        ```bash
        git config --local annex.synconlyannex true
        ```
    - [Preferred content](https://git-annex.branchable.com/git-annex-preferred-content/)
- Data management
    - Add data
        ```bash
        git-annex add [OPTIONS] DATA [-c annex.commitmessage="MESSAGE"]
        git commit -m "MESSAGE"
        ```
        - Replace files with symbolic links pointing to the contents stored in the `.git/annex/objects/`.
        - [OPTIONS]
            - `--force-small`: add to git as all files were small
            - `--force-large`: add to git-annex as all files were large
    - Move data
        ```bash
        git-annex move DATA [--from=LOCATION|--to=LOCATION]
        ```
    - Sync data
        ```bash
        git-annex sync [OPTIONS]
        ```
        - [OPTIONS]
            - `--only-annex`: only sync "git-annex" branches
            - `--cleanup`: clean up `git-annex`-created branches
    - Copy data
        ```bash
        git-annex copy DATA [--from=LOCATION|--to=LOCATION]
        ```
    - Export data
        ```bash
        git-annex export BRANCH --to LOCATION
        ```
        > [!Note]
        > - Use when `initremote` with `exporttree=yes`
        > - Less flexible than `copy`
    - Drop data
        ```bash
        git-annex drop DATA [OPTIONS]
        ```
        - [OPTIONS]
            - `--from=LOCATION`
            - `--force`
    - Reinject known data
        ```bash
        git-annex reinject SRC DEST
        ```
    - Get data
        - Download from URLs (e.g., web or bittorrent)
            ```bash
            git-annex addurl [OPTIONS] URL
            ```
            - [OPTIONS]
                - `--file=NAME`: download file as NAME
                - `--preserve-filename --pathdepth=N`: download with original filename with URL path depth of N (e.g., -1, 1, 3)
                - `--fast`/`--relaxed`: skip verfication
        - Remove URLs
            ```bash
            git-annex rmurl DATA URL
            ```
        - Register URLs
            ```bash
            KEY=$(basename `readlink DATA`)
            #KEY=$(git-annex lookupkey DATA)
            git-annex registerurl $KEY URL
            ```
        - Get annexed files
            ```bash
            git-annex get [OPTIONS] DATA
            ```
            - [OPTIONS]
                - `--debug` (`git-annex get --debug DATA 2>&1 | grep -v process`)
    - Switch backend (e.g. from URL to MD5)
        ```bash
        git-annex migrate [--backend=BACKEND] DATA
        ```
        - BACKEND: `SHA256E`, `MD5`, etc.
    - Unlocak files
        ```bash
        git-annex unlock DATA
        # chmod -R +w
        ```
        > [!Note]
        > - Since annexed files are write-protected, unlock them before editing.
    - Unannex data (e.g., for archiving)
        ```bash
        git-annex unannex DATA
        ```
- Data inspection
    - Show data information
        ```bash
        git-annex info [OPTIONS] [DATA]
        ```
        - [OPTIONS]
            - `--bytes`: display file sizes in bytes
        > [!Note]
        > - For keys starting with `URL-`, content cannot be verified (untracked).
        > - For key starting with `SHA256E-`/`MD5E-`, data integrity can be checked.
    - List repositories that contain data
        ```bash
        git-annex whereis [DATA]
        ```
    - Display a table of remotes that contain data
        ```bash
        git-annex list [DATA]
        ```
        - Output example
            ```bash
            here
            |REMOTE
            ||DIR
            |||web
            ||||bittorrent
            |||||
            X_X__ DATA
            ```
    - File system check
        ```bash
        git fsck # for git database
        git-annex fsck # for annexed files
        ```
- Remote Git repository
    - Add remote repositories
        ```bash
        git remote add REMOTE_REPO URL
        ```
    - Push data
        ```bash
        git push REMOTE_REPO main
        git push REMOTE_REPO git-annex
        ```
- Miscellaneous
    - Initialize a shared bare repository
        ```bash
        git init --share=group --bare BARE_REPO
        ```
    - Show git history
        ```bash
        git log --all --graph --decorate --oneline
        ```
    - Archive git repository
        ```bash
        git archive --format=tar.gz --prefix REPO_NAME/ -o ../archive.tgz HEAD
        ```

## References
- https://git-annex.branchable.com/git-annex/
- https://www.fun-mooc.fr/fr/cours/reproducible-research-ii-practices-and-tools-for-managing-comput/