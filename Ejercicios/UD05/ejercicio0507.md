<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 7: Pruebas con clientes gráficos</h1>

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

## 1. Configuración de la Conexión
Para acceder al servidor de forma visual, se utilizó el protocolo `ftp://` en el explorador de archivos. 

* **Servidor:** `localhost`
* **Protocolo:** FTP
* **Usuario:** `user01`

Al conectar, el sistema desplegó una ventana emergente de autenticación para validar las credenciales del usuario creado previamente.

> **Captura 1: Autenticación de usuario**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T7.png)

## 2. Incidencias y Resolución
Durante las pruebas iniciales, la transferencia bidireccional fallaba al intentar subir archivos al servidor (error de escritura).

* **Fallo detectado:** El servidor denegaba el permiso de escritura a pesar de que el usuario pertenecía al grupo correcto.
* **Causa:** Tras revisar la consola de administración, se comprobó que el cambio en la directiva `write_enable=YES` dentro del archivo `/etc/vsftpd.conf` no se había guardado correctamente o el servicio no había procesado la actualización.
* **Solución:** Se editó nuevamente el fichero, se aseguró la persistencia del cambio y se reinició el servicio mediante `sudo systemctl restart vsftpd`. Tras este ajuste, el servidor permitió la subida de datos.

## 3. Transferencia Bidireccional de Archivos
Una vez corregida la configuración, se realizaron las siguientes operaciones:

1.  **Subida (Upload):** Se transfirió con éxito el archivo `archivo_local.txt` desde la carpeta local al directorio del servidor.
2.  **Descarga (Download):** Se copió un archivo del servidor de vuelta al almacenamiento local para confirmar la bidireccionalidad.

> **Captura 2: Transferencia exitosa**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T71.png)



## 4. Conclusión
La prueba con clientes gráficos confirma que el servidor **vsftpd** está correctamente configurado para la edición de archivos. El incidente técnico permitió validar que la directiva `write_enable` es crítica para el funcionamiento de cualquier cliente que intente realizar operaciones más allá de la simple lectura.
