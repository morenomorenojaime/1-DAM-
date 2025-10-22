Configuración e instalación (verificación).

Flujo básico: init → add → commit → push.

Reescribir commits / correcciones.

Ramas: crear, cambiar, merge (fast-forward y recursive).

Conflictos y su resolución.

Stash, reset y revert.

Tags y releases.

Comandos de inspección (status, log).

Archivos de proyecto relevantes (.editorconfig, .gitattributes, .gitignore) — resumen útil.

Combinaciones y flujos prácticos listos para copiar.

1) Configuración inicial (identidad y editor)
# 1.1 Establecer tu nombre y correo para que Git los asocie a cada commit
git config --global user.name "Tu Nombre"
git config --global user.email "tuemail@ejemplo.com"

# 1.2 Opcional: establece el editor por defecto (ej. Visual Studio Code)
git config --global core.editor "code --wait"

# 1.3 Ver todas las configuraciones globales que hayas puesto
git config --global --list

# Explicación:
# - --global hace que la configuración valga para TODOS los repositorios en esa máquina.
# - Si omites --global, la configuración solo se aplica al repo actual (útil si quieres identificarte distinto por proyecto).

2) Verificar instalación
# 2.1 Ver la versión de Git instalada
git --version

# 2.2 Ver el estado de configuración efectiva (global y local)
git config --list

# Explicación:
# - git --version confirma que Git está instalado.
# - git config --list muestra las variables activas (puede salir mucho).

3) Crear un repositorio local (workflow básico)
# 3.1 Crear carpeta y entrar
mkdir git-workshop
cd git-workshop

# 3.2 Inicializar Git (crea .git)
git init
# => Ahora esta carpeta es un repositorio Git local (tiene la carpeta oculta .git)

# 3.3 Crear archivos (ejemplos)
# Linux/macOS:
touch README.md Jose.txt
# Windows (CMD/PowerShell):
# echo. > README.md
# echo. > Jose.txt

# 3.4 Añadir archivos al staging area (área de preparación)
git add README.md Jose.txt
# o para añadir TODO en el proyecto:
git add .

# 3.5 Hacer el primer commit con mensaje
git commit -m "Primer commit: agrega README.md y Jose.txt"

# Explicación:
# - git add mueve archivos al "staging area".
# - git commit guarda los cambios (crea un commit en el historial).
# - Mensajes de commit claros ayudan a entender la historia.

4) Reescribir / corregir el último commit
# 4.1 Renombrar un archivo en Git (git mv)
git mv Jose.txt Pablo.txt
# Luego reescribimos el commit anterior (sin crear uno nuevo)
git commit --amend -m "Renombra Jose.txt a Pablo.txt"

# Alternativa (sin --amend): eliminar y crear nuevo commit
# git rm Jose.txt
# git add Pablo.txt
# git commit -m "Elimina Jose.txt y agrega Pablo.txt"

# Explicación:
# - git commit --amend modifica el último commit: su contenido y mensaje.
# - Útil si aún NO has subido (push) ese commit al remoto. Si ya lo subiste, tendrás que forzar el push (peligroso).

5) Ramas: crear, cambiar, trabajar
# 5.1 Crear una nueva rama y cambiar a ella (dos pasos)
git branch yo++
git checkout yo++

# 5.2 Crear y moverte a una rama en un solo comando
git checkout -b yo++

# 5.3 Haces cambios, los añades y los confirmas en la rama
# (ej. editar "Pablo.txt" y añadir una descripción)
git add Pablo.txt
git commit -m "Añade descripción personal en Pablo.txt"

# 5.4 Volver a la rama principal (master o main)
git checkout master
# Nota: muchos repositorios usan "main" como rama principal en vez de "master"

# 5.5 Crear otra rama desde master
git checkout -b taller-info
# editar README.md
git add README.md
git commit -m "Añade descripción del taller en README.md"

# Explicación:
# - Cada rama es una línea de desarrollo independiente.
# - Cambios en una rama no aparecen en otra hasta fusionarlas.

6) Inspeccionar el historial y estado
# 6.1 Ver estado actual de archivos
git status

# 6.2 Ver el historial de commits (detallado)
git log
# versión compacta y gráfica
git log --oneline --all --graph --decorate

# 6.3 Ver commits en una sola línea
git log --oneline

# Explicación:
# - git status te muestra archivos modificados, staged, untracked, rama actual.
# - git log muestra el historial; --graph es útil para ver ramas & merges.

7) Fusionar ramas (merge)
# 7.1 Supongamos que queremos incorporar cambios de 'yo++' a 'master'
git checkout master
git merge yo++

# 7.2 Fast-forward merge:
# Si master no tiene commits nuevos desde que se creó yo++, git solo moverá el puntero: fast-forward.
# No crea commit de merge.

# 7.3 Merge recursive (merge con commit de merge):
# Si master y la rama a fusionar han avanzado en paralelo, git creará un commit de merge.
# Ejemplo:
git merge taller-info
# -> Git intentará combinar y puede crear un commit de merge que unirá ambas historias.

# Explicación:
# - git merge une los historiales de dos ramas.
# - Fast-forward: simple avance del puntero.
# - Recursive (o cr/3-way): si hay divergencia, se crea un commit de merge.

8) Conflictos de merge y resolución
# 8.1 Flujo típico que produce conflicto:
git checkout master
# modificar README.md (línea 1)
git add README.md
git commit -m "Cambia README desde master"

git checkout taller-info
# modificar README.md de manera distinta (línea 1)
git add README.md
git commit -m "Cambia README desde taller-info"

git checkout master
git merge taller-info
# -> Git indica: CONFLICT (merge conflict) in README.md

# 8.2 Resolver conflicto:
#  - Abre README.md, verás marcas: <<<<<<< HEAD / ======= / >>>>>>> taller-info
#  - Edita para dejar la versión final que quieras (o mezcla ambas).
#  - Elimina los marcadores <<<<<<< ======= >>>>>>>
git add README.md
# Completar el merge con commit (si el merge fue interrumpido por conflicto)
git commit -m "Resolución del conflicto en README.md"
# o, si usabas 'git merge --continue' tras resolver (en merges interactivos)
git merge --continue

# Explicación:
# - Git no decide por ti si dos cambios son incompatibles: te marca el conflicto y te toca elegir.
# - Siempre revisar el resultado con pruebas si son cambios de código.

9) Subir a GitHub: remote, push, y ramas remotas
# 9.1 Añadir remoto (solo la primera vez)
git remote add origin https://github.com/tu-usuario/tu-repositorio.git

# 9.2 Push de la rama master (la primera vez se suele usar -u para setear upstream)
git push -u origin master
# -u hace que 'origin master' sea la referencia por defecto para futuros 'git push' y 'git pull'.

# 9.3 Subir ramas locales al remoto (por separado)
git push origin taller-info
git push origin yo++

# 9.4 Clonar un repo remoto (otro ordenador)
git clone https://github.com/tu-usuario/tu-repositorio.git

# 9.5 Descargar y fusionar cambios del remoto
git pull origin master
# (equivale a git fetch + git merge origin/master)

# Explicación:
# - solo las ramas que subes aparecerán en GitHub.
# - git pull fusiona automáticamente (puede causar conflictos).

10) Deshacer cambios: reset vs revert
# 10.1 git reset --soft HEAD^
# Mueve HEAD al commit anterior, deja los cambios en staging (para reenviar un commit modificado)
git reset --soft HEAD^

# 10.2 git reset --hard HEAD^
# Mueve HEAD al commit anterior y borra cambios del working dir (¡Peligroso!)
git reset --hard HEAD^

# 10.3 git revert <commit_id>
# Crea un nuevo commit que deshace los cambios del commit indicado (ideal cuando ya compartiste el commit).
git revert abc1234

# Explicación:
# - reset reescribe historial local (útil antes de push). --soft conserva cambios staged; --hard los borra.
# - revert no borra historial: añade un commit inverso. Es seguro cuando el commit ya fue push.

11) Guardar cambios temporales: stash
# 11.1 Guardar (stash) los cambios actuales (working dir limpio)
git stash
# 11.2 Ver lista de stashes
git stash list
# 11.3 Aplicar y eliminar el stash top
git stash pop
# 11.4 Aplicar sin eliminar del stash
git stash apply stash@{0}
# 11.5 Borrar un stash concreto
git stash drop stash@{0}
# 11.6 Crear stash con mensaje descriptivo
git stash push -m "Trabajo parcial en feature X"

# Explicación:
# - git stash es muy útil para cambiar de rama sin comprometer commits intermedios.
# - pop aplica el stash y lo borra de la lista; apply lo aplica y lo mantiene.

12) Eliminar y renombrar archivos (git rm, git mv)
# 12.1 Eliminar archivo del repo y del disco
git rm archivo.txt
git commit -m "Elimina archivo archivo.txt"

# 12.2 Renombrar moviendo en Git (preserva historial)
git mv viejo.txt nuevo.txt
git commit -m "Renombra viejo.txt a nuevo.txt"

# Explicación:
# - git rm hace 'remove' y stage del borrado.
# - git mv = mv + git add + git rm (combinado).

13) Etiquetas/tags (marcar versiones)
# 13.1 Crear un tag ligero
git tag v1.0.0

# 13.2 Crear un tag anotado (recomendado para releases)
git tag -a v1.0.0 -m "Lanzamiento versión 1.0.0"

# 13.3 Enviar un tag al remoto
git push origin v1.0.0

# 13.4 Enviar todos los tags
git push origin --tags

# Explicación:
# - Tags sirven para marcar releases o hitos (p. ej. v1.0.0).
# - Los tags no se suben automáticamente: hay que "git push origin tag".

14) Comandos útiles adicionales (para el examen)
# Ver cambios en un archivo desde el último commit
git diff archivo.txt

# Ver cambios staged (preparados) vs HEAD
git diff --staged

# Mostrar las ramas y la rama actual (* indica la actual)
git branch

# Ver ramas remotas
git branch -r

# Mostrar la relación entre ramas y commits (útil para entender merges)
git log --oneline --graph --decorate --all

15) Resumen rápido de archivos de configuración del proyecto

.editorconfig (lo que tienes en tu repo)

Define reglas de estilo (EOL, codificación, trim de espacios).

Mantiene consistencia entre editores (VSCode, JetBrains, Vim...).

Ejemplo de líneas importantes:

end_of_line = lf
insert_final_newline = true
charset = utf-8
trim_trailing_whitespace = true


.gitattributes

Indica cómo Git debe tratar ciertos archivos (EOL, text/binary).

Evita problemas CRLF vs LF entre Windows y Linux.

Ejemplo:

* text=auto
*.sh text eol=lf
*.ps1 text eol=crlf
*.png binary


.gitignore (no lo pasaste explícitamente, pero probablemente lo uses)

Lista de archivos/carpetas que Git debe ignorar (node_modules, bin/, .vscode/, *.log).

Ejemplo mínimo:

node_modules/
bin/
.vscode/
*.log

16) Flujos y combinaciones prácticas (listas para copiar/pegar)
A) Flujo básico inicial (crear, agregar, commit, conectar a remoto, push)
# Crear repo local e inicializar
mkdir git-workshop
cd git-workshop
git init

# Crear archivos, añadir y commitear
echo "# Proyecto" > README.md
echo "Contenido" > Pablo.txt
git add .
git commit -m "Primer commit: README y Pablo.txt"

# Conectar remoto y subir master
git remote add origin https://github.com/tu-usuario/pablo-intro-git.git
git push -u origin master

B) Corregir último commit (antes de push)
# Cambiar nombre de archivo y enmendar el último commit
git mv Jose.txt Pablo.txt
git commit --amend -m "Renombra Jose.txt a Pablo.txt"
# Si ya hiciste push y necesitas actualizar remoto (con cuidado):
git push --force-with-lease origin master
# --force-with-lease es más seguro que --force (comprueba que nadie haya metido cambios en el remoto).

C) Crear rama para una funcionalidad, trabajar y fusionar
# Crear y trabajar en rama de feature
git checkout -b pagina_inicio
# editar archivos...
git add .
git commit -m "Añade sección inicio"

# Volver a master y fusionar (fast-forward si es posible)
git checkout master
git merge pagina_inicio

# Si quieres subir la rama al remoto
git push origin pagina_inicio

D) Resolver conflicto típico (paso a paso)
# 1) Intentas merge y aparece conflicto
git checkout master
git merge taller-info
# Git dice: conflicto en README.md

# 2) Edita README.md en tu editor para resolver las marcas <<<<<<< >>>>>>>
# 3) Añade el archivo resuelto
git add README.md

# 4) Finaliza el merge con commit
git commit -m "Resolver conflicto en README.md"

# 5) (Opcional) push
git push origin master

E) Guardar trabajo temporalmente (stash), cambiar de rama y recuperar
# Guardar cambios sin comprometer
git stash push -m "Trabajo parcial en feature X"

# Cambiar de rama
git checkout master
# Hacer tarea rápida...
git checkout pagina_inicio

# Recuperar los cambios
git stash pop

F) Revertir un commit ya publicado (forma segura)
# Revertir commit con id abc123 (no reescribe historial)
git revert abc123
git push origin master

17) Consejos prácticos para Visual Studio (IDE)

Abre la terminal integrada: View → Terminal o usa Git Changes / Team Explorer para acciones gráficas.

Mensajes de commit: escríbelos breves y con finalidad clara (tipo feat: ..., fix: ...).

Si usas VS (Visual Studio) o VSCode, puedes ver las ramas, hacer checkout y merges desde el panel Git (si prefieres GUI).

Para conflictos, el editor suele mostrar comparador visual (source control diff) que facilita elegir cambios.

18) Advertencias y buenas prácticas (importante para examen)

No uses git push --force si no entiendes sus consecuencias: puede sobreescribir trabajo de otros. Si debes reescribir el historial que ya subiste, usa --force-with-lease.

Antes de git merge o git rebase, haz git fetch y git status para saber dónde estás.

Si vas a --amend o reset commits que ya hiciste push, recuerda que cambiarás el historial remoto y otros tendrán problemas si han clonado.

Haz commits pequeños y frecuentes: facilitan la revisión y la recuperación.

19) Glosario corto (comando → qué hace en una frase)

git init → inicializa un repo Git en la carpeta actual.

git add → mueve archivos al área de preparación (staging).

git commit -m "msg" → crea un commit con los cambios staged.

git status → muestra el estado (modified, staged, untracked).

git log → muestra el historial de commits.

git branch → lista/crea ramas.

git checkout → cambia de rama (o restaura archivos).

git checkout -b → crea y cambia a la rama nueva.

git merge → fusiona otra rama en la actual.

git remote add origin <url> → añade un repositorio remoto llamado origin.

git push → sube commits al remoto.

git pull → descarga y fusiona cambios del remoto.

git clone → clona un repo remoto a local.

git mv → renombra/mueve archivos tracked.

git rm → elimina archivos y los stagea para commit.

git commit --amend → modifica el último commit.

git reset → mueve punteros de HEAD (cuidado: reescribe historial local).

git revert → crea un commit que deshace otro commit (seguro si ya compartiste).

git stash → guarda cambios temporales fuera del historial.

git tag → marca un commit con versión/hito.