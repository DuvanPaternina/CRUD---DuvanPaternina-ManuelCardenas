# CRUD Usuarios — Guía rápida de Git

Comandos esenciales para trabajar en equipo sin romper el repositorio.

> Acuerdo de trabajo, arquitectura y plan por fases: [`docs/ACUERDO.md`](docs/ACUERDO.md).

---

## 1. Configuración inicial (una sola vez por PC)

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@correo.com"
```

Verificar:

```bash
git config --global --list
```

---

## 2. Ver el estado

```bash
git status                        # estado completo
git status -s                     # estado resumido (corto)
git diff                          # cambios sin preparar
git diff --staged                 # cambios en la bandeja
git log --oneline --graph --all   # historial visual
git log --oneline -5              # últimos 5 commits
git branch                        # ramas locales
git branch -a                     # ramas locales y remotas
git branch -r                     # solo ramas remotas
git branch --show-current         # rama actual
git remote -v                     # remotos configurados
```

---

## 3. Guardar cambios

```bash
git add .                         # prepara todo
git add archivo                   # prepara un archivo
git add carpeta/                  # prepara una carpeta
git commit -m "tipo: mensaje"     # guarda
git commit --amend                # corrige el último commit
```

Formato del mensaje: `<tipo>: <descripción>`

| Tipo | Para qué |
|------|----------|
| `feat` | Funcionalidad nueva |
| `fix` | Corrección de error |
| `chore` | Configuración/dependencias |
| `docs` | Documentación |
| `refactor` | Reorganizar sin cambiar comportamiento |

---

## 4. Ramas

```bash
git switch -c feat/nueva          # crear y cambiar a la rama
git switch main                   # cambiar a main
git switch -                      # volver a la rama anterior
git branch nombre                 # crear sin cambiar
git branch -d nombre              # borrar rama ya fusionada
git branch -D nombre              # borrar rama a la fuerza
git push origin --delete nombre   # borrar rama remota en GitHub
```

> Antes de cambiar de rama: `git status`. Si hay cambios sin commitear, haz commit o
> `git stash` primero.

---

## 5. Sincronizar

```bash
git pull                          # traer y mezclar
git pull --rebase                 # traer y reubicar (historial limpio)
git fetch origin                  # solo descargar (no toca archivos)
git push                          # subir la rama actual
git push -u origin rama           # primera vez de una rama nueva
```

> `git pull` **suma**, no borra. Si tu trabajo está en un commit, no se pierde.

---

## 6. Deshacer

```bash
git restore archivo               # descartar cambios de un archivo
git restore --staged archivo      # sacar de la bandeja
git reset --hard HEAD             # descartar TODO lo no commiteado
git reset --soft HEAD~1           # deshacer último commit (deja cambios)
git reset --hard HEAD~1           # deshacer último commit (borra cambios)
git revert <hash>                 # deshacer un commit publicado (seguro)
git reflog                        # recuperar commits perdidos
```

> `git reset --hard` es destructivo. Úsalo solo en tu PC y **nunca** sobre `main`.

---

## 7. Guardar trabajo a medias

```bash
git stash                         # guarda en un cajón
git stash pop                     # recupera lo guardado
git stash list                    # ver cajones
git stash drop                    # borrar cajón
```

---

## 8. Flujo de trabajo del equipo

```bash
# 1. Actualizarse
git switch main
git pull

# 2. Crear rama
git switch -c feat/mi-rama

# 3. Trabajar
git add .
git commit -m "feat: descripción"

# 4. Subir la rama
git push -u origin feat/mi-rama

# 5. Crear PR en GitHub (manual)
# 6. El compañero revisa el diff y hace Merge
# 7. Volver a main actualizado
git switch main
git pull
git branch -d feat/mi-rama
```

---

## 9. Convenciones del proyecto

- Ramas: `<tipo>/<descripción>` → `feat/`, `fix/`, `chore/`, `docs/`, `refactor/`
- Commits: `<tipo>: <descripción>` → `feat: agrega endpoint DELETE`
- **Nunca** `git push --force` en `main`
- **Nunca** commitear `.env` ni `node_modules/`
- **Siempre** `git pull` antes de empezar
- Commitear **antes** de hacer `pull` si hay cambios

---

## 10. Comandos de emergencia

```bash
git status                        # siempre empieza aquí
git diff                          # ¿qué cambié?
git log --oneline -5              # ¿qué hice?
git reflog                        # ¿dónde estaba?
```
