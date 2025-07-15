## ¿Qué es `Git`?

Es un software que nos permite manejar el versionado de nuestro código.

---

## ¿Qué es `GitHub`?

Es un servicio que nos permite compartir y publicar código en la nube.

---

![flujoGityGitHub](/Git_%20&_GITHUB//image.png)

El `flujo básico` de git son 4 estados, 3 que suceden en el local y 1 en este caso en GitHub

1. **Modified (Modificado):**
   Se han realizado cambios en los archivos del proyecto, pero Git todavía no los está rastreando para el próximo commit.

2. **Staged (Preparado):**
   Con `git add`, los archivos modificados se agregan al área de preparación (staging area), listos para ser confirmados.

3. **Committed (Confirmado):**
   Usando `git commit`, los archivos en staging se guardan de forma segura en el historial del repositorio local.

4. **Remote (Remoto):**
   Con `git push`, los commits del repositorio local se envían al repositorio remoto, como GitHub, para respaldar o compartir los cambios.

5. **Pull (Obtener cambios remotos):**
   Con `git pull`, se descargan y aplican los cambios que otros hayan subido al repositorio remoto.

---

## `Ignorar archivos`

En el archivo .gitignore incluimos todo lo que **NO** queramos incluir en nuestro repositorio.

### Ejemplo de archivo `.gitignore`

```gitignore
# ignorar todos los archivos que terminen en .log
*.log
# excepto production.log
!production.log
# ignorar los archivos terminados en .txt dentro de la carpeta doc,
# pero no en sus subcarpetas
doc/*.txt
# ignorar todos los archivos terminados en .txt dentro de la carpeta doc
# y también en sus subcarpetas
doc/**/*.txt
```

Se puede crear automáticamente con [gitignore.io](https://www.toptal.com/developers/gitignore)

---

## `Clonar` repositorios

`git clone https://github.com/usuario/repositorio.git` sirve para copiar un repositorio remoto de GitHub a la computadora.

---

## `Ramas`

Una rama nos permite aislar una nueva funcionalidad en nuestro código que después podremos añadir a la versión principal.

```
# crear rama
git branch nombre-rama

# cambiar de rama
git checkout nombre-rama

# crear una rama y cambiarte a ella
git checkout -b rama

# eliminar rama
git branch -d nombre-rama

# eliminar ramas remotas
git push origin --delete nombre-rama

#eliminar rama (forzado)
git branch -D nombre-rama

# listar todas las ramas del repositorio
git branch

# lista ramas no fusionadas a la rama actual
git branch --no-merged

# lista ramas fusionadas a la rama actual
git branch --merged

# rebasar ramas
git checkout rama-secundaria
git rebase rama-principal
```

Cuando trabajamos con ramas en Git, es importante asegurarnos de estar ubicados en la rama main antes de crear nuevas ramas.
Esto garantiza que todas las ramas partan del mismo punto base

Cuando creamos nuevas ramas, si queremos respaldarlas en el repositorio remoto (como GitHub), podemos usar el comando:

`git push -u origin nombre-de-la-rama`

Esto sube la rama al remoto y establece un seguimiento para facilitar futuros `git push` y `git pull`.

También es válido mantener algunas ramas solo en local, especialmente si son temporales o experimentales, para no sobrecargar el repositorio remoto con muchas ramas que no se usan colaborativamente.

## Fusiones

Une dos ramas. Para hacer una fusión necesitamos:
1. Situarnos en la rama que se quedará con el contenido fusionado.
2. Fusionar.

Cuando se fusionan ramas se pueden dar 2 resultados diferentes:
* **Fast-Forward:** La fusión se hace automática, no hay conflictos por resolver.
* **Manual Merge:** La fusión hay que hacerla manual, para resolver conflictos de duplicación de contenido.
```
git checkout rama-principal

# ejecutamos el comando merge con la rama secundaria a fusionar
git merge rama-secundaria
```

---

## Cambios

Modificaciones al último cambio

```
# sin editar el mensaje del último commit
git commit --amend --no-edit

# editando el mensaje del último commit
git commit --amend -m "nuevo mensaje para el último commit"

# eliminar el último commit
git reset --hard HEAD~1

# cambiar a una rama
git checkout nombre-rama

# cambiar a un commit en particular
git checkout id-commit
```

---

## Registro Historial

```
git log

# muestra en una sola línea por cambio
git log --oneline

# guarda el log en la ruta y archivo que especifiquemos
git log > commits.txt

# muestra el historial con el formato que indicamos
git log --pretty=format:"%h - %an, %ar : %s"

# cambiamos la n por cualquier número entero y mostrará los n cambios recientes
git log -n

# muestra los cambios realizados después de la fecha especificada
git log --after="2025-07-07 00:00:00"

# muestra los cambios realizados antes de la fecha especificada
git log --before="2025-07-08 00:00:00"

# muestra los cambios realizados en el rango de fecha especificado
git log --after="2025-07-07 00:00:00" --before="2025-07-08 00:00:00"

# muestra una gráfica del historial de cambios, rama y fusiones
git log --oneline --graph --all

# muestra todo el registro de acciones del log
# incluyendo inserciones, cambios, eliminaciones, fusiones, etc.
git reflog

# diferencias entre el Working Directory y el Staging Area
git diff
```
## Reseteo del historial
```
#nos muestra el listado de archivos nuevos (untracked), borrados o editados
git status

# borra HEAD
git reset --soft

# borra HEAD y Staging
git reset --mixed

# borra todo: HEAD, Staging y Working Directory
git reset --hard

# deshace todos los cambios después del commit indicado, preservando los cambios localmente
git reset id-commit

# desecha todo el historial y regresa al commit especificado
git reset --hard id-commit
```
