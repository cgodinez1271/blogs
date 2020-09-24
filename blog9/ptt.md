---
templateKey: blog-post
title: La Magia del Temporizador 'Precise Throughput Timer'
date: 2020-08-01T12:00:00.000Z
featuredpost: false
featuredimage: title.png
description: La Magia del Temporizador 'Precise Throughput Timer'
tags:
  - JMeter
  - Templates
---
![image](title.png)

En esta entrada abordamos el uso de un temporizador (o timer en Inglés) que caracteriza por enviar transacciones en forma muy similar como usuarios reales usan una aplicación.

## Categorias de Temporizadores

JMeter ofrece dos tipos de temporizadores. Una categoría es usada principalmente para insertar pausas (llamado Think-time en Inglés) entre peticiones con el propósito de emular el comportamiento humano. En esta categoría esta el *Temporizador Constante*, *Temporizador Aleatorio Uniforme*, *Temporizador Aleatorio Guassiano*, y otros.

La segunda categoría esta compuesta por temporizadores que tienen como objetivo regular el ratio de arribo de los **Grupos de Hilos** (sesiones), en otras palabras, configurar el **throughput** deseado. Este ratio de arribo está determinado por el **intervalo** de tiempo de ejecución entre **hilos** (Vusers), que en Inglés se conoce como **Pacing**. Dentro de esta categoría están el [*Constant Throughput Timer*](https://jmeter.apache.org/usermanual/component_reference.html#Constant_Throughput_Timer) y el [*Precise Throughput Timer*](https://jmeter.apache.org/usermanual/component_reference.html#Precise_Throughput_Timer).

**NOTA**: ver mis entradas de blog [aqui](https://jmeterenespanol.org/blog/2020-01-28-pacingtechnique-1-2-carlos/) y [aqui](https://jmeterenespanol.org/blog/2020-02-14-pacingtechnique-2-2-carlos/) acerca de otra técnica para controlar el ratio de arribo.

## Carga vs. Concurrencia

Estos dos conceptos son críticos para enter la differencia en el uso de los tipos de temporizadores.

|Carga|Concurrencia
|---|---
|Determinada por el **Pacing**|Determinada por el **Think Time**
|Intervalo **entre sesiones** del universo usuarios|Intervalo **entre peticiones** dentro de una session
---

## Precise Throughput Timer (PTT)

Como mencionamos previamente, el *Precise Throughput Timer* (PTT) y *Constant Throughput Timer* (CTT) tienen como propósito regular el arribo de hilos (Vusers). ). La principal differencia entre estos dos temporizadores esta en que el CTT inserta un pausa **constante** mientras que el CTT inserta un pausa **aleatoria**. Esta pausa aleatoria modela un distribución de Poisson que (como se ha demostrado) es como los usuarios interactuan con una aplicación.

## Configuration del PTT

Para ello el temporizador pausa al *inicio* del Group de Hilos (ver configuración abajo

La configuracion del temporizador se basa un concepto simple: es establece el **gol/objetivo Throughput** deseado.

![image](img1.png)

En el ejemplo, el Throubhput (TXN/Sec) deseado:

```
Target throughput (samples x "Throughput period") / Throughput period (secs)

10 TXN por Segundo = 600 TXN / 60 Segundos
```

Describe how the PTT timer works ...

## Optima Ubicación del PTT

La ubicación ideal de este temporizador en el primer elemento de un Grupo de Hilos, probablemente como parte de una peticion de *Acción de Prueba* (ver gráfico anterior).

## Consideraciones

1. Usar un Grupo de Hilos normal.
2. Configurar el Grupo de Hilos con el número necesario de hilos/vusers.
3. Configurar el Grupo de Hilos con la misma duración (Test duration).
4. Configurar el Grupo de Hilos un *Ramp-up* y *Startup delay* igual a cero.
5. No usar expresiones variables en la configuración.

## Conclusión

El temporizador PTT nos ofrece un

Carlos A. Godinez - Senior Performance Engineer

https://www.linkedin.com/in/carlosgodinez/

Facebook: https://www.facebook.com/groups/jmeterenespanol

Slack: https://jmeterenespanol.slack.com/