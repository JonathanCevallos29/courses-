
# `Patrón Repositorio` en C# y EF Core

## ¿Qué es el patrón Repositorio?

Es un **patrón de diseño** que busca aislar la lógica de acceso a datos del resto de la aplicación. Sirve como una capa intermedia entre el DbContext y los controladores o servicios, encapsulando toda la lógica CRUD y evitando el acoplamiento directo con EF Core.

## Ventajas

- Mayor mantenibilidad del código.
- Facilita la prueba unitaria (puedes mockear el repositorio).
- Fomenta la separación de responsabilidades (SRP).
- Permite cambiar la fuente de datos sin modificar otras capas.

## `Estructura recomendada`

> Ubicar todo en una carpeta llamada Repositories:

```
Repositories/
│
├── IUsuarioRepository.cs     → Interfaz
└── UsuarioRepository.cs      → Implementación
```

## `Interfaz del repositorio` (IUsuarioRepository.cs)

`Ejemplo`:

```
public interface IUsuarioRepository
{
    Usuario GetById(int id);
    IEnumerable<Usuario> GetAll();
    void Add(Usuario usuario);
    void Update(Usuario usuario);
    void Delete(int id);
}
```

**`Implementación` (UsuarioRepository.cs)**

```
public class UsuarioRepository : IUsuarioRepository
{
    private readonly AppDbContext _context;

    public UsuarioRepository(AppDbContext context)
    {
        _context = context;
    }

    public Usuario GetById(int id)
    {
        return _context.Usuarios.Find(id);
    }

    public IEnumerable<Usuario> GetAll()
    {
        return _context.Usuarios.ToList();
    }

    public void Add(Usuario usuario)
    {
        _context.Usuarios.Add(usuario);
        _context.SaveChanges();
    }

    public void Update(Usuario usuario)
    {
        _context.Usuarios.Update(usuario);
        _context.SaveChanges();
    }

    public void Delete(int id)
    {
        var usuario = _context.Usuarios.Find(id);
        if (usuario != null)
        {
            _context.Usuarios.Remove(usuario);
            _context.SaveChanges();
        }
    }
}
```

## Explicaciones puntuales

### ¿Qué significa `readonly`?

Es una palabra clave que indica que el valor del campo solo puede asignarse en el constructor o en la declaración, y no se puede modificar después.

`Ejemplo`:

```
private readonly AppDbContext _context;
```

> Se usa para proteger dependencias como el DbContext, garantizando que no se reemplacen accidentalmente durante la ejecución.

## ¿Qué es `IEnumerable<Usuario>`?

IEnumerable<T> es una interfaz que representa una colección de objetos que se pueden recorrer con un foreach.

`Ventajas:`

- Es más liviana y flexible que List<T>.
- Es ideal para trabajar con consultas diferidas en LINQ.
- Oculta detalles de implementación.

## Métodos comunes de `DbSet<T>`

| Método               | Qué hace                                                                   |
| :---                 | :---                                                                       |
| `.Add()`             | Agrega una entidad al contexto                                             |
| `.AddRange()`        | Agrega varias entidades                                                    |
| `.Find()`            | Busca por clave primaria                                                   |
| `.Remove()`          | Elimina una entidad                                                        |
| `.Update()`          | Marca una entidad como modificada                                          |
| `.ToList()`          | Ejecuta la consulta y trae una lista                                       |
| `.Where(...)`        | Filtro personalizado (LINQ)                                                |
| `.FirstOrDefault()`  | Trae el primer resultado o null                                            |
| `.SingleOrDefault()` | Trae uno solo o null; lanza error si hay más de uno                        |
| `.Any()`             | Verifica si hay al menos un elemento que cumpla una condición              |
| `.Count()`           | Cuenta elementos                                                           |
| `.Include()`         | Carga relaciones (navegación) como `Usuarios.Include(u => u.Departamento)` |

## Repositorio Genérico en EF Core

### ¿Por qué usar un repositorio genérico?

En aplicaciones medianas o grandes, muchas entidades comparten operaciones comunes como `Add`, `Update`, `Delete`, etc. Crear una interfaz y clase para cada entidad puede volverse repetitivo.

> **Solución**: implementar un repositorio genérico reutilizable, que maneje operaciones básicas para cualquier entidad del sistema.

## Paso a paso para implementar

1. Crear la interfaz genérica `IGenericRepository<T>`

```
public interface IGenericRepository<T> where T : class
{
    T GetById(int id);
    IEnumerable<T> GetAll();
    void Add(T entity);
    void Update(T entity);
    void Delete(int id);
}
```

> Se restringe a class para asegurar que T sea una entidad de EF Core.

2. Crear la clase `GenericRepository<T>`

```
public class GenericRepository<T> : IGenericRepository<T> where T : class
{
    private readonly AppDbContext _context;
    private readonly DbSet<T> _dbSet;

    public GenericRepository(AppDbContext context)
    {
        _context = context;
        _dbSet = context.Set<T>(); 
    }

    public T GetById(int id)
    {
        return _dbSet.Find(id);
    }

    public IEnumerable<T> GetAll()
    {
        return _dbSet.ToList();
    }

    public void Add(T entity)
    {
        _dbSet.Add(entity);
        _context.SaveChanges();
    }

    public void Update(T entity)
    {
        _dbSet.Update(entity);
        _context.SaveChanges();
    }

    public void Delete(int id)
    {
        var entity = _dbSet.Find(id);
        if (entity != null)
        {
            _dbSet.Remove(entity);
            _context.SaveChanges();
        }
    }
}
```

## ¿Cómo usarlo?

Suponiendo que hay una entidad `Usuario`

```
public class UsuarioService
{
    private readonly IGenericRepository<Usuario> _usuarioRepo;

    public UsuarioService(IGenericRepository<Usuario> usuarioRepo)
    {
        _usuarioRepo = usuarioRepo;
    }

    public void CrearUsuario(Usuario usuario)
    {
        _usuarioRepo.Add(usuario);
    }

    public IEnumerable<Usuario> ObtenerTodos()
    {
        return _usuarioRepo.GetAll();
    }
}
```

## Buenas prácticas

Para operaciones más específicas (como GetUsuariosActivos(), etc), se pueden crear repositorios personalizados que hereden del genérico y agreguen métodos extra.

```
public interface IUsuarioRepository : IGenericRepository<Usuario>
{
    IEnumerable<Usuario> GetUsuariosActivos();
}
```

> Usar UnitOfWork para coordinar múltiples repositorios en una sola transacción.





