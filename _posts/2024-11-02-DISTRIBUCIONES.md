---
title: Distribuciones Linux para Pentesting y OSINT
date: 2024-11-01 22:00 +/-0000
categories: OSINT
tags:
  - osint
  - tools
author: okud4
toc: true
img_path: /assets/img/
image: /banners/logo-okuda-post.jpeg
---

## Hacking

Existen muchas distribuciones linux para estos fines, desde Kali Linux, Parrot o incluso Black Arch. Pero a pesar de que voy a hablar de unas cuantas de ellas, mi recomendación es que pruebes y te quedes con la que más cómodo te encuentres e incluso, construye una distribución personalizada con tus herramientas favoritas.

## Distribuciones para Hacking ético

Hablaré de mis tres distribuciones favoritas:

1. [Athena OS](https://athenaos.org/)

    Esta distribución está en constante desarrollo. En sus inicios estaba basada en Arch pero ha evolucionado y a dia de hoy, nos ofrece dos posibles instalaciones:

    - Opción basada en Arch
    - Opción basada en NixOS

    Las principales características son:

    - **Modelo de código abierto**: todos los elementos del sistema están disponibles en el árbol de desarrollo de Athena para que los usuarios accedan o editen según sus necesidades.
    - **Gratuito**: Athena OS es gratuito y accesible para todos, y lo será para siempre.
    - **Rendimiento**: se han aplicado varias estrategias para hacer el sistema más rápido, actuando desde el núcleo y pasando por la memoria virtual, RAM, gestor de paquetes, etc.
    - **Flexibilidad**: permite personalizar profundamente las configuraciones del sistema, permitiendo a los pentesters adaptar el entorno a requisitos específicos. Gracias a Nix, es posible expresar configuraciones en un lenguaje de alto nivel, ofreciendo más flexibilidad y adaptabilidad en comparación con los sistemas de gestión de configuración tradicionales.
    - **Herramientas de pentesting personalizadas**: ofrece herramientas de pentesting clasificadas por roles de ciberseguridad. Cada rol corresponde a un dominio de ciberseguridad con diferentes herramientas. Estas herramientas se mantienen continuamente para mejorar su eficacia y mantener un entorno limpio y seguro.
    - **Enfoque minimalista**: los paquetes y servicios de pentesting no están preinstalados. Este enfoque minimalista permite un mayor control sobre los recursos del sistema y reduce el uso innecesario, resultando en un mejor rendimiento y uso de recursos.
    - **Documentación completa**: incluye documentación exhaustiva que cubre guías de instalación, solución de problemas, gestión de paquetes, configuración del sistema, entre otros.
    - **Cumplimiento industrial**: el sistema está construido conforme a los estándares de jerarquía de archivos, ISO-9660 y FAT32.

    Ventajas destacadas de Athena OS como sistema operativo de pentesting:

    - **Reproducibilidad**: garantiza la consistencia del entorno de pruebas mediante configuraciones precisas y control de versiones.
    - **Flexibilidad y personalización**: ofrece ajustes detallados a las configuraciones, permitiendo personalizar el entorno.
    Rendimiento: optimización en el uso de recursos gracias a su diseño minimalista y estrategias de optimización.
    - **Seguridad**: obtención de paquetes seguros y alertas de CVEs, especialmente en la versión Nix.
    Documentación y accesibilidad: documentación extensa y gratuita, accesible para cualquier usuario.

    Comparación entre versiones Athena Arch y Athena Nix:

    - **Athena Arch**: es ideal para quienes buscan alta personalización y la posibilidad de utilizar herramientas variadas, gracias a su acceso a AUR y su filosofía "do-it-yourself". Su modelo de actualización continua garantiza siempre la última versión de herramientas sin necesitar reinstalaciones.

    - **Athena Nix**: está enfocada en la estabilidad y seguridad de los entornos, permitiendo configuraciones inmutables, previniendo conflictos entre paquetes, y ofreciendo una reproducibilidad ideal para pruebas en equipo o entornos de trabajo colaborativos.

2. [Parrot OS](https://parrotsec.org/download/)

    Esta es otra de las distribuciones más conocidas. Ideal para iniciarse en el mundo de el hacking ya que posee un buen número de herramientas ya preinstaladas.

    Las principales características incluyen:

    - **Ligereza** y rendimiento: Parrot OS está optimizado para usar menos recursos, lo que permite una ejecución fluida en equipos con menor capacidad, especialmente en comparación con otras distribuciones de pentesting.

    - **Interfaz amigable**: Integra el escritorio MATE, un entorno ligero y fácil de personalizar que resulta en una experiencia de usuario más fluida.
    Puedes explorar en [UnixPorn](https://www.reddit.com/r/unixporn/) acerca de la configuración de tu entorno y descargar el que más te guste, también sirve para otras distribuciones.

    - **Reproducibilidad de ambientes**: Parrot OS permite configuraciones personalizadas y reproducibles, especialmente útiles para clonar entornos de pentesting y establecer laboratorios virtuales.

    - **Herramientas preinstaladas**: Viene con una amplia variedad de herramientas para hacking ético, análisis forense, ingeniería inversa y desarrollo, como Metasploit, Burp Suite, y Aircrack-ng, facilitando el acceso a herramientas necesarias para investigaciones de seguridad.

    - **Seguridad y privacidad**: Parrot OS incorpora configuraciones de privacidad y herramientas de anonimato como Anonsurf, que redirige el tráfico a través de la red Tor, y cifrado de disco completo para proteger datos.

    - **Actualización continua y soporte**: Al estar basado en Debian, Parrot OS se beneficia de actualizaciones de seguridad frecuentes y una gran comunidad que mantiene el sistema actualizado y confiable.

3. [Kali Linux](https://www.kali.org)

    Desarrollado por Offensive Security, se centra en ofrecer una plataforma robusta y completa para pruebas de penetración y análisis de vulnerabilidades. Sus características destacadas incluyen:

    - **Amplia colección de herramientas de seguridad**: Kali Linux viene con más de 600 herramientas de pentesting preinstaladas, abarcando áreas como redes, aplicaciones web, sistemas de explotación y análisis forense.

    - **Facilidad de personalización**: Kali Linux permite configuraciones personalizadas a través de sus herramientas, soportando el uso de entornos de escritorio como Xfce, GNOME, y KDE.

    - **Modelo de lanzamiento continuo**: Kali Linux utiliza un sistema de actualización rolling-release, lo que significa que se actualiza constantemente y permite a los usuarios acceder a las últimas herramientas y parches sin reinstalaciones mayores.

    - **Seguridad avanzada**: Kali incluye características como cifrado de disco completo y soporte para operaciones en entornos seguros, ofreciendo opciones para un uso en modo forense que evita la alteración del sistema mientras se realiza el análisis.

    - **Compatibilidad con Windows Subsystem for Linux (WSL)**: Kali es compatible con WSL, lo que permite a los usuarios de Windows ejecutar herramientas de Kali Linux directamente en su sistema operativo sin necesidad de máquinas virtuales.

## Comparativa Final: Parrot OS, Kali Linux y Athena OS

A continuación, se presenta una comparación entre Parrot OS, Kali Linux y Athena OS, resaltando sus ventajas y desventajas en un entorno de pentesting:

1. **Uso de Recursos y Ligereza**: Parrot OS está optimizado para el uso eficiente de recursos, haciéndolo adecuado para sistemas de gama baja o con menos recursos. Kali Linux, aunque se puede ejecutar en una variedad de entornos, tiende a consumir más recursos, especialmente con configuraciones gráficas avanzadas como GNOME o KDE. Athena OS, al tener un enfoque minimalista y excluir módulos innecesarios, también es ligero, pero depende en gran medida de la configuración Arch o Nix que se elija.

2. **Colección de Herramientas**: Kali Linux ofrece la mayor cantidad de herramientas preinstaladas con más de 600 opciones, facilitando una configuración lista para usar. Parrot OS también incluye una amplia gama, pero con un enfoque en herramientas específicas y una mayor capacidad de personalización. Athena OS, especialmente en la versión Arch, ofrece un repositorio de 2800 herramientas, clasificadas según roles de ciberseguridad, lo cual es beneficioso para una organización estructurada.

3. **Flexibilidad y Personalización**: Athena OS destaca por su capacidad de configuración avanzada y flexibilidad, especialmente en su versión basada en Nix, que permite configuraciones inmutables y reproducibles ideales para equipos de trabajo. Kali Linux y Parrot OS ofrecen personalización, pero no alcanzan el nivel de reproducibilidad que proporciona Athena OS con Nix.

4. **Documentación y Comunidad**: Kali Linux cuenta con una comunidad y documentación ampliamente desarrolladas, además de cursos oficiales de Offensive Security, lo cual es una ventaja significativa. Parrot OS también dispone de una comunidad activa y documentación sólida. Athena OS es relativamente nuevo y, aunque tiene documentación extensa, aún está construyendo su base de usuarios.

5. **Modelo de Actualización**: Tanto Kali Linux como Athena OS en su versión Arch tienen un modelo de lanzamiento continuo, lo que garantiza que siempre estén actualizados sin necesidad de reinstalaciones. Parrot OS sigue el modelo Debian estable, con actualizaciones periódicas y centradas en la estabilidad.

6. **Seguridad**: Athena OS en su versión Nix sobresale en seguridad con una configuración inmutable y la capacidad de alertar sobre CVEs antes de instalar paquetes vulnerables, lo cual ofrece una ventaja para ambientes donde la seguridad y la consistencia son primordiales. Kali Linux y Parrot OS también ofrecen medidas de seguridad, como el cifrado de disco y herramientas de anonimato, pero no incluyen la prevención de conflictos ni la seguridad de paquetes específicos como lo hace Nix.

7. **Optimización para Equipos de Trabajo**: Athena OS, particularmente en su versión Nix, es ideal para entornos de trabajo colaborativos gracias a sus configuraciones reproducibles y soporte para configuraciones inmutables. Parrot OS y Kali Linux pueden usarse en equipos, pero no ofrecen las mismas capacidades de clonado y replicación de entorno que Nix en Athena.

8. **Facilidad de Uso**: Kali Linux es posiblemente el más accesible para principiantes debido a su extensa documentación y soporte oficial. Parrot OS también es amigable para usuarios con experiencia en Linux. Athena OS, al estar basado en Arch y Nix, requiere un conocimiento técnico algo más avanzado, aunque proporciona un entorno muy personalizable para quienes dominan su configuración.

## Conclusión de las distribuciones para pentesting

**Kali Linux** es ideal para quienes buscan una plataforma integral de pentesting que venga lista para usar con una vasta colección de herramientas, excelente documentación, y soporte educativo.

**Parrot OS** es una opción equilibrada, más ligera y eficiente, con herramientas de privacidad añadidas, ideal para usuarios que buscan un entorno de pentesting y anonimato flexible.

**Athena OS** sobresale en configuraciones avanzadas, personalización, y entornos colaborativos gracias a sus versiones Arch y Nix, siendo ideal para pentesters experimentados y ambientes donde la reproducibilidad y seguridad avanzada son fundamentales.

## Distribuciones especificas para OSINT

En este apartado solamente puedo hablar de una distribución que he probado y me gusta bastante, suelo usarla para realizar investigaciones.

1. **CSI Linux**

    CSI Linux es una de las distribuciones de Linux más especializadas en OSINT. Está diseñada para la investigación en inteligencia y ciberseguridad, con herramientas avanzadas que facilitan la recopilación de datos, el análisis forense y el rastreo digital. Sus características principales son:

    - **Colección de herramientas OSINT y de ciberinteligencia**: Incluye herramientas para analizar redes sociales, rastrear direcciones IP, y recopilar información desde fuentes abiertas. Algunas de estas herramientas incluyen Maltego, SpiderFoot, Recon-ng, y herramientas para redes sociales como Social-Engineer Toolkit y Twint.

    - **Módulos de formación y documentación**: CSI Linux incluye materiales de aprendizaje y guías para facilitar el uso de las herramientas y enseñar a los usuarios las mejores prácticas en OSINT.

    - **Entornos de investigación en entornos aislados**: Utiliza máquinas virtuales y contenedores para permitir que los investigadores trabajen en un entorno seguro sin comprometer sus sistemas principales.

    - **Navegación anónima y privacidad**: Tiene configuraciones preconfiguradas para el uso de Tor y VPNs, ofreciendo un entorno seguro y anónimo para la recolección de datos.

## Conclusión de las distribuciones para OSINT

**CSI Linux** es una excelente opción para aquellos que buscan una distribución OSINT integral con capacidades de anonimato y módulos preconfigurados para ciberinteligencia. Su enfoque en herramientas específicas y módulos de aprendizaje la hace muy adecuada para investigadores dedicados.

### Extra

Como regalo por llegar al final de este post, te dejo una herramienta que te puede resultar útil:

[Intel Techniques](https://inteltechniques.com/tools/index.html)
