
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Tomcat en contenedores (Docker)</h1>

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

## **📝 Enunciado**

1. Descarga una imagen oficial de Tomcat:
    docker pull tomcat:latest
2. Monta una aplicación en /usr/local/tomcat/webapps.
3. Explica las diferencias entre el Tomcat nativo y el Tomcat en contenedor.
4. Opcional: despliegue en la nube utilizando servicios tipo AWS EC2, Azure VM o Google Cloud.

-----

Entiendo perfectamente. Para un informe técnico o una memoria de proyecto, la profundidad es clave para demostrar que no solo se han ejecutado comandos, sino que se comprende la arquitectura subyacente.

Aquí tienes una versión mucho más extensa y detallada, redactada desde tu perspectiva, como si estuvieras documentando paso a paso tu investigación y ejecución.

---

## 1. Implementación del Entorno Contenedorizado

Para garantizar un entorno de ejecución inmutable, opté por la plataforma **Docker**. El objetivo fue encapsular todas las dependencias (Java, Tomcat, Librerías del SO) en una unidad lógica única.

### Estrategia de aprovisionamiento

En lugar de una instalación manual, utilicé el registro oficial para asegurar que el entorno cumpla con las mejores prácticas de seguridad y rendimiento:

1. **Pull de la imagen:** Descargué la imagen base oficial de Tomcat:
`docker pull tomcat:9.0-jdk11-openjdk`
2. **Mapeo de la aplicación:** Para el despliegue, utilicé un montaje de volumen vinculado (*bind mount*). Esta técnica me permitió separar el código de la aplicación del ciclo de vida del contenedor:
```bash
docker run -d \
  --name servidor-produccion \
  -p 8081:8080 \
  -v /home/usuario/proyectos/mi-app.war:/usr/local/tomcat/webapps/mi-app.war \
  --restart always \
  tomcat:latest

```


*Aquí, redireccioné el tráfico del puerto host 8081 hacia el 8080 interno, permitiendo una convivencia con otros servicios si fuera necesario.*

---

## 2. Análisis Comparativo: Arquitectura Nativa vs. Contenedores

Durante este proceso, realicé una evaluación crítica sobre por qué la transición a contenedores es superior en entornos modernos, identificando las siguientes diferencias clave:

### A. Gestión de Dependencias e Inmutabilidad

* **Nativo:** En mis pruebas anteriores, dependía de que el servidor tuviera instalada la versión exacta de la **JVM**. Cualquier actualización del sistema operativo podía romper la compatibilidad.
* **Docker:** La imagen es **autocontenida**. Si funciona en mi máquina de desarrollo, tengo la certeza absoluta de que funcionará igual en el servidor de producción, eliminando el clásico problema de *"en mi máquina funciona"*.

### B. Ciclo de Vida y Persistencia

* **Nativo:** El estado es persistente. Si modifico un archivo de configuración directamente en `/opt/tomcat/conf`, ese cambio se queda ahí para siempre, lo cual dificulta la recuperación ante desastres.
* **Docker:** El contenedor es **efímero**. Si el contenedor falla, simplemente se destruye y se levanta uno nuevo en segundos a partir de la imagen original. La persistencia la gestiono externamente mediante volúmenes, lo que facilita enormemente las copias de seguridad.

### C. Densidad y Aislamiento de Recursos

* **Nativo:** Ejecutar varias instancias de Tomcat en un mismo servidor requiere configurar diferentes puertos, usuarios y variables `CATALINA_BASE`, lo cual es propenso a errores humanos.
* **Docker:** Puedo levantar 10 contenedores de Tomcat en la misma máquina, cada uno aislado en su propio espacio de red y sistema de archivos, gestionando los recursos (CPU/RAM) mediante límites específicos en el comando `docker run`.

---

## 3. Estrategia de Despliegue en la Nube (Cloud Deployment)

Para escalar este proyecto, he analizado las opciones de despliegue en proveedores de nube pública, donde la portabilidad de Docker se convierte en nuestra mayor ventaja:

1. **Amazon Web Services (AWS):** * Utilizaría **ECS (Elastic Container Service)** con Fargate. Esto me permitiría ejecutar mis contenedores de Tomcat sin necesidad de gestionar instancias de EC2 (servidores virtuales), delegando el mantenimiento del parcheo del SO a AWS.
2. **Google Cloud Platform (GCP):** * La opción más eficiente sería **Cloud Run**, que permite un escalado a cero (si nadie usa la app, no hay coste) y escala automáticamente según el tráfico detectado en el puerto 8080.
3. **Microsoft Azure:** * Implementaría **Azure Container Instances (ACI)** para despliegues rápidos o **AKS (Azure Kubernetes Service)** si la aplicación creciera hasta requerir una orquestación compleja de múltiples servicios interconectados.

---

## 4. Conclusiones de la Tarea

El uso de Docker para Apache Tomcat no es solo una moda tecnológica, sino una mejora en la **fiabilidad del software**. Al haber pasado por la optimización del `server.xml` en la tarea anterior, ahora puedo integrar esos ajustes directamente en un `Dockerfile` para que cada nueva instancia que se levante en la nube ya venga pre-configurada con los hilos y tiempos de espera óptimos.
