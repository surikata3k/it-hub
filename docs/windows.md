### Check disk
winsat disk -drive F

### Edit network
ncpa.cpl

### View folder of current wallpaper
%AppData%\Microsoft\Windows\Themes\CachedFiles

### Right menu with all options
reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve

### Install skipping microsoft user
Pres Shit + F10

Type: start ms-cxh:localonly
OR
Type: OOBE\BYPASSNRO

### Mouse disable wake up from sleep
1. Find the exact name of your mouse by running this command:

```dos
powercfg -devicequery wake_armed
```

2. Disable the wake capability by running the following command (replace `DeviceName` with the name you copied):

```dos
powercfg -devicedisablewake "DeviceName"
```
* **Example:** `powercfg -devicedisablewake "HID-compliant mouse"`
