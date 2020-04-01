---
templateKey: blog-post
title: Sobreviviendo diférentes versiones de Java en su MacBook!
date: 2019-10-07T12:00:00.000Z
featuredpost: false
featuredimage: /img/SwitchingJDKVersions.png
description: Sobreviviendo diférentes versiones de Java en su MacBook!
tags:
  - Java
  - JMeter
---
![image](/img/SwitchingJDKVersions.png)

Un buen día, su manager, alertado de su amplia experiencia en JMeter, le pide que urgentemente desarrolle varios scripts para testear un sitio de web.  Siendo un ingeniero muy sagaz, decide consulta nuestro [blog](https://jmeterenespanol.org/blog/2019-10-01-instalacion-antonio) dónde nuestro experto Antonio clarifica que la version más apropiada para JMeter es Java 8.1+.  

Inmediatamente después de verificar que su MacBook tiene instalada la version Java 11 (que es requerida por otra application) las siguientes preguntas se le vienen a la mente:

1. Tengo reemplazar la existente version de Java por la version requerida por Jmeter?
2. Una vez terminado el proyecto de JMeter,  tengo que reinstalar version 11?
3. Sera posible tener las dos (o mas) versiones a la vez?

No se preocupe. Aquí, tenemos las respuestas: No, No, y ... 

Si es posible tener mas de una version del JDK en MacOS, y como consecuencia diferentes versiones del Java ejecutor (Runtime), requerido por JMeter.

Empezamos por descargar la version JDK 8.1+ para MacOS en este sitio:

https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8u211-later-5573849.html

Nota: las diferentes versiones de Java JDK estarán localizadas en este directorio: /Library/Java/JavaVirtualMachines

Después de haber instalado esta version habilitamos la version (usando un terminal) de la siguiente manera:

    JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home
    java -version
    JMeter

Listo. JMeter utilizara la version Java 1.8, pero solamente para la session, sin interferir con otra version de Java existente.

Algunos dirán: Pero Sr. Californio esto es mucho para acordarse! Y tienen razón! 

Inspirado por la ley del mínimo esfuerzo cree este “one liner” que comprende los comandos mencionados.

    JAVA_HOME="$(/usr/libexec/java_home -v 1.8.\*)"; java -version; jmeter &

Finalmente, para los fanáticos de la mencionada ley, les presento un alias (bash) que, una vez incorporado en el file .profile, les permitirá invocar JMeter con nombre corto: “jm" 

    alias jm='JAVA_HOME="$(/usr/libexec/java_home -v 1.8.\*)"; java -version; jmeter &’

Saludos.

Próximo blog: Alternado diferentes versiones de JMeter.
