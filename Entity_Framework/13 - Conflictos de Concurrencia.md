
## ¿Qué es la concurrencia?
Es la situación que ocurre cuando **dos o más procesos o usuarios intentan modificar los mismos datos al mismo tiempo** en una base de datos. Esto puede causar **conflictos**, pérdida de datos o comportamiento inesperado si no se maneja correctamente.

## ¿Por qué es importante manejarla?
Porque en aplicaciones multiusuario, como sistemas web o de escritorio conectados a una base de datos central, es muy común que distintos usuarios estén interactuando con la misma información al mismo tiempo.

## Tipos de concurrencia
- **Concurrencia optimista (Optimistic Concurrency):**
  - Supone que los conflictos no son frecuentes.
  - EF Core detecta si los datos cambiaron entre que fueron leídos y el momento en que se intenta guardarlos.
  - Ideal para aplicaciones web donde los usuarios no bloquean datos por mucho tiempo.
  - Se maneja con una columna especial como `RowVersion` o `Timestamp`.

- **Concurrencia pesimista (Pessimistic Concurrency):**
  - Se basa en bloquear activamente los datos mientras un usuario los modifica.
  - Evita que otros usuarios puedan leer o modificar los datos hasta que se liberen.
  - Se usa menos en EF Core, ya que depende del proveedor de base de datos (como SQL Server) y se implementa manualmente.

## ¿Cómo actúa EF Core con la concurrencia?
Cuando se usa concurrencia optimista y ocurre un conflicto:
- EF Core lanza una **excepción de concurrencia**.
- El desarrollador debe decidir si sobrescribir, cancelar o volver a intentar el cambio.

---

## Ejemplo de conflicto de concurrencia:

1. Dos empleados, Dulce y Cesar, abren el mismo formulario para editar el salario de un trabajador en una app web.
2. Ana cambia el salario de 1000 a 1200 y guarda.
3. Carlos (que aún ve el salario viejo de 1000), lo cambia a 1100 y también intenta guardar.
4. EF Core detecta que los datos fueron modificados entre la lectura de Carlos y su intento de guardado.
5. Se lanza una excepción de concurrencia que Carlos debe manejar (por ejemplo, recargar los datos o confirmar si desea sobrescribir).

---

## Estrategias comunes ante conflictos:
- **Reintentar**: Recargar los datos y volver a aplicar los cambios.
- **Cancelar**: Avisar al usuario que los datos han cambiado.
- **Sobrescribir**: Guardar de todos modos, ignorando los cambios ajenos (no recomendado sin validación).

