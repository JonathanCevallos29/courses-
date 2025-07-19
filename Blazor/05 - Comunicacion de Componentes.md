
# Comunicación entre Componentes en Blazor

En Blazor, los componentes son unidades modulares que pueden intercambiar información entre sí mediante distintos mecanismos. 

---

## Tipos de Comunicación

### 1. Padre → Hijo

Se realiza mediante `[Parameter]`. El componente padre envía datos al hijo a través de propiedades marcadas como parámetros. Es una comunicación unidireccional.

### 2. Hijo → Padre

Se logra usando `EventCallback`. El hijo notifica al padre de eventos, como clics, cambios o acciones. Permite mantener el hijo independiente del padre.

### 3. Comunicación entre hermanos o no relacionados

Se usa un servicio compartido o patrón "State Container", el cual se inyecta en ambos componentes. Los cambios en el estado pueden ser observados por cualquiera que lo consuma.

### 4. Cascading Parameters
Permite pasar información desde un ancestro hacia muchos componentes descendientes sin tener que intermediar manualmente en cada nivel jerárquico.

### 5. Gestión de estado mediante servicios
En aplicaciones más grandes, es preferible usar servicios `Scoped` o `Singleton` para manejar estados globales o eventos. Esto permite comunicación fluida entre múltiples componentes distribuidos.

---

## Flujo General

1. El componente necesita recibir o enviar información.
2. Se determina el tipo de relación entre los componentes (padre/hijo, hermanos, etc.).
3. Se implementa el mecanismo adecuado: `[Parameter]`, `EventCallback`, `CascadingParameter` o servicio compartido.
4. Se actualiza el estado del componente receptor o se invoca un evento.
5. El receptor reacciona (mostrando datos, actualizando la UI, procesando una acción).
6. Se actualiza visualmente el componente mediante el sistema de renderizado de Blazor.

---

## Recomendaciones

- Mantener los componentes **desacoplados** siempre que sea posible.
- Usar `EventCallback<T>` para que el flujo sea compatible con el sistema de renderizado de Blazor.
- Evitar usar `CascadingParameter` si la relación es directa entre padre e hijo.
- Para proyectos medianos y grandes, preferir servicios para manejar estados complejos o compartidos.
- Evitar acoplar hermanos directamente: preferir comunicación mediante servicios.

---

##  Aplicaciones comunes

- Comunicación entre formulario y lista de resultados.
- Componentes modales notificando a padres.
- Estados de sesión compartidos entre páginas.
- Envío de parámetros desde layouts o shells globales.
- Módulos como filtros, notificaciones, menús contextuales.

---
