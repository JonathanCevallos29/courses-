
## ¿Qué es un `DTO`?

_Data Transfer Object_ / _Objeto de Transferencia de Datos_

Un DTO es un objeto simple que se usa para transportar datos entre procesos, capas o servicios.  
**No** contiene lógica de negocio, solo atributos (campos) y sus getters/setters.

Este objeto puede usarse para enviar datos desde la base de datos al frontend, o entre APIs, pero sin exponer toda la estructura interna de la entidad real.

---

## ¿Qué es un `ViewModel`?

Un ViewModel es un objeto diseñado para la vista (interfaz de usuario).  
Contiene solo los datos que necesita la pantalla, incluso si esos datos provienen de múltiples entidades o DTOs. 

---

## ¿Qué es una `Entity`?

Una Entity (Entidad) es un objeto del dominio que representa una tabla de la base de datos o un concepto clave del negocio en la lógica de tu aplicación.

- Es una clase con identidad única (por ejemplo, un Id).
- Representa una estructura real del modelo de datos, generalmente una tabla si usas ORM (como Entity Framework).
- Se utiliza para persistir datos (crear, leer, actualizar, eliminar – CRUD).
- Tiene relaciones con otras entidades (1-1, 1-M, M-1, M-M).
- Suele contener lógica de negocio simple.

---

## Diferencias 

| Característica | Entity                               | DTO                               | ViewModel                            |
| :---           | :---                                 | :---                              | :---                                 |
| Relación       | Directa con la base de datos         | Para transferir datos entre capas | Para mostrar datos en la interfaz    |
| Propósito      | Persistir y mapear datos del dominio | Transportar datos de forma segura y eficiente | Adaptar datos a la presentación visual |
| Contenido      | Propiedades completas, relaciones    | Solo los campos necesarios         | Solo datos útiles para la vista       |
| Incluye lógica | Puede incluir lógica del dominio     | No (solo getters/setters)          | Puede incluir lógica de presentación |
| Estructura     | Completa, igual a la tabla           | Parcial y simplificada             | Formateada o combinada según la UI   |
| Uso típico     | En la capa de datos y dominio        | En servicios y controladores       | En controladores y vistas/UI         |
| Persistencia   | Sí                                   | No                                 | No                                    |


