
# Uso de JavaScript en Blazor (JavaScript Interop)

Blazor está diseñado para trabajar principalmente con C#, pero en ciertos casos es necesario usar funciones JavaScript, especialmente cuando se requiere acceso a funcionalidades del navegador o bibliotecas externas no disponibles en .NET.

Esto se logra a través del mecanismo llamado **JavaScript Interop**.

---

## ¿Qué es JavaScript Interop?

JavaScript Interop permite:

- Llamar funciones JavaScript desde componentes Blazor (C# → JS).
- Llamar métodos C# desde JavaScript (JS → C#).

Esto hace posible:
- Usar bibliotecas JS como Chart.js, SweetAlert, FileReader, etc.
- Acceder a APIs del navegador (clipboard, geolocalización, etc.).
- Integrar componentes UI externos o comportamientos del DOM.
- Realizar tareas que Blazor aún no soporta directamente.

---

## Flujo General

### 1. Llamadas de C# a JavaScript
- Se inyecta el servicio `IJSRuntime`.
- Se invoca una función JS mediante `InvokeVoidAsync` o `InvokeAsync<T>`.

### 2. Llamadas de JavaScript a C#
- Se define un método `[JSInvokable]` en una clase C#.
- Se llama desde JS con `DotNet.invokeMethodAsync` o `DotNetReference`.

---

## Métodos Comunes

| Método                        | Uso                                         |
|:---                           |:---                                         |
| `IJSRuntime.InvokeVoidAsync`  | Ejecutar código JS sin retorno               |
| `IJSRuntime.InvokeAsync<T>`   | Ejecutar código JS con retorno tipado        |
| `[JSInvokable]`               | Permitir que JS llame a un método C#         |
| `DotNetObjectReference`       | Pasar instancia de objeto .NET a JS          |

---

## Consideraciones importantes

- JS Interop es **asincrónico**, por lo tanto, siempre se debe usar `async/await`.
- El código JS debe estar accesible (ya sea en archivos `.js`, en `wwwroot` o en `index.html/_Host.cshtml`).
- JS Interop **no reemplaza** la lógica del backend, se limita a la interacción con el frontend.
- Puede **degradar el rendimiento** si se abusa de llamadas cruzadas JS-C#.
- En Blazor Server, las llamadas JS → C# pueden generar latencia por la conexión con el servidor.

---

## Cuándo usar JavaScript en Blazor

- Cuando se necesite funcionalidades del navegador no disponibles en .NET (copiar al portapapeles, manejar scroll, detección de eventos del DOM).
- Al integrar librerías frontend ya existentes (calendarios, sliders, tooltips).
- Para manipulación avanzada del DOM (clases, animaciones).
- Para lógica de UI que sería muy costosa o compleja en Blazor puro.

---

## Buenas prácticas

- Aislar el código JavaScript en archivos organizados y usa nombres claros para las funciones.
- No mezclar lógica de negocio en JS: usarlo solo para la capa de presentación.
- Documentar bien las funciones JS que se van a exponer a Blazor.
- Usar servicios o clases wrapper en C# si va a hacer múltiples llamadas a funciones JS.
- En Blazor Server, tener en cuenta la latencia al interactuar con JS desde el servidor.

---

## Limitaciones

- No se puede ejecutar JS sincrónicamente desde Blazor.
- En Blazor WebAssembly, el código JS se ejecuta en el mismo contexto, por lo tanto es más fluido.
- En Blazor Server, las llamadas dependen de la conexión SignalR, lo que puede provocar demoras.
- No se puede ejecutar JS desde clases estáticas sin pasar contexto.

---

## Ejemplos típicos de uso

- Mostrar notificaciones o alertas (`alert()`, SweetAlert)
- Copiar contenido al portapapeles (`navigator.clipboard.writeText`)
- Obtener el tamaño de la ventana o posición del scroll
- Integrar mapas interactivos (Leaflet, Google Maps)
- Usar máscaras de entrada o validaciones del lado del cliente

---

