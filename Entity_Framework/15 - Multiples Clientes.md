
# Aplicaciones con Múltiples Clientes (Multi-Tenancy)

## Introducción a Multi-Tenancy

**Multi-Tenancy** (multiarrendamiento) es un enfoque arquitectónico donde una sola instancia de una aplicación sirve a **múltiples clientes** (llamados "tenants").  
Cada cliente puede tener sus propios datos, configuraciones, usuarios y comportamiento personalizado, pero todos comparten la misma aplicación base.

Ejemplos comunes:
- Aplicaciones SaaS (Software as a Service).
- Sistemas ERP/CRM que sirven a múltiples empresas desde la misma plataforma.

---

## ¿Por qué usar Multi-Tenancy?

- **Eficiencia:** Reduce costos operativos y de infraestructura.
- **Mantenimiento único:** Se actualiza una sola instancia para todos.
- **Escalabilidad:** Se puede escalar horizontalmente agregando nodos.

---

## Estrategias para implementar Multi-Tenancy

Existen tres estrategias principales:

### 1. **Base de Datos por Tenant (Isolated)**
- Cada cliente tiene su propia base de datos independiente.

**Ventajas:**
- Mayor seguridad y aislamiento.
- Fácil de realizar backups o mover un tenant.

**Desventajas:**
- Requiere más recursos y mantenimiento.
- Complejidad para consultas cruzadas o agregadas.

---

### 2. **Esquema por Tenant**
- Una sola base de datos, pero cada tenant tiene su propio esquema (conjuntos de tablas).

**Ventajas:**
- Buen equilibrio entre aislamiento y eficiencia.
- Compartir base de datos facilita el despliegue.

**Desventajas:**
- Aumenta la complejidad del mantenimiento.
- Puede requerir herramientas específicas para manejar múltiples esquemas.

---

### 3. **Tablas compartidas (Shared)**
- Todos los clientes comparten las mismas tablas, y se distinguen mediante una columna `TenantId`.

**Ventajas:**
- Alta eficiencia y bajo costo.
- Sencillo de escalar horizontalmente.

**Desventajas:**
- Riesgo si no se maneja bien la seguridad (acceso cruzado).
- Las consultas deben filtrar siempre por `TenantId`.

---

## ¿Qué estrategia elegir?

| Criterio                         | Mejor opción                     |
|----------------------------------|----------------------------------|
| Alto aislamiento y seguridad     | Base de datos por tenant         |
| Balance entre rendimiento y gestión | Esquema por tenant             |
| Máxima eficiencia y bajo costo   | Tablas compartidas (shared)      |
| Escenarios SaaS masivos          | Tablas compartidas               |
| Clientes corporativos exigentes | Base de datos por tenant         |

---

## ¿Qué es "Multiplexar"?

**Multiplexar** es el proceso de **dirigir las solicitudes entrantes al contexto correcto del tenant**, generalmente basado en:

- El subdominio (ej: `cliente1.app.com`)
- El encabezado HTTP
- El token del usuario autenticado

Es fundamental para mantener la separación lógica entre los datos y recursos de cada cliente, incluso cuando comparten la misma aplicación o base de datos.

---

## Flujo general de implementación

1. **Identificación del Tenant**  
   - Extraer `TenantId` desde el subdominio, token JWT, cabecera HTTP, etc.

2. **Contextualización**  
   - Registrar y asociar ese `TenantId` con el ciclo de vida de la petición.
   - En EF Core, usar un `HasQueryFilter` global para filtrar automáticamente por `TenantId`.

3. **Configuración dinámica (opcional)**  
   - Cambiar la cadena de conexión o el esquema, si aplica.

4. **Ejecutar operaciones**  
   - Todas las consultas, escrituras y transacciones se ejecutan en el contexto del tenant correcto.

5. **Autorización y seguridad**  
   - Verificar que el usuario tenga permiso para actuar dentro del `TenantId` asociado.

6. **Respuestas personalizadas**  
   - Devolver solo la información que corresponde al tenant actual.

---

## Consideraciones adicionales

- La seguridad es **clave**: nunca permitir que un cliente acceda a datos de otro.
- Hay que prever migraciones y mantenimiento independientes por tenant.
- Algunas bibliotecas como **Finbuckle.MultiTenant** ayudan a manejar el ciclo de vida y contexto de tenants.
- El testing debe contemplar diferentes escenarios multicliente.

