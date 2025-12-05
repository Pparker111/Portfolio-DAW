
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Investigación y Descripción de Apache Tomcat</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 7 de Noviembre de 2025 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

# **Tomcat: Investigación y Descripción** 🐱🔥

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

### **1. Catalina** 🧩

Catalina es el **contenedor de servlets** de Apache Tomcat desde la versión 4.x.
Implementa las especificaciones de Sun Microsystems para **Servlets** y **JSP**, actuando como el núcleo del servidor:

* 🔁 Gestiona el ciclo de vida de los servlets
* 📦 Carga aplicaciones
* 🔐 Administra sesiones y seguridad

Su arquitecto principal fue **Craig McClanahan** dentro del Proyecto Jakarta.

---

### **2. Coyote** 🐺🌐

Coyote es el **conector HTTP** de Tomcat, compatible con **HTTP/1.1** y **HTTP/2**. Se ocupa de:

* 📡 Escuchar peticiones entrantes
* 📥 Recibir solicitudes HTTP
* 🔀 Reenviarlas al motor Catalina
* 📤 Devolver la respuesta al cliente

También permite que Tomcat funcione como un servidor web sencillo.
El conector **Coyote JK** reenvía peticiones a servidores como Apache HTTPD mediante el protocolo JK, mejorando rendimiento.

---

### **3. Jasper** ⚙️📄

Jasper es el **motor JSP** de Tomcat, encargado de:

* 🔍 Analizar archivos JSP
* 🔧 Compilarlos a servlets
* ♻️ Recompilar automáticamente cuando cambian

Desde Tomcat 5 se utiliza **Jasper 2**, compatible con JSP 2.0, que incorpora mejoras como:

* 🔗 Agrupación de bibliotecas de etiquetas
* 🧵 Compilación en segundo plano
* ♻️ Recompilación inteligente
* 🛠 Uso del compilador Java JDT

---

### **4. Manager y Host Manager** 🛠️🖥️

Tomcat incluye dos herramientas administrativas:

#### **Manager App** 📦

Permite:

* 🚀 Desplegar aplicaciones
* ⏹ Detener
* 🔄 Recargar
* 🗑 Eliminar

#### **Host Manager** 🏠

Permite gestionar **hosts virtuales**.

Para acceder a ambas se deben configurar usuarios en `conf/tomcat-users.xml` con roles como:

* `manager-gui`
* `admin-gui`

Se accede desde:

* `/manager/html`
* `/host-manager/html`

---

### **5. Estructura básica de directorios** 📁📂

| 📁 Directorio | 📌 Descripción                                          |
| ------------- | ------------------------------------------------------- |
| **bin**       | Scripts para iniciar/detener Tomcat (`startup.sh`).     |
| **conf**      | Configuración principal (`server.xml`, `web.xml`).      |
| **lib**       | Librerías JAR compartidas.                              |
| **logs**      | Registros del servidor.                                 |
| **webapps**   | Ubicación de aplicaciones desplegadas (WAR o carpetas). |
| **work**      | Archivos temporales y JSP compilados.                   |
| **temp**      | Archivos temporales del servidor.                       |

---

### **6. Flujo interno de funcionamiento** 🔄⚙️

1. **📥 Recepción de la petición**
   Coyote escucha solicitudes en puertos como 8080.

2. **➡️ Enrutamiento**
   El conector envía la petición al **Engine**.

3. **🏠 Selección del host**
   El Engine selecciona el **Host virtual** adecuado.

4. **📦 Selección del contexto**
   Se identifica la aplicación correspondiente (Context).

5. **🐱 Procesamiento por Catalina**
   Catalina gestiona:

   * Carga del servlet
   * Ejecución del método `service()`
   * Generación de la respuesta

6. **🚀 Despliegue de aplicaciones**
   El *deployer* procesa archivos WAR o carpetas.

7. **📤 Respuesta al cliente**
   El conector envía la respuesta generada.

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
