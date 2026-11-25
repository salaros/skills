---
name: wsl-bash
description: Prefer WSL2 Bash and Linux CLI tools over cmd.exe and PowerShell for all shell work on Windows — fetching URLs (curl/wget), text processing (sed/grep/awk/jq), file operations, archives, and scripting. Use whenever running shell commands on a Windows host, when the user mentions WSL, bash, curl, sed, or grep, or when a task would otherwise reach for PowerShell cmdlets like Invoke-WebRequest.
---

# WSL2 Bash over cmd.exe / PowerShell

On this Windows machine, always run shell work through WSL2 Bash instead of cmd.exe or PowerShell. Linux tools are preferred for everything: downloading, text processing, file manipulation, archives, and scripting.

## How to invoke

Wrap every command in WSL:

```
wsl.exe -e bash -lc '<command>'
```

- Use single quotes around the command; escape embedded single quotes as `'\''`.
- `-l` gives a login shell so PATH and installed tools resolve.
- For multi-line scripts, write the script to a file and run `wsl.exe -e bash -lc 'bash /mnt/c/path/to/script.sh'`.
- To target a specific distro: `wsl.exe -d <Distro> -e bash -lc '...'`. Otherwise use the default distro.

## Path translation

Windows and WSL see the same files at different paths:

- Windows → WSL: `C:\Users\foo\file.txt` → `/mnt/c/Users/foo/file.txt`
- Prefer `wslpath` over manual conversion: `wsl.exe -e wslpath -u 'C:\Users\foo\file.txt'` (Windows→Linux) and `wslpath -w /mnt/c/...` (Linux→Windows).
- Keep heavy I/O inside the WSL filesystem (`~` in WSL) when possible — `/mnt/c` is slow for many small files.

## Tool preferences

| Task | Use (in WSL) | Avoid |
|------|--------------|-------|
| Fetch URL | `curl -fsSL` / `wget` | `Invoke-WebRequest`, `Invoke-RestMethod`, PowerShell `curl` alias |
| Find text | `grep -rn` | `Select-String`, `findstr` |
| Replace text | `sed -i 's/old/new/g'` | PowerShell `-replace` |
| Column/field work | `awk`, `cut` | PowerShell object piping |
| JSON | `jq` | `ConvertFrom-Json` |
| List/find files | `ls`, `find` | `dir`, `Get-ChildItem` |
| Archives | `tar`, `zip`/`unzip`, `gzip` | `Compress-Archive`, `Expand-Archive` |
| Hashing | `sha256sum`, `md5sum` | `Get-FileHash` |
| Diff | `diff -u` | `Compare-Object`, `fc` |

## Rules

1. Never reach for PowerShell cmdlets or cmd built-ins when a standard Linux tool does the job — route through `wsl.exe -e bash -lc` instead.
2. Convert every Windows path in the command to its `/mnt/<drive>/...` form before passing it to bash.
3. Quote carefully: the command travels through the Windows shell first, then bash. Prefer single-quoted bash commands and double quotes inside them.
4. Line endings: files written by Windows tools may have CRLF. If a script or config misbehaves in WSL, run `dos2unix` (or `sed -i 's/\r$//'`) on it.
5. Long-running or interactive commands: add non-interactive flags (`-y`, `--no-input`) — there is no TTY interaction through `wsl.exe -e`.

## Fallback

Only if WSL is unavailable (check with `wsl.exe --status`; error or no default distro means unavailable):

1. Tell the user WSL2 wasn't found and that Windows-native tools will be used this time.
2. Then, and only then, fall back to PowerShell equivalents.
3. Never fall back silently.

## Windows-only exceptions

Some things legitimately require Windows tools — registry edits (`reg`), Windows services (`sc`, `Get-Service`), scheduled tasks (`schtasks`), MSI installs, COM/WMI queries. Use PowerShell for those, but keep any surrounding text processing of their output in WSL bash via piping or temp files.
