# Remote Access to Windows Server 2025

## Remote Connection with PowerShell

To connect remotely using **PowerShell** to a Windows Server 2025 server, the standard method is **WinRM** (*Windows Remote Management*) using the `Enter-PSSession` cmdlet.

---

### 1. Preparation on the Server (Windows Server 2025)

In Windows Server 2025, remote management is usually enabled by default. To verify or enable it manually, run the following command in a PowerShell console with **Administrator** permissions on the server:

```powershell
Enable-PSRemoting -Force
```

---

### 2. Connection from the Client Computer

#### Case A: If the server and client are in the same domain (Active Directory)

Open PowerShell on your client computer and run:

```powershell
Enter-PSSession -ComputerName "NomDelServidor" -Credential (Get-Credential)
```

> **Note:** Enter the credentials of a user with administrator permissions on the server when prompted.

#### Case B: If there is no domain (Workgroup)

When Active Directory is not used, you must add the server address to the Trusted Hosts (*TrustedHosts*) list on your client computer.

1. On your client computer, open PowerShell as **Administrator** and run:
   ```powershell
   Set-Item WSMan:\localhost\Client\TrustedHosts -Value "IP_DEL_SERVIDOR" -Concatenate -Force
   ```
2. *(Optional)* You can replace `"IP_DEL_SERVIDOR"` with `"*"` to trust any computer on the local network.
3. Connect by specifying the server IP:
   ```powershell
   Enter-PSSession -ComputerName "IP_DEL_SERVIDOR" -Credential "IP_DEL_SERVIDOR\Administrador"
   ```