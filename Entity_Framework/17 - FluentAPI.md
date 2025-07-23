## ¿Qué es Fluent API?

Es una forma **programática** (en `OnModelCreating`) de configurar tu modelo de datos en lugar de usar solo atributos (`[Key]`, `[Required]`, etc.).

> Es ideal para reglas complejas o cuando no querés contaminar tus clases con anotaciones.

## Lo más usado en Fluent API

| Uso / Propósito                           | Método Fluent API                        | Ejemplo                                                                                                          |
| ----------------------------------------- | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Nombre de tabla personalizada**         | `.ToTable("Nombre")`                     | `modelBuilder.Entity<User>().ToTable("Usuarios");`                                                               |
| **Nombre de columna**                     | `.Property().HasColumnName("Nombre")`    | `modelBuilder.Entity<User>().Property(u => u.Name).HasColumnName("NombreCompleto");`                             |
| **Clave primaria**                        | `.HasKey()`                              | `modelBuilder.Entity<User>().HasKey(u => u.Id);`                                                                 |
| **Relación 1 a muchos**                   | `.HasOne().WithMany().HasForeignKey()`   | `modelBuilder.Entity<User>().HasOne(u => u.Role).WithMany(r => r.Users).HasForeignKey(u => u.RolId);`            |
| **Relación 1 a 1**                        | `.HasOne().WithOne().HasForeignKey<T>()` | `modelBuilder.Entity<User>().HasOne(u => u.Profile).WithOne(p => p.User).HasForeignKey<Profile>(p => p.UserId);` |
| **Relación muchos a muchos (EF Core 5+)** | `.HasMany().WithMany()`                  | `modelBuilder.Entity<Student>().HasMany(s => s.Courses).WithMany(c => c.Students);`                              |
| **Eliminar comportamiento (Restrict)**    | `.OnDelete(DeleteBehavior.Restrict)`     | `modelBuilder.Entity<Assignment>().HasOne(a => a.CreatedBy).WithMany().OnDelete(DeleteBehavior.Restrict);`       |
| **Campo requerido**                       | `.IsRequired()`                          | `modelBuilder.Entity<User>().Property(u => u.Email).IsRequired();`                                               |
| **Longitud máxima**                       | `.HasMaxLength(n)`                       | `modelBuilder.Entity<User>().Property(u => u.Name).HasMaxLength(100);`                                           |
| **Valor por defecto**                     | `.HasDefaultValue(valor)`                | `modelBuilder.Entity<User>().Property(u => u.IsActive).HasDefaultValue(true);`                                   |
| **Conversión enum a string**              | `.HasConversion<string>()`               | `modelBuilder.Entity<User>().Property(u => u.Status).HasConversion<string>();`                                   |
| **Conversión personalizada**              | `.HasConversion(v => ..., v => ...)`     | `modelBuilder.Entity<Order>().Property(o => o.Total).HasConversion(v => v.Value, v => new Money(v));`            |
| **Índice simple**                         | `.HasIndex(p => p.Prop)`                 | `modelBuilder.Entity<User>().HasIndex(u => u.Email);`                                                            |
| **Índice único**                          | `.HasIndex().IsUnique()`                 | `modelBuilder.Entity<User>().HasIndex(u => u.Email).IsUnique();`                                                 |
| **Índice compuesto**                      | `.HasIndex(p => new { p.A, p.B })`       | `modelBuilder.Entity<Log>().HasIndex(l => new { l.UserId, l.Date });`                                            |
| **Filtro global (QueryFilter)**           | `.HasQueryFilter(expr)`                  | `modelBuilder.Entity<User>().HasQueryFilter(u => !u.IsDeleted);`                                                 |
| **Ignorar una propiedad**                 | `.Ignore()`                              | `modelBuilder.Entity<User>().Ignore(u => u.TemporalData);`                                                       |

## Otro tipo de Organización en proyectos

Usar `IEntityTypeConfiguration<T>` para cada entidad:

> Permite mantener limpio el DbContext y separar configuraciones.

**Ejemplo**:

```
// Configuración separada para User
public class UserConfiguration : IEntityTypeConfiguration<User>
{
    public void Configure(EntityTypeBuilder<User> builder)
    {
        builder.ToTable("Usuarios");
        builder.HasKey(u => u.Id);
        builder.Property(u => u.Name).HasMaxLength(100).IsRequired();
        builder.HasIndex(u => u.Email).IsUnique();
    }
}
```

```
// Asi queda en el DbContext
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new UserConfiguration());
    modelBuilder.ApplyConfiguration(new RoleConfiguration());
    modelBuilder.ApplyConfiguration(new AssignmentConfiguration());
}
```

## Uso de `ApplyConfigurationsFromAssembly`

Buscá en este ensamblado (proyecto) todas las clases que implementen IEntityTypeConfiguration<T> y aplicalas automáticamente al modelo.

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.ApplyConfigurationsFromAssembly(typeof(ApplicationDbContext).Assembly);
}
```
