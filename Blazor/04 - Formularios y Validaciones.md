
 ## ¿Qué es un `formulario` en Blazor?

Un formulario en Blazor es una interfaz para capturar datos del usuario, utilizando componentes como:

- Campos de entrada (InputText, InputNumber, etc.)
- Botones de envío
- Mensajes de error
- Validaciones automáticas
- Todo se basa en data binding (enlace entre UI y modelo de datos) y modelos con anotaciones.


## Componentes clave para formularios

1. `<EditForm>`

- Es el contenedor principal del formulario.
- Se enlaza a un modelo (Model) y ejecuta un método cuando se envía (OnValidSubmit o OnSubmit).

2. Componentes de entrada nativos de Blazor

- `InputText` (para texto)
- `InputNumber` (para números)
- `InputDate` (para fechas)
- `InputSelect` (para listas desplegables)
- `InputCheckbox` (para booleanos)

> Estos están ligados al modelo de datos y permiten validaciones automáticas.

## ¿Cómo se manejan los datos en un formulario?

- Se crea una clase modelo que representa los datos del formulario.
- Se usa binding bidireccional `(@bind-Value)` para que los cambios en la UI se reflejen en el modelo.

Blazor detecta automáticamente los cambios y puede validarlos.

## Validaciones en Blazor

Blazor usa el sistema de validación de `Data Annotations` de `.NET.` Se definen reglas directamente sobre las propiedades del modelo.

- `[Required]` – campo obligatorio
- `[StringLength]` – longitud máxima/mínima
- `[Range]` – valores numéricos permitidos
- `[EmailAddress]`, `[Phone]`, `[Compare]`, etc.

## Flujo típico de validación

- El usuario llena el formulario.
- Blazor valida el modelo automáticamente al hacer submit.
- Si es válido, se ejecuta el método del OnValidSubmit.
- Si no es válido, muestra los errores junto a cada campo.

## Clases auxiliares

`ValidationSummary`: muestra una lista de todos los errores del formulario.
`ValidationMessageFor`: muestra un error específico junto a un campo.

## Validaciones personalizadas

Se puede crear validaciones propias, implementando la interfaz `IValidatableObject` o escribiendo atributos personalizados.

Esto es útil para:

- Validaciones entre campos (por ejemplo: confirmar contraseña)
- Reglas de negocio complejas (por ejemplo: rango de fechas)

## Buenas prácticas en formularios

- Separar la lógica en una clase DTO o ViewModel, no usar directamente las entidades del dominio o la base de datos.
- Usar EditForm con modelo + Data Annotations en vez de formularios manuales.
- Validar tanto en cliente (Blazor) como en servidor (por seguridad).
- Agregar indicadores visuales (colores, mensajes) para mejorar la UX.
- Si un formulario es complejo, dividirlo en pasos (wizard) o secciones.
- Usar `@key` en listas para ayudar al renderizado eficiente de Blazor.