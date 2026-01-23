<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 8: Configuración de FTP seguro (FTPS)</h1>

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

## 1. Generación del Certificado de Seguridad
Para habilitar el cifrado, el primer paso consistió en generar un certificado auto-firmado mediante OpenSSL. Este certificado permite identificar al servidor y establecer el túnel cifrado con el cliente.

* **Comando:** `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem`
* **Resultado:** Creación de un par de claves (pública/privada) bajo el estándar RSA de 2048 bits.

## 2. Configuración del Servidor (FTPS Explícito)
Se modificó el archivo `/etc/vsftpd.conf` para obligar al servidor a rechazar conexiones inseguras y forzar el uso de TLS.

### Parámetros clave:
* `ssl_enable=YES`: Habilita la capacidad SSL.
* `allow_anon_ssl=NO`: Impide conexiones anónimas cifradas (solo usuarios registrados).
* `force_local_data_ssl=YES`: Obliga a cifrar la transferencia de archivos.
* `force_local_logins_ssl=YES`: Obliga a cifrar el envío de contraseñas.

## 3. Verificación de Cifrado mediante Terminal
Para comprobar que el cifrado se aplica correctamente, se utilizó la herramienta de diagnóstico `openssl s_client`. Este comando simula una conexión de cliente y extrae los detalles del protocolo de seguridad negociado.

> **Captura de Verificación de Sesión Segura (FTPS):**
> 
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD05/imagenes/T8.png)

## 4. Análisis de la Verificación
Como se observa en los resultados:
* **Protocol:** Se utiliza una versión moderna de TLS (v1.2 o v1.3), garantizando que el servidor es inmune a vulnerabilidades de protocolos antiguos como SSLv2 o SSLv3.
* **Cipher:** El algoritmo `ECDHE-RSA-AES256-GCM-SHA384` indica que se está usando un cifrado de grado militar de 256 bits con intercambio de claves elípticas, lo que ofrece la máxima seguridad actual.

---

## Conclusión
La implementación de FTPS transforma un servicio vulnerable en una herramienta profesional y segura. Al forzar el cifrado, cualquier intento de interceptación de tráfico en la red solo mostraría datos ilegibles, protegiendo tanto las cuentas de usuario como la propiedad intelectual de los archivos transferidos.
