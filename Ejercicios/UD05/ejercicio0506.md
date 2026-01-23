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

## 1. Preparación del Entorno

Antes de iniciar la conexión, se generó un archivo de prueba en el sistema local para validar la capacidad de subida del servidor.

* **Comando:** `echo "Archivo de prueba para Actividad 6" > test.txt`
* **Función:** Crea un fichero de texto plano llamado `test.txt` en el directorio actual del cliente.

## 2. Ejecución de Operaciones (Cliente `ftp`)

Se utilizó el cliente interactivo `ftp` para conectar a la interfaz de loopback (`localhost`). A continuación, se detallan los comandos ejecutados dentro de la sesión FTP:

| Comando | Función Técnica | Resultado Esperado |
| --- | --- | --- |
| **`ftp localhost`** | Inicia la conexión de control al puerto 21 de la propia máquina. | Solicitud de credenciales (User/Pass). |
| **`ls`** | Envía el comando `LIST` para obtener el contenido del directorio actual. | Muestra los archivos en la "jaula" del usuario. |
| **`put test.txt`** | Inicia una conexión de datos para subir el archivo local al servidor. | Mensaje "226 Transfer complete". |
| **`get test.txt desc.txt`** | Solicita la descarga del archivo remoto, guardándolo localmente con otro nombre. | Verificación de permisos de lectura. |
| **`bye`** | Envía la instrucción de cierre de sesión al demonio vsftpd. | Cierre del proceso y liberación de puertos. |

## 3. Pruebas con Clientes Alternativos (Sugeridos)

Además del modo interactivo, se verificó la posibilidad de realizar operaciones mediante comandos directos de una sola línea:

* **Listar con `curl`:**
* `curl -u user:pass ftp://localhost/`
* *Función:* Conecta, autentica y lista el contenido sin mantener la sesión abierta.


* **Descarga con `curl`:**
* `curl -u user:pass -O ftp://localhost/test.txt`
* *Función:* Descarga el archivo remoto utilizando el protocolo URL.



## 4. Conclusión de la Actividad

Las pruebas confirman que el servidor vsftpd permite operaciones completas de gestión de archivos desde la línea de comandos. Se ha comprobado que:

1. El sistema de **autenticación** funciona correctamente.
2. Los **permisos de escritura** asignados al grupo en la actividad anterior permiten la subida de ficheros (`put`).
3. La **descarga** (`get`) se realiza sin corrupción de datos.

---

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T6.png)
