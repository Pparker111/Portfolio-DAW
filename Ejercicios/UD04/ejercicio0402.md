

# **Tomcat: Identificación de archivos de configuración**

## 📂 **Archivos clave de configuración en Tomcat**

---

## **1. `conf/server.xml` ⚙️**

`server.xml` es el archivo **central de configuración** de Tomcat. Aquí se definen los componentes principales del servidor:

* **Connectors** (como Coyote): puertos, protocolos, timeouts, codificación…
* **Service**: relaciona los Connectors con Catalina.
* **Engine**: componente que procesa las peticiones.
* **Hosts virtuales**: permiten alojar múltiples sitios.
* **Contextos** (aunque no se recomienda definir aquí).

Es el archivo más crítico: un error puede impedir que Tomcat arranque.

---

## **2. `conf/web.xml` 📄**

Este archivo actúa como el **deployment descriptor global** de Tomcat. Define comportamientos por defecto para **todas** las aplicaciones web.

Permite configurar:

* Páginas de error globales.
* Tipos MIME.
* Parámetros por defecto del contenedor.
* Filtros o servlets aplicados globalmente.
* Timeout de sesión.

Cada aplicación puede sobrescribirlo desde su propio `WEB-INF/web.xml`.

---

## **3. `conf/tomcat-users.xml` 🔐**

Aquí se gestionan los **usuarios y roles internos** del servidor Tomcat. Se usa principalmente para acceder a:

* **Manager App**.
* **Host Manager**.
* Otras aplicaciones restringidas.

Permite definir:

* Usuarios: `<user username="" password="" roles="">`
* Roles: `<role rolename="">`

No es recomendable para producción, pero sí para desarrollo y pruebas locales.

---

## **4. `conf/context.xml` 🧩**

Define la **configuración por defecto de los Contextos**, es decir, de todas las aplicaciones desplegadas.

Permite configurar:

* Parámetros de aplicación (`<Parameter>`).
* Recursos: DataSources JDBC, JNDI, conexiones a BD.
* Configuración de sesiones.
* Rutas externas.
* Opciones de seguridad y caching.

Cada aplicación puede tener su propio `META-INF/context.xml`, que sobrescribe este archivo.

---

# 🗺️ **Mapa visual de dependencias (añadir aquí)**

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD04/imagenes/mapa.png)

---
