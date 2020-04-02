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

En esta entrada presentamos las mejores prácticas en JMeter propuestas por dos expertos en el mundo de JMeter: Phillipe Mouawad y Antonio Gomes Rodrigues. Ellos son los autores de un libro que, en mi opinión, es un de los mejores actualmente: [Master Apache JMeter From load testing to DevOps](https://leanpub.com/master-jmeter-from-load-test-to-devops). Les recomiendo la versión *ebook*. Yo he añadido comentarios y ejemplos para ampliar o/y clarificar algunos de los conceptos.

## Documentación obsoleta

Puesto que JMeter ha estado disponible por muchos años hay que tener cuidado en consultar blogs, videos, y otra documentación. Cualquier información que fue publicada hace dos años o se refiera a versions antes de 4.0 es probablemente obsoleta. La mejor práctica es consultar la [documentación official](https://jmeter.apache.org/usermanual/index.html) y la historia del cambios atravéz de las [differentes versiones](https://jmeter.apache.org/changes_history.html).

## Performance

La mejor práctica es **no** usar BeanShell o Javascript por que degradan significamente el performance de la herramienta. La mejor práctica is usar JSR223 que usa Groovy como default. Groovy es un lenguage moderno y poderoso que funciona con la últimas version de Java.

## Usar el modo Non-Gui

La mejor práctica es **no** user el modo Non-GUI para las pruebas de carga. El modo GUI se usa en la fase de desarrollo del script, pero que una vez esta fase está completa la recomendación is cambiar al modo non-GUI.

## Reporte HTML

JMeter ofrece muchas alternativas para graficar los resultados en el modo-GUI. Sin embargo la mejor práctica es producir el report HTML al final de la ejecución del test (en modo non-GUI):
```
jmeter -n -t [jmx file] -l [results file] -e -o [\Path to output folder]
```
Les recomiedo ver el manual en este [enlace](https://jmeter.apache.org/usermanual/generating-dashboard.html).

Si por alguna razón hubiera un gráfico que no este contenido el report HTML, siempre hay la oportunidad cargar el file de resultados en el modo GUI (después de completar el test).

## No modificar directamente el file XML

La mejor práctica es **nunca** manipular directamente el XML file donde JMeter guarda el test (*.jmx file).

## Dominar el concepto de orden de ejecución

En breve, el orden de ejecución de los elementos en JMeter obedece a reglas específicas. Nuestro ilustre colega Antonio presenta los detalles del orden de ejecución en este [post](https://jmeterenespanol.org/blog/2019-10-04-ejecucion-antonio/).

## Variables y Propiedades

La mejor práctica es entender claramente las diferencias entre Variables y Propiedades.  Una *Variable* es un valor dinámico que es exclusivo a un hilo o *Usuario virtual (VUsers)*. Variables usualmente se definen usando el elemento *User Defined Variables* al nivel del plan. Durante la ejecución de la prueba, el hilo hace una copia local de la variable y puede modificar este valor sin afectar los otros hilos de la prueba. Su uso esta normalmente limitado a data de usuarios y reglas de correlación. Variables son accesadas usando *${varName}*. Nuestro experimentado colega Delvis ilustra como usar variables en durante el proceso de correlación en este [post](https://jmeterenespanol.org/blog/2019-11-22-correlacion-delvis/).

Por otra parte, una *Propiedad* es un valor dinámico que es **común** a todos los hilos y que normalmente se usa para definir la información relativa al ambiente de ejecucion. Propiedades son accesadas usando la función *__P*. Por ejemplo, *propX* sera leida usando *{__P(propX)}*.

## Use Funciones

JMeter provee una lista muy completa de funciones que permiten optimizar la creación de *scripts* de pruebas. La mejor práctica es familiarizarse y usar estas funciones en véz de tratar de 'reinventar la rueda'. Por ejemplo, nuestro ilustre colega Antonio nos presenta algunas [funciones para especificar valores de tiempo](https://jmeterenespanol.org/blog/2019-11-15-functiempo-delvis/).

Por si esto fuera poco, JMeter tiene otro [*plugin*](https://jmeter-plugins.org/wiki/Functions/) que provee funciones addicionales.

Ejemplo muy común es usar la función UUID (Universal Unique ID) para crear una direción de email aleatoria de la siguiente forma:
```
{
    "email": "${__substring(${__UUID()}, 0, 8)}.${__substring(${__UUID()}, 25, 35)}@gmail.com", 
}
```
Nota: el ejemplo usa la función *substring* que es parte el *plugin* mencionado.

## Modelos Abiertos vs Modelos Cerrados

La mejor práctica es usar el modelo correcto para la aplicación que se esta probando. En un **modelo abierto** los usuarios arriban a la applicación en forma asincrónica, indenpendientemente del número de *transacciones* que estan siendo procesadas en el *sistema bajo prueba* (SUT). La métrica de esta tipo de prueba se define en términos de *transacciones x unidad-de-tiempo* (TPS). Un ejemplo típico son la aplicaciones *e-commerce* (Amazon).

Por otro lado, en un **modelo cerrado** el número usuarios es fijo, por lo tanto el número de transacciones esta limitado por la cantidad de usuarios en el sistema. Un ejemplo típico es un centro de llamadas (call center) donde la cantidad de llamadas aceptadas esta limitada por el número de operadores; si todos operadores estan ocupados, el sistema no acceptará mas llamadas. La métrica en este caso es la *concurrencia*.
