# Bella Pasta — Restaurante italiano

Aplicación web de administración con clientes, mesas, categorías, platos, reservas, pedidos y detalles de pedidos. Frontend HTML/CSS/JavaScript, API FastAPI y base de datos SQLite en local / PostgreSQL (Supabase) en Vercel.

## Ejecutar localmente

```bash
pip install -r requirements.txt
python -m uvicorn main:app --reload
```

Visita http://127.0.0.1:8000 y la API en `/docs`. Sin `DATABASE_URL` usa `restaurante.db` (SQLite).

## Subir a Vercel paso a paso

**El ZIP no se sube en el campo de repositorio de Vercel.** Primero hay que subir los archivos descomprimidos a GitHub y luego importar el repositorio en Vercel.

1. En GitHub: **New repository** → nombre `bella-pasta` → **Create repository**.
2. Dentro del repositorio: **Add file → Upload files**. Descomprime el ZIP, selecciona **los archivos y carpetas de dentro** (no la carpeta superior ni el ZIP) y pulsa **Commit changes**. Confirma que `main.py`, `database.py`, `requirements.txt`, `vercel.json` y la carpeta `static` estén directamente en la raíz.
3. En Vercel: **Add New → Project** → importa el repositorio `bella-pasta` de GitHub.
4. Framework Preset: **Other**. Root Directory: `./` (raíz). No hace falta Build Command.
5. Pulsa **Deploy**. El panel se abre con una base SQLite de demostración temporal incluso sin configurar Supabase. **Los cambios se pueden perder entre ejecuciones o despliegues** en este modo.
6. Para datos permanentes sigue la sección de Supabase de abajo y añade `DATABASE_URL` en **Settings → Environment Variables**; después ejecuta **Redeploy**.

## Publicar en Vercel con Supabase

1. Crea un proyecto en [Supabase](https://supabase.com). En **Connect**, copia el enlace PostgreSQL del **Session pooler** (puerto 5432) o el de conexión compatible con IPv4 que indique tu panel.
2. En **Vercel → bella-pasta → Settings → Environment Variables**, crea una variable llamada `DATABASE_URL` y pega la URL PostgreSQL completa como valor. No la subas a GitHub ni la compartas en capturas. La contraseña puede necesitar codificar caracteres especiales de URL.
3. Actualiza GitHub con los nuevos `main.py`, `database.py`, `requirements.txt`, `static/index.html` y `static/styles.css` (y demás archivos de este proyecto). Vercel debería generar un nuevo despliegue. Si agregaste la variable después, haz **Redeploy**.
4. Prueba crear un cliente, editarlo, refrescar la página, abrirla en otra ventana y verificar que el cambio persista. Luego prueba eliminar.

La API crea tablas y carga categorías, mesas y platos de ejemplo cuando la base está vacía. Datos previos de `restaurante.db` **no se migran** automáticamente a Supabase.

## Archivos

- `main.py`: rutas y reglas del negocio.
- `database.py`: esquema SQL, conectividad y datos de ejemplo.
- `static/index.html`, `styles.css`, `app.js`, `carbonara.jpg`: interfaz.
- `docs/diagrama_entidad_relacion.mmd`: diagrama ER.
- `requirements.txt`, `vercel.json`, `.gitignore`: configuración.

## API

Para clientes, mesas, categorias, platos, reservas, pedidos y detalles_pedido:
GET `/api/{tabla}`, GET `/api/{tabla}/{id}`, POST `/api/{tabla}`, PUT `/api/{tabla}/{id}`, DELETE `/api/{tabla}/{id}`. Adicionalmente GET `/api/resumen` y GET `/api/pedidos/{id}/total`.

**Seguridad:** esta aplicación es una práctica educativa sin autenticación. No ingreses datos reales de clientes hasta añadir controles de acceso.

## Nota importante sobre la rúbrica y publicación

La entrega académica incluye `restaurante.db` para evidenciar el funcionamiento con SQLite. Vercel no ofrece almacenamiento SQLite local persistente. Sin `DATABASE_URL` se usa una copia temporal de la base SQLite en `/tmp` para poder probar la aplicación, pero los cambios no están garantizados entre invocaciones. Para conservar los cambios se necesita `DATABASE_URL` conectado a PostgreSQL/Supabase. Se mantiene SQLite para ejecución local y entrega. La base de datos remota inicia con registros de ejemplo, no importa automáticamente el contenido del archivo SQLite.

Esta app no tiene usuarios ni autenticación: usar solo datos ficticios en la versión pública.
