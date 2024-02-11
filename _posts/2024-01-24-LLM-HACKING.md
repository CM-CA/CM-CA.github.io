---
title: Hackeando a la IA
date: 2024-01-24 16:30 +/-0000
categories: Portswigger LLM
tags:
  - burpsuite
  - llm
  - hacking
author: okud4
toc: true
---

# **PORTSWIGGER.**

Hoy intentaremos averiguar qué vulnerabilidades podemos encontrar en la IA. Para ello, utilizaremos el nuevo laboratorio que nos ofrece de forma gratuita PortSwigger.

Este laboratorio está creado con un modelo LLM (Large Language Model). En este laboratorio, nos explicarán los distintos métodos de ataque que podemos realizar a un asistente virtual de una tienda en línea.

![](/assets/img/capturas/portswigger/principal.png)

# **INTEGRACIÓN DE IA EN LOS SITIOS WEB.**

La integración de la IA mediante asistentes virtuales en los sitios web debería aplicarse de forma escalonada. Las empresas se exponen a todo tipo de ataques si implementan dichos modelos de lenguaje de forma rápida y sin realizar las pruebas de seguridad pertinentes.

Al exponerse de esta manera, los atacantes pueden interactuar con la API y obtener datos de clientes, credenciales de acceso, etc. Un ataque puede:

1. Recuperar datos a los que el LLM tiene acceso. Las fuentes comunes de estos datos incluyen el indicador del LLM, el conjunto de entrenamiento y las APIs proporcionadas al modelo.
2. Desencadenar acciones perjudiciales a través de APIs. Por ejemplo, el atacante podría utilizar un LLM para llevar a cabo un ataque de inyección SQL en una API a la que tiene acceso.
3. Desencadenar ataques en otros usuarios y sistemas que consultan al LLM. A grandes rasgos, atacar una integración de LLM es a menudo similar a explotar una vulnerabilidad de falsificación de solicitud del lado del servidor (SSRF, por sus siglas en inglés). En ambos casos, un atacante está abusando de un sistema del lado del servidor para lanzar ataques sobre un componente separado que no es accesible directamente.

# **¿CÓMO FUNCIONA UNA API EN LLM?.**

![](/assets/img/capturas/portswigger/apimage.png)

El flujo de una API para un asistente virtual puede variar según la implementación y los requisitos específicos del asistente virtual en cuestión. Sin embargo, a grandes rasgos, aquí hay un flujo general que podría describir el funcionamiento de una API para un asistente virtual:

1. **Registro y Autenticación:**
   - El desarrollador se registra para obtener acceso a la API del asistente virtual.
   - Se proporcionan credenciales de autenticación, como claves API, para asegurar el acceso autorizado.
2. **Configuración de la API:**
   - Se configuran los parámetros y opciones específicas del asistente virtual, como preferencias de idioma, personalización de respuestas y otros ajustes según las necesidades del desarrollador.
3. **Envío de Consultas:**
   - El desarrollador o la aplicación envía consultas al asistente virtual a través de la API. Estas consultas pueden ser en forma de texto, voz o datos estructurados, dependiendo de las capacidades del asistente virtual.
4. **Procesamiento de Consultas:**
   - La API procesa las consultas utilizando el modelo de lenguaje subyacente o las capacidades del asistente virtual. Esto implica comprender el contenido de la consulta y determinar la mejor manera de responder.
5. **Generación de Respuestas:**
   - El asistente virtual genera respuestas basadas en la información procesada. Esto puede incluir texto, respuestas de voz o acciones específicas, dependiendo de la naturaleza del asistente virtual.
6. **Retorno de Respuestas:**
   - El asistente virtual devuelve las respuestas a través de la API al desarrollador o a la aplicación que realizó la consulta.
7. **Interpretación de Respuestas:**
   - El desarrollador interpreta las respuestas recibidas y las utiliza en su aplicación o servicio. Puede implicar mostrar texto al usuario, realizar acciones específicas o continuar la interacción.
8. **Manejo de Conversaciones (opcional):**
   - En el caso de asistentes virtuales que admiten conversaciones más largas, la API puede manejar el seguimiento de contextos y estados de conversación para proporcionar respuestas coherentes y contextualmente relevantes.

# **DETECCIÓN DE VULNERABILIDADES EN LLM.**

La metodología recomendada por PortSwigger para detectar vulnerabilidades en LLM es:

1. Identificar las entradas del LLM, incluyendo tanto las directas (como un indicador) como las indirectas (como datos de entrenamiento).
2. Determinar a qué datos y APIs tiene acceso el LLM.
3. Sondear esta nueva superficie de ataque en busca de vulnerabilidades. Explotación de APIs, funciones y complementos de LLM. Los LLMs a menudo son alojados por proveedores de terceros especializados. Un sitio web puede dar a los LLMs de terceros acceso a sus funcionalidades específicas describiendo APIs locales para que el LLM las utilice.

# **PRIMER LABORATORIO DE PRUEBAS.**

En el primer laboratorio nos encontraremos con dos cosas realmente muy curiosas. Para empezar, somos el usuario atacante y debemos borrar el usuario Carlos. En teoría, solamente el usuario Carlos podría borrar su cuenta si estuviera logueado.

![](/assets/img/capturas/portswigger/user_carlos.png)

La solución que nos ofrecen es mediante un par de preguntas al chatbot, conseguir averiguar las funciones de la API y luego realizar una consulta por SQL en la que seleccionan al usuario Carlos y lo borran de la base de datos, es decir, eliminan su cuenta.

La sorpresa es la siguiente… Siendo el atacante, me identifico al chatbot como Carlos y le pido que elimine mi cuenta, el resultado es…

![](/assets/img/capturas/portswigger/lab_one.png)

Obviamente, esto es una falla muy grave en la seguridad de nuestro sitio web…

# **CONCLUSIONES.**

Implementar un modelo de IA en nuestro negocio es un riesgo entre medio/alto. Debería realizarse paulatinamente con modelos bien entrenados y que, durante su entrenamiento, no se utilicen datos sensibles de la empresa y menos aún, datos reales de nuestros clientes.

Se puede acceder de forma gratuita a estos laboratorios en la página de [PortSwigger](https://portswigger.net/)
