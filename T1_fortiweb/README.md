# Hands on Lab Seguridad Aplicaciones Web & API
## Objetivo del laboratorio
El objetivo de este laboratorio es conocer la propuesta de valor de Fortinet para proteger aplicaciones web y APIs a través de sus soluciones de FortiWeb Cloud, FortiDAST y FortiGSLB. Durante el workshop se publicarán 2 aplicaciones de forma segura a través de FortiWeb Cloud simulando un entorno corporativo, con un portal web y una API. En el proceso aprenderás a entrenar el modelo de Machine Learning (ML) de la API para conocer el esquema OpenAPI de la misma y aplicar mecanismos de protección sobre el mismo y también cómo proteger portales web frente a ataques TOP10 OWASP y otros ataques sofisticados.

Al margen de ello podremos comprobar el valor añadido que puede aportar nuestro servicio FortiDAST para evaluar de forma continua la postura de seguridad de nuestras aplicaciones y APIs y como nuestro servicio de Global Service Load Balancing (GSLB) mediante DNS, FortiGSLB, puede facilitar una publicación más eficiente de la aplicación en entornos distribuidos facilitando siempre el mejor acceso en base a la ubicación del usuario o a los health checks definidos para cada aplicación. 

El formato del laboratorio consiste en 3 laboratorios diferenciados cuyos datos de acceso se pueden encontrar en la siguiente URL introduciendo el token que se habrá facilitado previamente a cada asistente.

https://workshop.fortinetdemo.es

Una vez dentro del portal para conocer el detalle del entorno de cada usuario se debe consultar el fichero Credenciales_Acceso.xlsx disponible en este enlace y cuya contraseña es la misma que el token facilitado:

[Credenciales_acceso](https://drive.google.com/file/d/1mDOx0rxCCXkXzKrHJvAoF-t-Smxg2f-2/view?usp=sharing)

## Indice de laboratorios a completar
* **T1_fortiweb**: protección WEB y protección avanzada de APIs
* T2_FortiDAST: comprobaremos de forma dinámica la seguridad de nuestras aplicaciones web
* T3_FortiGSLB: añadiremos una entrada DNS para las aplicaciones

## T1: [T1_fortiweb](./T1_fortiweb)

En este laboratorio llevaremos a cabo las siguientes tareas:

- Creación de una nueva aplicación en FortiWeb Cloud con origen la aplicación web (DVWA) desplegada para cada usuario 
- Creación de una nueva aplicación en FortiWeb Cloud con origen la API (swagger pet store API) desplegada para cada usuario
- Añadiremos los perfiles de seguridad necesarios para proteger la aplicación Web y la API publicadas
- Creación de los FQDN asociados a cada aplicación para apuntar a la entrada de FortiWeb Cloud correspondiente
- Pruebas de carga contra FortiWeb para que aprenda los patrones de tráfico pueda aplicar protección avanzada no basada en firmas, mediante ML
- Ejercicios de RedTeam para probar la eficacia de la protección

## Pasos a seguir:

## 1. Conexión al servicio de FortiWeb Cloud

- Para acceder al servicio de FortiWeb Cloud se ha habilitado un acceso IAM donde cada usuario tendrá sus propias credenciales de acceso bajo una cuenta matriz donde se encuentra el servicio
- El acceso a FortiWeb Cloud puede llevarse a cabo desde la siguiente URL: [FortiWEB Cloud](http://www.fortiweb-cloud.com/)
- Seleccionamos la opción Login

![image1-3.png](images/image1-3.png)

- En las opciones de Login seleccionaremos la opción IAM Login y utilizaremos las credenciales que nos facilita el portal

![image1-2-2.png](images/image1-2-2.png)

![image1-1.png](images/image1-1.png)

En este primer login será necesario validar la cuenta de cada usuario para lo que se debe facilitar un token que se envía a la cuenta de correo del usuario. Para consultar dicho token se debe acceder al correo electrónico del usuario en el servidor https://mail.fortinetdemo.es con las mismas credenciales de acceso que se han facilitado en el fichero de credenciales

![image1-4.png](images/image1-4.png)

## 2. Creación aplicación portal Web (DVWA)

- Comprueba que tu aplicación es accesible desde Internet. Puedes encontrar la url a la misma en los datos del laboratorio: Acceso a tus aplicaciones > dvwa_url
- La creación de una nueva aplicación en FortiWeb Cloud es bastante sencilla. En este laboratorio realizaremos el alta via GUI en el portal, pero se puede automatizar realizando peticiones a la API del servicio. [FortiWEB Cloud API reference](http://www.fortiweb-cloud.com/apidoc/api.html)
- En el menú de la izquierda seleccionaremos `Global > Applications`

![image2-1-2.png](images/image2-1-2.png)

- Dentro de la sección Aplicaciones, clicaremos en `ADD APPLICATION` para arrancar el wizard de alta de la aplicación.

![image2-2.png](images/image2-2.png)

- Wizard step 1: Nombre de aplicación y dominio

    * Web Application Name: `user_id`-dvwa (Usuario FortiCloud asignado, ejemplo: fortixpert0-dvwa) - este valor no es más que un identificador dentro del portal para organizar las diferentes aplicaciones
    * Domain Name: `user_id`-dvwa.hol.fortinetdemo.es (ejemplo: fortixpert0-dvwa.hol.fortinetdemo.es) - este valor es el que va a determinar que FQDN va a tener nuestra aplicación y que posteriormente a nivel DNS redirigiremos a FortiWeb Cloud

![image2-3.png](images/image2-3.png)

- Wizard step 2: Protocolo, puertos e IP origen del servidor
  
    * Services allowed: HTTP y HTTPS (FWB Cloud se apoya en Let's Encrypt o en certificados propios del cliente para asignar el certificado a la aplicación de forma automática)
    * IP Address: (IP pública de la aplicación, ejemplo: dvwa_url  = http://***15.188.151.180***:31000)
    * Port: 31000 (Puerto TCP, ejemplo: dvwa_url  = http://15.188.151.180:***31000*** ))
    * Test Server: (comprobar conexión al servidor usando HTTP)

Nota: puedes obtener la IP pública de tu aplicación DVWA en los datos de laboratorio, en la parte de "Acceso a tus aplicaciones > dvwa_url"

![image2-4-2.png](images/image2-4-2.png)

- Wizard step 3: CDN
    * NO habilitaremos servicios de CDN
    * La plataforma nos ofrecerá como ubicación para nuestra instancia la región más cercana a nuestra aplicación (para el workshop usamos las regiones de Irlanda, Londres y París)

![image2-5.png](images/image2-5.png)

- Wizard step 4: Habilitar modo bloqueo y template
    * Enable Block mode: ON (habilitamos la protección)
    * Template: `dvwa-hol-template` (selecionamos este template en el desplegable)
    * NOTA: el template permite definir qué funcionalidades de FortiWeb Cloud queremos habilitar para nuestra aplicación

![image2-6.png](images/image2-6.png)

- Completado: 
    * El resultado es un nuevo FQDN que genera el servicio de FortiWeb Cloud de nuestra applicación para acceder a través de FortiWEB Cloud y poder actualizar nuestros servidores DNS.
    * Desde el nuevo FQDN fqdn podremos acceder a nuestra aplicación a través de FortiWEB Cloud.

Nota: copiar el nuevo FQDN para utilizar en el siguiente punto donde modificaremos los registros DNS de nuestro dominio

![image2-7.png](images/image2-7.png)

- En el menú general de aplicaciones podremos ver cómo FortiWeb Cloud de forma automática ha seleccionado el centro de limpieza de FortiWeb Cloud más cercano a la aplicación y en el mismo Cloud Provider. A continuación se facilita un listado con los diferentes scrubbing centers disponibles a día de hoy ([FortiWeb Cloud scrubbing centers](https://docs.fortinet.com/document/fortiweb-cloud/24.2.0/user-guide/847410/restricting-direct-traffic-allowing-fortiweb-cloud-ip-addresses))

![image2-8.png](images/image2-8.png)


### 2.1. Creación de nuevo CNAME para aplicación DVWA

Para facilitar el acceso seguro a la nueva aplicación a través de FortiWeb Cloud, vamos a añadir un nuevo CNAME en la zona DNS reservada para el workshop, que resuelva al FQDN proporciando por FortiWeb Cloud para nuestra aplicación. Para ello vamos a usar FortiGSLB como servidor DNS del dominio hol.fortinetdemo.es

- Con el mismo IAM user con el que hemos accedido a FortiWeb Cloud es posible acceder al servicio SaaS de FortiGSLB en la url [FortiGSLB](http://www.fortigslb.com/)

![image1-2.png](images/image1-2.png)

**Recuerda: acceso del tipo IAM login**

- Selecciona la organización HoL.

![image2-9.png](images/image2-9.png)

- Selecciona "DNS Services" en el menú de la izquierda, donde aparecen las zonas DNS relacionadas con esta organización.

![image2-10.png](images/image2-10.png)

- Selecciona la entrada "HoL" y editala para añadir una nueva entrada, bóton lapiz que encontrarás a la derecha. 

![image2-11.png](images/image2-11.png)

- En la parte inferior de la entrada, tras los parámetros de definición de la misma, encontrarás el menú para crear un nuevo registro de tipo CNAME. 

![image2-12.png](images/image2-12.png)

- La entrada CNAME corresponde al FQDN que has obtenido al dar de alta la aplicación en FortiWeb Cloud.
  
    * Alias: `user_id`-dvwa (ejemplo: fortixpert0-dvwa)
    * Target: `<FortiWeb Cloud FQDN>` (ejemplo: fortixpert0-dvwa.hol.fortinetdemo.es.P911111111.fortiwebcloud.net.)

![image2-13.png](images/image2-13.png)

> [!WARNING]
> Es importante que incluyas un "." al final de la entrada de Target

> [!WARNING]
> Es un entorno compartido, por favor no modifiques nada que no sea lo indicado en la guía o podrás afectar al resto de usuarios

- Una vez creada la entrada, ya puedes comprobar como la resolución de la nueva entrada, (ejemplo: fortixpert0-dvwa.hol.fortinetdemo.es), apunta al FQDN de la aplicación creada en FortiWeb Cloud.

- En este punto podrás comprobar al acceder a la aplicación a través de HTTPS como FortiWeb ha desplegado el certificado requerido para securizar la comunicación con la aplicación. Inicialmente se instala un certificado propio de FortiWeb y en cuestión de minutos se podrá comprobar como se despliega el certificado de Let's Encrypt de forma automatizada

> [!TIP]
> Si no has copiado el CNAME que ha generado FortiWeb para la aplicación puedes ir al menú "NETWORK > Endpoints" y recuperarlo.

## 2.2. Red Team sobre Aplicación Web

En este punto vamos a validar el correcto funcionamiento de los mecanismos de seguridad que hemos habilitado en FortiWeb. Empezamos los ejercicios de Red Team sobre las aplicaciones publicadas!

## Injection atacks

Los ataques de inyección ocurren cuando un atacante envía ataques simples basados en texto que explotan la sintaxis del intérprete objetivo. Casi cualquier fuente de datos puede ser un vector de inyección, como variables de entorno, parámetros, servicios web externos e internos, y todo tipo de usuarios. Las fallas de inyección ocurren cuando una aplicación envía datos no confiables a un intérprete. Las fallas de inyección son muy comunes, especialmente en código heredado. A menudo se encuentran en consultas SQL, LDAP, Xpath o NoSQL; comandos del sistema operativo; analizadores XML, encabezados SMTP, argumentos de programa, etc. Las fallas de inyección son fáciles de descubrir al examinar el código, pero más difíciles de descubrir mediante pruebas. Los escáneres y los fuzzers pueden ayudar a los atacantes a encontrar fallas de inyección. La inyección puede provocar pérdida o corrupción de datos, falta de responsabilidad o denegación de acceso. La inyección a veces puede llevar a la toma completa del host. [OWASP A03_2021-Injection] (https://owasp.org/Top10/A03_2021-Injection/)

Accede a tu aplicación DVWA que has dado de alta en FortiWeb Cloud en pasos anteriores: _fortixpert#-dvwa.hol.fortinetdemo.es_

> [!NOTE]
> Si es la primera vez que accedes al portal: Username: "root" (con password en blanco) y selecciona la opción Create / Reset Database

![image3-1-1-1-2.png](images/image3-1-1-1-2.png)

> Si ya has accedido y reseteado el acceso: Username: "admin" Password "password"

### 2.2.1 SQL Injection attack (sitio desprotegido)

**Deshabilita el modo bloqueo en tu aplicación dentro de FortiWeb Cloud si accedes a través de este frontal**

![image3-1-1-1-3.png](images/image3-1-1-1-3.png)

DVWA tiene un módulo simple utilizado para demostrar ataques de inyección SQL que espera valores de user-id como enteros (por ejemplo, 1, 2, 3). La aplicación mostrará información sobre el usuario asociado con un user-ID dado. En el siguiente ejercicio, inyectaremos comandos SQL para acceder a las contraseñas asociadas con los nombres de usuario.

Accede a tu aplicación DVWA que has dado de alta en FortiWeb Cloud en pasos anteriores: _fortixpert#-dvwa.hol.fortinetdemo.es_

![image3-1-1-1.png](images/image3-1-1-1.png)

Selecciona nivel de seguridad bajo para evitar que la propia aplicación aplique controles de seguridad sobre los comandos que vamos a ejecutar

![image3-1-1-2.png](images/image3-1-1-2.png)

Accede a la sección SQL Injection e introduce el siguiente texto en el campo USER ID: `% 'or '1'='1' -- ';`

> [!NOTE]
> En este escenario, estamos diciendo "mostrar todos los registros que sean falsos y todos los registros que sean verdaderos".
> %’ - Probablemente no será igual a nada y será falso.
> ‘1’=‘1’ - Es igual a verdadero, porque 1 siempre será igual a 1.

![image3-1-1-1-4.png](images/image3-1-1-1-4.png)

- Introduce el siguiente texto en el campo USER ID: `'or '1'='1' union select null, user() #'`

![image3-1-1-1-5.png](images/image3-1-1-1-5.png)

- Usa el siguiente comando para determinar el nombre de la base de datos: `%'or '1'='1' union select null, database() #'`

![image3-1-1-1-6.png](images/image3-1-1-1-6.png)

### 2.2.2 Injection atacks (sitio protegido)

¿Qué pasa si vuelves a lanzar los mismos ataques que en el punto 2.2.1, pero activando el modo bloqueo en FortiWeb Cloud?

### 2.2.3 Command Injection attack (sitio desprotegido)

**Deshabilita el modo bloqueo en tu aplicación dentro de FortiWeb Cloud si accedes a través de este frontal**

DVWA tiene un módulo simple utilizado para demostrar ataques de inyección de comandos que espera que un usuario introduzca una dirección IP. La aplicación luego enviará un ping a la dirección IP proporcionada. En el siguiente ejercicio, inyectaremos comandos además de la dirección IP que espera el módulo.

Accede a tu aplicación DVWA que has dado de alta en FortiWeb Cloud en pasos anteriores: _fortixpert#-dvwa.hol.fortinetdemo.es_

![image3-1-3-1.png](images/image3-1-3-1.png)

- Introduce el siguiente texto que va a permitir ejecutar un comando "pwd" y obtener información sobre el direcotrio donde esta la aplicación desplegada: `4.2.2.2; pwd`

![img-2-2-3.png](images/img-2-2-3.png)

- Pregunta:
    
    - ¿Sería posible ejecutar un comando como “nc” (netcat) y abrir una shell en el sistema?
    - ¿Sería posible consultar los metadatos de la instancia que este ejecutando el microservicio y obtener credenciales?

<details>
  <summary>Commandos:</summary>
  127.0.0.1; curl http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance
</details>


### 2.2.4 Command Injection attack (sitio protegido)

¿Qué pasa si vuelves a lanzar los mismos ataques que en el punto 3.1.3, pero activando el modo bloqueo en FortiWeb Cloud?

## Cross-Site Scripting (XSS) attacks 

Los ataques de Cross-Site Scripting (XSS) son un tipo de inyección en el cual se insertan scripts maliciosos en sitios web aparentemente benignos y de confianza. Los ataques XSS ocurren cuando un atacante utiliza una aplicación web para enviar código malicioso, generalmente en forma de script del lado del navegador, a un usuario final diferente. Las fallas que permiten que estos ataques tengan éxito son bastante comunes y pueden ocurrir en cualquier lugar donde una aplicación web permita la entrada de un usuario dentro de la salida que genera, sin validar ni codificarla.

Un atacante puede usar XSS para enviar un script malicioso a un usuario desprevenido. El navegador del usuario final no tiene forma de saber que el script no debe ser confiable y ejecutará el script. Debido a que el navegador cree que el script proviene de una fuente confiable, el script malicioso puede acceder a cualquier cookie, token de sesión u otra información sensible retenida por el navegador y utilizada con ese sitio. Estos scripts incluso pueden reescribir el contenido de la página HTML.

Si bien el objetivo de un ataque XSS siempre es ejecutar JavaScript malicioso en el navegador de la víctima, existen algunas formas fundamentalmente diferentes de lograr ese objetivo.

Los ataques XSS a menudo se dividen en tres tipos: XSS Persistente: donde la cadena maliciosa proviene de la base de datos del sitio web. XSS Reflejado: donde la cadena maliciosa proviene de la solicitud del usuario. El sitio web luego incluye esta cadena maliciosa en la respuesta enviada de vuelta al usuario. XSS basado en DOM: donde la vulnerabilidad está en el código del lado del cliente en lugar del código del lado del servidor. El XSS basado en DOM es una variante tanto de XSS persistente como de XSS reflejado. En un ataque XSS basado en DOM, la cadena maliciosa no se analiza realmente hasta que se ejecuta el JavaScript legítimo del sitio web. (https://owasp.org/www-community/attacks/xss/) Aquí hay un excelente análisis sobre XSS: https://excess-xss.com/

### 2.2.5 XSS attack (sitio desprotegido)

Accede a tu aplicación DVWA que has dado de alta en FortiWeb Cloud en pasos anteriores: _fortixpert#-dvwa.hol.fortinetdemo.es_

- Haz clic en la pestaña de seguridad de DVWA y asegúrate de que el nivel de seguridad esté configurado en Bajo.
- Haz clic en la pestaña XSS (Reflejado) a la izquierda para lanzar el módulo.
- Introduce un texto, ejemplo "john", para probar la funcionalidad del módulo.

![image3-1-3-5.png](images/image3-1-3-5.png)

- Vas a lanzar un XXS attach injectando el siguiente texto en el campo “what’s your name? “Field: `<script>alert(12345)</script>`

### 2.2.6 XSS attack (sitio protegido)

¿Qué pasa si vuelves a lanzar los mismos ataques que en el punto 3.1.5, pero activando el modo bloqueo en FortiWeb Cloud?

> [!TIP]
> Si has llegado a este punto del labaratorio,sin haber lanzado ningún ataque sobre tu aplicación, puedes hacerlo ahora intentando abrir estas URLs en tu navegador.

<detail>
<summary>URL:</summary>
- SQL injection: http://fortixpert`X`.hol.fortinetdemos.es/?p=1 or 1=1
- XSS: http://fortixpert`X`.hol.fortinetdemos.es/?p=<script>Alert("HACK")<\script>
- Command Injection: http://fortixpert`X`.hol.fortinetdemos.es/?p=cmd.exe
- Known Exploits: http://fortixpert`X`.hol.fortinetdemos.es/?wp-verify-link=test
- Trojan: http://fortixpert`X`.hol.fortinetdemos.es/?act=encoder

Donde fortixpert`X` corresponde a tu user_id del laboratorio.
<detail>


## 2.3 Observabilidad en FortiWEB

Una de las caracteristicas principales de FortiWeb Cloud, es [Threats Analytics](https://docs.fortinet.com/document/fortiweb-cloud/24.2.0/user-guide/920966/threat-analytics), que utiliza algoritmos de aprendizaje automático para identificar patrones de ataque en todos los activos de tu aplicación y los agrupa en incidentes de seguridad, asignándoles una gravedad. Ayuda a distinguir las amenazas reales de las alertas informativas y los falsos positivos, permitiéndote concentrarte en las amenazas que son importantes.

Principales ventajas:

    - Simplifica la detección y respuesta a amenazas.
    - Acelera la investigación de alertas de seguridad.
    - Ayuda a los analistas a concentrarse en las amenazas más importantes.
    - Proporciona sugerencias para fortalecer la seguridad basadas en hallazgos.
    - Ingiere eventos de todos tus entornos de nube híbrida.
    - Alivia la fatiga por alertas.

El acceso a Threat Analytics se realiza a través del portal de FortiWeb-Cloud, donde encontrarás los registros de ataques.

![image3-2-1.png](images/image3-2-1.png)

Chequea la IP desde la que se han lanzado los ataques.

![image3-2-2.png](images/image3-2-2.png)

### Afinamiento de falsos positivos
Desde los logs de ataques, es posible crear excepciones de una manera sencilla.

![image3-2-3.png](images/image3-2-3.png)

Todas las excepciones configuradas se reflejan desde `SECURITY RULES > Known Attacks`. Si el log de la aplicación, al que estamos creando la excepción, tiene asigando un template, estas quedarán reflejadas en dicho template y será donde deberiamos resetearlas para volver para la configuración incial del mismo.

También desde `FortiView` dentro de cada una de las aplicaciones, es posible encontrar información detallada sobre los ataques detectedados. 

![image3-2-5.png](images/image3-2-5.png)

### 3 Creación de aplicación API (Swagger Pet Store)

- Comprueba que tu aplicación es accesible desde Internet, puedes encontrar la url a la misma en los datos del laboratorio: Acceso a tus aplicaciones > api_url
- La creación de una nueva aplicación en FortiWeb Cloud es bastante sencilla. En este laboratorio realizaremos el alta via GUI en el portal, pero se puede automatizar realizando peticiones a la API del servicio. [FortiWEB Cloud API reference](http://www.fortiweb-cloud.com/apidoc/api.html)

Para dar de alta la aplicación, seguirás los mismos pasos que en el punto anterior. 

***Repetir los mismos pasos que en el punto 2.1 con las variaciones indicadas a continuación***

Cosas que debes tener en cuenta:

- Web Application Name: `user_id`-api (Usuario FortiCloud asignado, ejemplo: fortixpert0-api)
- Domain Name: `user_id`-api.hol.fortinetdemo.es (ejemplo: fortixpert0-api.hol.fortinetdemo.es)

- Template de protección a aplicar en FortiWEB Cloud: ***api-hol-template***

- DNS Alias: `user_id`-api (ejemplo: fortixpert0-api)

### 3.1. Creación de nuevo CNAME para aplicación DVWA

Para facilitar el acceso seguro a la nueva aplicación a través de FortiWeb Cloud, vamos a añadir un nuevo CNAME en la zona DNS reservada para el workshop, que resuelva al FQDN proporciando por FortiWeb Cloud para nuestra aplicación. Para ello vamos a usar FortiGSLB como servidor DNS del dominio hol.fortinetdemo.es

- Con el mismo IAM user con el que hemos accedido a FortiWeb Cloud es posible acceder al servicio SaaS de FortiGSLB en la url [FortiGSLB](http://www.fortigslb.com/)

![image1-2.png](images/image1-2.png)

**Recuerda: acceso del tipo IAM login**

- Selecciona la organización HoL.

![image2-9.png](images/image2-9.png)

- Selecciona "DNS Services" en el menú de la izquierda, donde aparecen las zonas DNS relacionadas con esta organización.

![image2-10.png](images/image2-10.png)

- Selecciona la entrada "HoL" y editala para añadir una nueva entrada, bóton lapiz que encontrarás a la derecha. 

![image2-11.png](images/image2-11.png)

- En la parte inferior de la entrada, tras los parámetros de definición de la misma, encontrarás el menú para crear un nuevo registro de tipo CNAME. 

![image2-12.png](images/image2-12.png)

- La entrada CNAME corresponde al FQDN que has obtenido al dar de alta la aplicación en FortiWeb Cloud.
  
    * Alias: `user_id`-dvwa (ejemplo: fortixpert0-dvwa)
    * Target: `<FortiWeb Cloud FQDN>` (ejemplo: fortixpert0-dvwa.hol.fortinetdemo.es.P911111111.fortiwebcloud.net.)

![image2-13.png](images/image2-13.png)

> [!WARNING]
> Es importante que incluyas un "." al final de la entrada de Target

> [!WARNING]
> Es un entorno compartido, por favor no modifiques nada que no sea lo indicado en la guía o podrás afectar al resto de usuarios

- Una vez creada la entrada, ya puedes comprobar como la resolución de la nueva entrada, (ejemplo: fortixpert0-dvwa.hol.fortinetdemo.es), apunta al FQDN de la aplicación creada en FortiWeb Cloud. 

> [!TIP]
> Si no has copiado el CNAME que ha generado FortiWeb para la aplicación puedes ir al menú "NETWORK > Endpoints" y recuperarlo.

## 3.2 Entrenamiento del módelo ML de API
El template de seguridad aplicado para vuestra aplicación API, lleva activado la protección de APIs mediante Machine Learning. Para que el modelo pueda aprender el patron de tráfico de la aplicación, vamos a forzar cierto tráfico mediante un par de script. Para revisar el template podeis hacerlo desde el menú de la izquierda `GLOBAL > templates`

![image3-3-0.png](images/image3-2-1.png)

Seleccionar el template `api-hol-template` y revisar los profile de seguridad aplicados en el menú de la izquierda, en este caso el que aplica a este punto es el de `API PROTECTION > ML Based API Protection`

### 3.2.1 Lanzar los scripts de entrenamiento y aprendizaje

- En la carpeta scripts de la guia del laboratorio, podrás encontrar script en bash o PowerShell.
- Copia los scripts para ejecutarlos desde tu PC. (Si tieneas algún problema con esto, pregunta para que te demos acceso a un entorno linux).
- Debes copiar dos scripts, que serán los que lancen las del entrenamiento via GET y POST. 

- Añade los permisos de ejecución a los scripts a ejecutar (caso de MAC o Linux):
```sh
chmod +x fwb_training_get.sh
chmod +x fwb_training_post.sh
```
- Ejecutar los scripts: (debes introducir la URL de tu aplicación API en formato correcto, ejemplo: fortixpert0-api.hol.fortinetdemo.es)
```sh
./fwb_training_get.sh <URL de la API> 
```
```sh
./fwb_training_post.sh <URL de la API> 
```

### 3.2.2 Comprobación de los patrones aprendidos

**IMPORTANTE: es posible que para los participantes que estén en zonas que no sean Irlanda (eu-west-1), la plataforma tarde hasta 30 minutos en mostrar los resultados del aprendizaje. Si es tu caso, puedes avanzar en el laboratorio y luego volver a este punto después**

- Iremos a la sección API colection de la aplicación, en el menú de la izquierda `API PROTECTION > ML Based API Protection`

![image3-3-1.png](images/image3-3-1.png)

- Cuando haya pasado un tiempo desde el lanzamiento de los scripts de entranmiento, que dependerá de la zona en la que se haya desplegado la aplicación, se presentarán los patrones de tráfico aprendidos por el modelo. 

![image3-3-2.png](images/image3-3-2.png)

- Se puede consultar el esquema API aprendido, incluso lo podemos descargar si fuera necesario, cambiando la vista a `API View` en la parte de la derecha. 

![image3-3-3.png](images/image3-3-3.png)

### 3.2.3 Aplicar bloqueo en las llamadas que no cumplan con el esquema

Por defecto, el esquema aprendido deja la protección en standby, de forma que las peticiones que no cumplan con dicho esquema, no son bloqueadas ni alertadas. Podemos cambiar este comportamiento en `Schema Protection`.

- Dentro de `API Collection`, donde aparecen los modelos aprendidos de API Paths, podemos dar a editar el comportamiento de protección, dandole al boton de editar que aparece a la derecha en la columna Action. 

![image3-3-4.png](images/image3-3-4.png)

- Dentro de la customización del API Path aprendido, entre otras cosas podemos modificar el comportamiento de protección, seleccionandolo en el desplegable de arriba a la derecha. 

![image3-3-5.png](images/image3-3-5.png)

## 3.2.4 Ataques sobre la API

En este apartado vamos a comprobar, como de forma automática, FortiWEB Cloud puede proteger las llamadas a la API, en función a lo aprendido en los patrones de tráfico y al esquema Swagger que ha definido. 

**IMPORTANTE: es posible que para los participantes que estén en zonas que no sean Irlanda (eu-west-1), la plataforma tarde hasta 30 minutos en mostrar los resultados del aprendizaje. Si es tu caso, puedes avanzar en el laboratorio y luego volver a este punto después**

En el punto 3.2.3, se ha modificado el comportamiento de protección frente a llamadas que no cumplan con el esquema. Comprobar este punto para esperar un comportamiento u otro en los siguientes test.

### 3.2.4.1 Query Parameter Violation

```sh
curl -v -X 'GET' 'http://fortixpert0-api.hol.fortinetdemo.es/api/pet/findByStatus?' -H 'Accept: application/json' -H 'Content-Type: application/json'
```

    "status" JSON parameter is missing in the JSON request and is blocked by FortiWeb-Cloud. The expected result is a Request query validation failed status.


### 3.2.4.2 URL Query Parameter Long

    "status" URL query parameter is too long. The expected result, JSON parameter length violation.

```sh
curl -v -X 'GET' 'http://fortixpert0-api.hol.fortinetdemo.es/api/pet/findByStatus?status=ABCDEFGHIJKL' -H 'Accept: application/json' -H 'Content-Type: application/json'
```

### 3.2.4.3 URL Query Parameter Short

    "status" URL query parameter is too short. The expected result is a parameter violation.

```sh
curl -v -X 'GET' 'http://fortixpert0-api.hol.fortinetdemo.es/api/pet/findByStatus?status=A' -H 'Accept: application/json' -H 'Content-Type: application/json'
```

### 3.2.4.4 Cross Site Script in URL

    "status" URL query parameter will carry a Command Injection attack. The expected result is a known signature violation.
```sh
curl -v -X 'GET' 'http://fortixpert0-api.hol.fortinetdemo.es/api/pet/findByStatus?status=<script>alert(123)</script>'  -H 'Accept: application/json' -H 'Content-Type: application/json'
```

### 3.2.4.5 Cross Site Script in Body

    "status" JSON body will carry an XSS attack. The expected result, the attack is being blocked by Machine Learning.

```sh
curl -v -X 'POST' 'http://fortixpert0-api.hol.fortinetdemo.es/api/pet' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"id": 111, "category": {"id": 111, "name": "Camel"}, "name": "FortiCamel", "photoUrls": ["WillUpdateLater"], "tags": [ {"id": 111, "name": "FortiCamel"}], "status": "<script>alert(123)</script>"}'
```

### 3.2.4.6 Zero Day Attacks

    We will now use some sample Zero Day Attacks.

    Cross Site Script in the Body

```sh
curl -v -X 'POST' 'http://fortixpert0-api.hol.fortinetdemo.es/api/pet' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"id": 111, "category": {"id": 111, "name": "Camel"}, "name": "javascript:qxss(X160135492Y1_1Z);", "photoUrls": ["WillUpdateLater"], "tags": [ {"id": 111, "name": "FortiCamel"}], "status": "available”}'
```

## Laboratorio completado

Una vez concluido este laboratoria es hora de Pasar al lab 2: [T2_fortigslb](http://github.com/fortidemoscloud/fwb-fgslb-hol/tree/main/T2_fortigslb)

# Support
This a personal repository with goal of testing and demo Fortinet solutions on the Cloud. No support is provided and must be used by your own responsability. Cloud Providers will charge for this deployments, please take it in count before proceed.


