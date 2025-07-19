## ¿Qué es `Blazor`?

Blazor (de Browser + Razor) permite desarrollar interfaces web ricas con `.NET` y `C#`, tanto en el cliente como en el servidor. Utiliza el motor de renderizado Razor (el mismo que en ASP.NET) para generar componentes `HTML` de forma declarativa.

## ¿Cómo funciona Blazor?

Tiene dos modos principales de funcionamiento:

1. Blazor Server

   - La lógica de la app corre en el servidor.
   - Usa SignalR (WebSockets) para enviar eventos y recibir actualizaciones de la interfaz.
   - La UI se renderiza en el servidor, y solo se envían los cambios al cliente.
   - (+) Menor tamaño de descarga inicial.
   - (-) Depende de la conexión constante con el servidor.

2. Blazor WebAssembly (WASM)
   - La app se ejecuta completamente en el navegador.
   - Compila el código C# a WebAssembly.
   - (+) Ideal para apps offline o con poca interacción con el servidor.
   - (-) Descarga inicial más pesada (carga DLLs, runtime de .NET).

## ¿Qué puede hacer Blazor?

- Crear componentes reutilizables con Razor (HTML + C#).
- Consumir APIs REST desde el cliente.
- Usar Inyección de dependencias, navegación, enrutamiento, validación de formularios, etc.
- Interoperar con JavaScript si se necesita (JS interop).
- Usar SignalR para tiempo real (como chats, dashboards).
- Compartir lógica entre el frontend y backend usando proyectos compartidos.
