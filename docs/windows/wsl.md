---
description: "WSL cheat sheet: manage, export, import and remove distros and users in the Windows Subsystem for Linux."
---

# WSL

## Manage distros

| Action | Short | Long |
|---|---|---|
| List installed distros | `wsl -l` | `wsl --list` |
| List with status and WSL version (1 or 2) | `wsl -l -v` | `wsl -l --verbose` |
| Run a distro | `wsl -d distro_name` | `wsl --distribution distro_name` |
| Shut down a distro | `wsl -t distro_name` | `wsl --terminate distro_name` |
| Shut down all distros | | `wsl --shutdown` |
| Set the default distro | `wsl -s distro_name` | `wsl --set-default distro_name` |

## Users

Run a distro as a specific user:

```powershell
wsl -u username -d distroname
wsl -u root -d Ubuntu-20.04
```

Change the default user of a distro:

```powershell
distributionName config --default-user Username
ubuntu config --default-user my_default_username
ubuntu2004.exe config --default-user johndoe  # Ubuntu 20.04 installed from the Microsoft Store
```

## Export and import

Export a distro as an image:

```powershell
wsl --export distro_name windows_path\tar_file_name.tar
```

Import an image as a new distro:

```powershell
wsl --import new_distro_name install_location_windows_path tar_file_name.tar --version 2
```

!!! example
    Install the new distro on a second disk:

    ```powershell
    wsl --import Ubuntu-20 D:\VMs\WSL\Ubuntu-20\ Ubuntu-20.04.tar --version 2
    ```

## Remove a distro

!!! danger
    `--unregister` also deletes all the files of the distro.

```powershell
wsl --unregister distro_name
```
