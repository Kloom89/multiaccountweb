# BrainStorm — Especificación Funcional

> **Versión:** 0.1 (borrador para validar)
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

### ✅ Qué SÍ va a tener (incluido en el producto)
- Registro e inicio de sesión (email + "Entrar con GitHub").
- Multiusuario con **aislamiento total de datos** (cada uno ve sólo lo suyo).
- Gestión de **Proyectos** con estado, tipo, stack, fechas y links.
- Organización con **Áreas/Carpetas** + **Etiquetas**.
- **Tareas** por proyecto (pendiente / en curso / hecha, prioridad, vencimiento).
- **Bitácora de actividad** por proyecto (timeline de todo lo que pasó).
- **Enlaces** guardados por proyecto (repo, web, deploy, docs, etc.).
- **Bóveda de credenciales cifrada** — **solo para el Owner** (apagada para el resto).
- **Integración con Claude** vía **API + servidor MCP remoto** (lee y escribe).
- **Integración con GitHub** (importar repos, traer último commit/actividad).
- **Resumen automático con IA** (digest de avances) — función **Pro**.
- **PWA instalable** (funciona en celular, tablet y compu; offline básico).
- **Planes Free / Pro / Equipos** con cobro (Stripe).
- Panel de **Admin** para el dueño del SaaS.

### ❌ Qué NO va a tener (al menos en v1, para no irnos de alcance)
- Apps nativas en tiendas (Android/iOS) — se evalúa **después** del MVP.
- Bóveda de contraseñas para usuarios que no sean el Owner (decisión deliberada por seguridad/legal).
- Chat de IA conversacional dentro de la app (la IA solo genera resúmenes/automatiza).
- Edición colaborativa en tiempo real estilo Google Docs.
- Gestión de facturación fiscal/contable, time-tracking por horas, CRM de clientes.
- Almacenamiento de archivos pesados (subir videos/imágenes grandes) — solo links.

---

## 4. Qué se puede subir / guardar y qué no

### Se puede guardar
- Texto: nombres, descripciones, notas, tareas, etiquetas, áreas.
- **Enlaces (URLs)**: repos, webs, deploys, documentos, paneles.
- Metadatos de proyecto: estado, tipo, stack, fechas, prioridad.
- **Credenciales (solo Owner)**: usuario + secreto **cifrado** + notas.
- Eventos/actividad generados por Claude, GitHub o manualmente.

### NO se sube / NO se guarda
- Archivos binarios grandes (videos, ZIPs, imágenes pesadas) → se guardan como **link** a su ubicación real.
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
| Invitar miembros | ✅ | ❌ | Según rol | ✅ |
| Ver métricas globales del SaaS | ❌ | ❌ | ❌ | ✅ |

---

## 6. Planes (modelo de venta)

| | **Free** | **Pro** (mensual) | **Equipos** (por miembro) |
|---|---|---|---|
| Proyectos | Hasta 3 | Ilimitados | Ilimitados |
| Áreas y etiquetas | Básico | Completo | Completo |
| Tareas y bitácora | ✅ | ✅ | ✅ |
| Integración con Claude (MCP/API) | ❌ | ✅ | ✅ |
| Integración GitHub | 1 repo | Ilimitado | Ilimitado |
| Resumen IA | ❌ | ✅ | ✅ |
| Proyectos compartidos / miembros | ❌ | ❌ | ✅ |
| Bóveda de credenciales | Solo Owner | Solo Owner | Solo Owner |

> **Cobro:** se diseña ahora; la conexión real con **Stripe** se implementa en la fase de monetización. Precios a definir (sugerencia inicial: Free $0, Pro ~$7–9/mes, Equipos ~$5–7/miembro/mes).

---

## 7. Pantallas (mapa completo de la app)

**Total: ~14 pantallas principales** (algunas con sub-secciones/tabs).

### Públicas (sin login)
1. **Landing / Marketing** — qué es, beneficios, planes, CTA de registro.
2. **Login / Registro** — email+contraseña y "Entrar con GitHub".
3. **Recuperar contraseña**.

### Onboarding (primer ingreso)
4. **Setup inicial** — crear primera Área, (opcional) conectar GitHub, generar la **API key / conectar MCP** con instrucciones paso a paso.

### App (con login)
5. **Dashboard / Tablero global** — vista **Kanban** por estado + vista **Lista**; filtros por área, etiqueta y estado; buscador.
6. **Detalle de Proyecto** — con tabs:
   - *Resumen* (estado, descripción, stack, fechas, links destacados)
   - *Tareas* (checklist con estados y prioridad)
   - *Enlaces*
   - *Credenciales* (solo Owner)
   - *Bitácora / Actividad* (timeline: Claude, GitHub, manual)
   - *Integraciones* (repo vinculado, deploy)
7. **Tareas globales** — todas las tareas pendientes de todos los proyectos, ordenables por prioridad/vencimiento.
8. **Áreas / Carpetas** — crear, renombrar, mover proyectos.
9. **Bóveda de credenciales** (solo Owner) — listado global cifrado, con buscador.
10. **Integraciones / Conexiones** — GitHub, deploys, y gestión de **API keys / MCP** (crear, revocar, ver scopes).
11. **Resúmenes IA** — digest semanal/por proyecto: "qué avanzaste, qué falta".
12. **Ajustes de cuenta** — perfil, seguridad (contraseña, 2FA futuro), sesiones activas, idioma.
13. **Planes y facturación** — ver plan, cambiar, historial de pagos (Stripe).
14. **Equipos** (plan Teams) — miembros, invitaciones, roles, proyectos compartidos.

### Extra
15. **Admin del SaaS** (solo Admin) — usuarios, métricas de uso, soporte.

---

## 8. Modelo de datos (entidades principales)

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
                  prioridad (baja/media/alta), vencimiento, asignado_a
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

## 9. Integración con Claude (el corazón del producto)

**Objetivo:** que en cualquier sesión de Claude (Desktop, Code o web/celular) puedas decir *"cuando termines esta tarea, actualizá el proyecto en BrainStorm"* y se haga solo.

**Cómo se logra:**
- BrainStorm expone un **servidor MCP remoto con login (OAuth)** → compatible con Claude Desktop, Claude Code y Claude.ai web/móvil **con una sola integración**.
- También una **API REST** con **API keys** para automatizaciones/scripts.

**Herramientas (tools) que Claude podrá usar vía MCP:**
- `list_projects` / `get_project` — leer estado actual.
- `create_project` — crear un proyecto.
- `update_project` — cambiar estado, descripción, stack.
- `list_tasks` / `add_task` / `complete_task` — gestionar tareas.
- `add_link` — guardar una URL.
- `log_activity` — registrar un avance en la bitácora ("terminé X").
- `get_pending` — "¿qué tengo pendiente?" (para retomar contexto).
- `add_credential` *(solo Owner)* — guardar/leer un secreto cifrado.

**Seguridad de la conexión:** cada token tiene **scopes** (lectura/escritura) y se puede **revocar**. Toda acción de Claude queda registrada en la bitácora con fuente `claude`.

---

## 10. Integración con GitHub

- "Conectar GitHub" vía OAuth (el mismo login sirve de base).
- Listar repos e **importarlos como proyectos** con un click.
- Sincronizar **último commit, ramas y actividad reciente** → se vuelca en la bitácora del proyecto.
- (Fase posterior) Webhooks para que un push actualice el proyecto **en tiempo real**.

---

## 11. Integración con Deploys (fase posterior)

- Conectar Vercel/Netlify/Play Store para reflejar **estado de despliegue/publicación** en el proyecto.
- Se especifica ahora, se construye después del MVP.

---

## 12. Resumen automático con IA (función Pro)

- Usa la **API de Claude** para generar un **digest** (semanal o on-demand) que junta notas + tareas completadas + commits y produce: *"Esta semana avanzaste en A y B; C sigue pausado; lo próximo sería D."*
- Tiene costo por uso → por eso es **Pro**.
- Configurable por proyecto o global.

---

## 13. Seguridad y privacidad (crítico porque se vende)

- **Aislamiento de datos** a nivel base (Row Level Security): cada usuario/equipo accede sólo a lo suyo.
- **Bóveda de credenciales:** cifrado fuerte (envelope encryption con clave gestionada). Disponible **solo para el Owner**; apagada por defecto para el resto.
  - ⚠️ *Trade-off a decidir:* para que **Claude pueda leer** una credencial y usarla, el servidor debe poder descifrarla (cifrado del lado servidor con clave gestionada). Si en cambio querés cifrado de "conocimiento cero" (ni el servidor puede leer), Claude **no** podría leerlas. **Recomendación:** cifrado del lado servidor con clave en gestor de secretos (KMS), porque tu caso de uso necesita que Claude las lea.
- **Tokens/API keys:** se guardan como hash; se muestran una sola vez; revocables.
- **Pagos:** 100% vía Stripe; no almacenamos datos de tarjeta.
- **Política de privacidad** y **Términos** actualizados (ya tenés base en el repo).
- 2FA y registro de auditoría: deseable, fase posterior.

---

## 14. Requisitos no funcionales

- **PWA instalable** (Android/iPhone/desktop), responsive, con caché offline básico.
- **Sin fronteras de dispositivo:** todo en la nube, accesible desde cualquier navegador.
- Rendimiento: cargas < 1s en vistas principales (datos cacheados).
- Idioma: **Español** primero; arquitectura lista para multi-idioma.
- Disponibilidad y backups automáticos de la base.

---

## 15. Stack técnico propuesto

| Pieza | Elección | Por qué |
|---|---|---|
| Frontend / PWA | **Next.js + TypeScript + Tailwind** | Una sola app, PWA fácil, gran ecosistema |
| Auth + Base de datos | **Supabase** (Postgres + Auth + RLS) | Login multiusuario y aislamiento casi gratis; GitHub OAuth de doble uso |
| API + MCP | **Node/TypeScript** (API REST + servidor MCP remoto con OAuth) | Reusa el mismo backend |
| Cifrado credenciales | **KMS / clave gestionada** + envelope encryption | Seguridad correcta para vender |
| GitHub | API oficial de GitHub | Importar/sincronizar |
| IA | **API de Claude** | Resúmenes |
| Pagos | **Stripe** | Estándar SaaS |
| Hosting | **Vercel** (front/API) + Supabase (datos) | Deploy simple, escala |

---

## 16. Roadmap por fases (siempre dejando algo usable)

**Fase 0 — Cimientos**
- Estructura del proyecto Next.js + Supabase, auth (email + GitHub), PWA básica, landing.

**Fase 1 — MVP "cerebro manual"**
- Proyectos (CRUD), Áreas, Etiquetas, Tareas, Enlaces, Bitácora, Dashboard Kanban/Lista. *(Usable de punta a punta.)*

**Fase 2 — Conexión con Claude (diferenciador clave)**
- API REST + **servidor MCP remoto** + API keys + pantalla de Integraciones. Claude lee y actualiza.

**Fase 3 — GitHub**
- Conectar GitHub, importar repos, sincronizar actividad → bitácora.

**Fase 4 — Bóveda de credenciales (Owner)**
- Cifrado del lado servidor, pantalla de bóveda, tool MCP de credenciales.

**Fase 5 — Monetización**
- Planes Free/Pro/Equipos, límites, **Stripe**, facturación.

**Fase 6 — IA + Equipos + pulido**
- Resúmenes con Claude, plan Equipos (miembros/roles), deploys, 2FA, métricas Admin.

---

## 17. Preguntas abiertas / a confirmar

1. **Precios concretos** de Pro y Equipos (puse una sugerencia en §6).
2. **Dominio** para el producto (ej. brainstorm.app / usebrainstorm.com…).
3. **Cifrado de credenciales:** ¿confirmás "lado servidor" para que Claude pueda leerlas? (recomendado en §13).
4. ¿La **bóveda** debería poder vincular credenciales a un proyecto, o también credenciales sueltas/globales? (la spec asume ambas).
5. ¿Querés **vencimientos/recordatorios** de tareas con notificaciones push desde la PWA? (asumido como deseable, fase posterior).
6. Para Equipos: ¿roles solo *admin/editor/lector* o algo más fino?

---

*Este documento se irá actualizando a medida que validemos cada punto. La idea es que sea la referencia única antes de escribir una sola línea de código de la app.*
