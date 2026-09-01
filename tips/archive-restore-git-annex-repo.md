--- 
name: Archive/Restore a Git/Git-Annex repository
tags: [Git, Git-Annex, Shell]
---

# Archive/Restore a Git/Git-Annex repository

This document demonstrates how to archive and restore a Git/Git-Annex repository based on the following repositories:
- Code repository (GitLab): https://gitlab.ruhr-uni-bochum.de/tengssh/fedas
- Data repository (Zenodo): https://zenodo.org/records/14673466

## I. Archiving

### Prepare a Git/Git-Annex repository

Clone a Git repository
```bash
REPO_GIT_URL="https://gitlab.ruhr-uni-bochum.de/tengssh/fedas.git"
git clone $REPO_GIT_URL
```

### Configure Git & Git-Annex

1. Set up configuration for git
```bash
USER_NAME="tengssh"
USER_EMAIL="tengssh@users.noreply.github.com"
git config --local user.name $USER_NAME
git config --local user.email $USER_EMAIL
```

2. Set up configuration for git-annex
```bash
git config --local anneex.largefile 'largerthan=10kb and include=data/*'
git config annex.private true
git config remote.origin.annex-readonly true
git config remote.origin.annex-ignore true
```

3. Check the configuration
```bash
git config --list
```

4. Initialize for Git-Annex
```bash
git annex init
```

### Add & annex data

1. Add data
```bash
REPO_DIR="fedas"
cd $REPO_DIR
mkdir data && cd data

REMOTE_DATA_REPO="https://zenodo.org/records/14673466/files"
wget $REMOTE_DATA_REPO/0-preliminary.zip
wget $REMOTE_DATA_REPO/1-T180DW_a1-field_hysteresis.zip
wget $REMOTE_DATA_REPO/DW_summary.csv

cd ../
```

2. Annex data
```bash
git annex add data/
git commit -m "annex data"
```

### Change data backend for Git-Annex (optional)

Change the default backend for data from SHA256E to MD5
```bash
basename `readlink data/0-preliminary.zip` # SHA256E-s...--...
git annex migrate --backend=MD5
basename `readlink data/0-preliminary.zip` # MD5-s...--...
```

### Register URLs & Drop local copies

1. Register URLs with remote data repository
```bash
cd data/
for file in *; do
    key=$(basename `readlink $file`)
    git annex registerurl $key $REMOTE_DATA_REPO/$file
done
cd ../
```

2. Drop local copies
```bash
git annex drop
#git annex list
```

### Create a checksum summary

1. Create a summary of checksums for later registration of files
```bash
cd data

> summary
for file in *; do
    [ "$file" = "summary" ] && continue

    #1 w/i local files
    size=$(stat -L -c %s "$file")
    hash=$(md5sum "$file" | awk '{print $1}') # or sha256sum
    key="MD5-s${size}--${hash}" # or SHA256E-s...--...

    #2 w/o local files
    target=$(readlink "$file")
    key=$(basename "$target")

    if [ -n "$key" ]; then
        echo "${file}, ${key}, ${REMOTE_DATA_REPO}/${file}" >> summary
    fi
done

cd ../
```

2. Add "data/summary" to Git
```bash
git add data/summary
git commit -m "add checksums list"
```

### Archive the repository

Zip the Git repository
```bash
git archive --format=tar.gz --prefix fedas/ -o /temp/archive.tgz HEAD
ls /temp/
```

## II. Restoring

> [!Note]
> Make sure [checksums list](#create-a-checksum-summary) was created in the archiving step.

### Restore Git repository

1. Unzip the archived Git repository
```bash
REPO_DIR="fedas"
USER_NAME="tengssh"
USER_EMAIL="tengssh@users.noreply.github.com"

cd /temp/
tar -xf archive.tgz
cd $REPO_DIR
```

2. Initialize the repository
```bash
git init -b main
git add .
git config --local user.name $USER_NAME
git config --local user.email $USER_EMAIL
git commit -m "1st commit"
git annex init
```
 
### Re-register URLs for Git-Annex

1. Register URLs from checksums summary for annexed files
```bash
cd data/
awk -F', ' '{print $2, $3}' summary | while read -r key url; do
    git annex registerurl "$key" "$url"
done
#git annex list
```

2. Get files from remote data repository
```bash
git annex get
#git annex list
#git annex whereis
```