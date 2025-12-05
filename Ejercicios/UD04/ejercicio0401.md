
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Investigación y Descripción de Apache Tomcat</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 5 de Diciembre de 2025 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## **📌 Enunciado**

Investiga y describe brevemente los siguientes elementos de Tomcat:

* Catalina
* Coyote
* Jasper
* Manager y Host Manager
* Estructura básica de directorios (bin, conf, webapps, lib, logs…)
* Flujo interno de funcionamiento: recepción de peticiones, contenedores, despliegue de aplicaciones

---

## **📚 Contenido**

---

## **1. Catalina** 🐱⚙️

Catalina es el **contenedor de servlets** de Tomcat y constituye el núcleo del servidor. Su función es gestionar el ciclo de vida de los servlets, procesar peticiones HTTP, manejar sesiones y ejecutar la lógica de las aplicaciones Java Web. Catalina se encuentra integrado directamente dentro del motor interno de Tomcat, ejecutándose tras la recepción de peticiones desde los conectores. Su configuración principal reside en el archivo `conf/server.xml`, donde se definen engines, hosts y contextos. Todo su funcionamiento está dentro de la estructura base del servidor, especialmente en directorios como `conf`, `webapps` y `work`, donde almacena clases compiladas y metadatos.

---

## **2. Coyote** 🐺🌐

Coyote es el componente que permite a Tomcat funcionar como **servidor HTTP**. Es un conector que escucha en puertos como 8080 y recibe peticiones HTTP/1.1 y HTTP/2, enviándolas después a Catalina para ser procesadas. Se ubica dentro del sistema como parte del módulo de **Connectors**, cuya configuración también se maneja en `conf/server.xml` dentro de etiquetas `<Connector>`. Coyote también es responsable de gestionar la comunicación con clientes externos, actuando como puente entre el navegador y el motor de servlets. Sin Coyote, Tomcat no podría recibir solicitudes web ni enviar respuestas.

---

## **3. Jasper** 📄⚡

Jasper es el motor encargado de procesar **JSP (JavaServer Pages)** dentro de Tomcat. Su función principal es compilar los archivos `.jsp` y convertirlos en clases Java que posteriormente pasan a ser servlets administrados por Catalina. Durante la ejecución, Jasper detecta cambios en los JSP y recompila automáticamente las páginas cuando es necesario. Sus archivos generados se almacenan temporalmente en el directorio `work`, donde se guardan las clases Java y compilaciones resultantes. Jasper está integrado como parte interna del motor web de Tomcat y funciona de manera transparente para el usuario.

---

## **4. Manager y Host Manager** 🛠️🖥️

El **Manager App** es una aplicación web incluida en Tomcat que permite **desplegar, detener, reiniciar y eliminar aplicaciones** directamente desde una interfaz web. El **Host Manager**, por su parte, permite crear y administrar **hosts virtuales** dentro del servidor. Ambos se ubican dentro del directorio `webapps` como aplicaciones preinstaladas (`/webapps/manager` y `/webapps/host-manager`). Para acceder a ellas es necesario configurar usuarios y roles en `conf/tomcat-users.xml`. Su acceso suele realizarse via `/manager/html` y `/host-manager/html`. Son herramientas esenciales para administrar Tomcat sin recurrir a terminal.

---

## **5. Estructura básica de directorios** 📁📂

La estructura de directorios de Tomcat organiza sus componentes internos. El directorio `bin` contiene los scripts de arranque y parada del servidor, mientras que `conf` alberga los archivos de configuración como `server.xml` o `web.xml`. `lib` almacena las librerías JAR compartidas y esenciales para Tomcat y sus aplicaciones. El directorio `logs` guarda registros de actividad y errores del servidor. `webapps` es el lugar donde se despliegan las aplicaciones web (carpetas o archivos WAR). `work` contiene archivos temporales y JSP compilados, y `temp` almacena datos provisionales usados durante la ejecución.

---

## **6. Flujo interno de funcionamiento** 🔄🚀

El flujo interno de Tomcat comienza cuando el conector **Coyote** recibe una petición HTTP. Esta se envía al **Engine**, que determina qué **Host virtual** debe atenderla. Dentro del host se localiza el **Contexto**, que representa la aplicación correspondiente ubicada en `webapps`. Después, la petición pasa al contenedor Catalina, que ejecuta el servlet adecuado mediante su método `service()`. Si la petición requiere una página JSP, Jasper interviene compilándola si es necesario. Finalmente, Catalina genera la respuesta y se devuelve al cliente a través de Coyote. Este proceso coordina todos los módulos y mantiene el funcionamiento del servidor.

---

## **📖 Bibliografía**

* 📚 Wikipedia – Tomcat
  [https://es.wikipedia.org/wiki/Tomcat](https://es.wikipedia.org/wiki/Tomcat)

* 📝 Videl Cloud – Error 403 en Tomcat 9
  [https://videlcloud.wordpress.com/2017/07/17/solucionar-error-403-access-denied-en-tomcat-9/](https://videlcloud.wordpress.com/2017/07/17/solucionar-error-403-access-denied-en-tomcat-9/)

* 🧩 Arquitectura Java – Tomcat Manager
  [https://www.arquitecturajava.com/tomcat-manager-y-su-configuracion/](https://www.arquitecturajava.com/tomcat-manager-y-su-configuracion/)

* ☁️ Arsys – Tomcat en servidores cloud
  [https://www.arsys.es/blog/tomcat-servidores-cloud/](https://www.arsys.es/blog/tomcat-servidores-cloud/)
