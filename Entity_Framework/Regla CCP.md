
# `Regla` del Centro Común (Common Closure Principle - CCP)

"Las clases que cambian juntas, deberían agruparse juntas."

También se interpreta como:

> "Si algo se repite tres veces o más, es señal de que debe abstraerse o centralizarse."

## ¿Para qué se utiliza?

- `Identificar` puntos de reutilización de código.
- `Detectar` duplicación de lógica.
- `Promover` la modularidad y cohesión.
- `Facilitar` el mantenimiento del software.

## Aplicación conceptual

- Si tres clases o módulos hacen validaciones similares, deberían usar un servicio común.
- Si el mismo bloque de lógica se repite en tres métodos diferentes, debería moverse a un método reutilizable.
- Si varios modelos manejan propiedades comunes como fecha de creación o modificación, se pueden unificar en una clase base.

## Reglas relacionadas

- `DRY (Don't Repeat Yourself)`: Evita duplicar lógica, reutiliza código.
- `SRP (Single Responsibility Principle)`: Cada módulo debe tener una única razón para cambiar.
- `Open/Closed Principle`: El código debe estar abierto para extensión, pero cerrado para modificación.
- `Rule of Three (Regla de Tres)`: Repetir algo tres veces indica que se necesita abstracción.

