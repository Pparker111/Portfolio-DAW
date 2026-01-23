<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 6: Pruebas con clientes en línea de comandos</h1>

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

## 1. Cliente Interactivo: `ftp`
Es el cliente tradicional para sesiones interactivas. Se utiliza abriendo una sub-consola donde se envían instrucciones directamente al servidor.

### Comandos utilizados y su función:
* **`ftp [dirección_IP]`**: Establece el canal de control con el servidor. Solicita el nombre de usuario y la contraseña.
* **`ls` / `dir`**: Solicita al servidor el listado de archivos en el directorio actual.
* **`get [archivo_remoto]`**: Descarga un archivo desde el servidor hacia el directorio local desde el que se invocó el comando ftp.
* **`put [archivo_local]`**: Sube un archivo desde la máquina local hacia la ruta actual en el servidor.
* **`ascii` / `binary`**: Cambia el modo de transferencia. `binary` es el recomendado para imágenes, ejecutables o archivos comprimidos para evitar corrupción de datos.
* **`quit` / `bye`**: Envía la señal de cierre de sesión y termina el proceso del cliente.

## 2. Cliente No Interactivo: `curl`
`curl` (Client URL) permite realizar transferencias sin entrar en una consola interactiva, enviando las credenciales y la instrucción en una sola línea.

### Comandos utilizados y su función:
* **`curl -u usuario:password ftp://[IP]/`**: 
    * *Función:* Realiza un listado del directorio raíz. El parámetro `-u` gestiona la autenticación.
* **`curl -u usuario:password -O ftp://[IP]/archivo.zip`**: 
    * *Función:* Descarga el archivo especificado. El parámetro `-O` (O mayúscula) mantiene el nombre original del archivo en local.
* **`curl -u usuario:password -T documento.pdf ftp://[IP]/`**: 
    * *Función:* Sube el archivo `documento.pdf` al servidor. El parámetro `-T` (Transfer-upload) indica que la operación es de subida.

## 3. Cliente Avanzado: `lftp`
Se ha probado brevemente `lftp` por su capacidad de gestionar múltiples transferencias en paralelo.

### Comandos utilizados y su función:
* **`lftp -u usuario,password [IP]`**: Inicia la sesión. Nota la diferencia de usar una coma en lugar de dos puntos para separar las credenciales.
* **`mirror -R [directorio_local]`**: 
    * *Función:* Realiza una subida recursiva (Reverse Mirror). Sube carpetas completas y su contenido manteniendo la estructura, algo que el cliente `ftp` básico no puede hacer de forma sencilla.

---

## Conclusión
El uso de clientes CLI confirma que el servidor responde correctamente a los estándares RFC del protocolo FTP. Mientras que `ftp` es útil para pruebas rápidas manuales, `curl` y `lftp` son las opciones preferidas para la integración en entornos de producción y copias de seguridad automáticas.
