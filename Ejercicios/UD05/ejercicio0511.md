<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 11: Disponibilidad y buenas prácticas</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 30 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## 1. Control de Carga y Límites de Conexión

Para evitar que el servidor se sature o sufra ataques de denegación de servicio (DoS), es vital limitar los recursos:

* **Límite de clientes totales:** No permitir conexiones infinitas; ajustar según la capacidad del hardware (ej. `max_clients=50`).
* **Límites por IP:** Restringir el número de sesiones simultáneas desde una misma dirección para evitar abusos (ej. `max_per_ip=3`).
* **Control de ancho de banda:** Limitar la velocidad de transferencia (`local_max_rate`) para que un solo usuario no consuma todo el ancho de banda de la red.

## 2. Logs y Auditoría

Un servidor sin registros es un servidor "ciego". La monitorización es clave para la seguridad:

* **Registro de transferencias:** Activar `xferlog_enable=YES` para saber exactamente qué archivos se suben y bajan, y quién lo hace.
* **Logs detallados:** Mantener un registro de intentos de login fallidos para detectar ataques de fuerza bruta.
* **Rotación de logs:** Configurar herramientas como `logrotate` para que los archivos de registro no llenen el disco duro del servidor con el paso del tiempo.

## 3. Estrategia de Copias de Seguridad (Backup)

La disponibilidad depende de la capacidad de recuperación ante fallos:

* **Backup de configuración:** Guardar copias de seguridad de `/etc/vsftpd.conf` y de los certificados SSL creados.
* **Backup de datos:** Programar tareas (cron jobs) que copien el contenido de la carpeta de datos (ej. `/var/www/html`) a un almacenamiento externo o en la nube.
* **Regla 3-2-1:** Mantener 3 copias de los datos, en 2 soportes diferentes y al menos 1 fuera de la ubicación física del servidor.

## 4. Seguridad de Red: Firewall y NAT

El servidor debe estar protegido del tráfico no deseado:

* **Configuración del Firewall (UFW/Iptables):** Solo deben estar abiertos los puertos estrictamente necesarios: 21 (Control) y el rango de puertos pasivos (ej. 40000-40100) que configuramos anteriormente.
* **NAT y FTP Pasivo:** En entornos con router, es fundamental mapear correctamente los puertos pasivos hacia la IP interna del servidor para que los clientes externos puedan conectar sin errores de "Data Connection".
* **Bloqueo de IPs:** Implementar herramientas como `Fail2Ban` para bloquear automáticamente IPs que intenten contraseñas erróneas repetidamente.

---

### Resumen 

| Categoría | Recomendación Principal | Beneficio |
| --- | --- | --- |
| **Disponibilidad** | Limitar conexiones por IP | Evita saturación y ataques DoS. |
| **Seguridad** | Uso de FTPS (TLS) | Cifra credenciales y evita robo de datos. |
| **Integridad** | Copias de seguridad diarias | Recuperación rápida ante desastres. |
| **Auditoría** | Revisión periódica de logs | Identificación de accesos no autorizados. |

---
