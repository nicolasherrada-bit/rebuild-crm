# REBUILD · CRM — guía para tu Claude Code

Sos el Claude Code de **May**. Este es un **proyecto propio y separado** de la app REBUILD: un **CRM** que May arma y
publica ella misma. Contestale siempre en lenguaje simple.

## Qué es y qué NO
- Es **su propio proyecto** (esta carpeta / este repo). No tiene el código de la app → **no puede tocar la app**.
- **Lee** toda la info de la app (clientes, pagos, ventas, captación, meets…) en **solo lectura**.
- **Guarda** lo del CRM en su **propio espacio** (el schema `crm` de la base).
- Como está separado, **May lo publica sola** (su propio Vercel) sin ningún riesgo para la app.

## Conexión a los datos (ya está enchufada)
En `.env.local` está `APP_DB_URL` = usuario **`may_ro`** (solo lectura). Garantizado por la base:
- ✅ `select ... from public.<tabla>` → lee cualquier tabla de la app.
- ⛔ insert/update/delete/alter/drop sobre `public.*` → la base lo **rechaza**.
- ✅ `crm.<tabla>` → acá SÍ podés crear y escribir (las tablas del CRM).

El helper ya está en `src/lib/db.ts` (`q("select ...")`). Para leer la app: `public.*`. Para guardar del CRM: `crm.*`.

## Arrancar
```
npm install
npm run dev     # http://localhost:3000 → tiene que decir "Conectado a la app ✅ — N clientes"
```
Si ves el número de clientes, la conexión lee bien. La página de inicio (`src/app/page.tsx`) es solo una prueba de
humo: reemplazala por el CRM de verdad.

## Qué construir (confirmá el alcance con May/Nico)
Un CRM para el pipeline comercial: leads/contactos, estado (respondió / no respondió / en seguimiento), notas, "lo
último que le enviamos", seguimiento. Se nutre de la data de la app (leyéndola) y agrega su capa en `crm.*`.
Primeras tablas sugeridas: `crm.leads`, `crm.notas`, `crm.interacciones`.

## Publicar (lo hace May sola)
Es un proyecto Vercel aparte. `vercel --prod` desde esta carpeta publica SOLO el CRM (no toca la app). Cargá
`APP_DB_URL` en las Environment Variables de ESE proyecto en Vercel. Para el equipo, se abre con un link desde el
panel de REBUILD (ese link lo agrega Nico cuando el CRM tenga URL).

## Reglas
- Nunca pidas ni uses la service-role key de la app (con eso sí se podría escribir). El CRM va SOLO con `may_ro`.
- Si algo necesitara **modificar** datos de la app (no solo leer), NO busques la vuelta: anotalo y que May se lo pida
  a Nico. El CRM, por diseño, no escribe sobre la app.
