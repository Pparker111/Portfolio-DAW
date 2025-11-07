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
> *[Aquí insertas tu **Foto 1** del comando `apt update/upgrade`](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/1.png)*
>
> **Problemas encontrados:** *(Opcional)*
> [cite\_start]*Ej: "No se encontraron problemas" o "El paquete X requirió una intervención manual...".* [cite: 31]

### 3.2. Instalación de Apache

**Paso 2: Instalación del paquete Apache2**

Una vez actualizado el sistema, se instaló el paquete `apache2` utilizando el gestor de paquetes `apt`.

```bash
sudo apt install apache2 -y
```

> **Evidencia:**
> *[Aquí insertas tu **Foto 2** mostrando la instalación completada.]*

**Paso 3: Verificación de la instalación**

Para confirmar que el servidor estaba activo y conocer la dirección IP local de la máquina, se ejecutó el comando `hostname -I`.

```bash
hostname -I
```

> **Evidencia:**
> *[Aquí insertas tu **Foto 3** donde se ve la dirección IP que te devolvió.]*
>
> **Nota:** *Aquí puedes añadir una frase como: "La IP obtenida (ej: 192.168.1.50) se utilizó para comprobar el acceso desde un navegador, mostrando la página de bienvenida de Apache".*

### 3.3. Configuración del Servidor

**Paso 4: Configuración de Usuario y Grupo**

Por motivos de seguridad y gestión de permisos, se modificó el archivo de entorno de Apache para que el servidor se ejecute con el usuario y grupo personal, en lugar del predeterminado `www-data`.

```bash
sudo nano /etc/apache2/envvars
```

Se modificaron las líneas `APACHE_RUN_USER` y `APACHE_RUN_GROUP` para reflejar el nuevo propietario (ej: `poner_tu_usuario` y `poner_tu_grupo`).

> **Evidencia:**
> *[Aquí insertas tu **Foto 4** (¡la que me enviaste antes\!) mostrando el editor `nano` con las líneas cambiadas.]*

**Paso 5: Configuración del Directorio Raíz**

Se editó el archivo principal de configuración (`apache2.conf`) para ajustar los permisos del directorio `/var/www/`, permitiendo que el servidor pueda seguir enlaces simbólicos (`FollowSymLinks`) y procesar archivos `.htaccess` (`AllowOverride All`).

```bash
sudo nano /etc/apache2/apache2.conf
```

> **Evidencia:**
> *[Aquí insertas tu **Foto 5** mostrando el bloque `<Directory /var/www/>` modificado.]*

**Paso 6: Habilitación de Módulos**

Para asegurar la funcionalidad de futuras configuraciones (como URLs amigables o reglas de reescritura), se habilitaron los módulos `headers` y `rewrite`.

```bash
sudo a2enmod headers
sudo a2enmod rewrite
```

> **Evidencia:**
> *[Aquí insertas tu **Foto 6** mostrando la salida de la terminal al activar los módulos.]*

**Paso 7: Ajuste de Propiedades del Directorio**

Se cambió la propiedad del directorio de documentos web (`/var/www/html`) al usuario actual, facilitando la edición y subida de archivos sin necesidad de permisos de superusuario.

```bash
sudo chown -R $USER:$USER /var/www/html
```

> **Evidencia:**
> *[Aquí insertas tu **Foto 7** (probablemente no muestre salida, o puedes mostrar un `ls -l` después para comprobarlo).]*

### 3.4. Reinicio y Aplicación de Cambios

**Paso 8: Reinicio del servicio Apache**

Finalmente, para que todos los cambios de configuración y la activación de nuevos módulos tuvieran efecto, se reinició el servicio de Apache.

```bash
sudo systemctl restart apache2
```

> **Evidencia:**
> *[Aquí insertas tu **Foto 8** (probablemente sin salida, o puedes complementarla con un `systemctl status apache2` para demostrar que está "active (running)").]*

-----

[cite\_start]Esta estructura te permite presentar la información de forma ordenada, impersonal (como pedía el PDF) [cite: 34] y cubriendo todos los requisitos de la tarea.

[cite\_start]¿Quieres que te ayude a buscar el siguiente tutorial que te pedía el profesor (por ejemplo, sobre **"control de acceso" en Apache**)? [cite: 26]
