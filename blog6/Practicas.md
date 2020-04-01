---
templateKey: blog-post
title: Mejores prácticas en JMeter
date: 2020-03-20T12:00:00.000Z
featuredpost: false
featuredimage: title.png
description: Mejores prácticas en JMeter
tags:
  - JMeter
  - prácticas óptimas 
---
![image](title.png)

En esta entrada presentamos las mejores prácticas en JMeter propuestas por dos expertos en el mundo de JMeter: Phillipe Mouawad y Antonio Gomes Rodrigues. Ellos son los autores de un libro que en mi opinion es un de los mejores actualemente: [Master Apache JMeter From load testing to DevOps]. Les recomiendo la version *ebook*. Yo he añadido comentarios y ejemplos para ampliar o/y clarificar algunos de los conceptos.

[Interview with two JMeter developers and authors of the book Master JMeter] (https://www.youtube.com/watch?v=0CcUqfuOi0I)

## Documentacion obsoleta

Puesto que JMeter ha estado en le mercado por muchos años hay que tener cuidado en consultar blogs, videos, y otra documentacion que probablemente este obsoleta. La mejor practica es consultar la [documentation official](https://jmeter.apache.org/usermanual/index.html) y la historia del cambios atravez de las [differentes versiones](https://jmeter.apache.org/changes_history.html)

## Performance

La mejor practica es **no** usar BeanShell o Javascript por que degradan significamente el performance de la herramienta. La mejor practica is usar JSR223 que usa Groovy como default. Groovy es un lenguage moderno y poderoso que funciona con la últimas version de Java.

## Usar el modo Non-Gui

La mejor practica es **no** user el modo Non-GUI para las pruebas de carga. El modo GUI se usa en la fase de desarrollo del script, pero que una vez esta fase esta completa la recomendacion is cambiar al modod non-GUI

## HTML Reported

JMeter ofrece muchos alternativas para graficar los resultados en el modo-GUI. La mejor práctica es 

https://jmeter.apache.org/usermanual/generating-dashboard.html

## No XML modificaciones directas

La mejor práctica es **nunca** manipular directamente el XML file donde JMeter archiva el test (*.jmx file). 

## Dominar el concepto de orden de ejecución 

En breve, el orden de ejecución en JMeter obedece a un jerarquía muy especifica. Nuestro ilustre colega Antonio presenta los detalles del [Orden de ejecución en JMeter](https://jmeterenespanol.org/blog/2019-10-04-ejecucion-antonio/)

## Variables y Propiedades

La mejor práctica es entender claramente la diferencias entre Variables y Propiedades.  Una *Variable* es un valor dinámico que es exclusivo a un hilo o *Vuser*. Variables usualmente se definen usando el elemento *User Defined Variables* al nivel del plan. La alternativa es definir la propiedades en la línea de commando en el modo *non-Gui*.

Por otra parte, una *Propiedad* es un valor dinámico que es común a todos los hilos (Vusers). Usualmente, las *propiedades* se definen en los files *.properties localizados en el directorio $JMETER_HOME/bin. La mejor práctica indica 

## Use Funciones 

JMeter provee una lista muy completa de funciones que permiten optimizar la creación de *scripts* de pruebas. La mejor práctica es familiarizarse y usar estas funciones en véz de tratar de 'reinventar la rueda'. Por ejemplo, nuestro ilustre colega Antonio nos presenta algunas [funciones para especificar valores de tiempo](https://jmeterenespanol.org/blog/2019-11-15-functiempo-delvis/).

Por si esto fuera poco, JMeter tiene un [*plugin*](https://jmeter-plugins.org/wiki/Functions/) que provee funciones addicionales.

Ejemplo muy común es usar la función UUID (Universal Unique ID) para crear una direción de email aleatoria de la siguiente forma:
```
{
    "email": "${__substring(${__UUID()}, 0, 8)}.${__substring(${__UUID()}, 25, 35)}@gmail.com", 
}
```
Nota: el ejemplo usa la función *substring* que es parte el *plugin* mencionado.

## Modelos Abiertos vs Modelos Cerrados

La mejor práctica es user el modelo correcto para la aplicacion que se esta probando. 
