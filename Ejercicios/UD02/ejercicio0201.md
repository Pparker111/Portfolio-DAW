Iniciación a la Virtualización
Instalacion de Docker Desktop en Ubuntu
1. Actualiza los repositorios:
   sudo apt update && sudo apt upgrade -y
   [CAPTURA]

2. Instala dependencias:
   sudo apt install ca-certificates curl gnupg -y
   [CAPTURA]

3. Añade el repositorio de Docker e instala Docker Desktop:
   curl -fsSL https://get.docker.com | sudo sh
   [CAPTURA]
   sudo apt install docker-desktop -y
   [CAPTURA]

4. Verifica que Docker funciona correctamente:
   docker --version
   [CAPTURA]
   docker run hello-world
   [CAPTURA]

Instalación de contenedores de servidor web y de aplicaciones
Conceptos:
- Servidor web: Atiende peticiones HTTP/HTTPS (ej.:Nginx, Apache).
- Servidor de aplicaciones: Ejecuta la lógica de negocio (ej.: Tomcat, WildFly).

1. Busca imágenes disponibles:
   docker search nginx
   [CAPTURA]
   docker search tomcat
   [CAPTURA]

2. Descargar e iniciar contenedores:
   docker run -d -p 8080:80 --name webserver nginx
   [CAPTURA]
   docker run -d -p 8081:8080 --name appserver tomcat
   [CAPTURA]

3. Verificar contenedores activos:
   docker ps
   [CAPTURA]

4. Abrir en el navegador:
   - Nginx: https://localhost:8080
   [CAPTURA]
   - Tomcat: https://localhost:8081
   [CAPTURA]
