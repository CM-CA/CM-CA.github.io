---
title: Técnica de Geolocalizacion e informe de ejemplo.
date: 2024-06-29 17:00 +/-0000
categories: OSINT Técnicas
tags: 
author: okud4
toc: true
img_path: /assets/img/
image: /banners/investigator2-banner.jpeg
published: "false"
---

## Aviso Legal: Fines Éticos y Legales

Todo el contenido y las acciones realizadas en esta plataforma tienen estrictamente fines éticos y legales. La información proporcionada ha sido obtenida de manera legal y de fuentes abiertas y públicas. Nos comprometemos a actuar de manera responsable, respetuosa y conforme a las normas y principios éticos establecidos. Nuestro objetivo es proporcionar información precisa y útil, fomentando siempre el bienestar y el respeto hacia todas las personas.

## INTRODUCCIÓN

En este post se explicará la técnica utilizada para geolocalizar la ubicación de una imagen. Como todo, requiere su tiempo para poder obtener un resultado bastante fiable.

La idea es tratar el siguiente reto como si fuese un caso real, añadiendo un poco de emoción al asunto. Por la mañana, llega una imagen a través del servicio de mensajería con el siguiente texto: "Hemos conseguido la imagen de una posible localización de donde se encuentran escondidos. Rei."

Manos a la obra...

## IMAGEN

Antes de nada, se procede a analizar la imagen a fondo, por si posee algún mensaje oculto y así revisar los metadatos que pueda contener. Para ello, se utiliza la web [Aperi Solve](https://aperisolve.com). Esta web muestra todos los datos que contiene la imagen, división por capas RGB, etc.

Tras comprobar que no tiene nada oculto, se descarga la imagen en el equipo y se sube a [Any Run](https://app.any.run). Aquí se comprueba que no tenga ningún código malicioso que pueda ejecutarse en el sistema operativo. Tras realizar todas las comprobaciones y no encontrar nada sospechoso, se pasa a la siguiente fase.

### 1. Primer análisis de la imagen

Este paso consiste en observar la imagen y tomar nota de los puntos de referencia que más llamen la atención a simple vista. No es necesario ser exhaustivo; en este caso, se toma la siguiente descripción breve:

```Markdown
Se aprecia un mar de fondo, gran cantidad de árboles y un conjunto de edificaciones que poseen el mismo estilo. Posible urbanización. Flores en una de las entradas. Farola decorativa.
```

Esto ya ayuda bastante para el siguiente paso.

### 2. Descomponer imagen

Aquí se abre la imagen con un editor de fotos (Gimp, Photoshop, etc.) y se analiza más a fondo. Se recortan los elementos más característicos observados, pero no lo primero que se ve, sino que se analiza la imagen en detalle. Una farola, un árbol, una simple planta... pueden proporcionar buenas pistas en la investigación.

![Cala del rei](capturas/calarei/recortes.png)

### 3. Segundo análisis de la imagen

En este punto, se empiezan a usar las herramientas de búsqueda conocidas. La típica es usar Google Lens, pero también se puede utilizar Yandex. Ambas opciones son bastante buenas e incluso combinándolas se pueden obtener mejores resultados.

Se empieza a identificar uno a uno los recortes: la vegetación, objetos (farolas, tipo de piedra) y edificaciones. La búsqueda lleva a las siguientes conclusiones:

1. Zona mediterránea: La vegetación y las características de construcción son comunes en esta zona.
2. Urbanización: La estructura de las viviendas, el material de construcción, etc., son idénticos, lo que sugiere que se trata de una urbanización de cierto poder adquisitivo.

Ya se tiene algo por donde buscar... Urbanizaciones costeras en el Mediterráneo.

Se vuelve a buscar con la foto original en Yandex, que arroja una posible ubicación en Croacia, mientras que Google sugiere dos ubicaciones en España: Begur (Cataluña) y Baleares.

### 4. Localizaciones obtenidas

Se pueden buscar en las tres localizaciones anteriormente mencionadas, **PERO**... se puede acotar más la búsqueda gracias a una palmera que aparece en la imagen:

![Palmito](capturas/calarei/pamito.png)

Ahora se juega con la suerte. Sabemos que ambas localizaciones son viables, así que ahora toca buscar por las viviendas... La web de [Idealista.com](https://www.idealista.com) es una buena fuente de búsqueda y esta vez, la suerte estuvo de nuestro lado. Señala la venta de un chalet en Begur, así que ahora toca hacer turismo con Google Street View.

## Google Street View

La web no dejó muy lejos esta vez... La búsqueda comienza en una calle cercana, pero despista un poco debido a las viviendas que se encuentran. No parece corresponder al tipo de viviendas de la foto. Según se avanza, se observa la foto original y el mapa para encontrar similitudes en el paisaje y las viviendas (hay tomas que llevan sin actualizarse 10 años) hasta que hay suerte y se encuentra una referencia: el monte.

Se avanza un poco más hasta llegar a un cruce y... ¡BINGO!

![imagen_ok](capturas/calarei/imagen_ok.png)

![imagen2_ok](capturas/calarei/imagen2_ok.png)

Podría darse por finalizada la investigación...

## Más datos

Se sigue investigando, sobre todo en tres chalets que llamaron la atención.

![Xalet del Rei](capturas/calarei/loc_inv.png)

Lo primero que se busca es ver las posibles conexiones a internet que poseen. Esto se hace en la web [Wigle](https://www.wigle.net) y se guarda la información.

![bssid](capturas/calarei/bssid_chalets.png)

El resto de la información se puede obtener de [IDEE](https://www.idee.es).

Aquí se obtuvo el catastro de los chalets:

![imagen_ok](capturas/calarei/catastro_parcela.png)

Y sobre todo, la información del que realmente interesaba:

![Xalet del Rei](capturas/calarei/xalet_rei.png)

![imagen_ok](capturas/calarei/catastro_rei.png)

![imagen_ok](capturas/calarei/catastro2_rei.png)