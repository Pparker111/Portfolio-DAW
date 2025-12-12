
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Configuración de seguridad de Apache Tomcat</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 12 de Diciembre de 2025 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## **📝 Enunciado**

Realiza una prueba de acceso autenticado y documenta el proceso.

Pistas:

Definir roles y usuarios en tomcat-users.xml.
Restringir el acceso al Manager.
Configurar HTTPS con un keystore y un conector SSL.
Activar security manager (opcional).

-----

### 📋 Tarea: Guía de Implementación y Configuración

#### 1\. Gestión de Identidad y Acceso (IAM)

Se define el *Realm* de Tomcat para autenticar usuarios contra un archivo local, asegurando el acceso restringido a las aplicaciones `manager` y `host-manager`.

| Archivo | Acción y Comandos | Notas Técnicas |
| :--- | :--- | :--- |
| `/opt/tomcat/conf/tomcat-users.xml` | Adición de roles necesarios y creación de la cuenta de administrador. **Sintaxis crítica** (XML): Debe estar contenida dentro de la etiqueta `<tomcat-users>`. <br> `**<role rolename="manager-gui"/>**` <br> `**<user username="admin" password="admin123" roles="manager-gui,admin-gui"/>**` | Los roles `manager-gui` y `admin-gui` son necesarios para acceder a las interfaces gráficas de las respectivas aplicaciones. |
| **Control del Servicio** | Reinicio del servicio para cargar la nueva configuración: <br> `**sudo systemctl restart tomcat**` | Un fallo en esta etapa indica un error de *parsing* XML en `tomcat-users.xml`. |

#### 2\. Restricción de Acceso Lógico (Control por IP)

Se aplica la política de seguridad más elemental en entornos *on-premise*: restringir el acceso a la aplicación Manager a una lista blanca de direcciones IP de gestión.

| Archivo | Acción y Comandos | Notas Técnicas |
| :--- | :--- | :--- |
| `/opt/tomcat/webapps/manager/META-INF/context.xml` | Modificación del atributo `allow` en la válvula `RemoteAddrValve` para definir el *whitelist* de IPs permitidas. <br> `allow="192\.168\.1\..*\|127\.0\.0\.1"` | Se utiliza una expresión regular (`.*`) para abarcar subredes, y `127\.0\.0\.1` para acceso local. |
| **Control del Servicio** | Reinicio del servicio: <br> `**sudo systemctl restart tomcat**` | Necesario para recargar el contexto de la aplicación Manager. |

#### 3\. Implementación del Cifrado de Capa de Transporte (SSL/TLS)

Se establece el cifrado HTTPS/SSL en el conector del puerto `8443`.

| Fase | Acción y Comandos | Notas Técnicas |
| :--- | :--- | :--- |
| **A. Generación de Keystore** | Generación del almacén de claves (Keystore) autofirmado para alojar el certificado SSL. <br> `**sudo keytool -genkeypair -alias tomcat -keyalg RSA -keystore /opt/tomcat/conf/keystore.jks -keysize 2048 -validity 365**` | Es **crucial** recordar la contraseña definida, ya que se usará en `server.xml`. |
| **B. Configuración del Conector** | Edición del conector de puerto `8443` en el archivo de configuración principal de Tomcat. Se utiliza la sintaxis compatible `protocol="HTTP/1.1"`. <br> `**sudo nano /opt/tomcat/conf/server.xml**` <br> Se configura la ruta y la contraseña del Keystore: <br> `**<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"**` <br> `**... keystoreFile="/opt/tomcat/conf/keystore.jks" keystorePass="CLAVE_CORRECTA" />**` | **IMPORTANTE:** Si el servicio falla, la causa más probable es una contraseña incorrecta en `keystorePass` o un error de sintaxis al descomentar el bloque XML. |
| **C. Verificación** | Se comprueba que el conector esté activo tras el reinicio: <br> `**sudo systemctl restart tomcat**` <br> `**sudo netstat -tulnp \| grep 8443**` | El comando `netstat` debe mostrar el proceso Java de Tomcat escuchando en el puerto `8443`. |

-----

### ❌ Estado Final y Hito de la Tarea

La implementación técnica de los pasos 1, 2 y 3 fue completada con éxito, incluyendo la resolución de errores críticos de sintaxis XML y la corrección de la configuración del conector SSL. Todos los servicios necesarios (puertos 8080 y 8443) están activos y funcionales a nivel de servidor.

**Objetivo Pendiente:** La fase de prueba final (`https://localhost:8443/manager/html`).

  * **Acceso HTTPS (8443):** Aunque el conector SSL se encuentra activo, no fue posible completar la prueba funcional para obtener la captura requerida que demuestre el acceso exitoso a la interfaz del Manager. Por lo tanto, el **Hito de la Prueba Funcional** no pudo realizarse.

**Hito Final para Captura:**
Demostrar el acceso exitoso a la interfaz del Manager tras la validación de credenciales sobre la conexión cifrada HTTPS (puerto 8443).

> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD04/imagenes/nose.png)
