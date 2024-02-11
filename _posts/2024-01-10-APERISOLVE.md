---
title: "Aperi'Solve"
date: 2024-01-10 14:20 +/-0000
categories: Herramientas Esteganografía
tags:
  - tools
  - imagenes
  - esteganografia
author: okud4
toc: true
---

AperiSolve es una herramienta que ns simplificará mucho el trabajo a la hora de tratar con imagenes, ya que nos permitirá poder obtener toda la informacion oculta que contengan.

Su uso es bastante sencillo, podemos utilizarlo via [web](https://www.aperisolve.com/) o instalando el cliente en nuestro SO.

```bash
sudo sh -c"$(curl -fs https://www.aperisolve.com/install.sh)"
aperisolve image.png
```

AperiSolve soporta los principales tipos de imagen: `.png`, `.jpg`, `.gif`, `.bmp`, `.jpeg`, `.jfif`, `.jpe`, `.tiff`.

La herramienta nos permite realizar las siguientes tareas:

- Visualiza cada capa de bits de cada canal para una imagen dada (es decir, el bit menos significativo del canal Rojo). Navega y descarga cada imagen de capa de bits.
- Visualiza información de zsteg, como texto codificado en el bit menos significativo (LSB). Descarga archivos zsteg, como mp3 codificados en LSB.
- Descarga archivos steghide utilizando una contraseña definida. Descarga archivos outguess utilizando una contraseña definida.
- Visualiza información de exiftool, como la geolocalización o el autor. Visualiza información de binwalk.
- Descarga archivos binwalk, como archivos zip en encabezados png. Descarga archivos foremost, como archivos zip en encabezados png.
- Visualiza la salida de cadenas (strings).

Si deseais colaborar con el proyecto, no olvideis visitar su [repositorio](https://github.com/Zeecka/AperiSolve).
