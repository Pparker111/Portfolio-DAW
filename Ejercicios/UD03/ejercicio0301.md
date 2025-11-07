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

### Bibliografía

* Puntocomunica. (s.f.). *Instalar servidor Apache 2 en Ubuntu.* Recuperado de: [https://foro.puntocomunica.com/viewtopic.php?t=312](https://foro.puntocomunica.com/viewtopic.php?t=312)
* IONOS. (s.f.). *Instalar Apache en Ubuntu: Guía de instalación y configuración.* Recuperado de: [https://www.ionos.es/digitalguide/servidores/configuracion/instalar-apache-en-ubuntu/](https://www.ionos.es/digitalguide/servidores/configuracion/instalar-apache-en-ubuntu/)
* Cumpi Linux. (21 ene 2025). *Cómo instalar Apache en Ubuntu 24 con configuración inicial paso a paso.* YouTube. [https://www.youtube.com/watch?v=VA-8hQt5hSo](https://www.youtube.com/watch?v=VA-8hQt5hSo)
* Ubuntu. (s.f.). *Install and Configure Apache.* Recuperado de: [https://ubuntu.com/tutorials/install-and-configure-apache#1-overview](https://ubuntu.com/tutorials/install-and-configure-apache#1-overview)

---
