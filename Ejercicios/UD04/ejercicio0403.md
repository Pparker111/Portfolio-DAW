
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Despliegue simple de una aplicación web de Apache Tomcat</h1>

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

Realiza las siguientes tareas en tu instalación de Apache Tomcat:

1. Descarga un archivo WAR de ejemplo (o genera uno simple con un Hello World JSP).
2. Despliega el archivo WAR dentro de la carpeta `webapps`.
3. Observa el proceso de despliegue automático e identifica los pasos internos que realiza Tomcat.
4. Accede a la aplicación desplegada y captura una evidencia de que funciona correctamente.

---

# **📘 Desarrollo del Ejercicio**

## **1. Descarga o creación del archivo WAR**

Se eligió generar un WAR sencillo llamado `hello.war`, que contiene un JSP con el mensaje **Hello World**.

Comandos utilizados:

```bash
mkdir hello
cd hello
mkdir -p WEB-INF
echo "<h1>Hello World desde Tomcat</h1>" > index.jsp
cat <<EOF > WEB-INF/web.xml
<web-app>
    <display-name>HelloApp</display-name>
</web-app>
EOF
jar -cvf hello.war *
```

---

## **2. Despliegue del WAR en Tomcat**

Se copió el archivo generado a la carpeta de despliegues automáticos:

```bash
sudo cp hello.war /opt/tomcat/webapps/
```

---

## **3. Problema encontrado y solución aplicada**

Al intentar comprobar el despliegue, Tomcat no arrancaba. El comando:

```bash
sudo systemctl status tomcat
```

aparecía con el estado **inactive (dead)**.

El error se debía a configuraciones inválidas en `/etc/systemd/system/tomcat.service`.

### ✔️ **Solución aplicada:**

Se reemplazó el archivo por una versión correcta:

```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Luego se recargó systemd y se inició correctamente:

```bash
sudo systemctl daemon-reload
sudo systemctl start tomcat
```

El servicio pasó a estado **active (running)**.

---

## **4. Despliegue automático de Tomcat**

Una vez Tomcat volvió a funcionar, el servidor realizó automáticamente:

1. Detección del archivo `hello.war` en `webapps/`.
2. Creación del directorio `webapps/hello/`.
3. Descompresión interna del contenido del WAR.
4. Lectura de:

   * `WEB-INF/web.xml` del WAR
   * `conf/web.xml` global
   * `conf/context.xml` de Tomcat
5. Registro de servlets y JSP.
6. Inicialización del contexto `/hello`.
7. Publicación de la aplicación en:

```
http://localhost:8080/hello/
```

---

## **5. Evidencia final del funcionamiento de la aplicación**

👇 **Inserta aquí la captura al acceder a la URL:**

```
http://localhost:8080/hello/
```

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD04/imagenes/helloWorld.png)
