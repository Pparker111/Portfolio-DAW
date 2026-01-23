<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/01/FileZilla_logo.svg" alt="FileZilla Logo" width="120"/>
</p>

<h1 align="center">Actividad 9: Uso del navegador como cliente FTP</h1>

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

## 1. Contexto Actual de los Navegadores
Históricamente, los navegadores permitían el acceso directo mediante la URL `ftp://`. Sin embargo, desde el año 2021, la mayoría de los motores de navegación (Chromium y Firefox) han **eliminado el soporte nativo** para el protocolo FTP por razones de seguridad (tráfico no cifrado por defecto) y falta de uso.

Para realizar esta prueba, se debe considerar que:
* En navegadores modernos, a menudo se requiere una extensión de terceros.
* En versiones antiguas o navegadores específicos, el acceso es de **solo lectura**.

## 2. Ventajas del Navegador como Cliente FTP
A pesar de sus limitaciones, el uso del navegador presenta algunos puntos positivos en contextos muy específicos:

* **Universalidad:** No requiere instalar software especializado como FileZilla o WinSCP; cualquier equipo con un navegador puede intentar la conexión.
* **Facilidad de uso:** La interfaz es idéntica a la navegación web, lo que la hace intuitiva para usuarios no técnicos.
* **Previsualización:** Permite visualizar rápidamente archivos de texto o imágenes directamente en la ventana del navegador sin necesidad de descargarlos primero al disco local.
* **Ideal para repositorios públicos:** Es una solución excelente para servidores de descarga anónima donde el usuario solo necesita bajar archivos (Modo lectura).

## 3. Desventajas y Limitaciones frente a Clientes Dedicados
El navegador es una herramienta limitada cuando se compara con un cliente profesional:

| Limitación | Navegador Web | Cliente Dedicado (FileZilla/WinSCP) |
| :--- | :--- | :--- |
| **Operaciones** | Generalmente solo permite **descarga**. | Permite subir, descargar, renombrar y borrar. |
| **Gestión de Permisos** | No permite cambiar atributos (CHMOD). | Control total sobre permisos de archivos. |
| **Transferencias masivas** | No gestiona colas de archivos ni pausas. | Permite transferir miles de archivos en paralelo. |
| **Seguridad (FTPS)** | Soporte muy limitado o nulo para TLS/SSL. | Soporte completo para certificados y cifrado. |
| **Modos de conexión** | No permite elegir entre Modo Activo o Pasivo. | Configuración avanzada de puertos y protocolos. |

## 4. Conclusión
El navegador web puede servir como una herramienta de **emergencia** o de **consulta rápida** para descargar un archivo puntual. Sin embargo, para la administración de un servidor FTP, la subida de contenidos o la gestión de seguridad (como el cifrado configurado en la Actividad 8), el uso de un **cliente dedicado es imprescindible**.

El navegador sacrifica potencia y seguridad en favor de la simplicidad de acceso.
