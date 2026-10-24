# Base de datos (MySQL)

| Ambiente | Base | Quién la crea |
|---|---|---|
| Tu Mac | MySQL 8 en Docker (`docker-compose.yml`) | `docker compose up -d mysql` |
| CI (cada PR) | MySQL 8 temporal, se borra al terminar | El Guardián |
| Staging (`main`) | MySQL gestionado, base de staging | El dueño, una vez |
| Producción | MySQL gestionado, **otra** base | El dueño, una vez |

## Conexión de staging y producción (una vez por ambiente)

1. Crea la base en el proveedor (por defecto **TiDB Cloud Starter**, gratis): una para staging y otra para producción.
2. **GitHub** → *Settings → Environments* → `staging` y luego `production` → *Add environment secret*:
   `MYSQL_HOST`, `MYSQL_PORT`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DATABASE`. Los usa el Guardián para aplicar las migraciones.
3. **Netlify** → *Site configuration → Environment variables*: las mismas variables más `MYSQL_SSL=1`, **marcadas como secretas** y con un valor distinto por contexto (*Production* = producción; *Deploy previews* y *Branch deploys* = staging). Las usa la página.
4. Permisos mínimos: la página, con un usuario que solo lee y escribe datos; las migraciones, con uno que puede cambiar tablas.

## Archivos subidos (imágenes del admin)

No se guardan en `public/` (Netlify no conserva archivos subidos): van a **Netlify Blobs** (`@netlify/blobs`), que viene con el sitio, sin cuenta extra.

## Previews: una base por PR (ADR 0032 del Guardián)

Cada preview usa su propia base `<repo>_pr_<N>` (con guiones bajos; p. ej. `mi_tienda_pr_12`) en el cluster de staging: el Guardián la crea, le aplica las migraciones del PR en cada deploy y la borra al cerrar el PR.

**En el código** (lo hace el primer ticket que conecta la base):
- La conexión usa `process.env.MYSQL_DATABASE_PREVIEW || process.env.MYSQL_DATABASE`.
- `next.config.ts`: `env: { MYSQL_DATABASE_PREVIEW: process.env.MYSQL_DATABASE_PREVIEW ?? "" }`. Staging y los previews comparten las variables de Netlify, así que el nombre de la base del PR viaja en el build (no es secreto).

**Una sola vez, el dueño:**
1. TiDB Cloud, cluster de **staging** → *SQL Editor* (cambia `PREFIJO` y `<repo>`):
   ```sql
   CREATE USER 'PREFIJO.preview'@'%' IDENTIFIED BY '<clave-larga>';
   GRANT ALL PRIVILEGES ON `<repo>\_pr\_%`.* TO 'PREFIJO.preview'@'%';
   GRANT SELECT, INSERT, UPDATE, DELETE ON `<repo>\_pr\_%`.* TO 'PREFIJO.app'@'%';
   ```
2. GitHub → *Settings → Environments → **preview*** → secretos `MYSQL_HOST` y `MYSQL_PORT` (los de staging), `MYSQL_USER` = `PREFIJO.preview` y `MYSQL_PASSWORD`.

El usuario `preview` solo alcanza las bases `<repo>_pr_*`: no puede leer ni tocar staging ni producción.

