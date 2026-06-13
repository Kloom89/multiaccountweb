# BrainStorm — Especificación Funcional

> **Versión:** 0.2 (borrador para validar)
> **Fecha:** 13 de junio de 2026
> **Autor / Owner:** matu30@gmail.com
> **Estado:** En definición — este documento es la fuente de verdad del producto.

---

## 1. Visión en una frase

**BrainStorm es "tu segundo cerebro" para proyectos: un panel web (instalable como app) donde vive TODO sobre lo que estás construyendo —estado, tareas, enlaces, notas y credenciales— y que Claude lee y actualiza solo, en segundo plano, cada vez que avanza en algo.**

El problema que resuelve: hoy la información de tus proyectos está dispersa (chats de Claude, repos, notas sueltas) y tenés que *recordar* en qué estaba cada cosa. BrainStorm centraliza eso y lo mantiene actualizado automáticamente.

---

## 2. Para quién es

| Tipo de usuario | Descripción |
|---|---|
| **Owner (vos)** | Dueño de la instancia. Acceso completo, incluida la **bóveda de credenciales cifrada**. |
| **Usuario individual** | Cliente que paga (o usa el plan Free) para gestionar sus propios proyectos. **Sin** bóveda de contraseñas. |
| **Miembro de equipo** | Usuario dentro de un Equipo (plan Teams) que comparte proyectos con otros. |
| **Admin del SaaS** | Vos (u otro), para ver métricas, usuarios y soporte. |

---

## 3. Alcance — Qué SÍ tiene y qué NO tiene

### ✅ Qué SÍ va a tener
- Registro e inicio de sesión (email + "Entrar con GitHub").
- Multiusuario con **aislamiento total de datos** (cada uno ve sólo lo suyo).
- Gestión de **Proyectos** con estado, tipo, stack, fechas y links.
- Organización con **Áreas/Carpetas** + **Etiquetas**.
- **Tareas** con estado, prioridad, vencimiento y **estimación de tiempo/esfuerzo**.
- **Plantillas de tareas reutilizables** (checklists base que aplicás a cualquier proyecto).
- **ETA agregado**: tiempo aproximado para terminar todas las tareas pendientes.
- **Bitácora de actividad** por proyecto (timeline de todo lo que pasó).
- **Enlaces** guardados por proyecto (repo, web, deploy, docs, etc.).
- **Bóveda de credenciales cifrada** — **solo para el Owner** (apagada para el resto).
- **Integración con Claude** vía **API + servidor MCP remoto** (lee y escribe).
- **Integración con GitHub** (importar repos, traer último commit/actividad).
- **Resumen automático con IA** (digest de avances) — función **Pro**.
- **Dashboard con gráficos** (progreso, actividad, distribución, números clave).
- **Tema oscuro (dark)** estilo neón futurista + corporativo.
- **Bilingüe Español / Inglés** con selector y auto-detección.
- **PWA instalable** (celular, tablet y compu; offline básico).
- **Planes Free / Pro / Equipos** con cobro (Stripe).
- Panel de **Admin** para el dueño del SaaS.

### ❌ Qué NO va a tener (al menos en v1)
- Apps nativas en tiendas (Android/iOS) — se evalúa **después** del MVP.
- Tema claro (el producto es **dark-only** por decisión de diseño).
- Bóveda de contraseñas para usuarios que no sean el Owner.
- Chat de IA conversacional dentro de la app (la IA solo resume/automatiza).
- Edición colaborativa en tiempo real estilo Google Docs.
- Time-tracking por horas/facturación contable, CRM de clientes.
- Almacenamiento de archivos pesados (videos/imágenes grandes) — solo links.

---

## 4. Qué se puede subir / guardar y qué no

### Se puede guardar
- Texto: nombres, descripciones, notas, tareas, etiquetas, áreas.
- **Enlaces (URLs)**: repos, webs, deploys, documentos, paneles.
- Metadatos de proyecto: estado, tipo, stack, fechas, prioridad, estimaciones.
- **Plantillas de tareas** propias.
- **Credenciales (solo Owner)**: usuario + secreto **cifrado** + notas.
- Eventos/actividad generados por Claude, GitHub o manualmente.

### NO se sube / NO se guarda
- Archivos binarios grandes → se guardan como **link** a su ubicación real.
- Contraseñas reales de usuarios que no sean el Owner.
- Datos de tarjetas/pagos → los maneja **Stripe**, nunca tocan nuestra base.
- Código fuente completo de los repos (se referencia GitHub, no se clona).

---

## 5. Roles y permisos

| Capacidad | Owner | Usuario | Miembro Equipo | Admin SaaS |
|---|:---:|:---:|:---:|:---:|
| Crear/editar sus proyectos | ✅ | ✅ | ✅ | ✅ |
| Ver proyectos de otros usuarios | ❌ | ❌ | Solo del equipo | ✅ (soporte) |
| Bóveda de credenciales | ✅ | ❌ | ❌ | ❌ |
| Conectar Claude (MCP/API) | ✅ | Pro | Pro | ✅ |
| Resumen IA | ✅ | Pro | Pro | ✅ |
| Plantillas de tareas | ✅ | ✅ | ✅ | ✅ |
| Invitar miembros | ✅ | ❌ | Según rol | ✅ |
| Ver métricas globales del SaaS | ❌ | ❌ | ❌ | ✅ |

---

## 6. Planes (modelo de venta)

| | **Free** | **Pro** (mensual) | **Equipos** (por miembro) |
|---|---|---|---|
| Proyectos | Hasta 3 | Ilimitados | Ilimitados |
| Áreas y etiquetas | Básico | Completo | Completo |
| Tareas, plantillas y bitácora | ✅ | ✅ | ✅ |
| Gráficos del dashboard | Básicos | Completos | Completos |
| Integración con Claude (MCP/API) | ❌ | ✅ | ✅ |
| Integración GitHub | 1 repo | Ilimitado | Ilimitado |
| Resumen IA | ❌ | ✅ | ✅ |
| Proyectos compartidos / miembros | ❌ | ❌ | ✅ |
| Bóveda de credenciales | Solo Owner | Solo Owner | Solo Owner |

> **Cobro:** se diseña ahora; la conexión real con **Stripe** se implementa en la fase de monetización. Precios sugeridos iniciales: Free $0, Pro ~$7–9/mes, Equipos ~$5–7/miembro/mes (a confirmar).

---

## 7. Pantallas (mapa completo de la app)

**Total: ~15 pantallas principales** (algunas con sub-secciones/tabs).

### Públicas (sin login)
1. **Landing / Marketing** — qué es, beneficios, planes, CTA de registro.
2. **Login / Registro** — email+contraseña y "Entrar con GitHub".
3. **Recuperar contraseña**.

### Onboarding (primer ingreso)
4. **Setup inicial** — crear primera Área, (opcional) conectar GitHub, generar la **API key / conectar MCP** con instrucciones paso a paso.

### App (con login)
5. **Dashboard / Tablero global** — vista **Kanban** + **Lista**, filtros por área/etiqueta/estado, buscador, y la **fila de gráficos** (ver §8.5).
6. **Detalle de Proyecto** — con tabs:
   - *Resumen* (estado, descripción, stack, fechas, progreso, ETA, links destacados)
   - *Tareas* (checklist con estados, prioridad y estimación; aplicar plantilla)
   - *Enlaces*
   - *Credenciales* (solo Owner)
   - *Bitácora / Actividad* (timeline: Claude, GitHub, manual)
   - *Integraciones* (repo vinculado, deploy)
7. **Tareas globales** — todas las tareas pendientes de todos los proyectos, ordenables por prioridad/vencimiento/estimación.
8. **Plantillas de tareas** — crear/editar checklists reutilizables y aplicarlas a proyectos.
9. **Áreas / Carpetas** — crear, renombrar, mover proyectos.
10. **Bóveda de credenciales** (solo Owner) — listado global cifrado, con buscador.
11. **Integraciones / Conexiones** — GitHub, deploys, y gestión de **API keys / MCP** (crear, revocar, ver scopes).
12. **Resúmenes IA** — digest semanal/por proyecto: "qué avanzaste, qué falta".
13. **Ajustes de cuenta** — perfil, seguridad, sesiones activas, **idioma (ES/EN)**, tema.
14. **Planes y facturación** — ver plan, cambiar, historial de pagos (Stripe).
15. **Equipos** (plan Teams) — miembros, invitaciones, roles, proyectos compartidos.

### Extra
16. **Admin del SaaS** (solo Admin) — usuarios, métricas de uso, soporte.

---

## 8. Diseño / UX visual

### 8.1 Modo de tema
- **Solo oscuro (dark-only).** Sin tema claro. Identidad visual fuerte y consistente.

### 8.2 Estilo / personalidad
- **Mezcla de "neón futurista" + "corporativo".** La base es sobria, ordenada y legible (jerarquía clara, grilla prolija, buena tipografía, tipo dashboard pro); encima se aplican **acentos neón** para que no sea aburrido.
- **Regla de oro de legibilidad:** el neón es **acento**, nunca fondo. Brilla en botones activos, hover, bordes de tarjetas seleccionadas, indicadores y **gráficos**; el texto siempre va en alto contraste sobre superficies oscuras y planas.

### 8.3 Paleta de color
- **Fondo:** casi negro / gris muy oscuro (ej. `#0B0E14`). Superficies elevadas un punto más claras (ej. `#141925`).
- **Texto:** alto contraste (blanco ~`#E6EAF2` para principal, gris claro para secundario) — cumpliendo contraste AA.
- **Acento primario:** **Cyan** (ej. `#22D3EE`) — botones, links, foco, líneas de gráficos.
- **Acento secundario:** **Magenta** (ej. `#E879F9` / `#FF4DCA`) — destacados, degradados, segundo color de gráficos.
- **Degradado de marca:** **cyan → magenta** para títulos clave, barras de progreso y bordes con glow.
- **Estados:** verde (ok/hecho), ámbar (advertencia/vencimiento), rojo (error/atrasado).

### 8.4 Tipografía y legibilidad
- Sans moderna y muy legible (ej. **Inter**), tamaños cómodos e interlineado generoso.
- Glow/neón sólo en acentos y elementos interactivos, **nunca** en bloques largos de texto.
- Todo **responsive**: se ve y funciona bien en celular (los gráficos se adaptan/colapsan).

### 8.5 Gráficos del dashboard (las visualizaciones que pediste)
1. **Tarjetas de números clave** (arriba): proyectos activos, tareas pendientes, lanzados este mes, etc.
2. **Progreso por proyecto:** barras de % completado (tareas hechas vs. pendientes), con degradado cyan→magenta.
3. **Actividad en el tiempo:** línea o **mapa de calor estilo GitHub** del avance por día/semana (racha).
4. **Distribución por estado/área:** dona/torta de cuántos proyectos hay en cada estado o área.
5. **Pendientes:** lista/contador de tareas abiertas priorizadas.
6. **ETA global:** **tiempo aproximado para terminar todas las tareas**, calculado sumando las estimaciones de las tareas pendientes (y, a futuro, ajustado por tu ritmo histórico).

### 8.6 Idiomas
- **Español (por defecto)** + **Inglés**, con **selector** en Ajustes y **auto-detección** según el navegador. Arquitectura i18n lista para sumar más idiomas.

---

## 9. Modelo de datos (entidades principales)

```
User            → identidad/login (gestionado por el proveedor de auth)
Profile         → user_id, rol (owner/user/admin), plan, idioma, fecha_alta
Team            → nombre, owner_id  (plan Equipos)
Membership      → team_id, user_id, rol (admin/editor/lector)
Area            → user_id|team_id, nombre, color, orden
Project         → owner (user/team), area_id, nombre, descripción, tipo
                  (web/app/script/otro), estado (idea/en_construccion/
                  lanzado/pausado/archivado), stack[], fecha_inicio,
                  fecha_actualizacion
Tag             → user_id|team_id, nombre, color
ProjectTag      → project_id, tag_id
Task            → project_id, título, estado (todo/doing/done),
                  prioridad (baja/media/alta), vencimiento, asignado_a,
                  estimacion (tiempo/esfuerzo), origen_template_id?
TaskTemplate    → user_id|team_id, nombre, descripción
TemplateItem    → template_id, título, prioridad, estimacion, orden
Link            → project_id, etiqueta, url, tipo (repo/web/deploy/doc/otro)
Credential      → SOLO OWNER. project_id|null, servicio, usuario,
                  secreto_cifrado, notas_cifradas
Activity        → project_id, fuente (claude/github/manual/deploy),
                  tipo, mensaje, payload(json), creado_en
Integration     → user_id, proveedor (github/vercel/...), tokens_cifrados,
                  estado
ApiKey/McpToken → user_id, nombre, hash, scopes[], último_uso, creado_en
Subscription    → user_id|team_id, plan, stripe_customer, stripe_sub,
                  estado, vence_en
AiSummary       → user_id, alcance (global/proyecto), período, contenido,
                  generado_en
```

---

## 10. Integración con Claude (el corazón del producto)

**Objetivo:** que en cualquier sesión de Claude (Desktop, Code o web/celular) puedas decir *"cuando termines esta tarea, actualizá el proyecto en BrainStorm"* y se haga solo.

**Cómo se logra:**
- BrainStorm expone un **servidor MCP remoto con login (OAuth)** → compatible con Claude Desktop, Claude Code y Claude.ai web/móvil **con una sola integración**.
- También una **API REST** con **API keys** para automatizaciones/scripts.

**Herramientas (tools) que Claude podrá usar vía MCP:**
- `list_projects` / `get_project` — leer estado actual.
- `create_project` / `update_project` — crear y actualizar proyectos.
- `list_tasks` / `add_task` / `complete_task` — gestionar tareas.
- `apply_template` — aplicar una plantilla de tareas a un proyecto.
- `add_link` — guardar una URL.
- `log_activity` — registrar un avance en la bitácora ("terminé X").
- `get_pending` — "¿qué tengo pendiente?" (para retomar contexto).
- `add_credential` *(solo Owner)* — guardar/leer un secreto cifrado.

**Seguridad:** cada token tiene **scopes** (lectura/escritura) y se puede **revocar**. Toda acción de Claude queda registrada en la bitácora con fuente `claude`.

---

## 11. Integración con GitHub

- "Conectar GitHub" vía OAuth (el mismo login sirve de base).
- Listar repos e **importarlos como proyectos** con un click.
- Sincronizar **último commit, ramas y actividad reciente** → bitácora del proyecto.
- (Fase posterior) Webhooks para que un push actualice el proyecto **en tiempo real**.

---

## 12. Integración con Deploys (fase posterior)

- Conectar Vercel/Netlify/Play Store para reflejar **estado de despliegue/publicación**.
- Se especifica ahora, se construye después del MVP.

---

## 13. Resumen automático con IA (función Pro)

- Usa la **API de Claude** para generar un **digest** (semanal o on-demand): junta notas + tareas completadas + commits y produce *"Esta semana avanzaste en A y B; C sigue pausado; lo próximo sería D."*
- Tiene costo por uso → por eso es **Pro**. Configurable por proyecto o global.

---

## 14. Plantillas de tareas y estimación de tiempo

- **Plantillas reutilizables:** definís un checklist base (ej. "Lanzar app: política de privacidad, ícono, screenshots, ficha de tienda, build firmado…") y lo **aplicás a cualquier proyecto** con un click (o vía Claude con `apply_template`). Sirve para estandarizar y automatizar lo repetitivo.
- **Estimación por tarea:** cada tarea puede llevar una estimación de tiempo/esfuerzo (horas o "puntos", a confirmar).
- **ETA global:** el dashboard suma las estimaciones de las tareas pendientes y muestra el **tiempo aproximado para terminar todo**; a futuro se ajusta por tu velocidad histórica real.

---

## 15. Seguridad y privacidad (crítico porque se vende)

- **Aislamiento de datos** a nivel base (Row Level Security): cada usuario/equipo accede sólo a lo suyo.
- **Bóveda de credenciales:** cifrado fuerte (envelope encryption con clave gestionada). Disponible **solo para el Owner**; apagada por defecto para el resto.
  - ⚠️ *Trade-off a decidir:* para que **Claude pueda leer** una credencial, el servidor debe poder descifrarla (cifrado del lado servidor con clave gestionada / KMS). Si se usara cifrado de "conocimiento cero", Claude **no** podría leerlas. **Recomendación:** cifrado del lado servidor con KMS, porque tu caso de uso necesita que Claude las lea.
- **Tokens/API keys:** se guardan como hash; se muestran una sola vez; revocables.
- **Pagos:** 100% vía Stripe; no almacenamos datos de tarjeta.
- **Política de privacidad** y **Términos** actualizados (ya hay base en el repo).
- 2FA y registro de auditoría: deseable, fase posterior.

---

## 16. Requisitos no funcionales

- **PWA instalable** (Android/iPhone/desktop), responsive, con caché offline básico.
- **Sin fronteras de dispositivo:** todo en la nube, accesible desde cualquier navegador.
- Rendimiento: cargas < 1s en vistas principales (datos cacheados).
- **Idiomas:** Español (default) + Inglés con selector; listo para multi-idioma.
- **Tema dark-only** con la paleta de §8.3 y contraste AA.
- Disponibilidad y backups automáticos de la base.

---

## 17. Stack técnico propuesto

| Pieza | Elección | Por qué |
|---|---|---|
| Frontend / PWA | **Next.js + TypeScript + Tailwind** | Una sola app, PWA fácil, gran ecosistema |
| UI / estilos | **Tailwind + componentes propios** (paleta neón cyan/magenta) | Control total del look dark futurista |
| Gráficos | **Recharts / Visx** (u opción liviana) | Charts responsive con degradados |
| i18n | **next-intl / i18next** | ES/EN con selector y auto-detección |
| Auth + Base de datos | **Supabase** (Postgres + Auth + RLS) | Login multiusuario y aislamiento casi gratis |
| API + MCP | **Node/TypeScript** (API REST + MCP remoto con OAuth) | Reusa el mismo backend |
| Cifrado credenciales | **KMS / clave gestionada** + envelope encryption | Seguridad correcta para vender |
| GitHub / IA / Pagos | API de GitHub · API de Claude · **Stripe** | Integraciones núcleo |
| Hosting | **Vercel** (front/API) + Supabase (datos) | Deploy simple, escala |

---

## 18. Roadmap por fases (siempre dejando algo usable)

**Fase 0 — Cimientos:** Next.js + Supabase, auth (email + GitHub), PWA, **tema dark + i18n ES/EN**, landing.

**Fase 1 — MVP "cerebro manual":** Proyectos (CRUD), Áreas, Etiquetas, Tareas (con estimación), **Plantillas**, Enlaces, Bitácora, **Dashboard con gráficos + ETA**. *(Usable de punta a punta.)*

**Fase 2 — Conexión con Claude:** API REST + **servidor MCP remoto** + API keys + pantalla de Integraciones.

**Fase 3 — GitHub:** conectar, importar repos, sincronizar actividad → bitácora.

**Fase 4 — Bóveda de credenciales (Owner):** cifrado lado servidor, pantalla de bóveda, tool MCP.

**Fase 5 — Monetización:** planes Free/Pro/Equipos, límites, **Stripe**.

**Fase 6 — IA + Equipos + pulido:** resúmenes con Claude, plan Equipos, deploys, 2FA, métricas Admin.

---

## 19. Preguntas abiertas / a confirmar

1. **Precios concretos** de Pro y Equipos (sugerencia en §6).
2. **Dominio** del producto (ej. brainstorm.app / usebrainstorm.com…).
3. **Cifrado de credenciales:** ¿confirmás "lado servidor" para que Claude pueda leerlas? (recomendado en §15).
4. **Unidad de estimación** de tareas: ¿horas o "puntos" de esfuerzo?
5. ¿La **bóveda** vincula credenciales a un proyecto, también sueltas/globales? (la spec asume ambas).
6. ¿Querés **vencimientos/recordatorios** con notificaciones push de la PWA? (asumido deseable, fase posterior).
7. Para Equipos: ¿roles solo *admin/editor/lector* o algo más fino?

---

*Este documento se irá actualizando a medida que validemos cada punto. Es la referencia única antes de escribir código de la app.*
