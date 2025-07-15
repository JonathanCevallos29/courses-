
# Inyección de Dependencias
## ¿Qué es Dependencia?

Una dependencia es cualquier clase u objeto que otra clase necesita para funcionar.  
`Ejemplo:` si una clase Controlador necesita usar un servicio ILoginService, entonces ILoginService es una dependencia de ese controlador.

---

## ¿Qué es inyección de dependencias (DI)?

Es un patrón de diseño que permite que un objeto reciba sus dependencias desde el exterior, en lugar de crearlas él mismo.

> En lugar de que la clase cree su propio servicio, otro componente se lo inyecta automáticamente.

En .NET, la inyección se gestiona a través de un contenedor de servicios.

---

## ¿Por qué usar inyección de dependencias (DI)?

- **Desacopla el código:** las clases no necesitan saber cómo se construyen sus dependencias.
- **Facilita pruebas unitarias:** se puede sustituir dependencias reales por "mocks".
- **Mejora la mantenibilidad:** los cambios en una clase no afectan a otras que la usan.
- **Centraliza la configuración:** se puede registrar servicios en un solo lugar (Program.cs o Startup.cs).

----

## Tipos de inyección de dependencias

|Tipo	                   |  Descripción                                                             |
| :---                   | :---                                                                     |
|**Constructor**	       | La dependencia se pasa en el constructor. Es la más común y recomendada. |
|**Setter (propiedad)**  | La dependencia se asigna a través de una propiedad pública.              | 
|**Método**	             | La dependencia se pasa como argumento en un método específico.           |

---

## Ciclos de vida en la Inyección de Dependencias

Cuando se registran servicios en el contenedor de dependencias de .NET, se define **cuánto tiempo vivirá una instancia** del servicio. 

---

## 1. Transient (Transitorio)

### ¿Qué es?  
Cada vez que el servicio es solicitado, se crea una nueva instancia.

`Analogía` 
Como si cada vez que vas a una tienda, te atiende un nuevo vendedor. Nunca repites con el mismo.

### Cuándo usarlo:
- Cuando el servicio **no necesita mantener estado**.
- Si el servicio es **liviano** y no necesita ser reutilizado.
- Ej: servicios de validación, helpers, transformadores de datos.

---

## 2. Scoped (Por solicitud)

### ¿Qué es?
Se crea **una única instancia por cada solicitud HTTP**. Todos los componentes que participan en esa solicitud usan la misma instancia.

`Analogía`  
Como cuando entras al banco y te atiende un asesor que te acompaña en todo el proceso, pero cuando llega otro cliente, otro asesor lo atiende.

### Cuándo usarlo:
- Cuando se necesita compartir datos **dentro de una misma solicitud**.
- Ideal para servicios como **acceso a base de datos (DbContext)** o lógica de negocio por usuario.
- Evita compartir información entre diferentes solicitudes o usuarios.

---

## 3. Singleton (Instancia única)

### ¿Qué es?
El servicio se **crea una vez sola** al iniciar la aplicación, y se **usa la misma instancia siempre**.

`Analogía`  
Como el director general de una empresa: solo hay uno y todos consultan con él cuando se necesita algo centralizado.

### Cuándo usarlo:
- Cuando el servicio debe **compartirse globalmente**.
- Cuando se trata de una clase **costosa de crear** o cuyos datos **no cambian**.
- Ej: servicios de configuración, caché, loggers, factory compartida.


