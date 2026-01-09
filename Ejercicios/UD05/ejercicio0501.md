<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 1: Introducción y arquitectura de FileZilla Server</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 9 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## 📌 Enunciado

### 🎯 Objetivo

Comprender en profundidad qué es un servidor **FTP/FTPS**, su funcionamiento interno y la arquitectura específica de **FileZilla Server**.

### 📚 Contenidos a tratar

* **Comparativa conceptual:** FTP vs. FTPS vs. SFTP.
* **Arquitectura:** Modelo Cliente–Servidor.
* **Redes:** Puertos implicados (Puerto 21, Rangos Pasivos, etc.).

---

### 📝 Tarea a realizar

> **Instrucción:**
> Realiza un esquema explicativo del **flujo de conexión** entre un cliente y un servidor FTP que detalle los siguientes puntos:

1. **Canales de comunicación:**
* Canal de Control.
* Canal de Datos.


2. **Modos de funcionamiento:**
* Diferencias clave entre **Modo Activo** y **Modo Pasivo**.

---

## 1. Esquema del flujo de conexión FTP

El protocolo FTP se diferencia de otros protocolos web (como HTTP) porque utiliza **dos canales de comunicación independientes** para funcionar. Comprender esta arquitectura es fundamental para configurar correctamente los firewalls y el servidor.

### 1.1. Canales de Comunicación

Para establecer una sesión, intervienen dos tipos de conexiones:

1. **Canal de Control (Puerto 21):**
* Se establece al inicio de la sesión mediante el protocolo TCP.
* Permanece **abierto** durante toda la conexión.
* Su función es transmitir **comandos** (usuario, contraseña, `CWD`, `LIST`, `RETR`) y recibir las respuestas de estado del servidor (ej. `200 OK`, `550 Error`).
* Es el "cerebro" de la operación; por aquí no viajan los archivos, solo las órdenes.


2. **Canal de Datos (Puerto Variable):**
* Se abre **bajo demanda** y de forma temporal.
* Solo se activa cuando se requiere transferir información (subir/bajar un archivo o listar el contenido de un directorio).
* Una vez terminada la transferencia, este canal se cierra.
* La forma en que se abre este canal depende del **Modo** elegido (Activo o Pasivo).



---

### 1.2. Modos de Conexión: Diferencias entre Activo y Pasivo

La diferencia principal radica en **quién inicia la conexión TCP** para el Canal de Datos.

* **Modo Activo (Active):** El cliente "abre la puerta" y espera. Envía el comando `PORT` diciéndole al servidor su IP y puerto. El **Servidor** (desde su puerto 20) inicia la conexión hacia el cliente.
* *Inconveniente:* Los firewalls del lado del cliente suelen bloquear esta conexión entrante externa.


* **Modo Pasivo (Passive):** El servidor "abre la puerta" en un puerto aleatorio (Rango Pasivo). El cliente envía el comando `PASV` y el servidor responde con la IP y el puerto donde debe conectarse. El **Cliente** inicia la conexión hacia el servidor.
* *Ventaja:* Es el estándar actual ("Firewall friendly"), ya que la mayoría de firewalls permiten las conexiones salientes iniciadas por el propio usuario.



---

CLIENTE (💻)                                SERVIDOR (🖥️)
           |                                            |
           |   🟢 1. CANAL DE CONTROL (Puerto 21)       |
           |===========================================>|
           |       (Usuario / Contraseña / Órdenes)     |
           |                                            |
           |         ¿CÓMO PASAMOS LOS DATOS?           |
           |                                            |
   ________V________                            ________V________
  |   OPCIÓN A:     |                          |   OPCIÓN B:     |
  |  MODO ACTIVO    |                          |  MODO PASIVO    |
  |_________________|                          |_________________|
           |                                            |
 1. Cliente envía: "PORT N"                   1. Cliente envía: "PASV"
 (Ábreme el puerto N)                                   |
           |                                  2. Servidor responde:
           |                                  "Ve al Puerto P (Pasivo)"
           |                                            |
 2. SERVIDOR inicia conexión                  3. CLIENTE inicia conexión
    DE DATOS (🔴)                                DE DATOS (🔴)
           |                                            |
 [Puerto 20] ----> [Puerto N]             [Puerto N] ----> [Puerto P]
           |                                            |
   (Problema con Firewalls)                   (Ideal para Firewalls)



sequenceDiagram
    autonumber
    participant C as 💻 Cliente
    participant S as 🖥️ Servidor FTP

    Note over C,S: 🟢 CANAL DE CONTROL (Puerto 21)<br/>Se establece al inicio y no se cierra
    C->>S: SYN / ACK (Conexión TCP al Puerto 21)
    C->>S: USER / PASS (Autenticación)
    S-->>C: 230 User logged in

    rect rgb(240, 240, 240)
        Note over C,S: ⬇️ AQUI SE ELIGE EL MODO DE TRANSFERENCIA ⬇️
    end

    alt ⚡ MODO ACTIVO (Original)
        Note left of C: Cliente abre Puerto X<br/>y "escucha"
        C->>S: Comando PORT (Mi IP, Puerto X)
        Note right of S: Servidor usa su Puerto 20<br/>para conectar al cliente
        S->>C: 🔴 CANAL DE DATOS (Origen 20 -> Destino X)
        S-->>C: ACK / Transferencia de archivo
    else 🛡️ MODO PASIVO (Recomendado)
        C->>S: Comando PASV
        Note right of S: Servidor abre Puerto Y<br/>(Rango Pasivo) y "escucha"
        S-->>C: 227 Entering Passive Mode (IP, Puerto Y)
        Note left of C: Cliente inicia la conexión<br/>hacia el servidor
        C->>S: 🔴 CANAL DE DATOS (Origen X -> Destino Y)
        S-->>C: ACK / Transferencia de archivo
    end

    Note over C,S: 🟢 El Canal de Control sigue activo para más comandos


---

### 1.3. Tabla Resumen de Diferencias

| Característica | Modo Activo | Modo Pasivo |
| --- | --- | --- |
| **Iniciador de Datos** | El **Servidor** conecta al Cliente. | El **Cliente** conecta al Servidor. |
| **Comando utilizado** | `PORT` | `PASV` |
| **Puerto Origen (Servidor)** | Puerto 20. | Puerto aleatorio (>1023). |
| **Configuración Crítica** | Requiere abrir puertos en el firewall del **Cliente**. | Requiere configurar el "Passive Port Range" en el **Servidor**. |

