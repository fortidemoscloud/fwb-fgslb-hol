# Hands on Lab FortiWEB Cloud y FortiGSLB
# Protección avanzada de portales WEB y APIs
## Objetivo del laboratorio
El objetivo de este laboratorio es conocer y operar los servicios de Fortinet: FortiWEB Cloud y FortiGSLB. Principalmente nos centraremos en la publicación de 2 aplicaciones de forma segura a través de FortiWEB Cloud, un portal WEB y una API. En el proceso aprenderás a entrenar el modelo de Machine Learning (ML) de la API para conocer el esquema OpenAPI de la misma y aplicar protección al portal web basado en TOP10 OWASP y otros ataques sofisticados.

Por último, configuraras nuestro servicio de Global Service Load Balancing (GSLB) mediante DNS, FortiGSLB, para que los usuarios de la aplicación accedan a la misma siempre a su región más cercana. 

El formato del laboratorio consiste en 2 laboratorios diferenciados y para poder realizarlos encontrarás todos los detalles en la siguiente URL, donde deberás introducir el token facilitado.

https://workshop.fortinetdemo.es

## Indice de laboratorios a completar
* T1_fortiweb: protección WEB y protección avanzada de APIs
* T2_fortigslb: añadiremos una entrada DNS para vuestras aplicaciones

## T1: [T1_fortiweb](./T1_fortiweb)

En este laboratorio realizaremos lo siguiente:
- Creación de una nueva aplicación en FortiWeb Cloud con origen la aplicación WEB (DVWA) desplegada detrás del FortiGate de tu laboratorio. 
- Creación de una nueva aplicación en FortiWeb Cloud con origen la API (swagger pet store API) desplegada detrás del FortiGate de tu laboratorio. 
- Añadiremos los profiles de seguridad necesarios para proteger la aplicación Web y la API. 
- Lanzaremos pruebas de carga contra FortiWeb para que aprenda los patrones de tráfico de la aplicación y pueda aplicar protección avanzada no basada en firmas, mediante ML.
- Ejercicios de RedTeam para probar la eficacia de la protección.

## T2: [T2_fortigslb](./T2_fortigslb)

En este entrenamiento realizaremos lo siguiente:
- **IMPORTANTE** se debe haber completado con éxito el laboratorio: T2
- Creación de un nuevo conector contra el Fortigate de laboratorio.
- Creación de un nuevo virtual server.
- Añadir una nueva entrada servidor al servicio GSLB ya configurado en FortiGSLB.
- Comprobación que el servicio añade a la resolución DNS de la aplicación nuestra IP.

## Diagrama general de los laboratorios

![architecture overview](images/image0.png)

# Support
This a personal repository with goal of testing and demo Fortinet solutions on the Cloud. No support is provided and must be used by your own responsability. Cloud Providers will charge for this deployments, please take it in count before proceed.

