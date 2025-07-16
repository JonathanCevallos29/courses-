
# `Migraciones`

## ¿Qué son las migraciones en EF Core?

Las **migraciones** permiten mantener sincronizados el modelo de clases (modelo de dominio) y la base de datos. Cada vez que se modifica una entidad o el **DbContext**, EF Core puede generar un archivo de migración que contiene instrucciones para aplicar esos cambios a la base de datos.

## ¿Cómo funcionan?

- EF Core compara el modelo actual (DbContext y entidades) con el estado anterior registrado en la última migración.
- Genera una clase que representa los cambios necesarios en la base de datos (crear tabla, agregar columna, eliminar campo, etc.).
- Luego se puede aplicar esa migración a la base de datos para actualizar su estructura.

## `Comandos` importantes

**`Crear` una migración**
```
dotnet ef migrations add NombreMigracion --project ./Ruta/Proyecto
```

**`Aplicar` las migraciones** (crear o actualizar la base de datos)
```
dotnet ef database update
```

**`Aplicar` una migración específica**
```
dotnet ef database update NombreMigracion
```

**`Revertir` la última migración**
```
dotnet ef migrations remove
```

**`Borrar` o `Rehacer` base de datos**
```
dotnet ef database drop
dotnet ef database update
```

## Buenas prácticas

- Mantén tus migraciones bajo control de versiones.
- Revisa los archivos de migración antes de aplicar (asegúrate de que no elimina nada accidentalmente).
- Usa nombres descriptivos (AddEmpleadoTable, UpdateFechaNacimientoColumn).
- Si trabajas en equipo, asegúrate de hacer update antes de añadir nuevas migraciones para evitar conflictos.



