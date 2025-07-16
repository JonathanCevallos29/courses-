
## ¿Qué es `IQueryable`?

IQueryable<T> es una interfaz genérica de .NET que representa una colección de datos consultables que pueden construir consultas expresadas con LINQ y diferir su ejecución hasta que realmente se necesiten (ej. al usar .ToList() o .FirstOrDefault()).

> IQueryable construye una consulta que se traduce a SQL y se ejecuta en la base de datos solo cuando se requiere el resultado final.

## ¿Cuándo se usa?

- Cuando se trabaja con `Entity Framework Core` o `LINQ` to `SQL`.
- Cuando se necesita construir consultas complejas y diferidas (se arma la consulta en pasos).
- Cuando se desea que la consulta se ejecute en la base de datos y no en memoria (mejor rendimiento).
- Cuando se va a aplicar filtros condicionales o construidos dinámicamente.

## Diferencias con `IEnumerable`

| Característica	     |       `IQueryable`	              |      `IEnumerable`                    |
| :---          	     |  :---          	              |  :---                               |
| Dónde se ejecuta	   |  En la base de datos (server)	| En memoria (cliente/app)            |
| Cuándo se ejecuta	   |  Ejecución diferida            | Ejecución inmediata (a veces)       |
| Soporta LINQ	       |  Sí, traducido a SQL	          | Sí, pero después de traer los datos |
| Ideal para...	       |  Consultas a BD grandes	      | Trabajar con datos ya cargados      |

## ¿Qué permite `IQueryable`?

- Construir consultas dinámicas usando expresiones LINQ como `.Where()`, `.OrderBy()`, `.Include()`, etc.
- Aprovechar la traducción automática a SQL en Entity Framework.
- Realizar `"query composition"`, o sea: armar la consulta por partes antes de ejecutarla.
- Evitar cargar datos innecesarios en memoria, mejorando el rendimiento.
- Aplicar `paginación`, `filtros condicionales`, `proyecciones`, etc.

