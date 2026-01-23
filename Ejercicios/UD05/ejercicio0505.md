<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 5: Pruebas en modo activo y pasivo</h1>

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

## 1. Configuración del Rango Pasivo
Para permitir conexiones a través de firewalls, se configuró un rango de puertos específico en `/etc/vsftpd.conf`:
- **Rango:** 40000 - 40100
- **Directivas:** `pasv_min_port=40000` y `pasv_max_port=40100`.

## 2. Metodología de Prueba (Línea de Comandos)
Se utilizó el cliente FTP de consola para inspeccionar el intercambio de comandos entre cliente y servidor. Los pasos fueron:

1. **Conexión:** `ftp 192.168.1.XX`
2. **Activación de Debug:** Se ejecutó el comando `debug` para visualizar los comandos de control (`PORT` y `PASV`).
3. **Alternancia de modos:** Se utilizó el comando `passive` para conmutar entre ambos estados.

### Resultados observados en consola:
* **En Modo Activo:** Al ejecutar `ls`, el cliente envió el comando `PORT X,X,X,X,Y,Y`, indicando al servidor a qué IP y puerto debía conectarse.
* **En Modo Pasivo:** El cliente envió el comando `PASV`, y el servidor respondió con `227 Entering Passive Mode`, abriendo un puerto dentro del rango 40000-40100.

## 3. Tabla Comparativa: Activo vs. Pasivo

| Característica | Modo Activo (PORT) | Modo Pasivo (PASV) |
| :--- | :--- | :--- |
| **Comando del cliente** | Envía comando `PORT` | Envía comando `PASV` |
| **Conexión de Datos** | Iniciada por el **Servidor** | Iniciada por el **Cliente** |
| **Problemas de Firewall** | Bloqueado frecuentemente en el cliente. | Atraviesa firewalls con facilidad. |
| **Configuración Server** | Por defecto (puerto 20). | Requiere rango `pasv_min/max`. |

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T5.png)

## 4. Análisis de Funcionamiento
En redes con firewall, el **Modo Pasivo es superior**. 

La mayoría de los firewalls modernos (incluido el de Windows o routers domésticos) bloquean por seguridad cualquier conexión entrante no solicitada. En el **Modo Activo**, el servidor intenta abrir una conexión hacia el cliente, lo que activa las defensas del firewall. En cambio, en el **Modo Pasivo**, todas las conexiones son salientes desde el punto de vista del cliente, cumpliendo con las políticas de seguridad estándar.

---

## 5. Evidencias de Conexión
A continuación se adjuntan las capturas donde se aprecia el uso del comando `debug` y la diferencia entre el envío de `PORT` (Activo) y `PASV` (Pasivo).

> **Captura de Verificación (Modo Consola):**

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T51.png)
