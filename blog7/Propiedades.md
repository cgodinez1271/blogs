---
templateKey: blog-post
title: Configurando propiedades en JMeter, dos opciones
date: 2020-05-01T12:00:00.000Z
featuredpost: false
featuredimage: title.png
description: Deep Dive into JMeter Properties
tags:
  - JMeter
  - propiedades
---
![image](title.png)

En esta entrada planteamos dos opciones como inicializar propiedades en JMeter.

Empezamos definiendo que es un propiedad en JMeter. Una *propiedad* es un valor *dinámico* que es **común** a todos los hilos, y que normalmente se usa para definir la información relativa al *ambiente de ejecución*. Acceso a las Propiedades es a través de la función *__P()*. Por ejemplo, la propiedad *PropX* será leída usando *${__P(PropX)}*.

**NOTA**: en mi último [blog](https://jmeterenespanol.org/blog/2020-04-13-practicas-carlos/), mencioné dos buenas prácticas relacionadas a este tema:

* Usar el modo Non-Gui (CLI)
* Variables y Propiedades

## Importancia

La habilidad de inicializar propiedades tiene la importante ventaja que permite modificar el funcionamiento del script **externamente** y en forma **dinámica**. Por ejemplo, supongamos que tenemos un script que valida el performance en un website. Durante el proceso de desarrollar la aplicación, el test se puede inicialmente aplicar al ambiente de *desarrollo*, y posteriormente al de *QA*. En este case, usando una propiedad para configurar el script podemos alterar el URL que representa el ambiente de prueba sin tener necesidad de editar/alterar el script.

## Ejemplo

En el siguiente gráfico, inicializamos la configuración de un *tread grupo* usando 3 propiedades:

![image](graph1.png)

## Primera Opción: en la línea de comando (CLI)

La primera opción es inicializar las propiedades en la línea de comando de la siguiente manera:

```
jmeter -n -t EjemploPropiedades.jmx -J HILOS=100 -J RAMPUP=10 -J DURACION=600
```

En este ejemplo, estamos asignando valores a las propiedades HILOS, RAMPUP, y DURACION.

## Segunda Opción: usando un archivo de propiedades

En esta opción, las propiedades se incluyen en un archivo como sigue:

Primero, creamos un archivo llamado **pruebaQA_05-12-20.properties**, con las propiedades a inicializar:

```
#######################################
# Ambiente QA: propiedades específicas
HILOS=100
RAMPUP=10
DURACION=600
#######################################
```
Segundo, utilizamos este archivo de propiedades en la línea de comando:
```
jmeter -p pruebaQA_05-12-20.properties -n -t EjemploPropiedades.jmx
```

En este caso, las variables son inicializadas usando los valores contenidos en el archivo de propiedades.

**NOTA**: es posible incluir estas propiedades directamente en el archivo **user.properties** (localizado en $JMETER_HOME/libexec/lib). Pero la mejor práctica es localizar el archivo en el mismo lugar donde se encuentran los scripts de prueba.

## ¿Cúal es la Mejor Opción?

Por varias razones, la mejor opción es ciertamente el uso de archivos de propiedades:

1. Flexibilidad: podemos crear files de propiedades para múltiples propósitos dependiendo de fase the pruebas.
2. Documentación: las propiedades estarán documentadas en detalle y disponibles.

## Conclusión

Usando files de propiedades cuando 





