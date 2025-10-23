# 🧠 Iniciación a la Virtualización  

**Autor:** *Pedro Ignacio Díaz-Alejo Marchante*  
**Fecha:** *23/10/2025*  

---

## 🚀 Instalación de Docker Desktop en Ubuntu  

### 1️⃣ Actualiza los repositorios  
   sudo apt update && sudo apt upgrade -y
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/2.png)

### 2️⃣ Instala dependencias
   sudo apt install ca-certificates curl gnupg -y
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/3.png)

### 3️⃣ Añade el repositorio de Docker e instala Docker Desktop
   curl -fsSL https://get.docker.com | sudo sh
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/4.png)
   sudo apt install docker-desktop -y
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/5.png)

### 4️⃣ Verifica que Docker funciona correctamente
   docker --version
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/6.png)
   docker run hello-world
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/7.png)

## 🧱 Instalación de contenedores de servidor web y de aplicaciones
💡 Conceptos

**Servidor web:** Atiende peticiones HTTP/HTTPS (*por ejemplo: Nginx, Apache*).

**Servidor de aplicaciones:** Ejecuta la lógica de negocio (*por ejemplo: Tomcat, WildFly*).

### 1️⃣ Buscar imágenes disponibles
   docker search nginx
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/8.png)
   docker search tomcat
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/9.png)

### 2️⃣ Descargar e iniciar contenedores
#### 🌐 Servidor Web (Nginx)
   docker run -d -p 8080:80 --name webserver nginx
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/10.png)
#### ⚙️ Servidor de Aplicaciones (Tomcat)
   docker run -d -p 8081:8080 --name appserver tomcat
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/11.png)

### 3️⃣ Verificar contenedores activos
   docker ps
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/12.png)

### 4️⃣ Abrir en el navegador
🌍 Nginx: https://localhost:8080
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/13.png)
☕ Tomcat: https://localhost:8081
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/14.png)
