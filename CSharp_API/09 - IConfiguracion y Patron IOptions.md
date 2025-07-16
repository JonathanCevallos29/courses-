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

# Patrón `IOptions` en .NET

## ¿Qué es el patrón `IOptions`?

El patrón `IOptions` permite trabajar con configuraciones fuertemente tipadas en .NET, basadas en clases que representan secciones del archivo de configuración (`appsettings.json`, variables de entorno, etc.).

Se utiliza en conjunto con `IConfiguration`, y su ventaja es que simplifica el acceso a configuraciones y permite validarlas fácilmente, sin tener que usar strings o claves sueltas.

---

## ¿Cómo se relaciona con IConfiguration?

- `IConfiguration` te permite acceder a datos de configuración usando claves (`["Seccion:Valor"]`).
- `IOptions<T>` permite mapear automáticamente esa sección a una clase `T`, evitando errores por cadenas mal escritas y facilitando pruebas.

---

## ¿Qué es `IOptions<T>`?

`IOptions<T>` es la forma más común y básica del patrón.

- Lee la configuración **una sola vez** al iniciar la aplicación.
- No cambia, incluso si los archivos de configuración cambian en tiempo de ejecución.
- Se recomienda usar en **servicios singleton** o cuando los valores **no cambian dinámicamente**.

### Cuándo usar:
- Configuraciones estáticas (por ejemplo, URL de una API, nombre de empresa, claves fijas).
- En clases con ciclo de vida Singleton o Transient.

---

## ¿Qué es `IOptionsSnapshot<T>`?

`IOptionsSnapshot<T>` permite obtener una nueva instancia de la configuración por cada request HTTP.

- Solo funciona en **servicios Scoped** (por request).
- Si el archivo `appsettings` cambia, la nueva configuración se aplica en el siguiente request.
- No es tiempo real, pero es útil para configuraciones que pueden cambiar entre peticiones.

### Cuándo usar:
- Cuando se necesita que cada request tenga su propia "versión actual" de los valores.
- Ideal en aplicaciones web con entornos que recargan configuración entre usuarios.
- **Solo para entornos `Scoped`** (por ejemplo, controladores web).

---

## ¿Qué es `IOptionsMonitor<T>`?

`IOptionsMonitor<T>` permite detectar cambios en tiempo real y notificar cuando los valores cambian.

- Se puede usar con cualquier ciclo de vida (`Scoped`, `Transient`, `Singleton`).
- Ofrece eventos (`OnChange`) para reaccionar cuando cambia la configuración.
- Ideal para valores que deben **actualizarse sin reiniciar la aplicación**.

### Cuándo usar:
- Cuando se necesita reaccionar a cambios sin reiniciar la app.
- Para aplicaciones de larga ejecución (como workers, servicios).
- Por ejemplo, para cambiar el nivel de logs, configuraciones dinámicas, recarga de API keys.


