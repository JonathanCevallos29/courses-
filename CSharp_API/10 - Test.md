
# Pruebas (Testing) en .NET

## ¿Por qué hacer pruebas?

El testing en .NET permite verificar que el código funcione correctamente, evitar regresiones y garantizar calidad en el desarrollo.  
Aporta confianza, facilita refactorizaciones y mejora el mantenimiento del sistema.

---

## Tipos principales de tests

| Tipo de test        | Descripción                                                  | Qué prueba                                                |
|---------------------|--------------------------------------------------------------|-----------------------------------------------------------|
| Unit Test           | Pruebas de funciones o métodos individuales                  | Lógica aislada (sin base de datos ni red)                 |
| Integration Test    | Pruebas de varios componentes trabajando juntos              | DB, servicios externos, autenticación, etc.               |
| Functional Test     | Verifica que una funcionalidad entera se comporte como debe  | Un caso de uso completo desde el punto de vista del usuario |
| End-to-End (E2E)    | Simula el comportamiento del sistema completo                | Flujo de aplicación como si fuera un usuario real         |

---

## Herramientas comunes en .NET

| Herramienta         | Uso principal                         |
|---------------------|----------------------------------------|
| xUnit               | Framework de pruebas unitarias         |
| NUnit               | Alternativa popular a xUnit            |
| MSTest              | Framework oficial de Microsoft         |
| Moq                 | Simulación de dependencias (Mocking)   |
| FluentAssertions    | Validaciones legibles y expresivas     |
| TestServer          | Pruebas HTTP sin servidor real         |
| WebApplicationFactory | Facilita el testing de APIs en ASP.NET Core |

---

## ¿Qué es un TestServer?

`TestServer` permite simular un servidor HTTP dentro del entorno de pruebas, sin necesidad de levantar la aplicación en un puerto real.

- Se usa para probar APIs o endpoints.
- No hace llamadas reales por red: todo ocurre en memoria.
- Ideal para **Integration Tests controlados**.

---

## ¿Qué es WebApplicationFactory?

`WebApplicationFactory<T>` es una clase que simplifica el uso de `TestServer` y permite:

- Simular toda una aplicación ASP.NET Core para testing.
- Inyectar dependencias personalizadas (mocks, bases de datos en memoria).
- Ejecutar peticiones HTTP contra tu API en pruebas.

---

## ¿Cómo probar APIs con dependencias (por ejemplo, base de datos)?

1. Crear una clase de prueba que extienda `WebApplicationFactory`.
2. Usar `ConfigureServices` para reemplazar dependencias reales por simuladas, como `DbContext` por una base en memoria.
3. Enviar peticiones con `HttpClient` para simular requests reales.
4. Validar respuestas, códigos HTTP, contenido, etc.

---

## Reemplazo de dependencias en tests

Cuando se hacen pruebas de integración, se puede:

- Reemplazar la base de datos real por **InMemoryDb** (`UseInMemoryDatabase`).
- Sustituir servicios por mocks (`AddSingleton<IMiServicio>(mock.Object)`).
- Usar `Moq` para generar objetos falsos con comportamientos controlados.

Esto evita efectos secundarios (escribir en bases reales) y hace los tests repetibles y rápidos.

---

# Stub, Fake y Mock

En pruebas automatizadas, especialmente al aislar dependencias, se utilizan objetos simulados como `Stub`, `Fake` y `Mock`.  
Aunque se parecen, tienen propósitos diferentes:

## Tabla comparativa

| Tipo     | ¿Qué es?                                     | ¿Qué hace?                                               | ¿Cuándo se usa?                                       |
|----------|----------------------------------------------|-----------------------------------------------------------|--------------------------------------------------------|
| Stub     | Objeto con respuestas **predefinidas**       | Devuelve valores fijos sin lógica real                   | Cuando se necesita un valor fijo para continuar una prueba |
| Fake     | Implementación **simplificada pero funcional** | Simula lógica real con estructuras internas (ej. listas) | Para simular una dependencia realista sin base externa |
| Mock     | Objeto simulado que **verifica interacciones**| Registra llamadas, permite validar si un método fue usado | Para comprobar si un método fue invocado correctamente |

## Ejemplos 

- **Stub**: Un repositorio que devuelve siempre el mismo resultado cuando se llama a `GetById(5)`, sin importar el ID.
- **Fake**: Un servicio que implementa una base de datos en memoria con una lista `List<T>` para agregar y buscar registros.
- **Mock**: Un objeto que verifica si se llamó a `EnviarCorreo()` y con qué parámetros, útil para validar el comportamiento.

## ¿Cuál usar?

| Necesidad                                       | Tipo recomendado |
|------------------------------------------------|------------------|
| Devolver un resultado fijo                     | Stub             |
| Simular un servicio o base de datos sencilla   | Fake             |
| Verificar que se ejecutó un método             | Mock             |

## Ejemplos en código

- **Stub**:
```
mock.Setup(x => x.ObtenerNombre()).Returns("Jonathan");
```

- **Fake**:

```
mock.Verify(x => x.EnviarCorreo(It.IsAny<string>()), Times.Once());
```

- **Mock**:

```
public class FakeRepositorio : IRepositorio
{
    private readonly List<Usuario> _usuarios = new();

    public Usuario GetById(int id) => _usuarios.FirstOrDefault(u => u.Id == id);
}
```

---

## Buenas prácticas en testing

- Los Unit Tests deben ser rápidos, independientes y no depender de infraestructura.
- Usar nombres descriptivos: `Metodo_CuandoCondicion_EntoncesResultadoEsperado()`.
- Separar los tests por tipo (Unit, Integration, etc.) en carpetas o proyectos distintos.
- Ejecutar las pruebas automáticamente en CI/CD.
- Mockear lo que no sea el objetivo del test.
- No mezclar tests unitarios con acceso a base de datos o red.

