
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 2: Instalación y configuración inicial del servidor</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 16 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

# Informe de Instalación y Configuración de Servidor FTP

Este documento detalla el proceso seguido para la puesta en marcha de un servicio de transferencia de archivos en un entorno Ubuntu Server.

## 1. Introducción y Selección de Software
Inicialmente, se consideró la instalación de **FileZilla Server**. Sin embargo, se identificó que este software está diseñado principalmente para entornos Windows y no posee una versión de servidor nativa para distribuciones Linux. Por lo tanto, se optó por **vsftpd** (Very Secure FTP Daemon), que es el estándar de la industria en sistemas Unix por su ligereza y seguridad.

## 2. Proceso de Instalación
La instalación se realizó sin mayores complicaciones utilizando el gestor de paquetes oficial:

```bash
sudo apt update
sudo apt install vsftpd -y

```

## 3. Configuración del Servicio

Para cumplir con los requisitos, se accedió a la consola de administración mediante la edición del archivo `/etc/vsftpd.conf`.

### Parámetros modificados:

* **Puerto de escucha:** Se definió el puerto mediante la directiva `listen_port`.
* **Dirección IP:** Se vinculó el servicio a la IP local de la máquina mediante `listen_address` para controlar el acceso por interfaz.
* **Modo de escucha:** Se aseguró que `listen=YES` estuviera activo para permitir el funcionamiento en modo independiente (standalone).

## 4. Gestión del Servicio e Inicio Automático

Para garantizar que el servidor esté disponible tras cualquier reinicio del sistema, se configuró el inicio automático con el siguiente comando:

```bash
sudo systemctl enable vsftpd
sudo systemctl restart vsftpd

```

## 5. Incidencias

No se reportaron problemas críticos durante la configuración de **vsftpd**. El proceso fue fluido una vez descartada la opción de FileZilla Server y entendida la sintaxis del archivo de configuración de Linux.

---

## 6. Verificación del Funcionamiento

A continuación, se adjunta la captura de pantalla donde se observa el servicio en estado **active (running)** tras aplicar los cambios:

```


¿Quieres que te ayude a redactar una sección extra sobre cómo comprobar desde otro equipo que el puerto está realmente abierto?

```
