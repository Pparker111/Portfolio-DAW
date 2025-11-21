<p align="center">
  <img src="https://assets.ubuntu.com/v1/29985a98-ubuntu-logo32.png" alt="Ubuntu Logo" width="100"/>
</p>

<h1 align="center">Documentación de Configuración de Apache2 con SSL en Ubuntu</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-alejo Marchante · 
  <b>Curso:</b> 2º DAW · 
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 21 de Noviembre de 2025 · 
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## 📑 Índice

1. [Investigación](#-investigación)
   - [Funcionamiento del protocolo HTTPS](#-1-funcionamiento-del-protocolo-https-y-su-importancia)
   - [Tipos de certificados SSL/TLS](#-2-tipos-de-certificados-ssltls)
   - [Módulos necesarios de Apache2](#-3-módulos-necesarios-de-apache2-para-ssltls)

2. [Resumen](#-resumen)

3. [Palabras clave](#-palabras-clave)

4. [Proceso de configuración](#-proceso-de-configuración)
   - [Instalación y verificación de Apache2](#1-instalación-y-verificación-de-apache2)
   - [Habilitar módulos SSL y Headers](#2-habilitar-módulos-ssl-y-headers)
   - [Generación del certificado SSLTLS](#3-generación-de-certificado-ssltls)
   - [Configuración del VirtualHost HTTPS](#4-configuración-del-virtualhost-para-https-443)
   - [Redirección HTTP → HTTPS](#5-redirección-http--https)
   - [Reiniciar y recargar Apache](#6-reiniciar-y-recargar-apache)
   - [Validación de la implementación](#7-validación-de-la-implementación)

5. [Conclusión](#-conclusión)

---

# 📘 Investigación

## 🔐 1. Funcionamiento del protocolo HTTPS y su importancia

HTTPS (HyperText Transfer Protocol Secure) es la versión segura de HTTP.  
Su principal diferencia es que:

- Cifra la comunicación entre cliente y servidor mediante **SSL/TLS**.  
- Evita que terceros intercepten, modifiquen o espíen los datos.  
- Asegura la identidad del servidor mediante un certificado digital.  
- Protege formularios, contraseñas, cookies y tráfico sensible.

En la actualidad es imprescindible por razones de seguridad, SEO y confianza del usuario.

---

## 🪪 2. Tipos de certificados SSL/TLS

### **✔ Certificado autofirmado**
- Generado por el propio servidor.
- Gratis.
- Válido para pruebas y entornos locales.
- Los navegadores muestran advertencia porque no proviene de una autoridad confiable.

### **✔ Certificado emitido por una CA (autoridad certificadora)**
- Firmado por entidades como Let’s Encrypt, DigiCert, etc.
- Los navegadores lo reconocen como seguro.
- Recomendado para producción.
- Let’s Encrypt ofrece certificados gratuitos y automáticos.

---

## ⚙ 3. Módulos necesarios de Apache2 para SSL/TLS

Para habilitar HTTPS en Ubuntu con Apache se requieren:

- **mod_ssl** → permite el uso de SSL/TLS.
- **mod_headers** → permite gestionar cabeceras como Strict-Transport-Security.
- **sites-available/default-ssl.conf** (plantilla opcional).

Estos módulos deben activarse antes de usar VirtualHosts en el puerto 443.

---

# 🎯 Resumen

El objetivo de esta práctica es **configurar un servidor Apache2 en Ubuntu para que funcione con HTTPS**, utilizando un certificado SSL/TLS (ya sea autofirmado o de Let’s Encrypt).  
Además, se incluye la redirección automática de HTTP → HTTPS, garantizando una navegación segura y cifrada.

---

## 🔑 Palabras Clave

`Apache2` · `Ubuntu 24.04` · `SSL` · `TLS` · `HTTPS` · `Certificado autofirmado` · `Let’s Encrypts` · `Certbot` · `Redirección` · `VirtualHost` · `Seguridad web`

---

# 🛠 Proceso de configuración 

## 1. Instalación y verificación de Apache2

``` bash
sudo apt update
sudo apt install apache2 -y
systemctl status apache2
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/21.png)

------------------------------------------------------------------------

## 2. Habilitar módulos SSL y Headers

``` bash
sudo a2enmod ssl
sudo a2enmod headers
sudo systemctl restart apache2
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/22.png)

------------------------------------------------------------------------

## 3. Generación de certificado SSL/TLS

### Opción A: Certificado Autofirmado

``` bash
sudo mkdir /etc/apache2/ssl
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/apache2/ssl/selfsigned.key \
  -out /etc/apache2/ssl/selfsigned.crt
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/23.png)
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/24.png)


------------------------------------------------------------------------

## 4. Configuración del VirtualHost para HTTPS (443)

Archivo: `tu-sitio-ssl.conf`

``` apache
<VirtualHost *:443>
    ServerName tu-dominio.com
    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/selfsigned.crt
    SSLCertificateKeyFile /etc/apache2/ssl/selfsigned.key

    <Directory /var/www/html>
        AllowOverride All
    </Directory>

    Header always set Strict-Transport-Security "max-age=31536000"
</VirtualHost>
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/25.png)
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/26.png)

------------------------------------------------------------------------

## 5. Redirección HTTP → HTTPS

Editar `000-default.conf`:

``` apache
<VirtualHost *:80>
    ServerName tu-dominio.com
    Redirect "/" "https://tu-dominio.com/"
</VirtualHost>
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/28.png)
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/29.png)

------------------------------------------------------------------------

## 6. Reiniciar y recargar Apache

``` bash
sudo a2ensite tu-sitio-ssl.conf
sudo systemctl restart apache2
```

------------------------------------------------------------------------

## 7. Validación de la implementación

### Navegador

Visitar:

    http://tu-dominio.com  → debe redirigir a →  https://tu-dominio.com

### curl

``` bash
curl -I http://tu-dominio.com
curl -I https://tu-dominio.com
```

> **Evidencia:**
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/30.png)
> ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD03/imagenes/31.png)

------------------------------------------------------------------------

## 🧩 Conclusión

No he tenido dificultades en la realización de esta práctica, pero no porque tenga un gran conocimiento previo, sino porque se me da muy bien seguir instrucciones técnicas paso a paso.

Es el mismo motivo por el que puedo montar muebles de IKEA sin problemas:
ya sea una estantería sencilla, un armario grande o una cómoda completa, si las instrucciones están claras, el proceso fluye sin complicaciones.

Ahora bien, si en mitad del proceso apareciera un error inesperado —como cuando falta un tornillo o un tablón en el mueble— ahí sí surgirían dificultades. Pero mientras la guía sea correcta y el material esté completo, ejecutar cada paso no supone ningún problema.

Gracias a ello, la configuración de Apache con HTTPS ha sido totalmente fluida y satisfactoria.

---

### 📖 Bibliografía

* Boucheron, Brian; Ellingwood, Justin. Cómo crear un certificado SSL autofirmado para Apache en Ubuntu 18.04. DigitalOcean Community, publicado el 9 de enero de 2020. Disponible en: https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-18-04-es

* SSLmarket. Redireccionamiento de HTTP a HTTPS. SSLmarket ayuda, artículo sin fecha específica. Disponible en: https://www.sslmarket.mx/ssl/help-redireccionamiento-de-http-a-https

* Blai Blog. Configurar HTTPS en Apache. Publicado en noviembre de 2018. Disponible en: https://www.blai.blog/2018/11/configurar-https-en-apache.html
 
