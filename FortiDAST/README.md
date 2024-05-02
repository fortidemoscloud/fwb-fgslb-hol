# Hands on Lab Seguridad Aplicaciones Web & API
## Objetivo del laboratorio
El objetivo de este laboratorio es conocer la propuesta de valor de Fortinet para proteger aplicaciones web y APIs a través de sus soluciones de FortiWeb Cloud, FortiDAST y FortiGSLB. Durante el workshop se publicarán 2 aplicaciones de forma segura a través de FortiWeb Cloud simulando un entorno corporativo, con un portal web y una API. En el proceso aprenderás a entrenar el modelo de Machine Learning (ML) de la API para conocer el esquema OpenAPI de la misma y aplicar mecanismos de protección sobre el mismo y también cómo proteger portales web frente a ataques TOP10 OWASP y otros ataques sofisticados.

Al margen de ello podremos comprobar el valor añadido que puede aportar nuestro servicio FortiDAST para evaluar de forma continua la postura de seguridad de nuestras aplicaciones y APIs y como nuestro servicio de Global Service Load Balancing (GSLB) mediante DNS, FortiGSLB, puede facilitar una publicación más eficiente de la aplicación en entornos distribuidos facilitando siempre el mejor acceso en base a la ubicación del usuario o a los health checks definidos para cada aplicación. 

El formato del laboratorio consiste en 2 laboratorios diferenciados cuyos datos de acceso se pueden encontrar en la siguiente URL introduciendo el token que se habrá facilitado previamente a cada asistente.

https://workshop.fortinetdemo.es

## Indice de laboratorios a completar
* T1_FortiWeb: protección WEB y protección avanzada de APIs
* T2_FortiDAST: comprobaremos de forma dinámica la seguridad de nuestras aplicaciones web
* T3_FortiGSLB: añadiremos una entrada DNS para las aplicaciones

## Diagrama general de los laboratorios

A continuación se recoge el diagrama general de los laboratorios disponibles para cada usuario:

![architecture overview](images/image0.png)

- Cada usuario dispone de dos aplicaciones desplegadas en AWS publicadas a través de un FortiGate
- Dicho FortiGate dispone de una VIP para publicar dichas aplicaciones en los puertos 31.000 y 31.001

## T1: [T1_fortiweb](./T1_fortiweb)

En este laboratorio llevaremos a cabo las siguientes tareas:

- Creación de una nueva aplicación en FortiWeb Cloud con origen la aplicación web (DVWA) desplegada para cada usuario 
- Creación de una nueva aplicación en FortiWeb Cloud con origen la API (swagger pet store API) desplegada para cada usuario
- Añadiremos los perfiles de seguridad necesarios para proteger la aplicación Web y la API publicadas
- Creación de los FQDN asociados a cada aplicación para apuntar a la entrada de FortiWeb Cloud correspondiente
- Pruebas de carga contra FortiWeb para que aprenda los patrones de tráfico pueda aplicar protección avanzada no basada en firmas, mediante ML
- Ejercicios de RedTeam para probar la eficacia de la protección

## T2: [T2_fortidast](./T2_fortidast)

En este laboratorio llevaremos a cabo las siguientes tareas:

- Integración de FortiWeb Cloud con el servicio de análisis de vulnerabilidades de FortiDAST
- Lanzamiento de escaneo sobre las aplicaciones desplegadas para identificar potenciales riesgos de las aplicaciones
- Análisis de los resultados de los análisis

## T3: [T3_fortigslb](./T3_fortigslb)

En este laboratorio realizaremos lo siguiente:
- **IMPORTANTE** se debe haber completado con éxito el laboratorio: T2
- Creación de un nuevo conector contra el FortiGate de laboratorio.
- Creación de un nuevo virtual server.
- Añadir una nueva entrada servidor al servicio GSLB ya configurado en FortiGSLB.
- Comprobación que el servicio añade a la resolución DNS de la aplicación nuestra IP.

# Support
This a personal repository with goal of testing and demo Fortinet solutions on the Cloud. No support is provided and must be used by your own responsability. Cloud Providers will charge for this deployments, please take it in count before proceed.

