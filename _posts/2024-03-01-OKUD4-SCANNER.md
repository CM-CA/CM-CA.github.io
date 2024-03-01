---
title: AutoScanner
date: 2024-03-01 17:30 +/-0000
categories: Herramientas, Scanner
tags:
  - autoscanner
  - vulnerabilidades
  - web
author: okud4
toc: true
img_path: /assets/img/
image: /banners/blacknoir-banner.png
---

Hoy compartiré un script que me facilita la búsqueda de vulnerabilidades en aplicaciones web.

Se puede introducir tanto una dirección _IP_ como una _URL_. En caso de introducir una URL, el script realiza un **nslookup** para obtener la _IP_ y trabajar con ella.

Este escáner realiza las siguientes tareas:

1. Escaneo de puertos mediante **Nmap**.
2. Búsqueda de directorios con **Dirb** (da opción a usar http:// o https://).
3. Búsqueda de subdominios con **ffuz** (da opción a usar http:// o https://).
4. Búsqueda de vulnerabilidad **LFI** y **RFI**.

Tiene varias opciones de ejecución; se puede lanzar todas las tareas o elegir una a una.

Crea un directorio y toma como nombre la IP que vamos a analizar. Dentro tendrás subdirectorios con toda la información obtenida.

Podéis descargarlo desde mi [repositorio de Github](https://github.com/CM-CA/Okud4-Scan)
