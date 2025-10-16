---
title: GUÍA PARA POWERSHELL
date: 2025-10-16 13:30 +/-0000
categories: Windows Guia
tags:
  - windows
  - guia
  - investigación
author: okud4
toc: true
img_path: /assets/img/
image: /banners/investigator-banner.jpg
---



# Guía Rápida de PowerShell

Referencia compacta en español con comandos listos para copiar/pegar. Incluye: básicos, gestión de usuarios/grupos, archivos/directorios, registro, búsqueda y filtrado, logs/eventos, información del sistema, búsqueda de host, ver archivos ocultos, búsqueda recursiva de archivos con contenido y técnicas para investigar módulos PowerShell (CTF/pentesting).

---

## 1. Comandos básicos e importantes

* `Get-Help <cmd>` — ayuda. Ej: `Get-Help Get-Process -Full`
* `Get-Command` — listar comandos disponibles.
* `Get-History` — historial de comandos.
* `Clear-Host` o `cls` — limpiar pantalla.
* Alias comunes: `ls` -> `Get-ChildItem`, `cd` -> `Set-Location`, `cat` -> `Get-Content`.

---

## 2. Archivos y directorios

* Listar: `Get-ChildItem -Path C:\Ruta`
* Cambiar directorio: `Set-Location C:\Windows`
* Crear carpeta: `New-Item -ItemType Directory -Name "Logs"`
* Crear archivo: `New-Item file.txt -ItemType File`
* Copiar: `Copy-Item file.txt D:\Backup\`
* Mover: `Move-Item file.txt D:\`
* Eliminar: `Remove-Item file.txt`
* Ver contenido: `Get-Content .\log.txt`
* Añadir contenido: `Add-Content .\log.txt "texto"`

---

## 3. Usuarios y grupos (locales)

*Se recomienda ejecutar PowerShell como Administrador.*

* Listar usuarios: `Get-LocalUser`
* Crear usuario: `New-LocalUser "juan" -Password (Read-Host -AsSecureString)`
* Habilitar/Deshabilitar: `Enable-LocalUser juan` / `Disable-LocalUser juan`
* Eliminar: `Remove-LocalUser juan`
* Grupos: `Get-LocalGroup`, `Add-LocalGroupMember -Group "Administrators" -Member "juan"`, `Remove-LocalGroupMember -Group "Administrators" -Member "juan"`.

---

## 4. Registro de Windows (Registry)

* Ver clave/valor: `Get-ItemProperty "HKLM:\SOFTWARE\MiClave"`
* Crear clave: `New-Item -Path HKCU:\Software\MiApp`
* Crear/Modificar valor: `New-ItemProperty` / `Set-ItemProperty`
* Eliminar valor/claves: `Remove-ItemProperty` / `Remove-Item -Recurse`.

**Ejemplo para CTF** — obtener `RegisteredOwner` (flag típico):

```powershell
(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").RegisteredOwner
```

O con `reg query`:

```powershell
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v RegisteredOwner
```

---

## 5. Información del sistema

* `Get-ComputerInfo` — información completa: WindowsVersion, BIOS, memoria, etc.
* Nombre del equipo: `hostname` o `$env:COMPUTERNAME`
* FQDN: `[System.Net.Dns]::GetHostEntry([System.Net.Dns]::GetHostName()).HostName`
* Uptime: `(Get-Date) - (Get-CimInstance Win32_OperatingSystem).LastBootUpTime`
* CPU: `Get-CimInstance Win32_Processor`
* RAM física: `Get-CimInstance Win32_PhysicalMemory | Select Manufacturer, Capacity`
* Discos: `Get-PSDrive` o `Get-CimInstance Win32_LogicalDisk`
* Interfaces/red: `Get-NetIPAddress`, `Get-NetAdapter`, `ipconfig /all`
* Procesos: `Get-Process`
* Servicios: `Get-Service`
* Hotfixes: `Get-HotFix`
* Programas instalados: `Get-WmiObject -Class Win32_Product` (lento)

---

## 6. Búsqueda y filtrado

* Filtrar: `Where-Object`
  `Get-Process | Where-Object {$_.CPU -gt 100}`
* Seleccionar columnas: `Select-Object`
* Ordenar: `Sort-Object`
* Buscar texto en archivos: `Select-String -Pattern "Error" -Path .\*.log -Recurse`
* Buscar archivos: `Get-ChildItem -Recurse -Filter "*.log"`

**Ejemplo — encontrar archivos no vacíos (útil para localizar un único .txt con contenido):**

```powershell
Get-ChildItem "C:\Users\TuUsuario\Documents" -Recurse -File |
  Where-Object { $_.Length -gt 0 } |
  Select-Object FullName, Length, LastWriteTime
```

---

## 7. Ver archivos ocultos

* Mostrar todos (incluye ocultos y system): `Get-ChildItem -Path C:\Ruta -Force`
* Filtrar solo ocultos:

```powershell
Get-ChildItem -Path C:\Ruta -Recurse -Force |
  Where-Object { ($_.Attributes -band [System.IO.FileAttributes]::Hidden) -ne 0 }
```

* Ocultos + system:

```powershell
Where-Object { ($_.Attributes -band [System.IO.FileAttributes]::Hidden) -ne 0 -or ($_.Attributes -band [System.IO.FileAttributes]::System) -ne 0 }
```

* Mostrar en Explorer (HKCU):

```powershell
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" -Name Hidden -Value 1
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" -Name ShowSuperHidden -Value 1
Stop-Process -Name explorer -Force; Start-Process explorer
```

---

## 8. Logs y eventos del sistema

* Listar logs: `Get-EventLog -List`
* Ver últimos eventos: `Get-EventLog -LogName System -Newest 20`
* Filtrar tipo: `Get-EventLog -LogName Application -EntryType Error`
* Nuevo sistema: `Get-WinEvent -LogName Security -MaxEvents 50`
* Filtrar por Id: `Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625}`
* Exportar: `Get-EventLog System -Newest 100 | Export-Csv system.csv`

---

## 9. Buscar nombre del host (local y remoto)

**Local:**

* `hostname`
* `$env:COMPUTERNAME`
* FQDN: `[System.Net.Dns]::GetHostEntry([System.Net.Dns]::GetHostName()).HostName`
* Otra opción:

```powershell
$cs = Get-CimInstance Win32_ComputerSystem
"$($cs.DNSHostName).$($cs.Domain)"
```

**Remoto o IP -> nombre:**

* `Resolve-DnsName -Name "1.2.3.4" -Type PTR`
* `nslookup 1.2.3.4`
* `.NET`: `[System.Net.Dns]::GetHostEntry("1.2.3.4").HostName`
* NetBIOS: `nbtstat -A 192.168.1.10`
* AD: `Get-ADComputer -Identity "NOMBRE_PC" -Properties DNSHostName` (requiere módulo ActiveDirectory y permisos)

---

## 10. Inspección de módulos PowerShell (CTF / pentesting)

**Objetivo típico:** encontrar flags/credenciales/keys dentro de módulos (.psm1/.psd1/.ps1/.dll) e intentar autenticarse en otro host (ej. DC via SSH).

### A. Enumerar módulos y rutas

```powershell
Get-Module -ListAvailable | Select-Object Name, Version, ModuleType, Path
```

### B. Búsqueda textual rápida en todos los módulos

```powershell
Get-Module -ListAvailable | ForEach-Object {
  $m = $_
  if (Test-Path $m.Path) {
    Get-ChildItem -Path $m.Path -Recurse -File -ErrorAction SilentlyContinue |
      Select-String -Pattern 'HTB\{.*?\}|FLAG\{.*?\}|BEGIN RSA PRIVATE KEY|ssh-rsa|password|passwd|credential' -CaseSensitive:$false -AllMatches |
      ForEach-Object {
        [PSCustomObject]@{
          Module    = $m.Name
          Path      = $_.Path
          Line      = $_.Line.Trim()
          Match     = ($_.Matches | ForEach-Object { $_.Value }) -join '; '
          LineNumber= $_.LineNumber
        }
      }
  }
} | Format-Table -AutoSize
```

### C. Buscar solo en archivos de texto (más rápido)

```powershell
Get-Module -ListAvailable | ForEach-Object {
  $m = $_
  Get-ChildItem -Path $m.Path -Recurse -Include *.ps1,*.psm1,*.psd1,*.txt -File -ErrorAction SilentlyContinue |
    Select-String -Pattern 'HTB\{.*?\}|FLAG\{.*?\}|BEGIN RSA PRIVATE KEY|ssh-rsa|password|passwd|credential' -CaseSensitive:$false -AllMatches |
    ForEach-Object {
      [PSCustomObject]@{ Module=$m.Name; File=$_.Path; LineNumber=$_.LineNumber; Match=$_.Matches.Value; Line=$_.Line.Trim() }
    }
}
```

### D. Extraer cadenas de DLLs (equivalente a `strings`)

```powershell
function Find-StringsInBin { param($file)
  $bytes = [System.IO.File]::ReadAllBytes($file)
  $text = [System.Text.Encoding]::ASCII.GetString($bytes)
  if ($text -match 'HTB\{.*?\}|BEGIN RSA PRIVATE KEY|ssh-rsa') { ,([PSCustomObject]@{ File=$file; Matches = ($Matches[0]) }) }
}
Get-Module -ListAvailable | ForEach-Object {
  Get-ChildItem -Path $_.Path -Recurse -Include *.dll -File -ErrorAction SilentlyContinue |
    ForEach-Object { Find-StringsInBin $_.FullName } | Where-Object { $_ -ne $null }
}
```

### E. Leer recursos embebidos en un assembly (.dll)

```powershell
$dll = 'C:\ruta\al\module.dll'
$a = [System.Reflection.Assembly]::LoadFile($dll)
$a.GetManifestResourceNames()
foreach($r in $a.GetManifestResourceNames()){
  $s = $a.GetManifestResourceStream($r)
  if ($s) {
    $sr = New-Object System.IO.StreamReader($s)
    $content = $sr.ReadToEnd()
    if ($content -match 'HTB\{.*?\}|BEGIN RSA PRIVATE KEY|ssh-rsa') { Write-Output "Encontrado en recurso $r : $Matches[0]" }
  }
}
```

### F. Decodificar Base64 simple

```powershell
$enc = 'BASE64_STRING_AQUI'
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($enc))
```

### G. Guardar clave PEM y usar SSH

```powershell
$pemText = @"
-----BEGIN RSA PRIVATE KEY-----
...líneas...
-----END RSA PRIVATE KEY-----
"@
$path = "$env:TEMP\id_rsa_found.pem"
$pemText | Out-File -FilePath $path -Encoding ascii
ssh -i $path usuario@172.16.5.155
```

> Nota: si mueves la clave a Linux, aplicar `chmod 600 id_rsa_found.pem`.

### H. Comando único (todo-en-uno) que exporta resultados a CSV

```powershell
$results = Get-Module -ListAvailable | ForEach-Object {
  $m = $_
  if (Test-Path $m.Path) {
    Get-ChildItem -Path $m.Path -Recurse -File -ErrorAction SilentlyContinue |
      Select-String -Pattern 'HTB\{.*?\}|FLAG\{.*?\}|BEGIN RSA PRIVATE KEY|ssh-rsa|password|passwd|credential' -CaseSensitive:$false -AllMatches |
      ForEach-Object {
        [PSCustomObject]@{ Module=$m.Name; File=$_.Path; LineNumber=$_.LineNumber; Match= ($_.Matches | ForEach-Object{$_.Value} -join '; '); Line=$_.Line.Trim() }
      }
  }
}
$results | Export-Csv -Path "$env:TEMP\module_search_results.csv" -NoTypeInformation
```

---

## 11. Comandos útiles adicionales

* Servicios: `Get-Service`, `Start-Service`, `Stop-Service`
* Procesos: `Get-Process`, `Stop-Process -Name notepad`
* Ejecutar script: `.\script.ps1`
* Política de ejecución: `Set-ExecutionPolicy RemoteSigned`
* Guardar salida en archivo: `Get-Service | Out-File servicios.txt`

---

## 12. Consejos y buenas prácticas

* Usa tab para autocompletar.
* Encadena con `|` para filtrar y formatear.
* Captura errores con `-ErrorAction SilentlyContinue` o `try/catch`.
* Para búsquedas pesadas (C:\ recursivo) ejecuta con privilegios elevados y guarda resultados a CSV.
* En CTFs, protege claves privadas y no las compartas públicamente; redacta cuando pidas ayuda.

---

