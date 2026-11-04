# SSH from Windows 11 to Ubuntu

How to set up an SSH server on Ubuntu and connect to it from Windows 11, including key-based login.

## 1. On Ubuntu: install and start the SSH server

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh        # should say "active"
```

If the UFW firewall is on, allow SSH:

```bash
sudo ufw allow ssh
```

Find the machine's IP address:

```bash
ip a          # look for "inet 192.168.x.x" on your network interface
```

!!! note "Ubuntu in a virtual machine"
    A NAT network adapter isn't reachable from the host. Set the adapter to **Bridged** so the VM gets an IP on your LAN, or set up port forwarding (for example host port 2222 → guest port 22).

## 2. On Windows 11: connect

Windows 11 already includes the OpenSSH client. Open PowerShell or Windows Terminal and run:

```powershell
ssh user@192.168.1.50
```

The first time, answer `yes` to accept the host fingerprint, then type the Ubuntu user's password.

If `ssh` isn't recognized, add it under **Settings → System → Optional features → OpenSSH Client**.

## 3. Use key authentication instead of passwords

Generate a key pair on Windows:

```powershell
ssh-keygen -t ed25519
```

Press Enter to accept the default path (`C:\Users\<you>\.ssh\id_ed25519`). Adding a passphrase is optional.

Windows has no `ssh-copy-id`, so copy the public key over with this one-liner:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh user@192.168.1.50 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

Run `ssh user@192.168.1.50` again and it should log you in without asking for a password.

## 4. Optional: a shortcut in `~/.ssh/config`

Create the file `C:\Users\<you>\.ssh\config` (no file extension):

```
Host ubuntu
    HostName 192.168.1.50
    User user
    IdentityFile ~/.ssh/id_ed25519
    # Port 2222   # only if you use port forwarding or a non-standard port
```

After that, you only need to type:

```powershell
ssh ubuntu
```

VS Code's Remote-SSH extension also reads this file.
