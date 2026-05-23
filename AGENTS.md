# Reglas del proyecto — Micro SaaS Inventario

## Next.js: Inicialización perezosa de clientes externos

NUNCA inicialices clientes de bases de datos o servicios externos en el scope global del módulo.

✅ Correcto: Usa `@supabase/ssr` con `createBrowserClient()` / `createServerClient()`
✅ Correcto: Usa hooks dentro del ciclo de vida del componente (`useEffect`, `useState`)
❌ Incorrecto: `export const supabase = createClient(url, key)` a nivel de módulo
❌ Incorrecto: Lazy singletons caseros (`getSupabaseClient()`)

**Motivo:** Next.js ejecuta SSG durante `next build`. Si el cliente se crea al importar el módulo y las variables de entorno no existen aún, el build falla con errores como `supabaseUrl is required`.

**Ubicación estándar de los clientes:**
- Cliente browser: `src/utils/supabase/client.ts` → `createBrowserClient()`
- Cliente servidor: `src/utils/supabase/server.ts` → `createServerClient()` + `cookies()`
