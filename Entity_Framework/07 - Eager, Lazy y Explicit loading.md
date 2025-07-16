
## ¿Qué es el `loading` en EF Core?

Cuando se trabaja con relaciones entre entidades (por ejemplo, un Empleado que pertenece a un Departamento), se necesita decidir cuándo y cómo se cargarán esas entidades relacionadas.

## Tres formas EFC

| Tipo de carga | ¿Cuándo se carga la entidad relacionada?         |
| :---          | :---                                             |
| **Eager**     | Al momento de la consulta principal              |
| **Lazy**      | Solo cuando se accede a la propiedad relacionada |
| **Explicit**  | Manualmente, mediante código                     |

 1. Eager Loading

 `Carga anticipada:` EF incluye las entidades relacionadas desde el principio, usando `Include()`.

### Cuándo usarlo:

- Cuando se necesite sí o sí los datos relacionados desde el inicio.
- Para evitar múltiples consultas (n+1 problem).
- Cuando se sabe que los datos serán usados inmediatamente.

`Ejemplo: `

```
var empleados = _context.Empleados
    .Include(e => e.Departamento)
    .ToList();
```

> Carga todos los empleados y sus departamentos en una sola consulta SQL.

2. Lazy Loading

`Carga diferida:` EF no carga los datos relacionados hasta que se accede a ellos por primera vez.

### Cuándo usarlo:

- Cuando no siempre se necesiten los datos relacionados.
- Para ahorrar recursos en consultas simples.
- Cuando no se sabe qué partes del objeto se usaran.

`Requisitos en EF Core:`

- **Instalar paquete:**

```
Microsoft.EntityFrameworkCore.Proxies
```

- **Habilitar en DbContext:**

```
options.UseLazyLoadingProxies();
```

- **Las propiedades de navegación deben ser virtual:**

```
public virtual Departamento Departamento { get; set; }
```

`Ejemplo:`

```
var empleado = _context.Empleados.First();
var nombreDepartamento = empleado.Departamento.Nombre; 
```

> EF hace una consulta cuando se accede a Departamento, no antes.

3. Explicit Loading

`Carga controlada manualmente:` se puede decidir cuándo cargar la relación, usando Entry().Reference() o Entry().Collection().

### Cuándo usarlo:

- Cuando se necesita más control sobre las consultas.
- Cuando no se quiere cargar automáticamente ni con Include.
- Para optimizar casos específicos.

`Ejemplo:`

```
var empleado = _context.Empleados.First();

_context.Entry(empleado)
    .Reference(e => e.Departamento)
    .Load();
```

> Aquí, primero se carga el empleado, luego manualmente se carga su departamento.

## NO CONFUNDIR

`Lazy Loading (EF Core)`
Carga automática de propiedades de navegación solo cuando se acceden por primera vez. Requiere que la propiedad sea virtual y esté habilitado el proxy.

`Lazy<T> (.NET)`
Permite posponer la creación de un objeto hasta que se necesite. Se usa para optimizar recursos, no tiene relación con bases de datos.

## Importante

- **Eager loading** (Include) es la opción más común y segura.
- **Lazy loading** puede mejorar el rendimiento, pero mal usado puede provocar muchas consultas pequeñas.
- **Explicit loading** da control, pero puede requerir más líneas de código.

