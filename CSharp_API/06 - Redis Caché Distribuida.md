
## ¿Qué es la `Caché`?

La caché es un sistema de almacenamiento temporal que guarda datos de acceso frecuente para acelerar la recuperación en futuras peticiones, evitando repetir operaciones costosas como consultas a bases de datos o llamadas a servicios externos.

## ¿Por qué utilizar caché?

- Mejora el rendimiento y velocidad de la aplicación.
- Reduce el consumo de recursos (menos consultas a la base de datos).
- Mejora la experiencia del usuario.
- Disminuye la latencia en servicios web o APIs.

##  ¿Qué es el almacenamiento en caché?

Es el proceso de guardar datos temporalmente en memoria o en algún medio rápido para evitar recalcularlos o volver a obtenerlos.  

`Ejemplo:`

- Guardar el resultado de una consulta SQL para no ejecutarla cada vez.
- Almacenar una respuesta de una API externa por 5 minutos.
- Guardar configuraciones, catálogos, listas que no cambian frecuentemente.


## Tipos de caché

|        Tipo         |        Descripción                                                        |
|  :---               |  :---                                                                     |
|  En memoria (local) |  Los datos se guardan en la RAM del servidor donde corre la app. Rápido.  |
|  Distribuida        |  La caché se guarda en un servidor aparte o compartido (ej. Redis).       |


## `MemeryCache` en C#

**MemoryCache** es el sistema de caché en memoria local que provee .NET. Es rápido, sencillo y muy útil para almacenar datos temporalmente dentro del mismo servidor o instancia de aplicación.

## ¿Qué es caché `distribuida`?

La **caché distribuida** es una forma de almacenar datos en un sistema externo o compartido (como Redis), de forma que múltiples instancias de una aplicación puedan acceder a los mismos datos en caché.

## ¿Qué es `Redis`?

**Redis** es un sistema de caché distribuido basado en memoria, extremadamente rápido.  
Permite guardar:

- Strings
- Listas
- Objetos serializados (como JSON)
- Valores clave (key-value)

Redis almacena los datos en `RAM`, pero también puede configurarse para que los persista en disco.

`Paquete a instalar:` 

**Oficial de Microsoft:**  
```
dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
```

**Alternativa:**  
```
dotnet add package EasyCaching.Redis
```

## Flujo de caché en Redis

![cacheredis](/CSharp_API/img/cacheredis.png)

## ¿Cúando usar caché?

|      Escenario                             |        ¿Usar caché?          |
|  :---                                      |  :---                        |
|  Consultas a base de datos muy frecuentes  |  **SI**                      |
|  Datos que cambian poco                    |  **SI**                      |
|  Llamadas a APIs externas lentas           |  **SI**                      |
|  Datos sensibles o confidenciales          |  **NO** (solo si se protegen)|
|  Datos en tiempo real que cambian mucho    |  **NO**                      |

## ¿Se puede usar tanto caché en memoria como distribuida?

**Si** 

### Flujo al usar ambas

![ambascache](/CSharp_API/img/ambascache.png)

## ¿Solo se usa en `microservicios`?

**No**. La caché se puede usar en cualquier tipo de aplicación:

- Web APIs.
- Aplicaciones de escritorio.
- Aplicaciones monolíticas.
- Microservicios.
- Blazor, MAUI, etc.

> La caché distribuida es más común en microservicios porque hay varias instancias, pero no es exclusiva de ellos.

