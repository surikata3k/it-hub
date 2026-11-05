---
description: "Use SSH on Windows 10 and 11: create keys, copy them to Linux servers, the config file, scp and the OpenSSH server."
---

# SSH on Windows

Windows 10 and 11 include the OpenSSH client, so `ssh`, `scp` and `ssh-keygen` work in PowerShell.

## Create a key

```powershell
ssh-keygen -t ed25519
```

The keys are saved in `%USERPROFILE%\.ssh\` (`id_ed25519` is private, `id_ed25519.pub` is public).

## Copy the key to a Linux server

Windows has no `ssh-copy-id`, so do it in one line:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh user@192.168.1.2 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

## Config file

Save your hosts in `%USERPROFILE%\.ssh\config` and connect with just `ssh files`:

```text title="%USERPROFILE%\.ssh\config"
Host files
    HostName 192.168.1.2
    User user
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

## Copy files

```powershell
scp .\report.pdf files:/home/user/
scp files:/var/log/syslog .
```

## OpenSSH server on Windows

Run in PowerShell as **Administrator**:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```
