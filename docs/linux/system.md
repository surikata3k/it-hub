---
description: "Linux system administration snippets: systemd services, iptables rules, GRUB, lid-close behaviour and more."
---

# System

## Services

List all services:

```bash
systemctl list-unit-files --type service --all
```

## Firewall: iptables rules

```bash
sudo iptables -L --line-numbers
```

## Show the IP before login

Adds the IP address to the login screen message:

```bash
echo 'IP: \4' | sudo tee -a /etc/issue
```

## Keep running when the laptop lid is closed

```ini title="/etc/systemd/logind.conf"
HandleLidSwitch=ignore
```

Apply the change:

```bash
sudo systemctl restart systemd-logind
```

## GRUB

```ini title="/etc/default/grub"
GRUB_TIMEOUT=0
GRUB_CMDLINE_LINUX="consoleblank=60"
```

Apply the changes:

```bash
sudo update-grub
```

!!! info
    `GRUB_TIMEOUT=0` skips the boot menu. `consoleblank=60` turns off the console screen after 60 seconds.

## Open a graphical editor from the terminal

On Linux Mint the default text editor is `xed`:

```bash
xed file.txt
```
