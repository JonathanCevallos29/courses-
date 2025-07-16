
## ¿Qué son las `Minimal APIs`?

Las **Minimal APIs** son una forma más ligera y concisa de crear servicios HTTP en ASP.NET Core, introducidas oficialmente en .NET 6.

Permiten construir `endpoints` directamente desde el archivo `Program.cs` sin necesidad de controladores (Controllers), clases extras ni anotaciones ([HttpGet], [Route], etc.).

## `Ventajas` de las Minimal APIs

| Ventaja                                | Detalle |
|----------------------------------------|---------|
| Menos código y más simple              | Todo puede estar en `Program.cs`. No se necesita controladores, clases ni anotaciones. |
| Arranque rápido                        | Ideal para microservicios, pruebas o servicios pequeños. |
| Rendimiento más rápido                 | Al eliminar el middleware de MVC (ruteo complejo, filtros, model binding), es ligeramente más rápido. |
| Fácil de aprender y usar               | Perfecto para nuevos desarrolladores que no necesitan toda la infraestructura de MVC. |
| Ideal para APIs pequeñas o prototipos  | Si la API tiene pocos endpoints, es mucho más rápido implementarla. |
| Integración con Swagger, Auth, etc.    | A pesar de ser minimalista, se puede agregar Swagger, validaciones, autenticación, filtros, etc. |

---

## `Desventajas` de las Minimal APIs

| Desventaja                             | Detalle |
|----------------------------------------|---------|
| Menos estructura en proyectos grandes  | Al tener toda la lógica en un archivo o muy dispersa, puede volverse difícil de mantener. |
| Poca separación de responsabilidades   | No hay capas como Controller → Service → Repository por defecto. Todo puede mezclarse si no se organiza bien. |
| No ideal para proyectos complejos      | Si se usa muchas rutas, middlewares, filtros personalizados, et

## ¿Cuándo usar Minimal APIs?

**Usarlas:**

- El proyecto es pequeño o mediano.
- Para construir microservicios o prototipos rápidos.
- Hacer algo rápido y sin muchas dependencias.
- Para trabajar solo o en un equipo pequeño.

**Evítarlas:**

- El proyecto es grande, con muchos módulos, roles y validaciones.
- Se necesita muchas capas (Service, Repo, etc.) y un código mantenible a largo plazo.
- Trabajar en un equipo grande que necesita orden y separación clara.
