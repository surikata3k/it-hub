---
description: "Windows tips: disk benchmark, classic right-click menu in Windows 11, install without a Microsoft account and more."
---

# Windows tips

## Benchmark a disk

```powershell
winsat disk -drive F
```

## Open network connections

```powershell
ncpa.cpl
```

## Find the current wallpaper

Cached copy of the wallpaper (paste it in the File Explorer address bar):

```text
%AppData%\Microsoft\Windows\Themes\CachedFiles
```

## Classic right-click menu (Windows 11)

Shows all options without clicking "Show more options". Restart Explorer or sign out to apply:

```powershell
reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
```

## Install without a Microsoft account

During setup, press ++shift+f10++ to open a command prompt and type one of these:

```text
start ms-cxh:localonly
```

```text
OOBE\BYPASSNRO
```

## Stop the mouse from waking the PC

1. Find the exact name of your mouse by running this command:

    ```bat
    powercfg -devicequery wake_armed
    ```

2. Disable the wake capability (replace `DeviceName` with the name you copied):

    ```bat
    powercfg -devicedisablewake "DeviceName"
    ```

!!! example
    `powercfg -devicedisablewake "HID-compliant mouse"`
