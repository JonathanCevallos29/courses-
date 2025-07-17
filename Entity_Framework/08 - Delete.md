
## ¿Qué es `Hard Delete`?

El **Hard Delete** es la eliminación física y permanente de un registro en la base de datos.  
Una vez que se elimina, **no puede recuperarse**, ya que se borra directamente con un `DELETE`.

### Ventajas:
- Libera espacio en la base de datos.
- Es simple de implementar.

### Desventajas:
- Se pierde historial o trazabilidad.
- Puede romper integridad referencial si no se maneja bien.

> En entornos empresariales NUNCA se borra la `información`

---

## ¿Qué es `Soft Delete`?

El **Soft Delete** es una técnica que **marca un registro como eliminado**, sin borrarlo de la base de datos.  
Generalmente, esto se hace usando un campo como `IsDeleted` (booleano o estado).

El registro permanece en la base de datos, pero **es ocultado en las consultas** normales.

### Ventajas:
- Permite restaurar registros eliminados.
- Conserva el historial de cambios o auditoría.
- No afecta relaciones referenciales.

### Desventajas:
- Aumenta la complejidad de las consultas si no se automatiza el filtrado.
- La base de datos puede crecer más rápidamente.

---

## `.HasQueryFilter` y su relación con Soft Delete

Entity Framework Core permite simplificar el uso de **Soft Delete** usando el método `.HasQueryFilter`.

Este método define un **filtro global** para una entidad, que se aplica en todas las consultas LINQ.  
Es ideal para que los registros "eliminados" (soft deleted) **no aparezcan automáticamente**, sin necesidad de escribir filtros repetitivos en cada consulta.

### ¿Qué permite hacer `.HasQueryFilter`?

- Excluir automáticamente entidades marcadas como eliminadas (`IsDeleted = true`).
- Mantener consultas limpias y consistentes.
- Aplicar reglas globales como multi-tenancy o visibilidad por usuario.
- Centralizar la lógica de visibilidad de datos.

---

## Comparación general

| Característica         | Hard Delete                     | Soft Delete + HasQueryFilter                  |
|------------------------|----------------------------------|-----------------------------------------------|
| Elimina físicamente    | Sí                            | No                                          |
| Reversible             | No                            | Sí                                          |
| Visibilidad controlada | Manual                        | Automática (gracias a `HasQueryFilter`)     |
| Uso de espacio         | Libera espacio                |  Ocupa más                                   |
| Seguridad de datos     | Riesgo de pérdida             | Conserva historial                          |
| Complejidad            | Simple                        | Moderada si no se automatiza con filtros    |

---

## Recomendación

Usar **Hard Delete** cuando no se necesite el historial ni recuperación.  
Usar **Soft Delete + HasQueryFilter** en sistemas donde se requiera auditoría, recuperación de datos, o mantener registros "ocultos" para el usuario.
