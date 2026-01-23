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
Se ha configurado el servidor para trabajar con un rango de puertos predecible, facilitando la apertura de reglas en el firewall del servidor:
- **Rango:** 40000 - 40100
- **Directivas:** `pasv_min_port` y `pasv_max_port`.

## 2. Resultados de las Pruebas
* **Conexión Modo Activo:** Se observaron dificultades cuando el equipo cliente se encontraba tras un router con NAT o firewall activo, ya que el servidor intentaba iniciar la conexión de datos hacia el cliente y era bloqueada.
* **Conexión Modo Pasivo:** Funcionó correctamente en todos los entornos, ya que es el cliente quien inicia ambas conexiones (control y datos) hacia el servidor.

## 3. Tabla Comparativa: Activo vs. Pasivo

| Característica | Modo Activo (PORT) | Modo Pasivo (PASV) |
| :--- | :--- | :--- |
| **Comando del cliente** | Envía comando `PORT` | Envía comando `PASV` |
| **Conexión de Datos** | Iniciada por el **Servidor** | Iniciada por el **Cliente** |
| **Problemas de Firewall** | Suele fallar en el lado del cliente. | Suele funcionar bien (estándar actual). |
| **Configuración Server** | Más sencilla. | Requiere definir rangos de puertos. |
| **Seguridad** | Menos seguro para el cliente. | Más seguro para el cliente. |

## 4. Análisis de Funcionamiento en Redes con Firewall
En redes modernas, el **Modo Pasivo funciona mejor**. 

Esto se debe a que la mayoría de los firewalls y routers permiten el tráfico de **salida** (del cliente al servidor) pero bloquean el de **entrada** no solicitado. En el modo activo, el servidor intenta "entrar" al cliente, lo cual es bloqueado por defecto. En el modo pasivo, el cliente "sale" hacia el servidor en ambos canales, algo que los firewalls permiten habitualmente.

---

## 5. Captura de Verificación
A continuación, se muestra la traza de una conexión exitosa en modo pasivo (usando el comando `PASV`):

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T5.png)

![Captura de conexión modo pasivo](URL_DE_TU_CAPTURA_AQUÍ)
