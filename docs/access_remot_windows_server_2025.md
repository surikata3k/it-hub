# Accés remot a Windows Server 2025

## Connexió remota amb PowerShell

Per connectar-te de manera remota mitjançant **PowerShell** a un servidor Windows Server 2025, el mètode estàndard és **WinRM** (*Windows Remote Management*) utilitzant el cmdlet `Enter-PSSession`.

---

### 1. Preparació al servidor (Windows Server 2025)

A Windows Server 2025 la gestió remota acostuma a estar activada per defecte. Per verificar-ho o habilitar-ho manualment, executa la següent ordre en una consola de PowerShell amb permisos d'**Administrador** dins del servidor:

```powershell
Enable-PSRemoting -Force
```

---

### 2. Connexió des de l'equip client

#### Cas A: Si el servidor i el client estan al mateix domini (Active Directory)

Obre PowerShell al teu equip client i executa:

```powershell
Enter-PSSession -ComputerName "NomDelServidor" -Credential (Get-Credential)
```

> **Nota:** Introdueix les credencials de l'usuari amb permisos d'administrador al servidor quan se't sol·licitin.

#### Cas B: Si no hi ha domini (Grup de treball / Workgroup)

Quan no s'utilitza Active Directory, cal afegir l'adreça del servidor a la llista de Hosts de Confiança (*TrustedHosts*) del teu equip client.

1. Al teu equip client, obre PowerShell com a **Administrador** i executa:
   ```powershell
   Set-Item WSMan:\localhost\Client\TrustedHosts -Value "IP_DEL_SERVIDOR" -Concatenate -Force
   ```
2. *(Opcional)* Pots substituir `"IP_DEL_SERVIDOR"` per `"*"` per confiar en qualsevol equip de la xarxa local.
3. Connecta't indicant la IP del servidor:
   ```powershell
   Enter-PSSession -ComputerName "IP_DEL_SERVIDOR" -Credential "IP_DEL_SERVIDOR\Administrador"
   ```