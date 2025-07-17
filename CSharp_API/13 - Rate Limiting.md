
## ¿Qué es `Rate Limiting`?

Es un mecanismo para limitar la cantidad de solicitudes que un cliente puede hacer a un recurso dentro de un período de tiempo determinado.

---

## ¿Por qué utilizar Rate Limiting?

- Prevenir ataques como DDoS y fuerza bruta.
- Evitar que usuarios abusen de los recursos disponibles.
- Controlar la carga sobre la base de datos y backend.
- Proteger servicios externos con cuotas limitadas.
- Garantizar un servicio justo y estable para todos los usuarios.
- Reducir costos de infraestructura por exceso de peticiones.

---

## ¿`Tipos` de Rate Limiting?

### 1. `Fixed Window` (Ventana fija)
- Límite fijo de solicitudes durante una ventana de tiempo específica.
- `Ejemplo`: máximo 100 peticiones por minuto.
- Simple, pero puede haber ráfagas justo antes y después del cambio de ventana.

---

### 2. `Sliding Window` (Ventana deslizante)
- Similar al fixed window pero con subintervalos dentro del período.
- **Ventajas**: más preciso y evita acumulaciones súbitas.
- **Ejemplo**: se permiten 100 solicitudes en los últimos 60 segundos, sin importar el segundo actual.

---

### 3. `Token Bucket` (Depósito de tokens)
- Se generan tokens periódicamente y se almacenan.
- Cada solicitud consume un token.
- Si **hay** tokens → la petición pasa.
- Si **no** hay tokens → se rechaza o espera.
- `Ejemplo`: 10 tokens por segundo → puedes hacer hasta 10 peticiones instantáneas si el balde está lleno.

---

### 4. `Concurrency Limit` (Límite de concurrencia)
- Limita la cantidad de solicitudes *simultáneas* que pueden procesarse.
- Cuando se alcanza el máximo, nuevas solicitudes deben esperar o ser rechazadas.
- **Ideal para**: proteger recursos con alta carga, como bases de datos o servicios lentos.
- **Ejemplo**: máximo 5 solicitudes activas a la vez.

---

### 5. `Leaky Bucket` (Cubo con fugas)
- Parecido al Token Bucket, pero con salida constante.
- Las solicitudes se procesan a un ritmo fijo, desbordes son descartados.
- No tan usado directamente en .NET pero útil en proxies como NGINX.

---

## ¿Cuándo usar cada uno?

| Tipo                   | ¿Cuándo usarlo?                                                                 |
|------------------------|----------------------------------------------------------------------------------|
| Fixed Window           | Casos simples, bajo tráfico o límites generales rápidos.                        |
| Sliding Window         | Si se quiere suavizar los picos de tráfico y evitar abusos momentáneos.         |
| Token Bucket           | Cuando se quiere permitir ráfagas controladas.                                  |
| Concurrency Limit      | Cuando se tienen recursos limitados (BD, CPU) o se desea controlar el uso vivo. |

---

## ¿Se pueden combinar?

Sí, en .NET puedes aplicar múltiples reglas por cliente o endpoint:

- Por IP
- Por nombre de usuario
- Por token de autenticación
- Por tipo de endpoint

---

## ¿Se puede usar caché distribuido con rate limiting?

Sí. Para que el control sea consistente entre instancias, se puede usar:

- **Redis**
- **MemoryCache** (solo si es una app de instancia única)

---

## ¿Se requiere microservicios?

**No.** Se puede usar rate limiting en:

- APIs monolíticas
- Microservicios
- Gateways
- Proxies inversos como NGINX o API Management

---
