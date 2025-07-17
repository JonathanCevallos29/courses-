
## ¿Qué es gRPC?

gRPC (Google Remote Procedure Call) es un framework moderno de comunicación entre servicios, desarrollado por Google. Permite que dos aplicaciones (o servicios) se comuniquen de manera eficiente mediante **llamadas a procedimientos remotos** (RPC), como si fueran funciones locales.

Está basado en HTTP/2 y utiliza **Protocol Buffers (protobuf)** como mecanismo de serialización de datos, lo que lo hace extremadamente rápido, compacto y eficiente.

---

## ¿Qué significa que sea "agnóstico al lenguaje"?

Significa que **gRPC no está atado a un solo lenguaje de programación**. Se puede tener un servicio gRPC escrito en C#, y un cliente que consuma ese servicio desde Python, Java, C#, etc.

Esto es posible porque el contrato entre cliente y servidor se define con archivos `.proto`, que luego se compilan a los lenguajes deseados.

---

## Características de gRPC

- **Comunicación eficiente y de alto rendimiento** gracias a HTTP/2.
- **Serialización binaria** con Protobuf, que es más rápida y ligera que JSON.
- **Contratos fuertemente tipados** a través de archivos `.proto`.
- **Soporte para streaming bidireccional** (cliente-servidor).
- **Multiplataforma y multilenguaje**.
- **Ideal para microservicios** o sistemas distribuidos.

---

## ¿Dónde utilizar gRPC?

gRPC es ideal en contextos como:

- Sistemas distribuidos y microservicios que requieren alta eficiencia.
- Comunicación entre servicios internos (dentro de un data center o VPC).
- Cuando el rendimiento de red y el tamaño del payload son críticos.
- Interacción entre diferentes lenguajes de programación.
- Aplicaciones en tiempo real (streaming).
- Interoperabilidad con otros entornos (backend .NET con cliente en Java, por ejemplo).

No es ideal para APIs públicas para navegadores, ya que no es tan compatible con herramientas como JavaScript en el navegador.

---

## Contratos en gRPC

El contrato gRPC se define mediante un archivo `.proto` (Protocol Buffer). En él se especifican:

- Los servicios que se van a exponer.
- Los métodos que ofrece cada servicio.
- Los mensajes (estructuras de datos) que envían y reciben los métodos.

Este contrato sirve como **fuente única de verdad**. A partir de él se generan automáticamente los archivos necesarios en el lenguaje objetivo (C#, Java, etc.), tanto para el cliente como para el servidor.

---

## Flujo de implementación de gRPC 

1. **Definir el contrato en un archivo `.proto`**: se describe qué métodos existen y qué datos reciben/devuelven.

2. **Generar código a partir del .proto**: el archivo se compila en cada lenguaje usando herramientas de gRPC (protoc).

3. **Implementar el servicio en el servidor**: se crea una clase que implementa los métodos definidos en el contrato.

4. **Configurar el servidor gRPC**: se registran los servicios en la aplicación (por ejemplo, en .NET).

5. **Crear el cliente gRPC**: se usa el código generado para que el cliente pueda invocar los métodos remotos.

6. **Conectar cliente y servidor**: mediante HTTP/2 y usando los contratos definidos, los servicios se comunican de forma rápida y segura.

---

## ¿gRPC reemplaza a REST?

No necesariamente. Ambos tienen casos de uso diferentes.

| Comparación         | REST                     | gRPC                            |
|---------------------|--------------------------|---------------------------------|
| Formato             | JSON                     | Binario (Protobuf)              |
| Protocolo           | HTTP/1.1                 | HTTP/2                          |
| Contratos           | No obligatorios          | Obligatorio (.proto)            |
| Lenguaje agnóstico  | Sí, pero con esfuerzo    | Sí, de forma nativa             |
| Navegadores         | Compatible               | No directamente compatible      |
| Rendimiento         | Medio                    | Alto                            |
| Streaming           | Limitado                 | Nativo (streaming bidireccional)|

---

