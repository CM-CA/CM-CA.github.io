---
title: INVESTIGACIÓN OSINT (RETO)
date: 2024-02-08 17:30 +/-0000
categories: OSINT Retos
tags:
  - osint
  - reto
  - investigación
author: okud4
toc: true
img_path: /assets/img/
image: /banners/investigator-banner.jpg
---

Hace unos días se nos propuso un reto de OSINT en el que consistía en verificar si una foto era real o generada por IA, además de poder obtener ciertas pistas si es que se pueden obtener:

- Ubicar la foto.
- Matrícula
- Detalles.
- La señora.
- El señor.

# Informe de un migrante subido a un coche fúnebre

## Resumen Ejecutivo

Se presenta la imagen de una persona de color subida a un coche fúnebre y solicitan saber si la foto es auténtica o generada por IA.

Los hechos de la imagen ocurrieron el pasado 2 de febrero en Totana (Murcia), en cuyo caso, como veredicto, podemos asegurar que es **real**.

## Objetivos de la Investigación

Comprobar si la foto es real o generada por IA.

Geolocalizar en donde ocurrieron los hechos.

## Metodología

- Para comprobar si la foto ha sido generada o manipulada por IA, utilizo de      forma la herramienta [Hive Moderation](https://hivemoderation.com) la cual arroja el resultado de que la imagen no fue manipulada ni creada por IA.

   ![negro-funebre](capturas/totana/image.png)

## Hallazgos

### Investigación sobre los hechos

- Los hechos se produce entorno a las 10:00 en frente de la Iglesia Parroquial de Santiago (**Totana**, Murcia).

  ![negro coche](capturas/totana/image-4.png)

- El servicio funerario pertenece a la familia **CAYUELA**.

 ![esquela](capturas/totana/isabel.png)

- La matricula es **9981HCY**.
  
  1. ![matricula-2](capturas/totana/image-7.png)
  2. ![matricula-3](capturas/totana/image-10.png)

- Características de el vehículo.

  ![caracteristicas](capturas/totana/image-8.png) ![car-2](capturas/totana/image-9.png)

- Según la información obtenida en [Maldita Migración](https://maldita.es/migracion/bulo/20240206/migrante-coche-funebre-totana-murcia/) el vehículo pertenece al tanatorio ***"Pichirichi".***

- Citando textualmente el contenido final de la noticia, nos indica que el hombre se encuentra ingresado.

> el ayuntamiento dice que el hombre fue **"trasladado por servicios sociales"** a un **centro de salud mental**. La Iglesia de Santiago el Mayor concreta que les **"consta que está ingresado"** en el **hospital Rafael Méndez de Lorca** (Murcia).

### Localización exacta

- Foto de el lugar de los hechos. Con respecto a la foto original, podemos apreciar que es la misma farola y misma papelera, el diseño de los edificios, los soportales con cristaleras y el balcón con el ventanal verde  coinciden al 100% con la foto original. Si es verdad que lo que no coincide, es la señalización que se encuentra en la farola, actualmente no existe. El edificio en cuestión es el **Museo Torre Totana.**
    ![lugar funebre](capturas/totana/image-3.png)

- Fotos tomadas desde la calle Plaza de la Constitución.
    ![foto desde farmacia](capturas/totana/image-2.png)

- Con cierta exactitud, podría haber sido tomada desde esta farmacia.
    ![farmacia](capturas/totana/image-1.png)

## Análisis

Tras finalizar ambas líneas de investigación y comprobar que los hechos concuerdan con las fotos obtenidas, concluimos que la foto es **REAL**.

## Limitaciones

Actualmente me resultó imposible comprobar a que funeraria pertenece el vehículo por no tener acceso a la DGT.

## Referencias

[Newtral](https://www.newtral.es/hombre-coche-funebre-totana-nos-preguntais-por/20240205/)

[Google Maps](https://www.google.com/maps/)

[CarFax EU](https://www.carfax.eu/es/comprobar-matricula)

[Esquelas Totana](https://esquelastotana.blogspot.com)

[Maldita Migración](https://maldita.es/migracion/bulo/20240206/migrante-coche-funebre-totana-murcia/)

## Acrónimos y Abreviaturas

**DGT**: Dirección General de Tráfico.
