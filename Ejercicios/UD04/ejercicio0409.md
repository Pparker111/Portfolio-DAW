
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">📘 Manual de Ingeniería: Arquitectura, Optimización y Despliegue de Apache Tomcat</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 9 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## 📑 Índice de Navegación Rápida

1. [🏗️ Arquitectura del Servidor](#-1-arquitectura-básica-de-tomcat)
2. [⚙️ Configuración y Tuning (Server.xml)](https://www.google.com/search?q=%232-configuraci%C3%B3n-y-tuning-serverxml)
3. [🌐 Integración Web (Proxy Inverso)](https://www.google.com/search?q=%233-integraci%C3%B3n-web-proxy-inverso)
4. [🛡️ Protocolos de Seguridad (Hardening)](https://www.google.com/search?q=%234-protocolos-de-seguridad-hardening)
5. [📈 Pruebas de Carga y Rendimiento](https://www.google.com/search?q=%235-pruebas-de-carga-y-rendimiento)
6. [🐳 Estrategia de Contenedores (Docker)](https://www.google.com/search?q=%236-estrategia-de-contenedores-docker)
7. [🛠️ Mejores Prácticas de Administración](https://www.google.com/search?q=%237-mejores-pr%C3%A1cticas-de-administraci%C3%B3n)

---

## 1. Arquitectura del Servidor

Para administrar Tomcat eficazmente, es necesario comprender cómo fluye una petición desde la red hasta el código Java. Tomcat sigue una jerarquía estricta de contenedores:

* **Server:** Representa la instancia completa de la JVM y el contenedor de servlets.
* **Service:** Agrupa uno o más **Connectors** (puertos de escucha) y un solo **Engine**.
* **Connector:** La interfaz de red (HTTP/1.1 o AJP). Aquí se define el puerto (ej: 8080) y el protocolo.
* **Engine:** El motor que procesa las peticiones recibidas por los conectores.
* **Host:** Representa un dominio virtual (ej: `api.miempresa.com`).
* **Context:** La aplicación web en sí misma (el archivo `.war`).

---

## 2. Configuración y Tuning (Server.xml)

El rendimiento de Tomcat depende en un 80% de la configuración del **Connector** y el **Executor** (Pool de Hilos).

### Configuración Recomendada (`conf/server.xml`)

En lugar de dejar que cada conector gestione sus hilos, definimos un `Executor` compartido para optimizar el uso de CPU:

```xml
<Executor name="tomcatThreadPool" 
          namePrefix="catalina-exec-"
          maxThreads="400" 
          minSpareThreads="25" 
          maxIdleTime="60000"/>

<Connector executor="tomcatThreadPool"
           port="8080" 
           protocol="org.apache.coyote.http11.Http11NioProtocol"
           connectionTimeout="20000"
           acceptCount="200" 
           redirectPort="8443" 
           enableLookups="false"
           compression="on" 
           compressableMimeType="text/html,text/xml,text/plain,application/json"/>

```

**Parámetros Clave:**

* `protocol="...NioProtocol"`: Utiliza entrada/salida no bloqueante (esencial para alta concurrencia).
* `maxThreads`: Límite máximo de procesamiento paralelo.
* `acceptCount`: Cola de espera del sistema operativo cuando el pool de hilos está lleno.

---

## 3. Integración Web (Proxy Inverso)

En entornos de producción, Tomcat **no** debe exponerse directamente a internet. Se debe colocar un servidor web (Nginx o Apache HTTPD) por delante.

### Ventajas:

1. **Terminación SSL/TLS:** Descarga a Tomcat del cifrado/descifrado.
2. **Archivos Estáticos:** Nginx sirve imágenes/CSS/JS mucho más rápido.
3. **Seguridad:** Oculta la topología de la aplicación interna.

**Ejemplo de bloque `server` en Nginx:**

```nginx
upstream tomcat_backend {
    server 127.0.0.1:8080 weight=1;
    server 127.0.0.1:8081 weight=1; # Balanceo de carga
}

server {
    listen 80;
    server_name mi-app.com;

    location / {
        proxy_pass http://tomcat_backend;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }
}

```

---

## 4. Protocolos de Seguridad (Hardening)

Un servidor seguro requiere aplicar capas de defensa antes del despliegue:

1. **Limpieza de Instalación:** Eliminar las aplicaciones `webapps/docs`, `webapps/examples`, `webapps/ROOT`, `webapps/manager`.
2. **Usuario sin Privilegios:** Ejecutar el servicio con un usuario dedicado (`tomcat`) sin acceso a shell (`/bin/false`).
3. **Ocultación de Versión:** Añadir `server="AppServer"` en la configuración del conector para evitar divulgar la versión exacta de Tomcat en las cabeceras HTTP.
4. **Deshabilitar Métodos:** Bloquear métodos HTTP peligrosos (PUT, DELETE, TRACE) si la aplicación no los requiere explícitamente.

---

## 5. Pruebas de Carga y Rendimiento

La validación empírica es obligatoria antes de salir a producción.

### Herramientas y Metodología

* **ApacheBench (ab):** Para medir "fuerza bruta" y capacidad de red.
* **JMeter:** Para simular comportamiento real de usuarios (login, navegación, etc.).

### Matriz de Validación (Ejemplo Real)

| Escenario | Configuración | RPS | Latencia Media | Resultado |
| --- | --- | --- | --- | --- |
| **A** | Default (200 hilos) | 150 | 450ms | Saturación temprana |
| **B** | Tuned (500 hilos + NIO) | 380 | 120ms | Estable |
| **C** | Tuned + G1GC (Java) | 410 | 95ms | **Óptimo** |

---

## 6. Estrategia de Contenedores (Docker)

La migración a Docker garantiza la inmutabilidad y facilita el escalado horizontal.

### Diferencias Clave: Nativo vs. Docker

| Característica | Tomcat Nativo | Tomcat en Docker |
| --- | --- | --- |
| **Dependencias** | Instaladas en el OS Host. | Empaquetadas en la imagen. |
| **Actualización** | Manual y riesgosa. | Cambio de tag en la imagen base. |
| **Configuración** | Archivos editados en disco. | Inyectados o copiados en el build. |

### Dockerfile de Producción

```dockerfile
# Etapa 1: Imagen Base Ligera
FROM tomcat:9.0-jdk11-openjdk-slim

# Mantenimiento
LABEL maintainer="ingenieria@miempresa.com"

# Hardening: Limpiar webapps default
RUN rm -rf /usr/local/tomcat/webapps/*

# Copiar configuración optimizada
COPY ./config/server.xml /usr/local/tomcat/conf/

# Desplegar aplicación
COPY ./build/mi-app.war /usr/local/tomcat/webapps/ROOT.war

# Exponer puerto y ejecutar
EXPOSE 8080
CMD ["catalina.sh", "run"]

```

---

## 7. Mejores Prácticas de Administración

Para mantener la salud del sistema a largo plazo:

1. **Gestión de Memoria (JVM):** Nunca usar los valores por defecto. Configurar `bin/setenv.sh`:
```bash
export CATALINA_OPTS="-Xms4G -Xmx4G -XX:+UseG1GC"

```


*(Asignar la misma memoria inicial y máxima evita el overhead de redimensionamiento).*
2. **Monitoreo:** No volar a ciegas. Implementar **JMX Exporter** para enviar métricas a Prometheus/Grafana y visualizar:
* Uso de Heap Memory.
* Hilos ocupados vs. libres.
* Conexiones a Base de Datos (Pool JDBC).


3. **Logs:** Configurar la rotación de logs para evitar llenar el disco, o mejor aún, enviar los logs a una salida estándar (`stdout`) para que Docker o un sistema ELK los capturen.

---
