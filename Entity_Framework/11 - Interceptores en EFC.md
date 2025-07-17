
## ¿Qué son los interceptores?

Los interceptores en Entity Framework Core son **componentes que permiten capturar y modificar el comportamiento de las operaciones que EF realiza sobre la base de datos**, tales como comandos SQL, conexiones, transacciones y eventos del contexto.

Funcionan como "hooks" o puntos de extensión donde se puede ejecutar código adicional antes o después de que EF Core realice ciertas acciones.

---

## ¿Para qué se usan?

- **Auditoría y logging:** Registrar consultas, comandos SQL ejecutados o cambios en las entidades.
- **Modificación o inspección de comandos:** Cambiar comandos SQL antes de enviarlos a la base de datos.
- **Gestión de conexiones y transacciones:** Controlar la apertura, cierre o uso de conexiones y transacciones.
- **Implementar políticas transversales:** Como manejo de seguridad, filtros globales o validaciones adicionales.
- **Diagnóstico y monitoreo:** Medir tiempos de ejecución, detectar cuellos de botella o analizar comportamiento.

---

## Tipos principales de interceptores en EF Core

EF Core incluye varios tipos de interceptores para diferentes operaciones, algunos de los más comunes son:

| Tipo de interceptor              | Qué intercepta                             | Uso típico                                         |
|---------------------------------|-------------------------------------------|---------------------------------------------------|
| **IDbCommandInterceptor**        | Comandos SQL enviados a la base de datos | Modificar o loggear sentencias SQL ejecutadas     |
| **IDbConnectionInterceptor**     | Eventos de conexión a la base de datos    | Controlar apertura o cierre de conexiones         |
| **IDbTransactionInterceptor**    | Eventos de transacciones                   | Manejar inicio, commit o rollback de transacciones|
| **ISaveChangesInterceptor**      | Operaciones de guardado (`SaveChanges`)   | Auditar cambios antes o después de guardarlos     |
| **IQueryInterceptor** (menos común) | Consultas LINQ o SQL                      | Modificar o inspeccionar consultas                  |

---

## Cómo funcionan los interceptores

- Se implementan como clases que heredan o implementan alguna de las interfaces anteriores.
- Se registran en el `DbContext` o en el `DbContextOptions`.
- EF Core llama automáticamente a sus métodos durante el ciclo de vida de las operaciones.

---

## Diferencias entre Triggers e Interceptores

| Característica          | Triggers (Base de datos)                      | Interceptores (Entity Framework Core)             |
|------------------------|-----------------------------------------------|---------------------------------------------------|
| Nivel de ejecución     | Se ejecutan directamente en la base de datos | Se ejecutan en el código de la aplicación (C#)   |
| Alcance                | Afectan operaciones específicas (insert, update, delete) a nivel tabla | Capturan eventos en el ciclo de vida de EF Core (consultas, comandos, conexiones, guardados) |
| Lenguaje               | SQL o lenguaje específico del motor de BD    | Código C# usando interfaces de EF Core            |
| Control                | Definidos y controlados por el DBA o scripts en la BD | Controlados por el desarrollador dentro de la app |
| Propósito común        | Mantener integridad, reglas de negocio en BD, auditoría a nivel BD | Auditoría, logging, modificación o cancelación de operaciones antes o después en la app |
| Impacto en rendimiento | Directo en la base de datos, pueden afectar transacciones | Puede afectar rendimiento si interceptores hacen operaciones pesadas |
| Visibilidad            | Transparente para la aplicación                | Visible y configurable desde la aplicación         |

> **No** son lo mismo

---

## Ejemplos de uso común

- **Triggers:** Controlar integridad referencial, actualizar campos automáticos, auditoría en BD.
- **Interceptores:** Registrar logs, medir tiempos, validar datos, modificar comandos SQL antes de ejecutarlos.
