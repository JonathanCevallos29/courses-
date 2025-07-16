
## ¿Qué es un `Middleware`?

Un **middleware** es un componente de software que forma parte del **pipeline** de procesamiento de solicitudes en ASP.NET Core

Cada middleware:
- **Recibe** la solicitud HTTP
- **Realiza** alguna acción (por ejemplo, autenticar, registrar, redirigir, etc.)
- **Llama** al siguiente middleware, o devuelve una respuesta

> Se ejecutan en orden **secuencial**, como una cadena.

`Ejemplos` comunes de middleware:  

- Autenticación
- Registro de logs
- Redirección HTTP a HTTPS.
- Manejo de excepciones
- CORS
- Ruteo
- Compresión de respuestas

## `Características` de los Middlewares en .NET

- Se ejecutan en orden de registro.
- Se puede crear middleware personalizados con lógica propia.
- Son globales, afectan a toda la aplicación.
- No dependen de controladores o rutas específicas.

## `Flujo` de los Middlewares
![flujoMiddleware](/CSharp_API/img/flujoMiddleware.png)

## ¿Qué es un `filtro` en ASP.NET?

Un **filtro** es un componente que se aplica dentro del ciclo de ejecución del controlador o acción, para realizar acciones antes o después de ejecutarlas.

## `Características` de los Filtros en ASP.NET

- Proveen mayor control por acción o controlador.
- Se integran directamente con el ciclo de ejecución del MVC.
- Son ideales para validaciones, autorización, logging, etc., en acciones específicas.

## `Tipos` de filtros en ASP.NET:  

| Tipo de filtro      | ¿Cuándo actúa?                                      |
|---------------------|-----------------------------------------------------|
| AuthorizationFilter | Antes de que se ejecute cualquier acción (acceso)   |
| ResourceFilter      | Antes y después del binding de modelo               |
| ActionFilter        | Antes y después de la acción del controlador        |
| ExceptionFilter     | Cuando ocurre una excepción en una acción           |
| ResultFilter        | Antes y después de generar el resultado (vista/API) |


### `Flujo` de un filtro
![flujoFiltro](/CSharp_API/img/flujoFiltros.png)

## `Diferencias`: Middleware vs Filtros

| Característica         | Middleware                           | Filtro                                 |
|------------------------|--------------------------------------|----------------------------------------|
| Ámbito de aplicación   | Global (toda la app)                 | Por acción, controlador o global       |
| Etapa de ejecución     | Antes del routing                    | Durante la ejecución del controlador   |
| Control del flujo      | Controla si se pasa al siguiente     | Controla si se ejecuta la acción       |
| Acceso a contexto HTTP | Completo                             | Limitado al contexto de acción         |
| Personalización        | Requiere escribir clases específicas | Usa interfaces como `IActionFilter`, etc. |
