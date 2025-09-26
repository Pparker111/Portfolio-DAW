# Seguridad en el repositorio

## Exclusión de archivos sensibles
Se utiliza un archivo `.gitignore` para evitar subir al repositorio:

- Archivos con contraseñas o claves (`.env`, `*.key`, `*.pem`).
- Archivos de configuración local (`config/*.php`, `config/*.json`, `config/*.yml`).
- Dependencias externas (`node_modules/`, `vendor/`).
- Archivos temporales, logs y backups (`*.log`, `*.tmp`, `*.bak`, `*.sql`).
- Archivos generados automáticamente por el sistema o el editor (`.DS_Store`, `Thumbs.db`, `.vscode/`, `.idea/`).

**Motivos principales:**
- Prevenir la filtración de datos sensibles (ej. claves de base de datos o API).
- Evitar problemas de compatibilidad entre entornos locales.
- Mantener el repositorio limpio y ligero sin archivos innecesarios.

---

## Accesibilidad y permisos
El repositorio en GitHub está configurado para que distintos colaboradores tengan niveles de acceso adecuados:

- **Lectura (Read)**: para revisores o miembros que solo necesitan consultar el código (ej. cliente, tester externo).
- **Escritura (Write)**: para desarrolladores que contribuyen con cambios en HTML, CSS, JS o PHP mediante commits y pull requests.
- **Administrador (Admin)**: para responsables del proyecto, encargados de la configuración del repositorio, gestión de ramas protegidas y control de accesos.

**Recomendación de uso en un equipo de desarrollo web:**
- Revisor → *Read*  
- Desarrollador → *Write*  
- Líder del proyecto → *Admin*  

---

## Buenas prácticas de seguridad
1. **Uso de ramas protegidas**: la rama principal (`main`) está protegida para requerir *pull requests* y revisiones antes de fusionar cambios.  
2. **Revisión de código**: todo cambio debe pasar por al menos una revisión para garantizar la calidad y seguridad.  
3. **Copias de seguridad**: el repositorio se aloja en GitHub (con backups automáticos). Además, cada desarrollador debería clonar el proyecto en un entorno seguro como respaldo adicional.  
4. **Recuperación ante fallos**:  
   - En caso de pérdida de datos, se puede restaurar el repositorio directamente desde GitHub.  
   - Para errores graves en commits, se recomienda el uso de `git revert` (deshacer cambios puntuales) o `git reset` (volver a un estado previo).  
   - Se sugiere mantener *tags* en versiones estables del proyecto web para facilitar la restauración.  
