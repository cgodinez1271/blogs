---
templateKey: blog-post
title: "El reporte HTML: la herramienta definitiva"
date: 2020-11-01T12:00:00.000Z
featuredpost: false
featuredimage: title.png
description: "El reporte HTML: la herramienta definitiva"
tags:
  - JMeter
  - HTLM Report
  - Tools
---
![image](title.png)

Una vez completada la ejecución de una prueba en modo CLI (non-GUI) hay dos opciones para visualizar los resultados: 1. leer el file de resultados (jtl) usando JMeter en modo GUI y usar *complementos* (plugins) para graficar los resultados, o 2. producir el reporte HTML. En esta entrada les presento un herramienta (shell script) que les permitirá generar el reporte HTML en forma eficiente, además de otros beneficios:

1. generar el reporte HTML usando el fichero generado por la ejecución de test (jtl)
2. archivar la información relevante del test

## Beneficio importante

El segundo punto es sumamente importante puesto que permite reunir **toda la información relevante** de un test en forma permanente e inequívoca: el script crea un directorio único (usando el sello del tiempo) donde se guardan los files requeridos para crear el reporte. Adicionalmente, el script copia los siguientes files al mencionado directorio:

1. ***.jmx** (jmeter script file)
2. ***.jtl** (jmeter log/bitácora file)
3. **jmeter.log** (jmeter run log file)
4. **errors.xml** (log/bitácora de errores - opcional)

## Uso

El uso es muy sencillo. El primer paso es ejecutar el JMeter script en modo CLI:

```
jmeter -n -t escenario.jmx -l escenario.jtl
```

En el segundo paso ejecutamos la herramienta usando el file **jtl** como parámetro:

```
jmRpt.sh escenario.jtl
```

El script crea un directorio único con un sello del tiempo (por ejemplo: 2020-09-25_18:36:58.391). El listado del directorio será aproximadamente asi:

```
ls -l 2020-09-25_18:36:58.391
total 760
-rw-r--r--  1 carlos  staff   36657 Sep 26 19:05 escenario.jmx
-rw-r--r--  1 carlos  staff  293822 Sep 25 18:27 escenario.jtl
drwxr-xr-x  5 carlos  staff     160 Sep 25 18:37 content
-rw-r--r--@ 1 carlos  staff    9678 Sep 25 18:37 index.html
-rw-r--r--  1 carlos  staff    2598 Sep 25 18:34 jmeter.log
drwxr-xr-x  7 carlos  staff     224 Sep 25 18:37 sbadmin2-1.0.7
-rw-r--r--  1 carlos  staff     992 Sep 25 18:37 statistics.json
```

Finalmente, cambie al directorio y abra el reporte HTML en el browser:

```
open index.html
```

## Descarga de la herramienta

La herramienta (y el README file) se pueden descargar de la siguiente manera:

```
git clone git@github.com:cgodinez1271/jmeter-dashboard-rpt.git
```

**Nota**: Este script ha sido diseñado para ser ejecutado en MacOS. Probablemente funcione en Linux.


