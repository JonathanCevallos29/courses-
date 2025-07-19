
## ¿Qué es una `ruta` en Blazor?

Una ruta es una `URL` asociada a un componente, que permite navegar entre vistas o páginas dentro de la aplicación. Blazor intercepta los cambios de URL y renderiza el componente correspondiente sin recargar toda la página.

## ¿Cómo se define una ruta?

Las rutas se definen con la directiva `@page` al inicio de un componente `.razor`.

```
@page "/login"
<h3>Página de Login</h3>
```

> Esto significa que cuando el usuario navegue a /login, se renderizará ese componente.

## ¿Dónde se configura la navegación?

1. En `App.razor`

Aquí se define el **Router global** que controla el flujo de la app

```
<Router AppAssembly="@typeof(App).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Página no encontrada.</p>
        </LayoutView>
    </NotFound>
</Router>
```

- `RouteView`: carga el componente asociado a la ruta.
- `NotFound`: renderiza algo si la ruta no existe.

## Tipos de rutas

1. Ruta simple

```
@page "/home"
```

2. Ruta con parámetros

```
@page "/producto/{id}"
@code {
    [Parameter] public int id { get; set; }
}
```

> {id} captura un valor de la URL y lo asigna al parámetro id.

3. Ruta con parámetros opcionales

Blazor no tiene parámetros opcionales directamente en la ruta, pero podés usar sobrecarga de rutas:

```
@page "/producto"
@page "/producto/{id:int}"
```

## Tipado de parámetros en ruta

Blazor permite restringir el tipo de parámetro directamente en la ruta:

- `{id:int}` → solo números enteros
- `{activo:bool}` → true/false
- `{slug}` → texto libre

**Esto evita errores de navegación**.

## Navegación con enlaces

Blazor reemplaza `<a href>` por el componente `<NavLink>` para navegación interna con estilo activo automático.

```
<NavLink href="/usuarios" class="nav-link" activeClass="active">
    Usuarios
</NavLink>
```

## Buenas prácticas

- No usar rutas largas ni con demasiados parámetros.
- Siempre proveer un componente para ruta no encontrada (NotFound).
- Organizar rutas por módulos: /empleados, /planillas, /evaluaciones, etc.
- Usar NavLink en vez de a para navegación interna.
- Si la ruta no tiene lógica, crear componentes livianos solo para navegación.



