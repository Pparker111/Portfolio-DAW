
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Integración Tomcat + Servidor web de Apache Tomcat</h1>

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

Accede a las interfaces Manager y Host Manager.
Investiga sus funciones principales: despliegue, recarga, parada, creación de hosts virtuales.
Elabora una ficha descriptiva de cada herramienta.

---

## 🛠 Guía de Acceso y Funciones: Manager App y Host Manager App

Para este ejercicio, utilizaremos tu cuenta de administrador (`admin`) que tiene los roles `manager-gui` (para la gestión de aplicaciones) y `admin-gui` (para la gestión de hosts virtuales).

### 1. Acceso a las Interfaces de Gestión

Accede a cada una de las interfaces a través del navegador:

| Interfaz | URL de Acceso (HTTP) | Rol Requerido |
| :--- | :--- | :--- |
| **Manager App** | `http://localhost:8080/manager/html` | `manager-gui` |
| **Host Manager App** | `http://localhost:8080/host-manager/html` | `admin-gui` |

### 2. Investigación de Funciones Principales

A continuación, se describen las funciones principales de cada herramienta, necesarias para completar la ficha descriptiva.

#### A. 🖥 Manager App: Gestión del Ciclo de Vida de Aplicaciones

La **Manager App** es la herramienta fundamental para el **despliegue, control y monitorización** de las aplicaciones web (`.war` o directorios de contexto) que se ejecutan bajo un Host Virtual específico (generalmente, `localhost`).

| Función Principal | Descripción Técnica |
| :--- | :--- |
| **Despliegue (Deploy)** | Permite la instalación de una nueva *webapp* subiendo un archivo `.war` directamente (Despliegue por WAR File) o especificando la ruta local del archivo o un directorio de contexto (Despliegue por Ruta). |
| **Recarga (Reload)** | Reinicia el contexto de una aplicación específica sin necesidad de detener y volver a iniciar todo el servicio Tomcat. Es útil para aplicar cambios menores sin interrumpir otras aplicaciones. |
| **Parada (Stop)** | Detiene la ejecución del código de una aplicación (la *webapp* ya no es accesible) pero **mantiene el contexto desplegado** en el servidor. |
| **Inicio (Start)** | Reactiva una aplicación que fue previamente detenida. |
| **Despliegue (Undeploy)** | Elimina completamente el contexto de la aplicación del servidor y borra su directorio de despliegue. |
| **Sesiones** | Muestra el número de sesiones HTTP activas para cada aplicación, permitiendo invalidarlas individualmente. |

#### B. 🌐 Host Manager App: Gestión de Hosts Virtuales

La **Host Manager App** se encarga de la configuración del motor de **Hosts Virtuales** dentro de Tomcat. Esto permite que una única instancia de Tomcat pueda servir contenido para múltiples nombres de dominio (como si fueran servidores web independientes).

| Función Principal | Descripción Técnica |
| :--- | :--- |
| **Listado de Hosts** | Muestra todos los Hosts Virtuales configurados en la instancia de Tomcat (por defecto, solo `localhost`). |
| **Creación de Hosts** | Permite **Añadir un Nuevo Host** especificando su **Nombre** (el dominio, ej., `mi-dominio.com`) y su `appBase` (la ruta del directorio donde buscará los archivos WAR para ese host). |
| **Parada de Host** | Detiene la ejecución de un Host Virtual completo, lo cual **detiene automáticamente todas las aplicaciones** desplegadas bajo ese Host. |
| **Recarga de Host** | Recarga la configuración de un Host Virtual. |

---

## 📄 Ficha Descriptiva de Herramientas de Gestión

Aquí está la ficha completa que documenta las funciones de cada herramienta.

### I. 🖥 Ficha Descriptiva: Manager App (Gestión de Aplicaciones)

| Atributo | Descripción/Función Principal |
| :--- | :--- |
| **Propósito** | Gestión del **ciclo de vida de las aplicaciones web (Contextos)** (`.war`) desplegadas bajo un Host Virtual. |
| **Acceso URL** | `/manager/html` |
| **Rol Mínimo** | `manager-gui` |
| **Funciones Clave** | * **Despliegue (Deploy):** Instalación de nuevas aplicaciones web. * **Control del Ciclo de Vida:** **Stop** (detener ejecución), **Start** (iniciar ejecución), **Reload** (reinicio suave del contexto) y **Undeploy** (desinstalar/borrar aplicación). * **Monitorización de Sesiones:** Muestra y permite invalidar las sesiones HTTP activas por *webapp*. |

### II. 🌐 Ficha Descriptiva: Host Manager App (Gestión de Hosts Virtuales)

| Atributo | Descripción/Función Principal |
| :--- | :--- |
| **Propósito** | Configuración y gestión del motor de **Hosts Virtuales** de Tomcat. Permite que la misma instancia del servidor aloje múltiples dominios. |
| **Acceso URL** | `/host-manager/html` |
| **Rol Mínimo** | `admin-gui` |
| **Funciones Clave** | * **Creación de Hosts:** Añadir nuevos Hosts Virtuales, definiendo su nombre de dominio y la ruta base (`appBase`). * **Control del Ciclo de Vida:** **Detener/Iniciar** un Host Virtual. * **Listado de Hosts:** Supervisión del estado operativo de cada Host Virtual configurado. |

---

Una vez que has accedido y verificado estas funciones, el ejercicio está completado. ¿Quieres que te ayude a crear la documentación final para este segundo ejercicio?
