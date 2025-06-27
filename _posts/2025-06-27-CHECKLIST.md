---
title: Checklist para Active Directory.
date: 2025-06-27 10:30 +/-0000
categories: Windows RedTeam
tags:
  - windows
  - redteam
  - activedirectory
author: okud4
toc: true
img_path: /assets/img/
image: /banners/adcheck-banner.jpg
---


## 🕵️ 1️⃣ Reconocimiento Inicial

- [ ] 🔍 **Escaneo de puertos**  
  Puertos típicos: `53`, `88`, `135`, `389`, `445`, `636`, `3268`, `5985`, `9389`

- [ ] 📛 **Descubrir el dominio AD (FQDN)**

- [ ] 🎭 **Enumeración NetBIOS/SMB**  
  Herramientas: `smbmap`, `enum4linux-ng`, `crackmapexec smb`

- [ ] 🌐 **Enumeración DNS**  
  Zone transfers con: `dig axfr`

- [ ] 📂 **Enumeración LDAP**  
  Herramientas: `ldapsearch`, `ldapdomaindump`

## 👤 2️⃣ Enumeración de Usuarios

- [ ] **Kerberos User Enum**  
  `kerbrute userenum`

- [ ] **AS-REP Roasting**  
  `GetNPUsers.py` (Impacket)

- [ ] **Kerberoasting**  
  `GetUserSPNs.py`

- [ ] **Password Spray**  
  `crackmapexec` con listas de passwords comunes

## 📁 3️⃣ Shares (SMB)

- [ ] **Descubrir shares**  
  `smbclient`, `smbmap`

- [ ] **Descargar SYSVOL / NETLOGON**

- [ ] **Buscar passwords en GPP**  
  (`Groups.xml`, cpassword)

- [ ] **Buscar scripts, backups o ficheros sensibles**

## 🩻 4️⃣ BloodHound (Mapeo de privilegios)

- [ ] **Recolección con BloodHound-python**

- [ ] **Importación en GUI BloodHound CE**

- [ ] **Queries esenciales**:
  - Shortest Paths to Domain Admin
  - Shortest Paths to High Value Targets
  - Users with GenericAll / GenericWrite
  - Users who can RDP / PSRemote
  - GPOs that can be modified
  - Shadow Admins
  - DCSync rights

## 🗡️ 5️⃣ Explotación de ACLs

- [ ] **CanAddMember → subir a grupo privilegiado**

- [ ] **GenericWrite → modificar objetos / cuentas**

- [ ] **Shadow Credentials / PKINIT**  
  Herramienta: `certipy`

- [ ] **GPO Abuse**  
  Herramienta: `SharpGPOAbuse` o `PowerView`

- [ ] **ForceChangePassword**

## 🎮 6️⃣ Acceso Remoto / RCE

- [ ] **WinRM (PowerShell remoto)**  
  Herramienta: `evil-winrm`

- [ ] **SMBExec / PSExec**  
  Herramienta: `psexec.py`, `wmiexec.py`

- [ ] **RDP**  
  Herramientas: `rdesktop`, `xfreerdp`, `mstsc`

## 🚀 7️⃣ Privilege Escalation & Persistence

- [ ] **Local Privilege Escalation**  
  Herramientas: `WinPEAS`, `PowerUp`

- [ ] **Dump de hashes**  
  Herramienta: `secretsdump.py`

- [ ] **Pass-the-Hash**  
  Herramientas: `psexec.py`

- [ ] **Pass-the-Ticket (Kerberos)**  
  Uso de ticket Kerberos TGT/CCACHE

- [ ] **DCSync Attack**  
  → Dump de todos los hashes del dominio

## 🏅 8️⃣ Golden Ticket & Domain Dominance

- [ ] **Golden Ticket Attack**  
  Herramienta: `ticketer.py`

- [ ] **Persistence avanzada**  
  - ACL abuse  
  - GPO malicioso  
  - SIDHistory

## 🛠️ Herramientas Esenciales

| Herramienta          | Uso principal |
|----------------------|----------------|
| BloodHound           | Mapeo de privilegios |
| BloodHound-python    | Recolección remota |
| Certipy              | Shadow Credentials |
| BloodyAD             | Abuso de ACLs |
| Crackmapexec         | Spray/Enum SMB/LDAP/Kerberos |
| Kerbrute             | Enum usuarios |
| Impacket             | Suite completa (SMB, Kerberos, secretsdump) |
| Evil-winrm           | Shell remota |
| PowerView            | Recon/abuso en PowerShell |
| SharpHound.ps1       | Recolección (si tienes RCE) |
| WinPEAS              | Priv Esc local |
| Secretsdump.py       | Dump de hashes |

## 🧠 Filosofía de ataque en Active Directory

✅ Siempre busca *paths de escalada de privilegios*, no solo DA  
✅ Cada objeto en AD puede ser un vector de abuso (grupos, ACL, GPO)  
✅ Grupos "secundarios" como *SERVICE ACCOUNTS*, *HelpDesk*, *IT Support* suelen ser claves  
✅ El abuso de ACL es el presente → No dependas solo de credenciales  
✅ DCSync es uno de los objetivos más potentes: controla el dominio
