---
title: ¿Como descargar la ultima release de un repositorio de Github?
date: 2023-12-29 17:00 +/-0000
categories: Scripts
tags:
  - scripts
  - github
author: okud4
toc: true
---

A la hora de descargar la última release, podemos utilizar el siguiente #scripts lo que nos facilitará la vida :)

- Usando `curl` obtenemos el JSON de la ultima release.
- Usa `grep` para encontrar la linea que contiene la URL
- Usa `cut` y `tr` para extraer la URL
- Usa `wget` para descargar

```bash
curl -s https://api.github.com/repos/jgm/pandoc/releases/latest \
| grep "browser_download_url.*deb" \
| cut -d : -f 2,3 \
| tr -d \" \
| wget -qi -
```

No olvides marcar con una ⭐ la [Fuente](https://gist.github.com/steinwaywhw/a4cd19cda655b8249d908261a62687f8) original.
