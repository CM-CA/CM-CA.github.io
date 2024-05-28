---
title: Brick Heist
date: 2024-05-27 20:30 +/-0000
categories: TryHackMe Máquina
tags:
  - tryhackme
  - osint
  - easy
author: okud4
toc: true
img_path: /assets/img/
image: /banners/threemillions-banner.png
---

## INTRODUCCIÓN

En este documento detallaré el proceso de reconocimiento, escaneo, enumeración y explotación de la máquina [BricksHeist](https://tryhackme.com/r/room/tryhack3mbricksheist), de la plataforma [TryHackme](https://www.tryhackme.com), en un entorno controlado. El objetivo es demostrar cómo se puede identificar y explotar vulnerabilidades en un servidor Linux con un sitio web basado en WordPress. A lo largo del proceso, utilizaré herramientas como `ping`, `nmap` y `nuclei` para la fase de reconocimiento y escaneo, y un script de explotación específico para la vulnerabilidad identificada. Finalmente, analizaré archivos sospechosos y rastrearé transacciones de criptomonedas relacionadas con posibles actividades maliciosas.

---

## RECONOCIMIENTO

Empiezo realizando un ping a la máquina para averiguar si está activa y comprobar si es una máquina Linux o Windows a través de la respuesta ttl.

```bash
ping -c 1 $IP
PING 10.10.91.176 (10.10.91.176) 56(84) bytes of data.
64 bytes from 10.10.91.176: icmp_seq=1 ttl=63 time=54.8 ms
```

La máquina a la que me voy a enfrentar es Linux ya que su ttl oscila entre 63 y 64. Si se tratase de un SO Windows, oscilaría entre 127 y 128.

Más información [aquí](https://ostechnix.com/identify-operating-system-ttl-ping/).

También añado al archivo **hosts** que la dirección IP apunte al dominio **bricks.thm**:

```bash
# etc/hosts
10.10.91.176        bricks.thm
```

## ESCANEO Y ENUMERACIÓN

Con Nmap voy a realizar un escaneo de puertos para comprobar cuáles están abiertos. Pero antes, es mejor crear un directorio de trabajo con el nombre de la máquina y con sus subdirectorios para tenerlo todo organizado. Una vez creado todo lo necesario, entro en la carpeta donde guardaré los archivos de escaneo.

Realizo el primer escaneo solamente para identificar los puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -oN puertos $IP
```

![firstscan](capturas/brickheist/first_scan.png)

Luego de obtener los puertos que están abiertos, los escaneo más a fondo. En este caso, veo que tiene un servidor web, pero el puerto _443_ es el que me llama la atención.

![target_scan](capturas/brickheist/target_scan.png)

Me voy a enfrentar a un WordPress. En mi caso, realizaré un escaneo en busca de vulnerabilidades usando la herramienta **nuclei**.

Para ello, ejecutaré el siguiente comando:

```bash
nuclei -u bricks.thm -as -o report
```

Esto realizará un autoscaner utilizando Wappalyzer con la flag **-as** y luego al finalizar, se genera un reporte que se guarda en un archivo **report**.

Tras realizar el escaneo, obtengo el siguiente resultado:

```bash
[apache-detect] [http] [info] https://bricks.thm ["Apache"]
[metatag-cms] [http] [info] https://bricks.thm ["WordPress 6.5"]
[switch-protocol] [http] [info] https://bricks.thm ["h2"]
[waf-detect:apachegeneric] [http] [info] https://bricks.thm
[wordpress-detect:version_by_js] [http] [info] https://bricks.thm ["6.5"]
[mysql-detect] [tcp] [info] bricks.thm:3306
[openssh-detect] [tcp] [info] bricks.thm:22 ["SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.11"]
[ssh-auth-methods] [javascript] [info] bricks.thm:22 ["["publickey"]"]
[CVE-2024-25600] [http] [critical] https://bricks.thm/wp-json/bricks/v1/render_element
[wordpress-login] [http] [info] https://bricks.thm/wp-login.php
[wordpress-readme-file] [http] [info] https://bricks.thm/readme.html
[apache-detect] [http] [info] https://bricks.thm ["Apache"]
[metatag-cms] [http] [info] https://bricks.thm ["WordPress 6.5"]
[switch-protocol] [http] [info] https://bricks.thm ["h2"]
```

He encontrado una vulnerabilidad crítica en el tema Bricks, la cual explotaré con el script de [Chocapikk](https://github.com/Chocapikk/CVE-2024-25600).

## EXPLOTACIÓN

Antes de nada, me gusta aislar cada máquina con su propio entorno de Python. Para ello, uso **pyenv** junto con **virtualenv**, lo que me permite tener las instalaciones de librerías que se hagan a mayores, de una forma aislada del sistema.

![pyenv](capturas/brickheist/pyenv.png)

Una vez instalados los requisitos, ejecuto el exploit:

```bash
python exploit.py -u URL --payload-type code
```

Y ya estoy dentro. Aquí encontraré la primera flag.

![inside_bricks](capturas/brickheist/inside_bricks.png)

Ahora creo una reverse shell con Python y la estabilizo:

![reverse shell](capturas/brickheist/reverseshell.png)

![reverse shell](capturas/brickheist/reverseshell2.png)

![reverse shell](capturas/brickheist/insidebricks1.png)

Luego, comprobaré los servicios en busca de alguno sospechoso usando el comando:

```bash
systemctl list-units
```

Esto me listará todos los servicios del sistema y, después de revisarlo, encuentro un servicio sospechoso.

![ubuntu services](capturas/brickheist/ubuntuservice.png)

Ahora, con `systemctl status <nombre>.service`, obtengo la información necesaria para localizar el archivo malicioso. Dentro del directorio donde se encuentra alojado, dispongo llevarlo a mi equipo para poder analizarlo.

![dataminer](capturas/brickheist/dataminer.png)

![export](capturas/brickheist/export_miner.png)

Ya con el archivo en mi equipo, me dispongo a analizarlo en [AnyRun](https://any.run/), una web que permite analizar nuestros archivos. Tiene versión de pago y gratuita. Incluso con la versión gratuita, posee máquinas Windows y una máquina Ubuntu.

Después de analizarlo, veo que en el reporte se obtiene la dirección de dos wallets de bitcoin:

![firstscan](capturas/brickheist/anyrunscan.png)

Ahora solo queda buscar más información del wallet. Para ello, me dirijo a la web [Blockchair.com](https://blockchair.com) e introduzco la primera dirección que he obtenido (nota: las direcciones empiezan con **bc1**) y me proporciona la información de las transacciones.

![firstscan](capturas/brickheist/walletissue.png)

Aquí me dirijo a la primera transacción y la exploro, ya que tiene una issue. Luego tomo la nueva dirección y en [OpenSanctions](https://www.opensanctions.org/) la introduzco para obtener toda la información necesaria. Aquí obtengo el nombre de la persona a quien pertenece dicha wallet.

![firstscan](capturas/brickheist/russianhacker.png)

Ahora, para finalizar, solo tengo que buscarlo en Google y obtengo así a qué grupo de ciberdelincuentes pertenece.

![firstscan](capturas/brickheist/ransomwareteam.png)
