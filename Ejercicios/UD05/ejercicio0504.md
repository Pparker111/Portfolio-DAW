<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 4: Configuración de acceso anónimo</h1>

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

## 1. Procedimiento de Configuración
Se ha procedido a habilitar el acceso sin credenciales específicas para permitir la descarga de archivos públicos.

### Ajustes realizados:
* **Habilitación:** Se modificó la directiva `anonymous_enable` a `YES`.
* **Restricción de Directorio:** Se estableció `/srv/ftp/` como raíz para usuarios anónimos, aislándolos del resto del sistema de archivos.
* **Política de Permisos:** Se ha configurado un entorno de **Solo Lectura**. Se verificó que las opciones de subida (upload) y creación de carpetas estuvieran desactivadas para evitar que usuarios externos llenen el almacenamiento del servidor.

## 2. Verificación de Conexión
Se realizó una prueba de conexión desde un cliente FTP utilizando el usuario `anonymous` y dejando la contraseña en blanco (o usando un correo electrónico ficticio).

* **Resultado:** El acceso fue exitoso.
* **Prueba de Permisos:** Se intentó subir un archivo de prueba, obteniendo un error `550 Permission denied`, lo que confirma que la política de solo lectura está operativa.

---

## 3. Evidencia del Servicio
A continuación se muestra la captura de la sesión FTP donde se observa el listado de archivos públicos y la denegación de permisos de escritura.

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T4.png)
