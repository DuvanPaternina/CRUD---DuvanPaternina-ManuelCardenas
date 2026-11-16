# Acuerdo de trabajo y plan por fases

CRUD Usuarios — React + Vite + Node.js + Express + MySQL

Actividad formativa del **tercer corte** de Desarrollo Web.
Documento de referencia: `12 actividad 20262.pdf` (Ing. José Manuel Santos Sánchez).

Aplicación web tipo **CRUD** (Create, Read, Update, Delete) para gestionar usuarios
sobre la base de datos `bdsantos`.

> Este documento conserva el acuerdo de trabajo, arquitectura y plan por fases.
> La guía de comandos de Git está en el [`README.md`](../README.md).

---

## 1. Arquitectura

| Capa | Tecnología | Puerto | Descripción |
|------|-----------|--------|-------------|
| Base de datos | MySQL 8.x | 3306 | BD `bdsantos` con tablas `usuario`, `estado`, `tipo_usuario` |
| Backend | Node.js + Express + mysql2 | 4000 | API REST (`/api/usuarios`, `/api/estados`, `/api/tipos`) |
| Frontend | React 19 + Vite | 5173 | SPA con componentes `UserForm` y `UserTable` |

Flujo de datos:

```
Usuario → React (Axios) → API REST → Express → mysql2 → MySQL
                                        ←←←← JSON ←←←←←←
```

---

## 2. Integrantes y roles

| Integrante | Rol | Responsable de |
|------------|-----|----------------|
| **Duvan Paternina** | Backend | Carpeta `server/` (conexión MySQL, rutas, API REST) |
| **Manuel Cárdenas** | Frontend | Carpeta `client/` (componentes React, formularios, tabla) |
| **Ambos** | Integración | Pruebas end-to-end, merge final, documentación y entrega |

> Regla de oro del reparto: **cada quien trabaja en su carpeta dentro de su propia rama.**
> Esto reduce a casi cero los conflictos de merge.

---

## 3. Estructura del proyecto

```
.
├── README.md               # guía de comandos Git
├── docs/
│   └── ACUERDO.md          # este documento
├── database/
│   └── bdsantos.sql        # script de creación de la BD
├── .gitignore              # ignora node_modules, .env, dist
├── server/                 # BACKEND (Node.js + Express + MySQL)
│   ├── package.json
│   ├── .env                # ⚠️ NO se sube a GitHub (credenciales)
│   └── src/
│       ├── db.js           # pool de conexiones a MySQL
│       ├── index.js        # punto de entrada, middlewares y montaje de rutas
│       └── routes/
│           ├── estados.routes.js
│           ├── tipos.routes.js
│           └── usuarios.routes.js   # CRUD completo
└── client/                 # FRONTEND (React + Vite)
    ├── package.json
    ├── index.html
    └── src/
        ├── main.jsx        # monta la app en el DOM (#root)
        ├── App.jsx         # orquesta formulario + tabla
        ├── api.js          # cliente Axios y servicios CRUD
        └── components/
            ├── UserForm.jsx    # crear/editar (React Hook Form + Yup)
            └── UserTable.jsx   # listar/eliminar
```

---

## 4. Reglas de trabajo con GitHub

### 4.1 Ramas

`main` es la rama **principal**: siempre debe funcionar y es la que se entrega.
**Nadie hace push directo a `main`.**

Convención de nombres: `<tipo>/<descripción>`

| Prefijo | Uso | Ejemplo |
|---------|-----|---------|
| `feat/` | Funcionalidad nueva | `feat/backend-rutas` |
| `fix/` | Corrección de error | `fix/error-al-eliminar` |
| `chore/` | Configuración/dependencias | `chore/deps` |
| `docs/` | Documentación | `docs/readme` |

Ramas de este proyecto:

```
main                        # oficial, siempre estable
feat/backend-rutas          # Duvan  → server/
feat/frontend-formulario    # Manuel → client/
```

### 4.2 Commits

Formato: `<tipo>: <descripción corta>`. El mensaje explica **por qué**, no solo el qué.

```
feat: agrega endpoint DELETE /api/usuarios/:username
fix: corrige JOIN que duplicaba usuarios en el listado
chore: configura pool de conexiones con mysql2/promise
```

Tipos válidos: `feat`, `fix`, `refactor`, `docs`, `chore`.

Reglas:
- Un commit = **una sola cosa lógica**.
- Commit **temprano y seguido** (cada commit es un punto de guardado).
- Nunca commitear `node_modules/`, `.env` ni `dist/`.

### 4.3 Pull Requests (PR)

Todo cambio entra a `main` **mediante un Pull Request**, para que el otro lo revise.

```
1. git switch main && git pull        # actualizarse
2. git switch -c feat/mi-rama         # crear el carril
3. ... trabajo ...
4. git add . && git commit -m "..."   # guardar
5. git push                           # sube SOLO mi rama
6. GitHub → Pull requests → New pull request  → crear el PR
7. El compañero revisa el diff y hace Merge
8. git switch main && git pull        # traer el merge
9. git branch -d feat/mi-rama         # borrar la rama ya fusionada
```

### 4.4 Reglas invariables

| # | Regla | Por qué |
|---|-------|---------|
| 1 | Nunca `git push --force` en `main` | Rompe el repo del compañero |
| 2 | Nunca commitear `.env` | Contiene contraseñas |
| 3 | Nunca commitear `node_modules/` | Se regenera con `npm install` |
| 4 | Siempre `git pull` antes de empezar a trabajar | Evita conflictos y trabajo perdido |
| 5 | Si hay cambios sin commitear, commitear **antes** de hacer pull | Un pull nunca borra commits; lo que se pierde es lo no commiteado |
| 6 | Ante un conflicto, **no usar** `git reset --hard` sobre `main` | Resolver el conflicto editando el archivo |

---

## 5. Configuración inicial (para quien clone el repositorio por primera vez)

```bash
git clone https://github.com/DuvanPaternina/CRUD---DuvanPaternina-ManuelCardenas.git
cd CRUD---DuvanPaternina-ManuelCardenas

# Dependencias del backend
cd server && npm install && cd ..

# Dependencias del frontend
cd client && npm install && cd ..
```

`node_modules/` **no** está en el repositorio: se regenera siempre con `npm install`
en `server/` y en `client/`.

### Variables de entorno (archivo `server/.env`)

Cada desarrollador crea el suyo (no se sube al repo). Mismos nombres de variable,
credenciales propias de su MySQL local:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=tu_password
DB_NAME=bdsantos
DB_PORT=3306
PORT=4000
```

### Base de datos

Ejecutar `database/bdsantos.sql` en MySQL Workbench (PDF, páginas 5–9).

---

## 6. Plan de trabajo por fases

### ✅ Fase 0 — Cimientos (completada)

- [x] Configuración de Git (`user.name`, `user.email`)
- [x] Repositorio en GitHub + colaborador invitado
- [x] `.gitignore` con `node_modules/`, `.env`, `dist/`, `*.log`
- [x] Estructura `server/` y `client/` creada
- [x] Primer commit y push a `main`

### ✅ Fase 1 — Backend (Duvan) — rama `feat/backend-rutas`

Guía: PDF `12 actividad 20262.pdf`, páginas 11–27.

- [x] `server/package.json`: `"type": "module"`, scripts `dev` y `start` (pág. 13)
- [x] `server/.env` con las variables de entorno (pág. 14)
- [x] `server/src/db.js`: pool de conexiones con `mysql2/promise` (pág. 15)
- [x] `server/src/routes/estados.routes.js` (pág. 17)
- [x] `server/src/routes/tipos.routes.js` (pág. 18)
- [x] `server/src/routes/usuarios.routes.js`: CRUD completo + bcryptjs (pág. 19)
- [x] `server/src/index.js`: Express, CORS, `/api/health`, montaje de rutas (pág. 26)
- [x] Base de datos `bdsantos` creada en MySQL Workbench (pág. 7)
- [x] Pruebas con `curl` de los 4 endpoints del CRUD (pág. 28–32)

### 🔄 Fase 2 — Frontend (Manuel) — rama `feat/frontend-formulario`

Guía: PDF `12 actividad 20262.pdf`, páginas 33–51.

- [ ] `client/src/api.js`: instancia Axios + `UsuariosAPI` + `CatalogosAPI` (pág. 34)
- [ ] `client/src/components/UserForm.jsx`: React Hook Form + Yup (pág. 38)
- [ ] `client/src/components/UserTable.jsx`: tabla con editar/eliminar (pág. 46)
- [ ] `client/src/App.jsx`: estado global `selected` / `reload` (pág. 49)
- [ ] `client/src/main.jsx`: montaje con `ReactDOM.createRoot` (pág. 52)
- [ ] `client/index.html`: `<div id="root">` y título (pág. 54)

### 🔄 Fase 3 — Integración (ambos) — rama `feat/integracion`

- [x] Script SQL de `bdsantos` guardado en el repo (`database/bdsantos.sql`)
- [ ] Levantar backend (`npm run dev` en `server/`) y frontend (`npm run dev` en `client/`)
- [ ] Probar Create, Read, Update y Delete **desde la interfaz web**
- [ ] Capturas de pantalla como evidencia funcional (rúbrica: 15%)
- [ ] Verificar que la tabla se actualiza sin recargar manualmente

### 🔄 Fase 4 — Entrega (ambos)

- [ ] PR final a `main` con el proyecto completo
- [ ] `README.md` actualizado con las instrucciones finales
- [ ] Comprimir `server/` y `client/` en un `.zip` (PDF pág. 59)
- [ ] Revisión final de la rúbrica:
  - Backend con Express, rutas modulares, `.env`, pool MySQL, CRUD funcional — **30%**
  - Frontend React + Vite, componentes modulares, Axios, validación Yup — **30%**
  - Estado y flujo de datos entre componentes (`useState`, `useEffect`, callbacks) — **20%**
  - Integración frontend ↔ backend con las 4 operaciones CRUD probadas — **15%**
  - Estructura, nomenclatura, comentarios y documentación — **5%**

---

## 7. Entrega

La entrega consiste en un archivo `.zip` que contiene las carpetas `server` y `client`
(PDF `12 actividad 20262.pdf`, página 59). **No** incluir `node_modules/` ni `.env`
dentro del `.zip`; la base de datos se crea con el script SQL sobre MySQL del evaluador.
