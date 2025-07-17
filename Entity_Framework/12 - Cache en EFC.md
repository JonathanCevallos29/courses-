# Caché en Entity Framework Core (EFC)

## ¿Qué es el caché en EF Core?

El **caché** en EF Core es un mecanismo para almacenar temporalmente datos en memoria con el fin de **reducir la cantidad de accesos a la base de datos**, mejorar el rendimiento y optimizar las consultas repetitivas.

EF Core implementa principalmente **caché en memoria (local)** por defecto, pero también puede integrarse con otras soluciones de caché externas para escenarios más avanzados.

---

## Tipos de caché en EF Core

EF Core maneja dos tipos principales de caché:

### 1. **First-Level Cache** (Caché de primer nivel)
- **¿Qué es?**  
  Es el caché interno que mantiene el `DbContext` en su ciclo de vida.
- **Características:**
  - Está habilitado por defecto.
  - Solo dura mientras el `DbContext` está en memoria.
  - Al consultar una entidad por segunda vez dentro del mismo contexto, no va a la base de datos, sino que la obtiene desde este caché.
- **Ejemplo típico:** Dos consultas del mismo registro dentro del mismo `DbContext`.

---

### 2. **Second-Level Cache** (Caché de segundo nivel)

- **¿Qué es?**  
  Es un mecanismo adicional que almacena resultados entre distintas instancias de `DbContext`, incluso entre diferentes peticiones de una aplicación.

- **Características:**
  - No está disponible por defecto en EF Core.
  - Requiere instalar bibliotecas externas como **EFCore Second Level Cache Interceptor**.
  - Permite almacenar consultas y resultados en memoria, Redis, etc.
  - Se puede configurar con políticas de expiración, invalidación por cambios, entre otros.
  - Utiliza **interceptores** que actúan en tiempo de ejecución para capturar y almacenar los resultados de las consultas antes de que se ejecuten realmente.
- **Uso típico:** Mejorar rendimiento en aplicaciones con muchas lecturas y pocos cambios en datos (como dashboards o catálogos).


---

## ¿Cómo se implementa el caché en EF Core?

### First-Level Cache
- No requiere configuración. Se usa automáticamente durante el ciclo de vida de un `DbContext`.

### Second-Level Cache
- Requiere instalar un paquete de terceros.
- Se configura en el `DbContext` y en los servicios.
- Permite definir qué consultas se deben cachear y por cuánto tiempo.

---

## Beneficios del uso de caché en EF Core

- Reducción de consultas innecesarias a la base de datos.
- Mejora del rendimiento general de la aplicación.
- Disminución de la carga sobre el servidor de base de datos.
- Respuestas más rápidas en interfaces de usuario.

---

## Consideraciones importantes

- La caché de primer nivel **no comparte datos entre `DbContext` diferentes**.
- Hay que tener cuidado con la **sincronización de datos**, ya que una caché desactualizada puede mostrar datos incorrectos.
- No todas las consultas deben ser cacheadas (especialmente las que cambian constantemente).
- El uso de **Second-Level Cache** debe evaluarse según el contexto de la aplicación (por ejemplo, APIs de solo lectura lo aprovechan muy bien).

