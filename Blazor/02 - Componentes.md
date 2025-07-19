
## `Componentes` en Blazor
Los componentes son bloques modulares que encapsulan interfaz, lógica y comportamiento. Representan páginas, formularios, controles, secciones, etc.

Son:

- Reutilizables
- Componibles (pueden contener otros)
- Reactivos al estado

## Flujo de uso

- Se crea una interfaz visual con HTML y Razor.
- Definición de propiedades y parámetros
- Se reciben datos del exterior o se definen variables internas.
- Gestión de eventos
- Se define cómo reacciona el componente a acciones del usuario.
- Comunicación entre componentes
- Se permite pasar información entre padres e hijos.
- Control del ciclo de vida
- Se aprovechan métodos como `OnInitialized`, `OnParametersSet`, `Dispose`, etc.

## Tipos de componentes

1. Componente simple

- Contiene `HTML` + `C#` directamente en el archivo `.razor`.

- Se usa para `UI` básica o lógica sencilla.

2. Componente con partial class

- Divide la lógica en un archivo `.razor.cs`.
- Mantiene el archivo .razor solo con la vista.
- Ideal para mantener el código ordenado y escalable.

3. Componente base (herencia)

- Se define una clase base con lógica común.
- Otros componentes heredan de ella usando `@inherits` o como `clase parcial`.
- Muy útil cuando varios componentes comparten comportamiento.

## `Partial class`

Una partial class permite dividir la definición de una clase en varios archivos. En Blazor se usa para:

- Separar la lógica del componente en C# puro (archivo .razor.cs)
- Evitar mezclar HTML con lógica pesada
- Facilitar pruebas unitarias
- Promover la separación de responsabilidades
- Blazor compila automáticamente ambos archivos como una sola clase.

## `Code-Behind`

- Es una forma tradicional de separar lógica de presentación. Similar a cómo se hacía en Windows Forms, WPF o `ASP.NET` Web Forms.
- Se tiene el archivo visual (.razor) y un archivo de código (.razor.cs o .cs) que contiene eventos, - propiedades, etc.
- Aunque en Blazor no es obligatorio, se recomienda en proyectos medianos o grandes.
- En Blazor moderno, code-behind se implementa con partial class.

## `Herencia` de componentes

Un componente puede heredar de otro componente o clase base para reutilizar comportamiento. Esto permite:

- Crear plantillas de comportamiento base (como formularios estándar)
- Aplicar lógica común a varios componentes
- Centralizar validaciones, estilos o funciones compartidas
- Esto mejora la mantenibilidad, especialmente cuando la app crece.

## `Comunicación` entre componentes

**Padre → Hijo**

- Se hace a través de [Parameter]
- Permite pasar datos desde el componente padre hacia el hijo.

**Hijo → Padre**

- e usa EventCallback
- l hijo puede notificar al padre sobre un evento (como un clic).

**CascadingParameter**

- Para valores que deben estar disponibles en muchos niveles (por ejemplo, usuario logueado, idioma, tema).
- Se inyectan desde un componente ancestro a todos sus descendientes.

## Ciclo de vida de componentes

Blazor permite reaccionar a eventos del ciclo de vida del componente:

- **OnInitialized** / **OnInitializedAsync**: se ejecuta al iniciar el componente (bueno para cargar datos).
- **OnParametersSet**: cuando se reciben nuevos parámetros del padre.
- **OnAfterRender**: cuando se renderiza por primera vez.
- **Dispose**: se ejecuta cuando el componente se elimina (ideal para liberar recursos).
- Esto permite controlar cuándo cargar, actualizar y liberar datos o procesos.

## Usos

- Usar partial class desde el inicio si la lógica crece o el componente hace muchas cosas.
- Usar componentes base (herencia) para mantener consistencia en formularios, validaciones, controles.
- La comunicación entre componentes es clave: no hay que abusar de CascadingParameter, usarlo solo cuando de verdad sea un valor global.
- Organizar los componentes en carpetas por rol: Shared, Pages, Components, Forms, etc.
- Evitar componentes gigantes: es mejor dividir en subcomponentes reutilizables.