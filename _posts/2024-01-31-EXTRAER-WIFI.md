---
title: Extraer Clave Wifi
date: 2024-01-31 22:00 +/-0000
categories: Windows
tags:
  - scripts
  - windows
  - python
author: okud4
toc: true
---
Con este script podemos obtener la clave wifi a la que el equipo se encuentra conectado. Funcionará únicamente en Windows, ya que el sistema de almacenamiento en en totalmente distinto en Unix y Linux.

El script puede convertirse en un ejecutable **.exe** y requiere permisos de administrador para poder ejecutar **netsh**.

````python
import subprocess

import re

  

# Obtener el nombre de la red Wi-Fi a la que está conectado el equipo

result = subprocess.run(['netsh', 'wlan', 'show', 'interfaces'], capture_output=True, text=True)

output = result.stdout

# Utilizar expresiones regulares para encontrar el nombre de la red

match = re.search(r'SSID\s+: (.+)', output)

if match:

  ssid = match.group(1).strip()

else:

  ssid = None

if ssid:

  # Obtener la contraseña de la red Wi-Fi
  result = subprocess.run(['netsh', 'wlan', 'show', 'profile', 'name=' + ssid, 'key=clear'], capture_output=True, text=True)

  output = result.stdout

  print(output)

else:

  print("No se pudo determinar el nombre de la red Wi-Fi a la que está conectado el equipo.")

`````
