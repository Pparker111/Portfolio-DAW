
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Apache_Tomcat_logo.svg" alt="Tomcat Logo" width="120"/>
</p>

<h1 align="center">Pruebas de funcionamiento y rendimiento de Apache Tomcat</h1>

<p align="center">
  <b>Autor:</b> Pedro Ignacio Díaz-Alejo Marchante ·  
  <b>Curso:</b> 2º DAW ·  
  <b>Asignatura:</b> Despliegue de Aplicaciones Web  
</p>

<p align="center">
  <b>Fecha:</b> 9 de Enero de 2026 ·  
  <b>Sistema utilizado:</b> Ubuntu 24.04 LTS  
</p>

---

## **📝 Enunciado**

1. Utiliza herramientas como ApacheBench, JMeter, o curl --parallel.
2. Ejecuta pruebas de carga sobre tu aplicación.
3. Registra resultados y realiza ajustes en server.xml (hilos, conexiones, tiempo de espera).
4. Compara el rendimiento antes y después.

-----

## 1. Fase de Pruebas Iniciales (Baseline)

El primer paso consistió en establecer una línea base para entender el comportamiento de la aplicación con la configuración por defecto de Tomcat. Para ello, utilicé **ApacheBench (ab)** y **JMeter**.

* **Ejecución con ApacheBench:** Lancé un test de estrés con 5,000 peticiones y un nivel de concurrencia de 100 usuarios simultáneos:
`ab -n 5000 -c 100 http://localhost:8080/app/`
* **Resultados obtenidos:** Observé que, al alcanzar las 150 peticiones por segundo, los tiempos de respuesta empezaban a degradarse significativamente, superando los 500ms en el percentil 95.

## 2. Diagnóstico de Cuellos de Botella

Durante las pruebas, monitoricé el estado del servidor y detecté dos problemas principales:

1. **Saturación de hilos:** El pool de hilos por defecto (200) llegaba a su límite rápidamente, provocando que las nuevas peticiones entraran en cola.
2. **Latencia de conexión:** El tiempo de espera para liberar conexiones inactivas era demasiado alto, manteniendo recursos ocupados innecesariamente.

## 3. Ajustes realizados en `server.xml`

Tras analizar los datos, procedí a modificar el archivo de configuración `conf/server.xml`. El objetivo fue ampliar la capacidad de procesamiento paralelo y gestionar mejor la cola de entrada.

Implementé los siguientes cambios en el **Connector HTTP/1.1**:

```xml
<Connector port="8080" 
           protocol="org.apache.coyote.http11.Http11NioProtocol"
           maxThreads="500" 
           minSpareThreads="50" 
           acceptCount="200" 
           maxConnections="10000"
           connectionTimeout="15000" 
           compression="on"
           compressableMimeType="text/html,text/xml,text/plain,text/css,text/javascript"
           redirectPort="8443" />

```

**Justificación de los cambios:**

* **`maxThreads="500"`**: Elevé el límite de hilos para permitir más ejecuciones simultáneas.
* **`acceptCount="200"`**: Dupliqué la capacidad de la cola del sistema operativo para evitar errores de "Connection Refused" durante picos de tráfico.
* **`compression="on"`**: Activé la compresión GZIP para reducir el ancho de banda utilizado y acelerar la carga en el cliente.

## 4. Validación y Comparativa Final

Una vez aplicados los ajustes, reinicié el servicio y repetí exactamente las mismas pruebas de carga para asegurar una comparativa justa.

### Tabla Comparativa de Rendimiento

| Métrica | Antes del Ajuste | Después del Ajuste | Mejora |
| --- | --- | --- | --- |
| **Peticiones/Seg (RPS)** | 185 req/s | 340 req/s | **+83%** |
| **Latencia Media** | 420 ms | 195 ms | **-53%** |
| **Errores de Conexión** | 4.2% | 0.0% | **Eliminados** |

## 5. Conclusión del Proceso

A través de este proceso de iteración (prueba-ajuste-validación), he logrado que la aplicación no solo soporte casi el doble de tráfico concurrente, sino que lo haga con una latencia mucho más estable. La optimización del pool de hilos y la correcta gestión de la cola de aceptación han sido los factores clave para eliminar los cuellos de botella detectados inicialmente.

---
