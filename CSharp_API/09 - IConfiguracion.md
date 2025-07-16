# IConfiguration en .NET

## ¿Qué es IConfiguration?

`IConfiguration` es una interfaz integrada en .NET que permite acceder a configuraciones clave/valor desde diversas fuentes como:

- Archivos JSON (`appsettings.json`)
- Variables de entorno
- Línea de comandos
- Secretos de usuario
- Servicios remotos (Azure, etc.)

Se utiliza para centralizar parámetros y configuraciones que pueden cambiar entre entornos sin modificar el código fuente.

---

## ¿Para qué sirve?

- Separar lógica del entorno de ejecución
- Configurar conexiones, rutas, claves y ajustes
- Cambiar valores sin recompilar la aplicación
- Aplicar configuraciones por entorno (`Development`, `Production`, etc.)

---

## ¿Cuándo se usa?

- Siempre que se necesite acceder a configuraciones externas
- Cuando una aplicación cambia de entorno
- Cuando se implementa inyección de configuraciones en clases o servicios

---

## Archivos de configuración principales

| Archivo                        | Propósito                                         |
|-------------------------------|---------------------------------------------------|
| `appsettings.json`            | Configuración base común para todos los entornos |
| `appsettings.Development.json`| Configuración específica para desarrollo          |
| `appsettings.Production.json` | Configuración para producción                     |
| `secrets.json`                | Configuración secreta para desarrollo local       |
| Variables de entorno          | Para sobrescribir configuraciones sin modificar archivos |

---
