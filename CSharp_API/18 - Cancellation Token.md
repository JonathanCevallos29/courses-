
# CancellationToken en .NET

## ¿Qué es un `CancellationToken`?

El `CancellationToken` es una estructura de .NET que permite **cancelar tareas en ejecución de manera controlada y cooperativa**. Es útil para operaciones asincrónicas o de larga duración que podrían necesitar detenerse antes de completarse, por ejemplo, por un timeout, una acción del usuario o un cierre de la aplicación.

---

## ¿Cómo funciona?

El token funciona como un canal de comunicación entre quien inicia la tarea y quien la ejecuta. Cuando se activa una solicitud de cancelación:

- El **emisor** (por ejemplo, un controlador o servicio) dispara la cancelación.
- El **receptor** (la lógica de la tarea) debe revisar periódicamente si el token ha solicitado cancelación, y finalizar su ejecución voluntariamente si es así.

Es un enfoque **cooperativo**, no forza el cierre abrupto de la operación, sino que permite que esta termine limpiamente.

---

## Flujo de implementación de `CancellationToken`

1. **Creación**: Se crea un `CancellationTokenSource`, que es el objeto que controla cuándo se desea cancelar.

2. **Propagación**: Se extrae el `CancellationToken` de la fuente y se pasa a los métodos que puedan ser cancelados.

3. **Consumo**: Los métodos que reciben el token deben consultar si se ha solicitado la cancelación y, si es así, deben terminar apropiadamente.

4. **Activación**: En algún punto, el token source se cancela (por ejemplo, porque el usuario canceló la operación o hubo un timeout).

5. **Limpieza**: Los métodos liberan recursos, finalizan procesos o devuelven un estado de cancelación.

---

## ¿Se debe pasar el `CancellationToken` a todos los métodos?

**No necesariamente**, pero:

- Se recomienda **pasarlo a todos los métodos asincrónicos** que realicen tareas de larga duración o I/O (como llamadas HTTP, acceso a base de datos, procesamiento intensivo, etc.).
- En métodos internos (helper methods), solo si ellos también hacen trabajo cancelable.
- Para métodos triviales, no es obligatorio.

El objetivo es **propagar el control de cancelación hasta donde sea necesario**, sin complicar innecesariamente el diseño.

---

## ¿Dónde se utiliza `CancellationToken`?

- Llamadas HTTP con `HttpClient`.
- Operaciones con Entity Framework Core (consultas largas).
- Lecturas/escrituras de archivos.
- Tareas con `Task.Run`, `async`/`await`.
- Servicios de background (`BackgroundService` en ASP.NET Core).
- Operaciones en colas o workers.
- Procesos en pipelines (como middleware o flujos de procesamiento).

---

## ¿Qué beneficios ofrece?

- **Evita sobrecargar el sistema** al cancelar tareas innecesarias.
- **Mejora la experiencia del usuario** (respuestas más rápidas cuando se cancela).
- **Conserva recursos** como memoria, CPU, conexiones.
- **Permite limpieza ordenada** de operaciones (liberación de archivos, rollback, etc.).

---

## ¿Qué sucede si se ignora el token?

Si el método no comprueba el token:

- La tarea **continuará ejecutándose** aunque ya no sea necesaria.
- Puede **malgastar recursos** y **generar efectos secundarios** no deseados.
- El sistema puede **verse ralentizado o inestable** si se acumulan muchas tareas no canceladas.

Por eso es importante que **los métodos colaboren** revisando periódicamente el token.

---

## ¿Se puede combinar con timeout?

Sí. Se puede crear un `CancellationTokenSource` con un límite de tiempo, de forma que se cancele automáticamente después de X milisegundos. Esto es útil en tareas que no deben exceder un tiempo máximo.

---

