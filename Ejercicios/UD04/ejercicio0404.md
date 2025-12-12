
Instala Apache en tu máquina virtual, si no está ya instalado.

Configura un conector AJP o un reverse proxy hacia Tomcat.

Verifica que la aplicación web responde a través del servidor web Apache.

Documenta los pasos realizados.

Incluye una captura final con el resultado funcionando.

📑 Documentación del Proceso
✅ 1. Instalación de Apache

Lo primero fue actualizar repositorios e instalar Apache:

sudo apt update
sudo apt install apache2 -y


Después comprobé que Apache estaba activo:

systemctl status apache2


Accedí en el navegador a:

http://localhost


y verifiqué que mostraba la página de bienvenida de Apache.

⚠️ 2. Primer fallo encontrado y solución

Al activar los módulos del proxy, inicialmente me dio error porque no estaban instalados correctamente o no habían sido cargados.

Comando utilizado:

sudo a2enmod proxy
sudo a2enmod proxy_http


❌ Fallo: Apache mostraba un error al recargar la configuración.
✔ Solución: Revisé la sintaxis del VirtualHost y corregí un error en la ruta del ProxyPass. Después ejecuté:

sudo systemctl restart apache2


Una vez corregido, Apache recargó sin problemas.

🟦 3. Configuración del Reverse Proxy hacia Tomcat (método elegido)

Habilitamos los módulos necesarios:

sudo a2enmod proxy
sudo a2enmod proxy_http
sudo systemctl restart apache2


Creamos un nuevo sitio de configuración:

sudo nano /etc/apache2/sites-available/tomcat-proxy.conf


Contenido del archivo:

<VirtualHost *:80>
    ServerName localhost

    ProxyPreserveHost On
    ProxyPass /hello http://localhost:8080/hello
    ProxyPassReverse /hello http://localhost:8080/hello
</VirtualHost>


Activación del sitio:

sudo a2ensite tomcat-proxy.conf
sudo systemctl reload apache2

✅ 4. Verificación final

Accedí a la aplicación a través de Apache:

http://localhost/hello


La aplicación respondió correctamente sin necesidad de usar el puerto 8080, lo que confirma que el reverse proxy funciona.
