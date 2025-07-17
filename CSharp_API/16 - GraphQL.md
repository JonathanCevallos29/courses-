
## ¿Qué es `GraphQL`?

GraphQL es un lenguaje de consulta para APIs y un entorno de ejecución que permite a los clientes especificar exactamente qué datos necesitan. Fue desarrollado por Facebook como alternativa a REST, con el objetivo de hacer más eficientes y flexibles las solicitudes de datos.

## ¿Cómo funciona GraphQL?

- El **cliente** envía una consulta con la estructura exacta de los datos que necesita.
- El **servidor** analiza la consulta y responde solo con esos datos, en formato JSON.
- **Evita** el problema de **overfetching** (recibir más datos de los necesarios) y **underfetching** (tener que hacer múltiples llamadas para obtener toda la información).

---

## `Consultas` en GraphQL

Las **queries** son el mecanismo para obtener datos. El cliente define qué campos quiere, y el servidor devuelve exactamente eso. GraphQL utiliza un único endpoint generalmente (`/graphql`) para todas sus operaciones, a diferencia de REST que define un endpoint por recurso.

---

## `Comparación` de `Query` GraphQL vs `Endpoint` REST

| Característica             | REST                         | GraphQL                          |
| :---                       | :---                         | :---                             |
| Endpoints                  | Múltiples (por recurso)      | Uno solo                         |
| Datos devueltos            | Fijos por endpoint           | Personalizados por consulta      |
| Overfetching/Underfetching | Sí                           | No                               |
| Relación de recursos       | Requiere múltiples llamadas  | Se manejan dentro de la consulta |
| Versionado                 | Versión por URL o cabecera   | No requiere versionado           |

---

## `Mutaciones` en GraphQL

Las **mutations** en GraphQL son el equivalente a operaciones como POST, PUT, DELETE en REST. Permiten modificar datos del servidor. Al igual que las queries, pueden retornar solo los campos deseados tras la operación.

---

## Múltiples queries o mutaciones

GraphQL permite enviar múltiples queries o múltiples mutaciones en una sola solicitud, lo que reduce el número de llamadas HTTP y mejora el rendimiento en redes lentas.

---

## `Relaciones` entre objetos

GraphQL permite consultar relaciones entre objetos directamente dentro de la misma consulta. Por ejemplo, al pedir información de un producto, también se puede incluir la categoría, el proveedor, etc., sin realizar múltiples peticiones.

---

## `Autenticación` en GraphQL

GraphQL no define una estrategia de autenticación por sí mismo, pero es compatible con cualquier esquema: `JWT`, `OAuth`, `API Keys`, `cookies`, etc. La autenticación y autorización se manejan a nivel del servidor, por resolver o por directiva.

---

## Librería `Hot Chocolate`

**Hot Chocolate** es una de las principales librerías para implementar servidores GraphQL en .NET.

- Permite definir `esquemas`, `tipos`, `queries` y `mutaciones`.
- Soporta integración con `Entity Framework`.
- Tiene soporte para `autorización`, `filtros`, `paginación`, `suscripciones` (realtime).
- Está activa y bien mantenida.

---

## ¿Qué es `GraphQL Federation`?

GraphQL Federation es una arquitectura que permite dividir un esquema GraphQL en varios sub-esquemas, gestionados por distintos microservicios. Luego, un **gateway federado** combina esos esquemas en uno solo. Fue desarrollado por Apollo y es ideal para sistemas distribuidos.

---

## ¿Se debe implementar GraphQL?

**Depende del caso de uso**

### Cuándo **USAR** GraphQL:
- Cuando el cliente necesita control sobre qué datos recibir.
- Aplicaciones con múltiples frontends (móvil, web, etc.).
- Relaciones complejas entre entidades.
- Necesidad de optimizar llamadas a red.
- Backend for frontend (BFF) pattern.

### Cuándo **NO** es necesario:
- APIs simples o CRUD sin relaciones complejas.
- Cuando el rendimiento no es crítico y REST es suficiente.
- Equipos sin experiencia en GraphQL (puede tener curva de aprendizaje).
- Casos donde el versionado tradicional de REST es más práctico.

