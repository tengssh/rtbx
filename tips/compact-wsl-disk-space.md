--- 
name: Compact WSL disk space
tags: [Windows, WSL, PowerShell]
---

# Compact WSL disk space

As described in a [WSL issue](https://github.com/microsoft/WSL/issues/4699#issuecomment-627133168), the WSL virtual hard disk file (`.vhdx`) that has grown does not automatically shrink back. Therefore, it has to be manually shrunk, as follows:

1. Open PowerShell as Admin
2. Shut down WSL
    ```
    wsl --shutdown
    ```
3. Get WSL distro disk file (e.g., Ubuntu) location 
    ```
    (Get-ChildItem -Path $env:LOCALAPPDATA\Packages\*Ubuntu*\LocalState\ext4.vhdx -Recurse).FullName
    ```
4. Use the built-in disk management tool
    ```
    diskpart
    ```
5. Select the `vhdx` file (use the full path from 3.) and compact the disk space
    ```
    select vdisk file="C:\...\ext4.vhdx"
    attach vdisk readonly
    compact vdisk
    detach vdisk
    exit
    ```