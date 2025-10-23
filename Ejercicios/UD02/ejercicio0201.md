# 🧠 Iniciación a la Virtualización  

**Autor:** *Pedro Ignacio Díaz-Alejo Marchante*  
**Fecha:** *23/10/2025*  

---

## 🚀 Instalación de Docker Desktop en Ubuntu  

### 1️⃣ Actualiza los repositorios  
```bash
sudo apt update && sudo apt upgrade -y
```
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/2.png)  

### 2️⃣ Instala dependencias
```bash
sudo apt install ca-certificates curl gnupg -y
```
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/3.png)  

### 3️⃣ Añade el repositorio de Docker e instala Docker Desktop
```bash
curl -fsSL https://get.docker.com | sudo sh
```
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/4.png)  
```bash
sudo apt install docker-desktop -y
```
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/5.png)  

### 4️⃣ Verifica que Docker funciona correctamente
   docker --version  
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/6.png)  
   </div>
   docker run hello-world
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/7.png)  
   </div>

## 🧱 Instalación de contenedores de servidor web y de aplicaciones
💡 **Conceptos**

**Servidor web:** Atiende peticiones HTTP/HTTPS (*por ejemplo: Nginx, Apache*).

**Servidor de aplicaciones:** Ejecuta la lógica de negocio (*por ejemplo: Tomcat, WildFly*).

### 1️⃣ Buscar imágenes disponibles
   docker search nginx
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/8.png)  
   </div>
   docker search tomcat
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/9.png)  
   </div>

### 2️⃣ Descargar e iniciar contenedores
#### 🌐 Servidor Web (Nginx)
   docker run -d -p 8080:80 --name webserver nginx
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/10.png)  
   </div>
#### ⚙️ Servidor de Aplicaciones (Tomcat)
   docker run -d -p 8081:8080 --name appserver tomcat
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/11.png)  
   </div>

### 3️⃣ Verificar contenedores activos
   docker ps
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/12.png)  
   </div>

### 4️⃣ Abrir en el navegador
🌍 **Nginx**: https://localhost:8080
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/13.png)  
   </div>
☕ **Tomcat**: https://localhost:8081
   <div style="text-align: center;">
   ![](https://github.com/Pparker111/Portfolio-DAW/blob/main/Ejercicios/UD02/imagenes/14.png)
   </div>
