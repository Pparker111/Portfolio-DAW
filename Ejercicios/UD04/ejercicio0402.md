
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Identificación de Archivos de Configuración de Apache Tomcat</h1>

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

## **📝 Enunciado**

Localiza en tu instalación de Tomcat los archivos clave:

* `conf/server.xml`
* `conf/web.xml`
* `conf/tomcat-users.xml`
* `conf/context.xml`

👉 **Para localizarlos hemos usado el comando:**

```
cd /opt/tomcat/conf
ls -l
```
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD04/imagenes/localiza.png)

Explica la función de cada archivo, qué elementos se pueden configurar y elabora un **mapa visual** de las dependencias entre ellos.

---

## **📂 Contenido**

---

## **1. `conf/server.xml` ⚙️ — Configuración principal del servidor**

`server.xml` es el archivo de configuración central de Tomcat. En él se definen los componentes estructurales del servidor:

* **Connectors (Coyote)** → Configuración de puertos, protocolos, timeouts, codificación.
* **Service** → Relación entre Connectors y Catalina.
* **Engine** → Procesamiento de peticiones.
* **Hosts virtuales** → Múltiples sitios en un mismo servidor.
* **Contextos** (no recomendado definir aquí).

Es el archivo más delicado: un error puede impedir que Tomcat arranque.

---

## **2. `conf/web.xml` 📄 — Descriptor de despliegue global**

Este archivo actúa como el archivo de configuración **global** para todas las aplicaciones del servidor.

Permite definir:

* Páginas de error por defecto.
* Tipos MIME.
* Parámetros del contenedor.
* Filtros y servlets globales.
* Timeout de sesión.

Cada aplicación puede sobrescribirlo con su propio `WEB-INF/web.xml`.

---

## **3. `conf/tomcat-users.xml` 🔐 — Gestión de usuarios y roles**

Archivo encargado de gestionar **usuarios internos y roles** de Tomcat. Se utiliza para:

* Acceder a **Manager App**.
* Acceder a **Host Manager**.
* Gestionar autenticación interna.

Configura:

* Roles: `<role rolename="">`
* Usuarios: `<user username="" password="" roles="">`

Se recomienda solo para entornos locales o educativos.

---

## **4. `conf/context.xml` 🧩 — Configuración por defecto de las aplicaciones**

Define parámetros y configuraciones aplicables a **todos** los contextos (aplicaciones).

Permite configurar:

* Parámetros de aplicación.
* Recursos (DataSources JDBC, JNDI, conexiones BD).
* Gestión de sesiones.
* Rutas externas.
* Configuración de seguridad o caching.

Cada aplicación puede sobrescribirlo con `META-INF/context.xml`.

---

## **🗺️ Mapa visual de dependencias**

```
                          ┌─────────────────────┐
                          │      server.xml     │
                          │  - Connectors       │
                          │  - Engine           │
                          │  - Hosts            │
                          └─────────┬───────────┘
                                    │
                        ┌───────────┴───────────┐
                        │                       │
           ┌────────────▼─────────────┐  ┌──────▼───────────┐
           │        context.xml       │  │     web.xml      │
           │ Config. global Contextos │  │  Config. global  │
           │  Recursos, BD, sesiones  │  │ servlets/filtros │
           └────────────┬─────────────┘  └──────────────────┘
                        │
           ┌────────────▼────────────┐
           │    tomcat-users.xml     │
           │    Usuarios y roles     │
           │  Manager & Host Manager │
           └─────────────────────────┘
```

---

## 📚 **Bibliografía**

* Documentación oficial de Apache Tomcat
* [https://tomcat.apache.org/tomcat-9.0-doc/config](https://tomcat.apache.org/tomcat-9.0-doc/config)
* [https://www.arsys.es/blog/tomcat-servidores-cloud](https://www.arsys.es/blog/tomcat-servidores-cloud)
* [https://es.wikipedia.org/wiki/Tomcat](https://es.wikipedia.org/wiki/Tomcat)

