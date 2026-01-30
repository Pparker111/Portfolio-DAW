# 🐙 Diario de Reflexión (Tema 5): Servidores FTP (vsftpd/FileZilla) ✍️

## ¿Qué he aprendido? 🎓

He aprendido que un servidor FTP profesional requiere algo más que una instalación básica: exige gestionar **enjaulamientos (chroot)** para la seguridad y configurar **cifrado SSL/TLS** para proteger los datos. He comprendido que **vsftpd** es la potencia en el servidor y **FileZilla** la herramienta para controlarlo visualmente.

## ¿Qué no entiendo? 🤔

Me resulta todavía confuso el funcionamiento técnico de los **puertos pasivos** y cómo el tráfico negocia la apertura de conexiones a través de firewalls sin perder la seguridad.

## ¿Qué es lo que más me ha gustado y qué es lo que menos?

* **👍 Lo que más:** La **integración web**. Subir un archivo por FTP y que aparezca al instante en el navegador vía Apache me ha parecido muy útil.
* **👎 Lo que menos:** Las peleas con los **permisos de escritura** (`write_enable`). Es frustrante cuando un pequeño ajuste impide que todo el sistema funcione.

## ¿Qué más me gustaría saber? 🚀

Me gustaría profundizar en la **automatización de despliegues** (que los archivos se suban solos al guardar) y entender las diferencias de rendimiento entre FTPS y SFTP.

---
