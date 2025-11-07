## 3\. Relación de las Actividades Realizadas

En esta sección se detalla el proceso técnico llevado a cabo para la instalación y configuración de un servidor web Apache2 en un sistema Ubuntu. El proceso se ha documentado paso a paso, incluyendo los comandos ejecutados y las evidencias gráficas (capturas de pantalla) de cada etapa.

### 3.1. Preparación del Sistema

**Paso 1: Actualización del sistema**

Se procedió a actualizar los repositorios de paquetes y el sistema operativo para asegurar que todas las dependencias estuvieran en su última versión.

```bash
sudo apt update
sudo apt upgrade -y
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/1.png)  

### 3.2. Instalación de Apache

**Paso 2: Instalación del paquete Apache2**

Una vez actualizado el sistema, se instaló el paquete `apache2` utilizando el gestor de paquetes `apt`.

```bash
sudo apt install apache2 -y
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/2.png)  

**Paso 3: Verificación de la instalación**

Para confirmar que el servidor estaba activo y conocer la dirección IP local de la máquina, se ejecutó el comando `hostname -I`.

```bash
hostname -I
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/3.png)  

### 3.3. Configuración del Servidor

**Paso 4: Configuración de Usuario y Grupo**

Por motivos de seguridad y gestión de permisos, se modificó el archivo de entorno de Apache para que el servidor se ejecute con el usuario y grupo personal, en lugar del predeterminado `www-data`.

```bash
sudo nano /etc/apache2/envvars
```

Se modificaron las líneas `APACHE_RUN_USER` y `APACHE_RUN_GROUP` para reflejar el nuevo propietario (ej: `poner_tu_usuario` y `poner_tu_grupo`).

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/4.png)

**Paso 5: Configuración del Directorio Raíz**

Se editó el archivo principal de configuración (`apache2.conf`) para ajustar los permisos del directorio `/var/www/`, permitiendo que el servidor pueda seguir enlaces simbólicos (`FollowSymLinks`) y procesar archivos `.htaccess` (`AllowOverride All`).

```bash
sudo nano /etc/apache2/apache2.conf
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/5.png)

**Paso 6: Habilitación de Módulos**

Para asegurar la funcionalidad de futuras configuraciones (como URLs amigables o reglas de reescritura), se habilitaron los módulos `headers` y `rewrite`.

```bash
sudo a2enmod headers
sudo a2enmod rewrite
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/6.png)

**Paso 7: Ajuste de Propiedades del Directorio**

Se cambió la propiedad del directorio de documentos web (`/var/www/html`) al usuario actual, facilitando la edición y subida de archivos sin necesidad de permisos de superusuario.

```bash
sudo chown -R $USER:$USER /var/www/html
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/7.png)

### 3.4. Reinicio y Aplicación de Cambios

**Paso 8: Reinicio del servicio Apache**

Finalmente, para que todos los cambios de configuración y la activación de nuevos módulos tuvieran efecto, se reinició el servicio de Apache.

```bash
sudo systemctl restart apache2
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/8.png)

¡Claro! Entiendo que quisiste decir "bibliografía".

Aquí tienes la lista de las fuentes que has utilizado, formateada para que la puedas incluir en tu documento.

También he añadido el enlace del tutorial de Ubuntu que mencionaba tu profesor en el PDF, ya que es la fuente principal de la parte práctica.

Bibliografía
Puntocomunica. (s.f.). Instalar servidor Apache 2 en Ubuntu. Recuperado de: https://foro.puntocomunica.com/viewtopic.php?t=312

IONOS. (s.f.). Instalar Apache en Ubuntu: Guía de instalación y configuración. Recuperado de: https://www.ionos.es/digitalguide/servidores/configuracion/instalar-apache-en-ubuntu/

Cumpi Linux.  21 ene 2025. Cómo Instalar Apache en Ubuntu 24 con Configuración Inicial Paso a Paso. YouTube. https://www.youtube.com/watch?v=VA-8hQt5hSo

Ubuntu. (s.f.). Install and Configure Apache. Recuperado de: https://ubuntu.com/tutorials/install-and-configure-apache#1-overview

-----
