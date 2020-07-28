---
templateKey: blog-post
title: Usando Templates Para Pruebas de Carga APIs
date: 2020-05-01T12:00:00.000Z
featuredpost: false
featuredimage: title.png
description: Usando Templates Para Pruebas de Carga APIs
tags:
  - JMeter
  - Templates
---
![image](tbd.png)

En esta entrada abordamos el tema de usar plantillas (templates) en proceso de pruebas de carga para APIs. Este blog es, en cierta forma, una continuación del nuestro [blog](https://jmeterenespanol.org/blog/2020-06-02-properties-carlos/) anterior donde proponíamos las ventajas de alternar los script de JMeter **externamente** y forma **dinámica**. 

## El Ejemplo

Como sabemos, normalmente las pruebas de carga para APIs requieren enviar un bloque de data en el formato JSON. (Nota: esta bloque de data se conoce en inglés como *payload*). Este bloque esta compuesto por un conjunto de pares de nombre/valores. Por ejemplo:

```
{
  "Paciente":{
    "Identificación": "123456789",
    "Médico": "Dr. MataSanos",
    "Hospital": "LaBuenaMuerte"
  }
}
```
Como el propósito de la prueba es crear *pacientes*, incluimos un POST donde añadimos el *payload* en el *Body Data* de el pedido:

![image](graph1.png)

En este gráfico definimos la **Identificación** como un número random usando una porción de la variable [__UUID](https://jmeter.apache.org/usermanual/functions.html#__UUID). También definimos el **Médico** y **Hospital** como variables que serán leídas usando el [CSV Data Set Config](https://jmeter.apache.org/usermanual/component_reference.html#CSV_Data_Set_Config). Este es el proceso estándar.

## El Problema

El problema surge cuando los parámetros del API cambian causando que tengamos que editar el script para alterar la estructura JSON. Este problema se multiplica cuando el JSON es utilizado en múltiples scripts.

## La Alternativa

Una mejor alternativa es create una plantilla/template que contiene la estructura JSON y allí definir los valores en forma dinámica. 
 
 ```
{
  "Paciente":{
    "Identificación": "LT${__substring(${__UUID()}, 0, 8)}",
    "Médico": "${__eval(${MD_NAME})}",
    "Hospital": "${__eval(${HOSPITAL_NAME})}"
  }
}
```

Esta plantilla solamente contiene las **referencias** al nombre de las variables. La data actualmente esta contenida en el file CSV. Noten que usamos la función [__eval](https://jmeter.apache.org/usermanual/functions.html#__eval) para forzar la evaluación de las variables al momento de la ejecución.

Implementamos esta técnica en tres pasos:

### Paso 1:
![image](graph2.png)

Usando el elemento de Configuración **User Defined Variables** creamos/asignamos ...

### Paso 2:
![image](graph3.png)

Usando el elemento de Pre-Processor **User Parameters** creamos/asignamos ..

### Paso 3:
![image](graph4.png)

Usando el elemento de HTTP Request" **User Defined Variables** creamos/asignamos ..

## Conclusión

El uso de plantillas/template en un script proporciona la flexibilidad y productividad que resultan en un incremento substantivo en la eficiencia en la ejecución de las pruebas de carga.