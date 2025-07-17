
# Stored Procedures en EF Core

## ¿Qué es un Stored Procedure?

Un **Stored Procedure** (procedimiento almacenado) es un conjunto de instrucciones SQL precompiladas, almacenadas directamente en la base de datos.  
Actúa como una función que se puede invocar para ejecutar lógica compleja, realizar operaciones CRUD, validaciones o transacciones.

---

## ¿Los Stored Procedures están en desuso?

No están completamente en desuso, pero su uso ha disminuido **en muchas arquitecturas modernas** por las siguientes razones:

- El enfoque actual favorece la lógica de negocio en el código de la aplicación (domain-driven design).
- Son menos portables entre distintos motores de base de datos.
- Dificultan la trazabilidad del código (no están en el repositorio principal de la app).
- Menor flexibilidad para pruebas unitarias y mantenimiento.

> Siguen siendo **útiles y válidos** en escenarios específicos donde brindan ventajas reales.

---

## ¿Cuándo usar Stored Procedures?

Se recomienda usarlos en casos como:

- Procesos complejos que involucran múltiples pasos y necesitan rendimiento óptimo.
- Operaciones que deben ejecutarse de forma atómica y segura (lógica transaccional).
- Integraciones con sistemas legados que dependen de procedimientos existentes.
- Casos donde el acceso a la base de datos está restringido a procedimientos por políticas de seguridad.

---

## ¿Hay que usarlos?

No necesariamente.  
En EF Core y arquitecturas modernas, es preferible usar LINQ, repositorios y el control desde código.

---

## Flujo para implementarlo en EF Core 

1. **Crear el Stored Procedure en la base de datos**  
   - Define el procedimiento con los parámetros y lógica que se necesite.

2. **Configurar el modelo en EF Core**  
   - Define el tipo de resultado esperado (puede ser una entidad o un DTO).

3. **Llamar al procedimiento desde EF Core**
   - Usar métodos como `FromSqlRaw` o `ExecuteSqlRaw`, según se necesite resultado o no.

4. **Mapear resultados si aplica**
   - Si retorna filas, se puede mapear a entidades, vistas o modelos personalizados.

5. **Integrarlo en la lógica de negocio**
   - Consumirlo desde servicios, repositorios u otros puntos clave de la aplicación.

---

## Relación con EF Core

EF Core **soporta** Stored Procedures para:

- **Consultas**: usando `FromSqlRaw` o `FromSqlInterpolated`.
- **Comandos** (insert, update, delete): con `ExecuteSqlRaw`.

En EF Core 7 en adelante, se puede incluso **configurar entidades para usar procedimientos almacenados automáticamente en operaciones `SaveChanges()`**.

---

## Comparación: Stored Procedure vs EF Core puro

| Aspecto                  | Stored Procedure                   | EF Core (LINQ/Fluent API)          |
|--------------------------|------------------------------------|------------------------------------|
| Rendimiento              | Alta eficiencia (precompilado)     | Eficiente, pero dependiente del ORM |
| Mantenibilidad           | Menor, está fuera del código fuente | Mayor, todo en un solo proyecto     |
| Pruebas unitarias        | Difícil                             | Fácil con mocks e in-memory DB     |
| Portabilidad             | Baja (dependiente del motor)       | Alta (cross-platform)              |
| Seguridad de acceso      | Alta (por roles en SQL Server)     | Aplicación debe gestionarla        |
