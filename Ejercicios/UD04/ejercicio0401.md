
Aquí tienes todo el contenido organizado, limpio y con buen formato en **Markdown**, siguiendo la estructura solicitada: **título, enunciado, contenido y bibliografía**.

---

# **Tomcat: Investigación y Descripción**

## **Enunciado**

Investiga y describe brevemente los siguientes elementos de Tomcat:

* Catalina
* Coyote
* Jasper
* Manager y Host Manager
* Estructura básica de directorios (bin, conf, webapps, lib, logs…)
* Flujo interno de funcionamiento: recepción de peticiones, contenedores, despliegue de aplicaciones

---

## **Contenido**

### ## **1. Catalina**

Catalina es el **contenedor de servlets** de Apache Tomcat desde la versión 4.x.
Implementa las especificaciones de Sun Microsystems para **Servlets** y **JSP**, funcionando como el núcleo encargado de gestionar:

* El ciclo de vida de los servlets
* La carga de aplicaciones
* La gestión de sesiones y seguridad

Fue diseñado dentro del Proyecto Jakarta de Apache, y su arquitecto principal fue **Craig McClanahan**.

---

### ## **2. Coyote**

Coyote es el **conector HTTP** de Tomcat, compatible con los protocolos **HTTP/1.1** y **HTTP/2**.
Su función principal es:

* Detectar conexiones entrantes en un puerto TCP
* Recibir solicitudes HTTP
* Enviarlas al motor interno de Tomcat (Catalina)
* Devolver la respuesta al cliente

Coyote permite que Tomcat funcione también como un servidor web básico.
Existe además **Coyote JK**, que reenvía las peticiones a otros servidores como Apache HTTPD mediante el protocolo **JK**, mejorando el rendimiento en arquitecturas mixtas.

---

### ## **3. Jasper**

Jasper es el **motor JSP** de Tomcat. Se encarga de:

* Analizar archivos JSP
* Compilarlos a código Java (servlets)
* Permitir la recarga automática cuando un JSP cambia

Desde Tomcat 5 se utiliza **Jasper 2**, compatible con JSP 2.0, añadiendo:

* Agrupación de bibliotecas de etiquetas JSP
* Compilación en segundo plano
* Recompilación cuando se detectan cambios
* Uso del compilador Java JDT

---

### ## **4. Manager y Host Manager**

Tomcat incluye dos aplicaciones web administrativas:

#### **Manager App**

Permite gestionar aplicaciones web:

* Desplegar
* Detener
* Recargar
* Eliminar

#### **Host Manager**

Permite administrar **hosts virtuales** dentro del servidor Tomcat.

Ambas requieren configurar usuarios y roles en **conf/tomcat-users.xml**, utilizando roles como:

* `manager-gui`
* `admin-gui`

Se acceden normalmente vía navegador en:

* `/manager/html`
* `/host-manager/html`

---

### ## **5. Estructura básica de directorios**

| Directorio  | Descripción                                                                              |
| ----------- | ---------------------------------------------------------------------------------------- |
| **bin**     | Scripts y ejecutables para iniciar, detener y administrar Tomcat (p. ej., `startup.sh`). |
| **conf**    | Archivos de configuración global, como `server.xml` y `web.xml`.                         |
| **lib**     | Librerías JAR compartidas por todo el servidor y aplicaciones.                           |
| **logs**    | Archivos de registro del servidor (acceso, errores, eventos).                            |
| **webapps** | Ubicación de aplicaciones desplegadas (carpetas o archivos WAR).                         |
| **work**    | Archivos temporales, como clases Java generadas al compilar JSP.                         |
| **temp**    | Archivos temporales generados por Tomcat.                                                |

---

### ## **6. Flujo interno de funcionamiento**

El funcionamiento interno de Tomcat sigue este flujo general:

1. **Recepción de la petición**
   El conector Coyote escucha peticiones HTTP en un puerto (como 8080).

2. **Conexión y enrutamiento**
   La solicitud entra al **Connector**, que la envía al **Engine** del servidor.

3. **Procesamiento por el Engine**
   El Engine determina qué **Host virtual** debe atender la petición.

4. **Selección de la aplicación (Contexto)**
   Se identifica el **Context** correspondiente (aplicación dentro de webapps).

5. **Contenedor de servlets (Catalina)**
   Catalina gestiona:

   * La carga del servlet correspondiente
   * La ejecución del método `service()`
   * La respuesta generada

6. **Despliegue de aplicaciones**
   El *deployer* carga archivos WAR o carpetas de aplicación, crea el contexto y lo registra.

7. **Respuesta al cliente**
   La salida del servlet vuelve al conector, que la envía al cliente.

---

## **Bibliografía**

* Wikipedia – Tomcat
  [https://es.wikipedia.org/wiki/Tomcat](https://es.wikipedia.org/wiki/Tomcat)

* Solución error 403 en Tomcat 9 (Videl Cloud)
  [https://videlcloud.wordpress.com/2017/07/17/solucionar-error-403-access-denied-en-tomcat-9/](https://videlcloud.wordpress.com/2017/07/17/solucionar-error-403-access-denied-en-tomcat-9/)

* Tomcat Manager y su configuración (Arquitectura Java)
  [https://www.arquitecturajava.com/tomcat-manager-y-su-configuracion/](https://www.arquitecturajava.com/tomcat-manager-y-su-configuracion/)

* Tomcat en servidores cloud (Arsys)
  [https://www.arsys.es/blog/tomcat-servidores-cloud/](https://www.arsys.es/blog/tomcat-servidores-cloud/)

---

¿Quieres que convierta esto también en PDF, Word o presentación?
