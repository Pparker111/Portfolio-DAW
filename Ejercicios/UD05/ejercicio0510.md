
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 10: Integración de FTP con servidor web</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 23 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## 1. Preparación del Servidor Web

El primer paso consistió en asegurar que el servidor Apache estuviera instalado y que el directorio de publicación tuviera los permisos necesarios para el usuario FTP.

* **Configuración de permisos:**
Para permitir que el usuario `user01` pueda subir archivos a la carpeta web, se cambió el propietario y se ajustaron los privilegios:
`sudo chown -R $USER:$USER /var/www/html`
`sudo chmod -R 775 /var/www/html`

---

## 2. Vinculación del Usuario FTP

Para que el flujo sea directo, se configuró el directorio personal (Home) del usuario FTP para que coincida con la ruta raíz de Apache.

* **Comando de vinculación:**
`sudo usermod -d /var/www/html user01`
* **Reinicio del servicio:**
`sudo systemctl restart vsftpd`

---

## 3. Procedimiento de Subida y Publicación (Paso a Paso)

El flujo de trabajo seguido para publicar la web fue el siguiente:

1. **Creación del contenido:** Se generó un archivo `index.html` básico en el equipo local.
2. **Conexión y Navegación:** Se accedió vía terminal o explorador a la ruta `/var/www/html`.
3. **Transferencia de archivos:** Se ejecutó el comando `put index.html` (en CLI) o "copiar y pegar" (en interfaz gráfica) para mover el archivo al servidor.
4. **Verificación en el Servidor:** Se comprobó físicamente que el archivo reside en el directorio de Apache.

### Evidencia de integración

En la siguiente captura se observa que el archivo `index.html` ha sido transferido correctamente a la ruta de destino del servidor web:

> **Captura de Verificación:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T10.png)

---

## 4. Comprobación Final vía HTTP

Una vez confirmada la presencia del archivo en el servidor, se procedió a la visualización externa:

* **URL de acceso:** `http://localhost/index.html`
* **Resultado:** El servidor Apache sirve el contenido web de forma inmediata tras la subida por FTP.

## 5. Conclusión

La integración es exitosa. Este método permite separar las capas de administración: el desarrollador utiliza **FTP** para gestionar archivos y el cliente final utiliza **HTTP** para consumir el servicio. Se ha demostrado que el servidor **vsftpd** es una herramienta eficaz para el despliegue de aplicaciones web en entornos Linux.

---
