
## ¿Qué es una `API Key`?

Una API Key (clave de API) es una cadena única (como un token) que identifica y autentica a un cliente o aplicación al consumir una API.

### Se usa para:

- `Identificar` quién está haciendo la solicitud.
- `Restringir` el acceso a ciertas funciones o recursos.
- `Limitar` el uso (rate limiting) por usuario, app, o servicio.
- `Registrar` actividad (logging, monitoreo por cliente).


## ¿Cuándo usar API Keys?

| Escenario                                         | ¿Usar API Key?                             |
| :---                                              | :---                                       |
| APIs públicas con acceso controlado básico        |  Sí                                        |
| Servicios entre microservicios en una red privada |  Sí (opcional, depende de la arquitectura) |
| Para identificar apps que consumen la API         |  Sí                                        |
| Para uso por terceros (desarrolladores externos)  |  Sí                                        |

## ¿Cuándo `NO` usar API Keys?

| Escenario                                                       | ¿Usar API Key? |
| :---                                                            | :----:         |
| Autenticación de usuarios finales (login, permisos, etc.)       |  No            |
| Seguridad robusta (necesidad de encriptar, roles, expiración)   |  No            |
| Cuando se necesita control de sesiones, revocación o scopes     |  No            |

## Limitaciones de las API Keys

- No tienen expiración por defecto (a menos que tú lo programes).
- Se pueden filtrar fácilmente si no se protegen (por ejemplo, en frontend o repos públicos).
- No manejan identidades de usuarios, solo de aplicaciones.
- No permiten granularidad de permisos (como "solo lectura").

## ¿Qué hacer si una API Key se compromete?

1. `Revocar` o `deshabilitar` inmediatamente la clave comprometida.
2. `Generar` una nueva API Key y `actualizar` los sistemas que la usan.
3. `Investigar` dónde fue **filtrada** (repositorio público, logs, cliente, etc.).
4. `Reforzar` seguridad:
    - **No** subir claves a GitHub.
    - `Usar` variables de entorno o vaults.
    - `Aplicar` control de acceso por IP, **rate limiting**, y expiración.



