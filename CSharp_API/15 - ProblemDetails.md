
## ¿Qué es `ProblemDetails`?

**ProblemDetails** es una clase estándar definida en [RFC 7807](https://datatracker.ietf.org/doc/html/rfc7807) para representar errores HTTP de forma estructurada y legible por máquinas. ASP.NET Core la implementa para devolver errores en formato JSON de forma uniforme.

## ¿Para qué sirve?

Sirve para:
- Estandarizar la forma en que los errores son devueltos por la API.
- Proveer información clara sobre qué salió mal.
- Facilitar la integración con clientes (como frontends o apps móviles).

---

## ¿Cómo se ve un `ProblemDetails`?

```
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "00-df95fabcbdccfb3b0...-00",
  "errors": {
    "Email": ["El campo Email es obligatorio."]
  }
}
```

## Principales propiedades

| Propiedad    | Descripción                                     |
| :---         | :---                                            |
| `type`       | URI que identifica el tipo de error (opcional). |
| `title`      | Breve descripción del error.                    |
| `status`     | Código de estado HTTP.                          |
| `detail`     | Detalles específicos del error.                 |
| `instance`   | URI del recurso donde ocurrió el error.         |
| `extensions` | Diccionario adicional para agregar más info.    |

## ¿Cuándo se usa?

- Errores de validación del modelo (ModelState inválido).
- Respuestas con BadRequest() usando datos inválidos.
- Errores no manejados (cuando se usa UseExceptionHandler)


## Personalización de errores
Se puede construir manualmente instancias de `ProblemDetails` para retornar errores más claros y adaptados al contexto de la API, incluyendo detalles personalizados, título, estado, y URI de la solicitud fallida.

---

## Validación automática con `[ApiController]`

Cuando un controlador está decorado con el atributo `[ApiController]`, ASP.NET Core se encarga automáticamente de validar el modelo de entrada (`ModelState`) y retornar una respuesta con formato `ProblemDetails` cuando la validación falla, sin necesidad de escribir lógica adicional.

---

## Personalizar globalmente el comportamiento

ASP.NET Core permite configurar globalmente cómo deben responder los controladores ante errores de validación, usando las opciones del comportamiento de API (`ApiBehaviorOptions`). Esto permite definir una estructura uniforme para todos los errores de validación, con títulos, mensajes y rutas personalizadas.


