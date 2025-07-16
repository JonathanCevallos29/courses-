
# Data Seeding en Entity Framework Core (EF Core)

## ¿Qué es el `Data Seeding`?  

**Data Seeding** es el proceso de insertar datos iniciales en la base de datos automáticamente durante una migración. Es útil para poblar la base de datos con datos predefinidos o estáticos.

## ¿Para qué se usa?

Se utiliza comúnmente para cargar datos necesarios para el funcionamiento inicial del sistema, como:  
- Roles (ej. Admin, Usuario)
- Categorías
- Países / ciudades
- Estados (ej. Activo, Inactivo)
- Valores similares a enums (pero almacenados en tablas)
- Datos de prueba o por defecto

## ¿Cómo se implementa?

**`Opción básica` (directa en OnModelCreating)**

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Rol>().HasData(
        new Rol { Id = 1, Nombre = "Admin" },
        new Rol { Id = 2, Nombre = "Usuario" }
    );
}
```  

> Esta forma puede volverse poco mantenible si hay muchos datos o múltiples entidades.

**`Opción recomendada` (estructura limpia y escalable)**

1. Crear carpeta `Seeds`  

Crea una carpeta llamada Seeds para agrupar todas las clases de configuración de datos iniciales.

2. Crear clase para cada entidad, implementando `IEntityTypeConfiguration<T>`

```
public class UserSeed : IEntityTypeConfiguration<User>
{
    public void Configure(EntityTypeBuilder<User> builder)
    {
        builder.HasData(
            new User { Id = 1, Name = "Admin", Email = "admin@ejemplo.com" },
            new User { Id = 2, Name = "User", Email = "user@ejemplo.com" }
        );
    }
}
```

3. Aplicar configuración en el `DbContext`

**Forma 1**: Automática (mejor para grandes proyectos)

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfigurationsFromAssembly(typeof(AppDbContext).Assembly);
}
```

**Forma 2**: Manual (más controlado y explícito)

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new UserSeed());
    modelBuilder.ApplyConfiguration(new RoleSeed());
    // Otros seeds...
}
```

## Aplicar los datos `seed`

Para que los datos se inserten en la base de datos, **se debe ejecutar una migración**, ya que EF Core genera instrucciones SQL dentro de la migración para insertar esos datos:

```
dotnet ef migrations add SeedInitialData
dotnet ef database update
```

> EF Core no vuelve a aplicar los datos si ya existen con la misma clave primaria (ID). Solo los inserta una vez, a menos que se cambie el ID u otra propiedad marcada como clave.

##  Buenas prácticas

- Usar IDs fijos para los datos sembrados (evitar IDs generados automáticamente).
- Agrupar los datos en clases separadas por entidad.
- No hacer seeding de datos sensibles o temporales.
- Asegúrar de que el `HasData()` coincida con el modelo (si se cambia una propiedad, se necesita nueva migración).
- Verificar los cambios en la migración generada antes de aplicarlos.

