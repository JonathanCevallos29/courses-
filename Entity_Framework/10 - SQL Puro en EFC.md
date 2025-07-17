
# Uso de SQL Puro en Entity Framework Core

## ¿Qué es SQL puro (Raw SQL) en EF Core?

Es la capacidad de ejecutar comandos o consultas SQL directamente en la base de datos utilizando Entity Framework Core, sin depender del sistema de traducción de LINQ a SQL.

---

## ¿Por qué y cuándo utilizarlo?

El uso de SQL puro se recomienda en los siguientes escenarios:

- Consultas complejas que no se pueden representar fácilmente con LINQ.
- Optimizaciones de rendimiento, cuando la consulta generada por LINQ es ineficiente.
- Acceso a vistas, funciones o procedimientos almacenados.
- Consultas personalizadas que devuelven columnas calculadas o no mapeadas directamente a entidades.
- Reportes y dashboards que necesitan operaciones SQL agregadas específicas.
- Migración de proyectos legados donde ya existe lógica SQL establecida.
- Consultas que requieren funcionalidades específicas del proveedor de base de datos (ej. SQL Server, PostgreSQL).

---

## Métodos comunes para usar SQL puro

EF Core ofrece varios métodos para ejecutar SQL directamente:

### 1. `FromSqlRaw()`

Permite ejecutar consultas SQL en bruto (`raw`), aceptando una cadena de texto que representa el comando SQL. Los parámetros deben pasarse explícitamente para evitar inyección SQL. Ideal para consultas sin interpolación.

### 2. `FromSqlInterpolated()`

Similar a `FromSqlRaw()`, pero permite interpolación de cadenas directamente en el SQL. Es más seguro porque usa parámetros automáticamente, evitando la inyección SQL. Se recomienda siempre que se necesiten insertar valores dinámicos.

#### Diferencia clave:

| Método                  | Interpolación segura | Uso recomendado            |
|-------------------------|----------------------|-----------------------------|
| `FromSqlRaw()`          | No                   | SQL fijo, parámetros aparte |
| `FromSqlInterpolated()` | Sí                   | SQL con variables dinámicas |

### 3. `ExecuteSqlRaw()`

Se usa para ejecutar comandos SQL que **no devuelven datos**, como `INSERT`, `UPDATE`, `DELETE`. No permite interpolación segura.

### 4. `ExecuteSqlInterpolated()`

Funciona como `ExecuteSqlRaw()` pero con interpolación segura. Es preferido cuando se necesitan incluir parámetros dentro del SQL.

---

## Consideraciones importantes

- Las consultas con `FromSqlRaw()` o `FromSqlInterpolated()` deben devolver columnas compatibles con las propiedades de las entidades mapeadas.
- No abusar del SQL puro: úsalo solo cuando LINQ no sea suficiente o no brinde el rendimiento esperado.
- Las entidades involucradas deben estar registradas en el `DbContext`, salvo que se use proyección a clases personalizadas.
- Usar interpolación segura evita la inyección SQL (por eso se prefiere `FromSqlInterpolated()`).

---

## Buenas prácticas

- Usar SQL puro en una capa de infraestructura o repositorio, no directamente en controladores.
- No escribir cadenas SQL extensas directamente en el código; extraerlas a constantes o recursos externos si son complejas.
- Validar que el resultado de las consultas coincida con la estructura de las entidades esperadas.
