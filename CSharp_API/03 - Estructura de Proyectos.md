
## Primera Estructura

Este tipo de estructura está basada en carpetas organizadas por responsabilidad, no en proyectos conectados directamente en una solución .sln.

![estructura](/CSharp_API/img/estructura.png)

### ¿Qué significa cada parte?

`WebPersonal Repo (root)`  
Carpeta principal del repositorio Git. Aquí podría vivir un README.md, configuración global, workflows, etc.

`src/`
- Carpeta principal que contiene el código fuente del sistema.

`BackEnd (API)/`
- Proyecto del servidor: API REST creada con ASP.NET Core.
- Contiene:
  - **src/:** Código de producción (controladores, servicios, modelos, etc.).
  - **test/:** Pruebas unitarias o pruebas específicas para la API.

`FrontEnd (Blazor)/`
- Proyecto del cliente web, hecho con Blazor WebAssembly o Blazor Server.
- Contiene:
  - **src/:** Componentes Blazor, servicios, páginas, etc.
  - **test/:** Pruebas para el frontend (unitarias o de integración).

`Shared/`
- Código compartido entre frontend y backend.
- Ejemplo: modelos, DTOs, validaciones, enums, etc.

`IntegrationTest/`
Carpeta dedicada a las pruebas de integración, que pueden involucrar a varios proyectos a la vez.

---

## Estructura con solución y múltiples proyectos (.sln)

Esta es la estructura más común y recomendada para desarrolladores .NET que trabajan en Visual Studio, especialmente cuando se usa MAUI, Blazor y ASP.NET Core Web API.

###  Pasos para crear la solución

#### 1. Crear una solución vacía
- Abrir Visual Studio.
- Seleccionar: **Blank Solution**.
- Nombrar, por ejemplo: `MiSistema.sln`.

#### 2. Agregar proyectos nuevos a la solución

**Proyecto 1: API REST**
- Tipo: ASP.NET Core Web API
- Nombre: `MiSistema.API`

**Proyecto 2: App MAUI con Blazor**
- Tipo: .NET MAUI Blazor App
- Nombre: `MiSistema.MAUI`

**Proyecto 3: Librería de Modelos Compartidos**
- Tipo: Class Library
- Framework: .NET Standard o .NET 6/7+
- Nombre: `MiSistema.Shared`

**Proyecto 4: Pruebas**
- Tipo: xUnit / MSTest / NUnit
- Nombre: `MiSistema.Tests`

### Referenciar el proyecto compartido

Tanto el proyecto `API` como el de `MAUI` deben agregar una referencia al proyecto `Shared`:

1. Clic derecho en el proyecto (`API` o `MAUI`) > **Add** > **Project Reference…**
2. Seleccionar el proyecto `MiSistema.Shared`.

> Esto permite usar modelos, DTOs o clases compartidas en ambos proyectos sin duplicar código.

## Estructura final esperada

```
MiSistema.sln
├── MiSistema.API (ASP.NET Core Web API)
│ └── Referencia a MiSistema.Shared
├── MiSistema.MAUI (Aplicación móvil y escritorio con Blazor)
│ └── Referencia a MiSistema.Shared
├── MiSistema.Shared (Modelos, DTOs, reglas comunes)
├── MiSistema.Tests (Pruebas unitarias)
```

### ¿Qué representa cada parte?

#### `.sln (Solución)`
- Archivo que agrupa todos los proyectos.
- Permite depurar, compilar y administrar todo desde un mismo entorno.

#### `MiSistema.API`
- Proyecto que expone servicios REST.
- Contiene controladores, endpoints, lógica de negocio, configuración.

#### `MiSistema.MAUI`
- App para dispositivos móviles y de escritorio.
- Usa Blazor como interfaz (Blazor Hybrid).
- Consume la API para mostrar datos y realizar operaciones.

#### `MiSistema.Shared`
- Proyecto de clase compartido.
- Incluye modelos de dominio, DTOs, validaciones, enums, utilidades comunes.

#### `MiSistema.Tests`
- Proyecto de pruebas unitarias y de integración.
- Verifica el comportamiento de la lógica del sistema.

## Otro ejemplo de estructura
```
Task.sln
│
├── Task.API               ← Web API con controladores
│   ├── Controllers/
│   ├── Extensions/
│   └── ...

├── Task.Application       ← Casos de uso, servicios
│   ├── Interfaces/
│   ├── Services/
│   └── ...

├── Task.Domain            ← Entidades, lógica de negocio
│   ├── Entities/
│   ├── Enums/
│   └── ...

├── Task.Infrastructure    ← EF Core, acceso a datos
│   ├── Data/
│   ├── Repositories/
│   └── ...

├── Task.Shared            ← DTOs, Helpers, constantes
│   ├── DTOs/
│   ├── Constants/
│   └── ...

├── Task.MAUI              ← Aplicación móvil / escritorio (Blazor)
│   ├── Pages/
│   ├── Components/
│   ├── Services/
│   ├── wwwroot/
│   └── Main.razor

├── Task.Tests             ← Pruebas unitarias (opcional)
│   └── UnitTests/
```