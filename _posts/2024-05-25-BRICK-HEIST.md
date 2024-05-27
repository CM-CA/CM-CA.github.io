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

## RECONOCIMIENTO

Empezamos realizando un ping a la máquina para averiguar si está activa y comprobar si es una máquina Linux o Windows a tráves de la respuesta ttl. 
```
ping -c 1 $IP
PING 10.10.91.176 (10.10.91.176) 56(84) bytes of data.
64 bytes from 10.10.91.176: icmp_seq=1 ttl=63 time=54.8 ms
```

La máquina a la que me nos vamos a enfrentar es Linux ya que su ttl oscila entre 63 y 64, si se tratase de un SO Windows oscilaria entre 127 y 128.

Más información [aquí](https://ostechnix.com/identify-operating-system-ttl-ping/).

Tambien añadiremos al archivo **hosts** que la direccion ip apunte al dominio **bricks.thm**

```bash
# etc/hosts
10.10.91.176        bricks.thm
```
## ESCANEO Y ENUMERACIÓN

Con Nmap vamos a a realizar un scaneo de puertos para comprobar cuales están abiertos, pro antes, es mejor crear un directorio de trabajo con el nombre de nuestra máquina y con sus subdirectorios de trabajo para tenerlo todo organizado. Una vez creado todo lo necesario, entramos a la carpeta en donde guardaremos los archivos de scaneo.

Realizamos el primer scaneo solamente para identificar los puertos abiertos:

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -oN puertos $IP
```

![firstscan](capturas/brickheist/first_scan.png)

Luego de obtener los puertos que estan abiertos, los scaneamos mas a fondo. En este caso vemos que tiene un servidor web, pero el puerto _443_ es el que me llama la atencion.

![target_scan](capturas/brickheist/target_scan.png)

Nos vamos a enfrentar a un wordpress, en mi caso realizaré un scaneo en busca de vulnerabilidades usando la herramienta **nuclei**. 

Para ello ejecutaré el comando:

```bash
nuclei -u bricks.thm -as -o report
```

Lo que va a realizar es un autoscaner utilizando wappalyzer con la flag **-as**
y luego al finalizar, se genera un reporte que se guarda en un archivo **report** 

Tras realizar el scaner obtengo el resultado:

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

Obtenemos una vulnerabilidad crítica en el tema Bricks, la cual explotaré con el script de [Chocapikk](https://github.com/Chocapikk/CVE-2024-25600)
## EXPLOTACIÓN

Antes de nada, en mi caso me gusta aislar cada máquina con su propio entorno de python. Para ello uso **pyenv** junto con **virtalenv** lo que me permite tener las instalaciones de librerias que se hagan a mayores, de una forma aislada del sistema.

![pyenv](capturas/brickheist/pyenv.png)

Una vez instalados los requisitos, ejecutamos el exploit:

```bash
python exploit.py -u URL --payload-type code
```

Y ya estamos dentro, aqui encontraremos la primera flag.

![inside_bricks](capturas/brickheist/inside_bricks.png)

Ahora creamos una revershell con python y la estabilizamos. Ahora, comprobaré los servicios en busca de alguno sospechoso usando el comando 
```bash
systemctl list-units
```

Esto me listará todos los servicios del sistema y despues de revisarlo, encuentro el servicio sospechoso.

![ubuntu services](capturas/brickheist/ubuntuservice.png)

Ahora con `systemctl status <nombre>.service` obtengo la información necesaria para localizar el archivo malicioso. Dentro de el directorio en donde se encuentra alojado, dispongo a llevarlo a mi equipo para poder analizarlo.

![dataminer](capturas/brickheist/dataminer.png)

![export](capturas/brickheist/export_miner.png)

Ya con el archivo en mi equipo, me dispongo a analizarlo en [AnyRun](https://any.run/), es na web que nos permite analizar nuestros archivos, tienes versión de pago y gratuita. Incluso con la version gratuita posee maquinas windows y una maquina ubuntu.

Despues de analizarlo, veo que en el reporte, se obtiene la direccion de dos wallets de bitcoin:

![firstscan](capturas/brickheist/anyrunscan.png)

Ahora ya solo queda buscar mas informacion de el wallet. Para ello me dirigo a la web [Blockchair.com](https://blockchair.com) e introduzco la primera direccion que he obtenido (nota: las direcciones empiezan con **bc1**) y me proporciona la informacion de las transacciones.

![firstscan](capturas/brickheist/walletissue.png)

Aqui me dirijo a la primera transaccion, y la exploro... ya que tiene una issue. Luego tomo la nueva direccion y en [OpenSanctions](https://www.opensanctions.org/) la introduzco para obtener toda la informacion necesaria. Aqui obtengo el nombre de la persona a quien pertenece dicha wallet.

![firstscan](capturas/brickheist/russianhacker.png)

Ahora, para finalizar, solo tenemos que buscarlo en google y obtenemos así a que grupo de ciberdelincuentes pertenece.

![firstscan](capturas/brickheist/ransomwareteam.png)

