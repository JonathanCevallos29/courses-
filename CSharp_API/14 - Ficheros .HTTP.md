
# Ficheros HTTP

## ¿Qué es un fichero HTTP?

Un fichero HTTP (`.http` o `.rest`) es un archivo de texto que permite escribir y ejecutar solicitudes HTTP directamente desde el editor de código (como Visual Studio Code o Rider).

Contiene peticiones como `GET`, `POST`, `PUT`, `DELETE`, cabeceras, cuerpos de solicitud, etc., y puede ejecutarse desde el editor para obtener una respuesta del servidor.

---

## ¿Para qué sirve?

- Probar endpoints de una API.
- Simular solicitudes HTTP sin salir del entorno de desarrollo.
- Automatizar pruebas básicas de integración.
- Compartir fácilmente pruebas o ejemplos de uso de la API.

---

## ¿Qué se puede hacer en un fichero .http?

- Definir múltiples solicitudes en un solo archivo.
- Usar variables y entornos (`@token`, `{{url}}`, etc.).
- Ejecutar peticiones sin depender de herramientas externas.
- Ver respuesta en el mismo editor.

---

## Ventajas de los ficheros HTTP

- Ligero y rápido.
- Se mantiene junto al código fuente (versionable en Git).
- Ideal para pruebas rápidas mientras se desarrolla.
- Soporta entornos personalizados.
- Facilita documentación técnica.


---

# Comparación: Fichero HTTP vs Postman vs Swagger

| Característica              | Fichero HTTP (.http/.rest)          | Postman                               | Swagger (OpenAPI UI)                 |
|----------------------------|-------------------------------------|----------------------------------------|--------------------------------------|
| ¿Qué es?                   | Archivo de texto para enviar peticiones HTTP desde el editor | Herramienta externa para probar y documentar APIs | Interfaz web auto-generada para interactuar con la API |
| Interfaz                   | Texto plano en editor  | Gráfica, amigable                      | Gráfica (basada en la documentación de la API) |
| Requiere instalación       | Solo una extensión        | Sí (app o versión web)                 | No. Se auto-genera desde el backend  |
| Ligereza                   | Muy ligero                          | Medio                                  | Ligero, se sirve desde la API        |
| Documentación              | No integrada                        | Sí, puede documentar                   | Sí, basada en OpenAPI/Swagger.json   |
| Ideal para...              | Desarrolladores backend             | QA, testers, clientes externos         | Probar la API y visualizarla con documentación |
| Compartir                  | Fácil (es solo texto)               | Exportar/importar colección            | Acceso directo en el navegador       |
| Integración con Git        | Sí                                  | Parcial (requiere exportar)            | No aplica directamente               |
| Pruebas automáticas        | Limitadas                           | Avanzadas (pre/post scripts, tests)    | No pensada para pruebas              |
| Variables/entornos         | Sí (REST Client)                    | Sí                                     | No (a menos que se modifique manualmente) |
| Seguridad (tokens, etc.)   | Sí, pero manual                     | Soporta autenticación avanzada         | Depende de la configuración          |
| Facilidad de uso           | Fácil para desarrolladores          | Intuitivo para testers y no técnicos   | Muy accesible para usuarios externos |

---

## ¿Cuándo usar cada uno?

### Fichero HTTP
- Durante el desarrollo rápido.
- Para mantener junto al código fuente.
- Cuando se quiere hacer pruebas simples sin herramientas externas.

### Postman
- Para pruebas avanzadas, automatizadas o colaborativas.
- Cuando el equipo incluye testers o QA.
- Para compartir fácilmente una API con clientes o terceros.

### Swagger
- Para documentar la API de forma auto-generada.
- Cuando otros puedan probar la API sin herramientas externas.
- Ideal para APIs públicas o documentadas.

---

## ¿Se pueden usar todos?

Sí, un flujo profesional suele ser:

1. **Fichero `.http`** durante el desarrollo.
2. **Swagger** para documentación inmediata y pruebas básicas.
3. **Postman** para pruebas avanzadas, QA, colaboración y automatización.

