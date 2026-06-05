# Brief de build — Fulfillment "Los Tres Pilares del Analista de Datos"
### Para construir con Claude Code · ARKOS PRESS

---

## 0. Qué estamos construyendo y por qué

Un sitio en **Vercel** (mismo dominio que ya usás: `erickemmanueldorado.vercel.app`) con tres páginas que resuelven el fulfillment del libro, que se vende en **Gumroad**:

1. **Landing** — página pública de venta. Manda a comprar a Gumroad.
2. **Área de miembros** — el "hogar" post-compra del lector.
3. **Página de recursos** — vive dentro del área de miembros; es donde están los archivos descargables. Es el destino del QR del libro.

**Flujo completo a lograr (esto es lo que NO puede fallar):**
```
Compra en Gumroad  →  email automático con el acceso  →  área de miembros  →  página de recursos  →  descargas funcionando
```

> Regla de oro de la hoja de ruta: **hacé una compra de prueba real antes de lanzar.** Si el email no llega o un archivo no abre = reembolsos y chargebacks.

---

## 1. Stack recomendado (decile esto a Claude Code)

- **Framework:** Next.js (App Router) o HTML/CSS/JS plano si querés algo ultra simple. Next.js te conviene porque ya estás en Vercel.
- **Hosting:** Vercel.
- **Estilos:** Tailwind. Usá la paleta de marca ARKOS (cyan / negro / azul profundo) para que matchee la portada del libro y no repitas la deb. 16.
- **Sin base de datos** para el MVP. Todo estático.

---

## 2. Cómo proteger el acceso (elegí un nivel)

**Nivel 1 — MVP (recomendado para lanzar ya):**
La página de recursos vive en una URL difícil de adivinar (ej. `/recursos-a8f3k2`). Gumroad la entrega solo a quien compró, en el email automático y en la pantalla post-compra. No es seguridad militar, pero para un ebook de bajo precio es el estándar y es suficiente para arrancar.

**Nivel 2 — Upgrade (cuando tengas volumen):**
Gumroad genera *license keys* por compra. Una serverless function en Vercel (`/api/verify`) valida la key contra la API de Gumroad antes de mostrar los links de descarga. Más trabajo, mejor protección. Dejalo para después del lanzamiento.

> Empezá por el Nivel 1. No sobre-ingenierices el primer lanzamiento.

---

## 3. Configuración en Gumroad (esto NO es código, lo hacés vos en Gumroad)

1. En el producto → **Content / "Contenido posterior a la compra"**: poné un botón/enlace grande a la URL del área de miembros.
2. En **Receipt / email automático**: que incluya el mismo enlace, bien visible, con una línea tipo *"Tu acceso está acá 👇"*.
3. Activá el email de recibo automático de Gumroad (viene por defecto, confirmá que esté ON).
4. **Importante:** NO pongas datos sensibles en la URL. El enlace es el acceso, nada más.

---

## 4. Página 1 — LANDING (pública)

**Objetivo:** que un desconocido entienda en 10 segundos qué es, para quién es, y haga clic a comprar.

**Secciones y copy (en voz rioplatense, editá a gusto):**

- **Hero**
  - Título: *Los Tres Pilares del Analista de Datos*
  - Subtítulo: *El libro que te enseña lo que ningún curso enseña: pensar, comunicar y entender el negocio. Para los que vienen de cero o en transición.*
  - Botón: **Conseguir el libro** → link a Gumroad.

- **El problema** (conecta con el dolor del lector)
  - *¿Acumulaste certificados de SQL, Python y Power BI y seguís sin conseguir tu primer trabajo de analista? El problema no son las herramientas. Es todo lo que viene después.*

- **Qué vas a aprender** (3 pilares)
  - Pilar 1 — Mentalidad: pensar como analista.
  - Pilar 2 — Soft skills: comunicar hallazgos que generan decisiones.
  - Pilar 3 — Contexto de negocio: entender qué le importa a cada industria.

- **Qué incluye**
  - El libro completo (3 pilares + 3 casos reales).
  - Cuaderno de trabajo con 18 ejercicios prácticos.
  - Kit de recursos: dataset CSV, queries SQL, dashboard Power BI (.pbix), plantillas y 25 prompts de IA.

- **Sobre el autor** (usá la bio honesta que ya tenés)

- **Testimonios** (dejá los slots vacíos; los llenás con los de tus lectores fundadores)

- **CTA final** + botón a Gumroad.

---

## 5. Página 2 — ÁREA DE MIEMBROS (post-compra)

**Objetivo:** que el comprador sienta que llegó a un lugar ordenado y sepa exactamente qué hacer.

**Contenido:**
- **Bienvenida:** *¡Bienvenido/a! Ya sos parte. Acá tenés todo lo que viene con el libro.*
- **Tarjetas de navegación:**
  1. 📥 **Recursos descargables** → link a la página de recursos.
  2. 📖 **Cómo usar el libro** → breve guía de por dónde empezar (cruza con deb. 18).
  3. ✉️ **Contacto / soporte** → tu email o `erickemmanueldorado.vercel.app`.
- **Nota de "lectores fundadores"** (mientras dure esa etapa): *Si entraste como lector fundador, te pido dos cosas: contame qué te falló (si algo falló) y, si te sirvió, dejame un testimonio. Eso ayuda a que esto crezca.*

---

## 6. Página 3 — RECURSOS (dentro del área de miembros · destino del QR)

**Objetivo:** que cada archivo prometido en el libro esté acá y se descargue sin fricción.

**Lista de recursos (cada uno con su botón de descarga):**

| Recurso | Archivo | Nota |
|---|---|---|
| Dataset del caso EcomShop | `ecomshop_pedidos.csv` | 1.000 filas (la muestra del libro) |
| Queries SQL | `queries.sql` | Las del análisis de logística |
| Dashboard Power BI | `dashboard_ecomshop.pbix` | El de control operativo |
| Plantillas | `plantillas.zip` | One-pager, storytelling 5 pasos, etc. |
| 25 prompts de IA | `25_prompts_ia.pdf` | Los de la sección 4.3 |

- **Hosting de los archivos:** subilos a `/public` en Vercel, o a un bucket (Vercel Blob / S3). Para el MVP, `/public` alcanza.
- **Verificá que cada archivo abra** en una máquina limpia antes de lanzar (el .pbix necesita Power BI Desktop; aclaralo al lado del botón).
- Encabezado: *Estos son los recursos del libro. Descargalos y practicá con datos reales — es la diferencia entre leer y saber.*

---

## 7. Checklist de build (orden sugerido para Claude Code)

1. [ ] Scaffold del proyecto Next.js + Tailwind en Vercel.
2. [ ] Paleta de marca ARKOS (cyan / negro / azul profundo) como design tokens.
3. [ ] Landing con todas las secciones del punto 4.
4. [ ] Área de miembros (punto 5) en URL difícil de adivinar.
5. [ ] Página de recursos (punto 6) con los 5 archivos.
6. [ ] Subir los archivos reales y testear cada descarga.
7. [ ] Deploy a Vercel.
8. [ ] Configurar Gumroad (punto 3) con la URL del área de miembros.
9. [ ] **PRUEBA REAL:** comprar tu propio producto en Gumroad → confirmar que el email llega → entrar al área → descargar los 5 archivos → abrir cada uno.
10. [ ] Recién ahí, dar deb. 1 por cerrado.

---

## 8. Email automático post-compra (texto para pegar en Gumroad)

> **Asunto:** Tu acceso a Los Tres Pilares del Analista de Datos 🎉
>
> ¡Gracias por confiar! Ya tenés todo listo.
>
> 👉 Entrá a tu área de miembros acá: **[URL del área de miembros]**
>
> Ahí vas a encontrar el kit de recursos completo: el dataset, las queries SQL, el dashboard de Power BI, las plantillas y los 25 prompts de IA.
>
> Empezá por la guía "Cómo usar el libro" y arrancá con el caso EcomShop.
>
> Si algo no abre o tenés una duda, respondé este mail.
>
> — Erick
