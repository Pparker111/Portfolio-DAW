# Documentación de Configuración de Apache2 con SSL en Ubuntu

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

## Conclusión

Con estos pasos, Apache2 queda configurado correctamente con SSL/TLS y
redirección HTTPS.
