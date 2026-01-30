<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 12: Documentación final</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 30 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

Para completar tu proyecto con éxito, he consolidado todo el trabajo que hemos realizado en un único documento de **Documentación Final**. Este archivo sigue la estructura profesional requerida por el criterio **i** y está listo para ser exportado a PDF o guardado como Markdown.

---

## 1. Instalación del Servidor

Se ha seleccionado **vsftpd** (Very Secure FTP Daemon) por su robustez y eficiencia en entornos Linux.

* **Comando de instalación:** `sudo apt update && sudo apt install vsftpd -y`
* **Verificación:** `sudo systemctl status vsftpd` para confirmar que el demonio está activo.

## 2. Configuración Básica

El archivo principal de configuración se ubica en `/etc/vsftpd.conf`. Se realizaron los siguientes ajustes iniciales:

* `anonymous_enable=NO`: Desactiva el acceso sin credenciales.
* `local_enable=YES`: Permite el acceso a usuarios locales del sistema.
* `write_enable=YES`: Habilita la capacidad de subir y modificar archivos.

## 3. Usuarios y Permisos

Se implementó un sistema de acceso restringido para evitar riesgos de seguridad:

* **Creación de Grupo:** Se creó el grupo `ftp_limitado`.
* **Enjaulamiento (chroot):** Se activó `chroot_local_user=YES` para que los usuarios no puedan salir de su directorio personal.
* **Permisos de Carpeta:** Se aplicó `chmod 775` y `chown` sobre el directorio de datos para permitir el trabajo colaborativo entre miembros del grupo.

## 4. Modos Activo y Pasivo

Se configuró el **Modo Pasivo** para garantizar la compatibilidad con firewalls y conexiones modernas:

* **Rango de puertos:** Se definieron `pasv_min_port=40000` y `pasv_max_port=40100`.
* **Funcionamiento:** En este modo, el servidor abre un puerto aleatorio dentro del rango para que el cliente inicie la conexión de datos, evitando bloqueos por parte del cliente.

## 5. Seguridad (FTPS)

Para proteger las credenciales, se configuró **FTP sobre TLS (FTPS Explícito)**:

* **Certificado:** Generado con OpenSSL (`vsftpd.pem`).
* **Obligatoriedad:** Se añadieron las directivas `force_local_logins_ssl=YES` y `force_local_data_ssl=YES`.
* **Verificación:** Comprobada mediante `openssl s_client`, confirmando el uso de protocolos seguros (TLSv1.2/v1.3).

## 6. Clientes Utilizados

Se verificó la disponibilidad del servicio mediante distintos tipos de clientes:

* **CLI (Línea de comandos):** Uso de `ftp` y `curl` para operaciones rápidas y scripts.
* **Gráficos:** Uso del explorador de archivos nativo y FileZilla para gestión visual y transferencias bidireccionales.

## 7. Integración Web

Se vinculó el servidor FTP con el servidor web **Apache2**:

* **DocumentRoot:** El directorio FTP se redirigió a `/var/www/html`.
* **Flujo de Publicación:** Los archivos subidos por FTP son servidos inmediatamente vía HTTP, permitiendo una actualización ágil de contenidos web.

## 8. Recomendaciones de Administración

Para mantener el servidor en un entorno de producción, se establecen las siguientes pautas:

* **Límites:** Configurar `max_clients` y `max_per_ip` para prevenir ataques DoS.
* **Auditoría:** Revisión periódica de `/var/log/vsftpd.log`.
* **Mantenimiento:** Realizar copias de seguridad de la configuración y de los datos bajo la regla 3-2-1.
* **Firewall:** Mantener cerrados todos los puertos excepto el 21 y el rango pasivo definido.

---

