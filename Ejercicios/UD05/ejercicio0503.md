<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 3: Creación de usuarios y grupos</h1>

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


## 1. Creación de Estructura de Usuarios y Grupos
Para organizar el acceso al servidor, se ha creado un grupo de seguridad y dos cuentas de usuario vinculadas a este, garantizando que el acceso no sea administrativo sino limitado a sus funciones.

### Acciones realizadas:
* **Creación del grupo:** Se ejecutó `sudo groupadd ftp_limitado`.
* **Creación de usuarios:** Se añadieron los usuarios `user01` y `user02`, asignándolos directamente al grupo mediante el comando `useradd`.
* **Seguridad:** Se establecieron contraseñas seguras para ambos perfiles.

## 2. Definición del Directorio Raíz y Permisos de Archivo
Se ha configurado un entorno controlado (jaula chroot) para que los usuarios no puedan acceder a archivos críticos del sistema operativo.

* **Ruta del directorio:** `/srv/ftp_limitado/datos`
* **Asignación de propiedad:** Se transfirió el control del directorio al grupo `ftp_limitado`.
* **Configuración de permisos (rwx):** Se aplicaron permisos **770**, lo que permite:
    * **Lectura:** Los miembros del grupo pueden ver el contenido.
    * **Escritura:** Los miembros pueden subir nuevos archivos.
    * **Borrado:** Los miembros pueden eliminar archivos existentes.

## 3. Límites de Conexión
Para evitar la saturación del servidor y garantizar la disponibilidad, se han modificado las directivas de control en el archivo `vsftpd.conf`.

> **Captura de configuración de límites:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T3.png)

## 4. Incidencias
El proceso se ha completado satisfactoriamente sin incidencias. La integración de los usuarios en el grupo y la aplicación de la jaula `chroot` funcionaron correctamente desde el primer intento.

---

## 5. Diferencias entre Permisos de Usuario y Permisos de Grupo

Como parte fundamental de la administración de sistemas Linux, se identifican las siguientes diferencias:

| Característica | Permisos de Usuario (Propietario) | Permisos de Grupo |
| :--- | :--- | :--- |
| **Aplicación** | Se aplican exclusivamente al usuario dueño del archivo (UID). | Se aplican a todos los usuarios miembros de un grupo específico (GID). |
| **Propósito** | Garantizar la privacidad y control total del individuo sobre sus datos. | Facilitar la colaboración y el acceso compartido a recursos comunes. |
| **Jerarquía** | El sistema comprueba primero estos permisos. Si el usuario es el dueño, se ignoran los del grupo. | Se aplican si el usuario no es el dueño, pero pertenece al grupo que tiene acceso. |
| **Ejemplo** | El archivo `.bashrc` de un usuario (solo él puede editarlo). | Una carpeta de un departamento (varios usuarios leen y escriben). |
