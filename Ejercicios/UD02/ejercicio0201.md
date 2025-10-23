# 🧠 Iniciación a la Virtualización  

**Autor:** *Pedro Ignacio Díaz-Alejo Marchante*  
**Fecha:** *23/10/2025*  

---

## 🚀 Instalación de Docker Desktop en Ubuntu  

### 1️⃣ Actualiza los repositorios  
sudo apt update && sudo apt upgrade -y
   ![]()
   [CAPTURA]

### 2️⃣ Instala dependencias
   sudo apt install ca-certificates curl gnupg -y
   [CAPTURA]

### 3️⃣ Añade el repositorio de Docker e instala Docker Desktop
   curl -fsSL https://get.docker.com | sudo sh
   [CAPTURA]
   sudo apt install docker-desktop -y
   [CAPTURA]

### 4️⃣ Verifica que Docker funciona correctamente
   docker --version
   [CAPTURA]
   docker run hello-world
   [CAPTURA]

## 🧱 Instalación de contenedores de servidor web y de aplicaciones
💡 Conceptos

**Servidor web:** Atiende peticiones HTTP/HTTPS (*por ejemplo: Nginx, Apache*).

**Servidor de aplicaciones:** Ejecuta la lógica de negocio (*por ejemplo: Tomcat, WildFly*).

### 1️⃣ Buscar imágenes disponibles
   docker search nginx
   [CAPTURA]
   docker search tomcat
   [CAPTURA]

### 2️⃣ Descargar e iniciar contenedores
#### 🌐 Servidor Web (Nginx)
   docker run -d -p 8080:80 --name webserver nginx
   [CAPTURA]
#### ⚙️ Servidor de Aplicaciones (Tomcat)
   docker run -d -p 8081:8080 --name appserver tomcat
   [CAPTURA]

### 3️⃣ Verificar contenedores activos
   docker ps
   [CAPTURA]

### 4️⃣ Abrir en el navegador
🌍 Nginx: https://localhost:8080
   [CAPTURA]
☕ Tomcat: https://localhost:8081
   [CAPTURA]
