
## ¿Qué es un ORM?
Un ORM (Object-Relational Mapper) permite trabajar con bases de datos usando clases y objetos en lugar de escribir SQL directo.

`Ejemplo`:  
Se puede hacer db.Empleados.ToList() en lugar de SELECT * FROM Empleados.

---

## ¿Qué es `LINQ`?
**LINQ (Language Integrated Query)** es una forma de hacer consultas en C# sobre listas, arreglos, bases de datos, etc., usando una sintaxis parecida a SQL, pero desde el código.

`Ejemplo`:  
var activos = empleados.Where(e => e.Activo);

---

## ¿Qué es `Entity Framework Core`?  
Es un ORM de Microsoft para .NET que permite acceder y trabajar con bases de datos usando clases, sin escribir SQL manualmente. Es ligero, moderno y multiplataforma.

## Diferencias entre Entity Framework y Entity Framework Core  

| Entity Framework	                |  Entity Framework Core            |
| :---                              | :---                              |
| Solo funciona en Windows	        |  Funciona en Windows, Linux y Mac |
| Más antiguo                       |  Más moderno y rápido             |
| No recibe actualizaciones nuevas	|  Sí, activamente mantenido        |
| Tiene más herramientas gráficas	  |  Más código, menos interfaz       |

## Code First vs Database First vs Model First
`Code First`:  
Primero haces las clases en C#, y EF crea la base de datos.
Ideal para proyectos nuevos.

`Database First`:  
Partes de una base ya existente, y EF genera las clases.
Útil si ya hay una base de datos hecha.

`Model First`:  
Diseñar un modelo visual, y de ahí se crea todo.
Poco usado hoy, no muy recomendado.

---

## ¿Qué es DbContext y DbSet?  

**DbContext**: es la clase principal que representa la conexión a la base de datos y maneja las operaciones.

**DbSet<T>**: 
Representa una tabla de la base de datos en forma de colección.

`Ejemplo`:  
public DbSet<Empleado> Empleados { get; set; }

---



