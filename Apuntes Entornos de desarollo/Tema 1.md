
Git – Comandos Básicos

1. Configuración inicial

git config --global user.name "Tu Nombre"
Define el nombre que aparecerá en los commits.

git config --global user.email "tu@email.com"
Define el correo asociado a los commits.

2. Crear o clonar repositorios

git init
Crea un nuevo repositorio en la carpeta actual.

git clone URL
Clona un repositorio existente desde GitHub u otra fuente.

3. Estados de los archivos

git status
Muestra el estado actual de los archivos (modificados, listos para commit, no rastreados).

4. Añadir y confirmar cambios

git add archivo
Añade un archivo al área de preparación (staging).

git add .
Añade todos los archivos modificados.

git commit -m "Mensaje"
Confirma los cambios con un mensaje descriptivo.

5. Ver historial

git log
Muestra el historial de commits.

git log --oneline
Historial resumido en una sola línea por commit.

6. Sincronización con remoto

git remote add origin URL
Conecta el repositorio local con uno remoto.

git push origin main
Sube los commits al repositorio remoto (rama main).

git pull origin main
Descarga y combina cambios del remoto a local.

7. Ramas (branches)

git branch
Lista las ramas existentes.

git branch nombre_rama
Crea una nueva rama.

git checkout nombre_rama
Cambia a otra rama.

git merge nombre_rama
Fusiona una rama con la actual.

8. Otros útiles

git diff
Muestra las diferencias entre archivos modificados y lo último confirmado.

git reset archivo
Quita un archivo del área de preparación (antes del commit).