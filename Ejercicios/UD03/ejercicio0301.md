<p align="center">
  <img src="https://assets.ubuntu.com/v1/29985a98-ubuntu-logo32.png" alt="Ubuntu Logo" width="100"/>
</p>

<h1 align="center">Instalación y Configuración de un Servidor Web Apache2 en Ubuntu</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante · 
  <b>Curso:</b> 2º DAW · 
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 7 de Noviembre de 2025 · 
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## 🧭 Resumen

Este documento describe el proceso completo de instalación y configuración de un servidor web Apache2 en un sistema operativo Ubuntu. A lo largo del trabajo, se documentan paso a paso los comandos utilizados, las configuraciones aplicadas y las evidencias gráficas que demuestran el correcto desarrollo del proceso.  
Durante la práctica se crearon hosts virtuales, se modificaron permisos y se realizaron pruebas para verificar la conectividad. Aunque la página final no funcionó correctamente, se analizan las causas más comunes del problema y se proponen soluciones.  
El trabajo refleja el aprendizaje práctico sobre administración de servidores Linux y la importancia de comprender los fundamentos del entorno Ubuntu para el desarrollo profesional.

---

## 🔑 Palabras Clave

`Apache2` · `Ubuntu` · `Linux` · `Servidor Web` · `Configuración` · `Host Virtual` · `Administración de Sistemas`

---

## 📚 Índice

1. [Relación de las Actividades Realizadas](#3-relación-de-las-actividades-realizadas)  
   1. [Preparación del Sistema](#31-preparación-del-sistema)  
   2. [Instalación de Apache](#32-instalación-de-apache)  
   3. [Configuración del Servidor](#33-configuración-del-servidor)  
   4. [Reinicio y Aplicación de Cambios](#34-reinicio-y-aplicación-de-cambios)  
   5. [Creación de un Sitio Web Personalizado (Host Virtual)](#35-creación-de-un-sitio-web-personalizado-host-virtual)  
   6. [Pruebas del Host Virtual y Resolución de Problemas](#36-pruebas-del-host-virtual-y-resolución-de-problemas)  
2. [Resultados y Conclusiones](#resultados-y-conclusiones)  
3. [Bibliografía](#bibliografía)

---

## 3. Relación de las Actividades Realizadas

En esta sección se detalla el proceso técnico llevado a cabo para la **instalación y configuración de un servidor web Apache2** en un sistema operativo **Ubuntu**.  
El procedimiento se ha documentado paso a paso, incluyendo los comandos ejecutados y las evidencias gráficas correspondientes a cada fase del proceso.

---

### 3.1. Preparación del Sistema

**Paso 1: Actualización del sistema**

Se procedió a actualizar los repositorios de paquetes y el sistema operativo para garantizar que todas las dependencias estuvieran en su última versión.

```bash
sudo apt update
sudo apt upgrade -y
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/1.png)

---

### 3.2. Instalación de Apache

**Paso 2: Instalación del paquete Apache2**

Una vez actualizado el sistema, se instaló el paquete `apache2` mediante el gestor de paquetes `apt`.

```bash
sudo apt install apache2 -y
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/2.png)

**Paso 3: Verificación de la instalación**

Para confirmar que el servicio de Apache se encontraba activo y conocer la dirección IP local de la máquina, se utilizó el siguiente comando:

```bash
hostname -I
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/3.png)

---

### 3.3. Configuración del Servidor

**Paso 4: Configuración de usuario y grupo**

Por motivos de seguridad y gestión de permisos, se modificó el archivo de entorno de Apache para que el servicio se ejecute con el usuario y grupo personal, en lugar del predeterminado `www-data`.

```bash
sudo nano /etc/apache2/envvars
```

Se editaron las líneas `APACHE_RUN_USER` y `APACHE_RUN_GROUP` para reflejar el nuevo propietario (por ejemplo: `tu_usuario` y `tu_grupo`).

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/4.png)

**Paso 5: Configuración del directorio raíz**

Se editó el archivo principal de configuración (`apache2.conf`) para ajustar los permisos del directorio `/var/www/`, permitiendo que el servidor siga enlaces simbólicos (`FollowSymLinks`) y procese archivos `.htaccess` (`AllowOverride All`).

```bash
sudo nano /etc/apache2/apache2.conf
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/5.png)

**Paso 6: Habilitación de módulos**

Para garantizar la funcionalidad de futuras configuraciones (como URLs amigables o reglas de reescritura), se habilitaron los módulos `headers` y `rewrite`.

```bash
sudo a2enmod headers
sudo a2enmod rewrite
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/6.png)

**Paso 7: Ajuste de propiedades del directorio**

Se cambió la propiedad del directorio de documentos web (`/var/www/html`) al usuario actual, permitiendo editar y subir archivos sin requerir permisos de superusuario.

```bash
sudo chown -R $USER:$USER /var/www/html
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/7.png)

---

### 3.4. Reinicio y Aplicación de Cambios

**Paso 8: Reinicio del servicio Apache**

Finalmente, para aplicar todos los cambios de configuración y la activación de los nuevos módulos, se reinició el servicio de Apache.

```bash
sudo systemctl restart apache2
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/8.png)

---

### 3.5. Creación de un Sitio Web Personalizado (Host Virtual)

Siguiendo las instrucciones del tutorial de Ubuntu, se procedió a crear un segundo sitio web (un **Host Virtual**) además del sitio por defecto.
El objetivo era alojar un sitio personalizado en el subdominio **gci.example.com**.

**Paso 1: Creación del directorio y del archivo web**

Primero, se creó un nuevo directorio llamado `gci` dentro de `/var/www/` para alojar los archivos del nuevo sitio.

```bash
sudo mkdir /var/www/gci/
```

Luego, se creó un archivo `index.html` básico dentro del nuevo directorio:

```bash
cd /var/www/gci/
nano index.html
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/11.png)


El contenido añadido fue el siguiente:

```html
<html>
  <head>
    <title>Ubuntu rocks!</title>
  </head>
  <body>
    <p>I'm running this website on an Ubuntu Server!</p>
  </body>
</html>
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/12.png)

**Paso 2: Configuración del archivo VirtualHost**

Para que Apache reconozca el nuevo sitio, se creó un archivo de configuración basado en el archivo por defecto.

```bash
cd /etc/apache2/sites-available/
sudo cp 000-default.conf gci.conf
```


> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/10.png)

A continuación, se editó el archivo `gci.conf` con el siguiente comando:

```bash
sudo nano gci.conf
```

Se realizaron los siguientes cambios:

* `ServerAdmin`: actualizado al correo del administrador (por ejemplo: `yourname@example.com`)
* `DocumentRoot`: cambiado a `/var/www/gci/`
* `ServerName`: añadida la línea `ServerName gci.example.com`

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/13.png)

**Paso 3: Activación del nuevo sitio y reinicio de Apache**

Una vez guardada la configuración, se activó el nuevo host virtual con el siguiente comando:

```bash
sudo a2ensite gci.conf
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/14.png)

Finalmente, se recargó el servicio de Apache para aplicar los cambios:

```bash
sudo service apache2 reload
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/15.png)

---

### 3.6. Pruebas del Host Virtual y Resolución de Problemas

**Prueba inicial y problemas detectados**

Siguiendo el tutorial, se realizó una prueba final accediendo desde el navegador a la dirección:

```
http://gci.example.com
```

**Resultado:** la página web no funcionó correctamente.

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/16.png)

---

### 🧩 Resultados y Conclusiones

Durante la realización de esta práctica no se presentaron errores críticos en la instalación o ejecución de los comandos. Sin embargo, el proceso supuso un desafío importante debido a mi falta de familiaridad con Ubuntu y los sistemas Linux.
Aunque los pasos fueron seguidos correctamente, la comprensión de lo que cada comando hacía en detalle resultó compleja. En ciertos momentos, la tarea se sintió como "hablar en otro idioma", ya que no tengo aún una base sólida en administración de servidores.

A pesar de ello, la experiencia fue valiosa. Me permitió dar mis primeros pasos en la configuración de servidores web y comprender la lógica detrás de los archivos de configuración de Apache.

El problema final con el Host Virtual se debió a una causa muy común: el navegador no reconoce el dominio gci.example.com porque no está configurado en ningún servidor DNS público. Es decir, Apache sí sabe qué hacer cuando se le solicita ese dominio, pero el navegador no logra encontrarlo.

La solución pasa por añadir una entrada en el archivo hosts del sistema local o realizar pruebas con la dirección IP directamente.
Esto confirma que, a pesar del fallo aparente, la configuración técnica del servidor fue realizada correctamente.

En resumen, aunque la página final no se mostró, los objetivos principales se cumplieron: se instaló, configuró y personalizó Apache con éxito.
Me llevo un aprendizaje importante y la motivación de mejorar cada día un poco más para dominar estos entornos, fundamentales para mi desarrollo profesional.

---

### 📖 Bibliografía

* Puntocomunica. (s.f.). *Instalar servidor Apache 2 en Ubuntu.* Recuperado de: [https://foro.puntocomunica.com/viewtopic.php?t=312](https://foro.puntocomunica.com/viewtopic.php?t=312)
* IONOS. (s.f.). *Instalar Apache en Ubuntu: Guía de instalación y configuración.* Recuperado de: [https://www.ionos.es/digitalguide/servidores/configuracion/instalar-apache-en-ubuntu/](https://www.ionos.es/digitalguide/servidores/configuracion/instalar-apache-en-ubuntu/)
* Cumpi Linux. (21 ene 2025). *Cómo instalar Apache en Ubuntu 24 con configuración inicial paso a paso.* YouTube. [https://www.youtube.com/watch?v=VA-8hQt5hSo](https://www.youtube.com/watch?v=VA-8hQt5hSo)
* Ubuntu. (s.f.). *Install and Configure Apache.* Recuperado de: [https://ubuntu.com/tutorials/install-and-configure-apache#1-overview](https://ubuntu.com/tutorials/install-and-configure-apache#1-overview)

---
