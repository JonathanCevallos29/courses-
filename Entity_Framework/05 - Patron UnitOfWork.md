
## ¿Qué es el `Patrón Unit of Work`?

**Unit of Work** (UoW) es un patrón de diseño que coordina el trabajo de múltiples repositorios bajo una única transacción. Su objetivo es garantizar que todos los cambios realizados en una operación se guarden o rechacen de forma atómica (todo o nada).

> Si algo falla en medio de varias operaciones con repositorios, el UnitOfWork puede cancelar todo, evitando que se guarde un estado inconsistente.

## ¿Para que se utiliza?

- Para agrupar múltiples operaciones de base de datos en una sola transacción.
- Para centralizar la confirmación (SaveChanges) de los cambios.
- Para trabajar con múltiples repositorios relacionados y mantener la coherencia de los datos.
- Para mejorar la organización del código en capas.

## ¿Por qué va de la mano con el `Patrón Repositorio`?

Porque el patrón repositorio abstrae las operaciones **CRUD** por entidad, pero no gestiona **transacciones**. 

- Usar repositorios para acceder a los datos (`Add`, `Get`, `Delete`, etc.).
- Usar UnitOfWork para decir: "ahora guarda todos los cambios juntos".

### Hace que sea:

- Más modular,
- Más fácil de probar,
- Y más robusta frente a errores.

## Implementación del patrón Unit of Work

1. Crear interfaz IUnitOfWork

```
public interface IUnitOfWork : IDisposable
{
    IUsuarioRepository Usuarios { get; }
    IRolRepository Roles { get; }
    int Complete(); // SaveChanges()
}
```

> **IDisposable**	Permite liberar recursos como conexiones a BD (con Dispose())

2. Crear clase de implementacion UnitOfWork

```
public class UnitOfWork : IUnitOfWork
{
    private readonly DbContext _context;

    public IUserRepository Users { get; }
    public IAssignmentRepository Assignments { get; }

    public UnitOfWork(DbContext context, IUserRepository userRepository, 
        IAssignmentRepository assignmentRepository)
    {
        _context = context;
        Users = userRepository;
        Assignments = assignmentRepository;
    }

    public async Task<int> CompleteAsync()
    {
        return await _context.SaveChangesAsync();
    }

    public void Dispose()
    {
        _context.Dispose();
    }
}
```

Ir a registrar en el program.cs todos:

```
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddScoped<IAssignmentRepository, AssignmentRepository>();

builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();
```

> Este patrón también se puede combinar con el repositorio genérico.

3. Registrar en el contenedor de inyección de dependencias (por ejemplo, en Program.cs)

```
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();
```

4. Usarlo en un servicio o controlador

```
public class UsuarioService
{
    private readonly IUnitOfWork _unitOfWork;

    public UsuarioService(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    public void CrearUsuario(Usuario usuario)
    {
        _unitOfWork.Usuarios.Add(usuario);
        _unitOfWork.Complete(); 
    }

    public void CrearUsuarioYRol(Usuario usuario, Rol rol)
    {
        _unitOfWork.Usuarios.Add(usuario);
        _unitOfWork.Roles.Add(rol);
        _unitOfWork.Complete();
    }
}
```





