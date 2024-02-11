---
title: Ciberataque a Fuden
date: 2024-02-07 20:30 +/-0000
categories: Noticias
tags: 
author: okud4
toc: true
img_path: /assets/img/
image: banners/fuden-banner.jpg
---

Hace unos dias saltó la noticia de que FUDEN, una plataforma online de cursos de enfermeria, fue hackeada a través de un #malware. Según indican algunos medios, el atacante procede de EEUU y secuestró los datos personales de los alumnos a cambio de dinero.

## FUDEN.

Para seguir con la noticia, me gusta primero indagar en la empresa que fue hackeada, si fué la primera vez o tuvo ya mas ciberataques…

1. Empiezo con una búsqueda por Google, buscando noticias relacionadas. Para ello uso [dorksearch.com](https://dorksearch.com/) y me centro en buscar en Incibe, ya que, cuando existe una brecha de seguridad, debe comunicarse a Incibe… y al parecer no ha sido la primera vez, ya el 30 de Mayo de 2019 tuvieron una brecha de filtración de datos.
2. Dentro de la propia notificación de el Incibe, me encuentro con los siguientes datos:

	![Incibe descripcion](capturas/fuden/incibe-data.png){: width="700" height="400" }

	![Incibe datos](capturas/fuden/incibe-detalle.png){: width="700" height="400" }

3. Lo mas curioso que me he encontrado, en una de las noticias es lo siguiente:

	> Los afectados, enfadados, aseguran haberse enterado del ciberataque a través de un foro en lugar de por la propia Fuden. Sin embargo, y a pesar de las numerosas reacciones a través de las redes sociales, desde la fundación le restan importancia: **"Se le está dando más relevancia de la que tiene".**
	  Fuentes de Fuden aseguran a _Redacción Médica_ que los datos sustraídos son de alumnos de formación continuada y que la situación "**no es alarmante**" porque "no se han **robado datos económicos"**.

	[Redacción Médica](https://www.redaccionmedica.com/secciones/enfermeria/fuden-a-los-enfermeros-hackeados-le-dan-mas-relevancia-de-la-que-tiene--6472)

	Bien, que no se haya robado datos económicos, parece set que es lo que menos les preocupa, cuando en realidad el robo de los datos personales de 50.000 enfermeros sí que es preocupante, y mas preocupante todavía es pensar en quien controla nuestros datos.

## MALWARE.

La información que nos dan sobre el ataque, es que se produjo a través de un #malware .

Hasta aquí, todo bien. Como han podido colar el malware? phishing? un insider? acceso a través de su plataforma?… Usando #wappalizer, descubro que su plataforma es un Wordpress (prefiero no seguir indagando más…) y obtengo el email: `info@fuden.es`

Introduzco el dominio en #PhoneBook y … bueno, podeis comprobarlo vosotros mismos.

¿Y el login? Bueno, pues al parecer emplean **Moodle**, con una autenticacion **SimpleSAMLphp** y… Sí, una versión **PHP 7.4.27**.

![php release](capturas/fuden/php-release.png){: width="700" height="400" }

## CONCLUSIÓN.

Lo alarmante de todo esto, es como se trata los datos de los usuarios. Si tenemos una brecha de seguridad, debe comunicarse de forma inmediata. A los usuarios se les debe notificar los datos que le han sido robados y los pasos a seguir para que comprueben que su identidad no ha sido suplantada (para pedir prestamos, por ejemplo…), cambiar sus contraseñas, que sus números de teléfono no sean usados para ciberestafas…

La empresa debe mantener su plataforma actualizada y **NUNCA** puede menospreciar los datos de los usuarios.
