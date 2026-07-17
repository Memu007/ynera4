# YNERA — Biblia de marca y brief de la landing

> Documento de identidad + handoff de implementación. Todo lo que se construya
> (código, imágenes, copy) debe ser coherente con este archivo. Si algo del
> código contradice este documento, manda este documento.

---

## 1. La idea central: **la Y es la bifurcación**

Toda empresa llega a un punto donde el camino se abre en dos: **seguir
improvisando o construir un sistema**. YNERA vive exactamente en ese punto.

La letra **Y** es esa bifurcación hecha símbolo — y la leemos al revés:
**dos ramas que convergen en un solo tronco.**

- Las dos ramas son **datos + seguridad** (las dos disciplinas del estudio).
- También son **los dos founders** (uno viene de la seguridad y el código, el
  otro de la ciencia de datos).
- El tronco donde convergen es **el sistema**: una sola cosa sólida corriendo
  en producción.

De ahí sale el titular de la casa, que ya existía y ahora tiene fundamento:

> **"Tu problema tiene un sistema."**

Todo lo visual deriva de esta idea. La Y no es un logo pegado: es el
protagonista de la página. El material es **vidrio** porque el vidrio es
*claridad* (así trabaja el estudio: sin humo, se ve todo por dentro), y
refracta dos luces internas: **coral** (impulso, decisión, lo que empuja) y
**teal** (estructura, calma, lo que sostiene). El fondo es **navy profundo**:
la noche de Buenos Aires, el estudio encendido mientras el resto duerme.

---

## 2. Personalidad: **elegante con filo**

Estudio boutique premium. Habla con calma y precisión; el poder se demuestra,
no se grita.

| Sí | No |
|---|---|
| "Tres sistemas propios en producción lo respaldan." | "Consultoría hardcore" |
| "Protegemos tu operación." | "Seguridad de grado militar" |
| Frases cortas. Verbos en presente. | Jerga corporativa, buzzwords ("sinergia", "disruptivo") |
| Números y hechos concretos | Promesas infladas, superlativos vacíos |
| Silencios y espacio negro en el diseño | Ruido visual, gradientes de moda, emojis en la página |

**Prueba del filo:** cada frase debería poder decirse en voz baja en una
reunión seria y aun así imponer. Si necesita signos de exclamación, está mal.

## 3. Voz: **tuteo neutro**

- Tuteo en español neutro: *"escuchamos lo que te frena"*, *"tu operación"*.
- **Nada de voseo** (no "tenés", no "querés") — apunta a clientes regionales.
- **Nada de usted** — no somos un banco.
- Buenos Aires aparece como **sello de origen** ("Estudio de Buenos Aires"),
  no como dialecto.
- Primera persona plural: *escuchamos, diseñamos, protegemos, construimos*.

---

## 4. Sistema visual

### 4.1 Paleta

| Token | Hex | Rol |
|---|---|---|
| `--navy` | `#081826` | Fondo profundo (páginas, escenas generadas) |
| `--navy-ui` | `#0d1117` | Fondo de UI / secciones fuera del loop |
| `--ink` | `#fdfdfd` | Texto principal |
| `--ink-soft` | `#8b949e` | Texto secundario / eyebrows |
| `--coral` | `#FF6B57` | Acento primario: CTAs, palabras en *em*, energía |
| `--teal` | `#4B9B9B` | Acento secundario: estructura, detalles, hovers suaves |

Regla: **coral solo donde hay decisión** (botones, la palabra clave del
titular). Si todo es coral, nada es coral. Teal nunca en CTAs.

### 4.2 Tipografía

- **Playfair Display** (700, itálica para énfasis) → titulares únicamente.
  La palabra clave de cada titular va en `<em>` itálica coral.
- **Inter** (400/500/600) → cuerpo, UI, navegación.
- **Monospace del sistema** (ui-monospace) → eyebrows/etiquetas en mayúsculas
  con tracking `0.2em` (ej: `DATOS E INFRAESTRUCTURA`).

### 4.3 Marca gráfica

- Wordmark: **`Y N E R A`** — Inter 600, mayúsculas, tracking `0.2em`.
- Monograma: la **Y** (usar `ynera-logo.svg` existente; la versión lockup es
  `ynera-logo-lockup.svg`).
- Playfair **nunca** se usa para el logo. Solo titulares.
- La Y de vidrio (imágenes generadas) es el motivo hero; el SVG es la firma
  funcional (nav, favicon, footer).

### 4.4 Texturas de la casa

- Grano/noise fijo a opacidad ~0.03 sobre toda la página (cinematográfico).
- Grilla de puntos sutil (`radial-gradient` 1px, opacidad ~0.08) en secciones
  de UI fuera del loop.
- Polvo flotante y profundidad de campo viven **dentro de las imágenes**, no
  se simulan con CSS.

---

## 5. La página: **una historia en dos actos**

Un solo `index.html` + `assets/`. Deploy: **Railway**. Idioma: español.

### Acto 0 — Intro (animación de apertura)

Pantalla negra. **Dos hilos de luz — uno coral, uno teal — entran desde los
bordes, se buscan y convergen formando la Y.** Flash suave de vidrio, y la Y
terminada se funde con el fondo del hero (`bg-hero`). Duración total 2–2.5s,
se puede saltar con scroll/click, y **solo se muestra la primera visita**
(sessionStorage). Implementación sugerida: SVG stroke-dashoffset sobre negro
→ crossfade a la imagen hero. Nada de librerías pesadas.

La intro ES la tesis de marca en 2 segundos: dos disciplinas convergen en un
sistema.

### Acto 1 — La historia (dentro del loop de fondos)

Fondos fijos a pantalla completa que se **funden entre sí** al scrollear
(crossfade sticky). Texto sobre el tercio izquierdo (las imágenes dejan ese
espacio libre). Blur-reveal al entrar cada sección.

| # | Sección | Eyebrow | Titular (em = coral itálica) | Cuerpo | Fondo |
|---|---|---|---|---|---|
| 1 | Hero | `ESTUDIO DE BUENOS AIRES` | Tu problema tiene un *sistema.* | Datos, inteligencia artificial y seguridad. Escuchamos lo que te frena, diseñamos la solución y la dejamos corriendo en producción. | `bg-hero` |
| 2 | Datos | `DATOS E INFRAESTRUCTURA` | Infraestructura sin *fricción.* | Estructuramos tu información y eliminamos los cuellos de botella. Arquitectura de datos para empresas que no pueden fallar. | `bg-datos` |
| 3 | IA | `INTELIGENCIA ARTIFICIAL` | Decisiones, no *dashboards.* | Modelos diseñados para ejecutar, no para mirar. Convertimos tu complejidad operativa en ventaja competitiva. | `bg-ia` |
| 4 | Seguridad | `CIBERSEGURIDAD` | Protegemos tu *operación.* | Seguridad ofensiva y arquitecturas sólidas. Anticipamos las amenazas antes de que lleguen a tu negocio. | `bg-seguridad` |

### Acto 2 — El aterrizaje (fuera del loop, fondo sólido `--navy-ui`)

El vuelo termina; el negocio es en tierra firme. Scroll normal, sin fondos
cinematográficos. Transición: el último fondo (`bg-seguridad`) se funde a
`bg-finale` breve y luego a fondo sólido.

| # | Sección | Contenido |
|---|---|---|
| 5 | Productos | Eyebrow `SISTEMAS PROPIOS EN PRODUCCIÓN`. Titular: "No es teoría. Está *corriendo.*" Tres tarjetas: **Aira** (Salud · gestión clínica — agenda, historias clínicas y facturación para psicólogos y psiquiatras) [screenshot pendiente, placeholder elegante]; **CDI** (Comercio exterior · aduanas — documentos digitalizados y vencimientos bajo control) → `https://cdi-production.up.railway.app`, `cdi-preview.jpg`; **ReservaYá** (Hotelería · reservas — reservas online, check-in digital y ocupación) → `https://reservasya-ynera-production.up.railway.app`, `reservaya-preview.jpg`. |
| 6 | Contacto | Eyebrow `CONTACTO`. Titular: "Construyamos el *futuro.*" (frase genérica provisoria — se revisará). Form simple (nombre, email, mensaje) + botón coral. **Datos reales pendientes** — usar placeholders marcados `TODO-CONTACTO` (no inventar teléfonos/emails). |

**El CTA de toda la historia lleva acá.** Un solo botón primario coral
("Hablemos →") presente en nav y al final de la historia; ancla a `#contacto`.

### Navegación

Fija arriba, `mix-blend-mode: difference` o blur sutil. Izquierda: monograma
Y + `Y N E R A`. Derecha: `DATOS · IA · SEGURIDAD · SISTEMAS` + botón
`Hablemos`. En mobile: colapsa a monograma + botón.

### Reglas de implementación

- Standalone: HTML + CSS + JS vanilla. Sin frameworks ni librerías externas
  (solo Google Fonts: Playfair Display + Inter).
- Crossfade de fondos con IntersectionObserver (patrón ya validado en el
  prototipo `sticky-backgrounds` / `.bg-layer.active`).
- Mobile first en serio: los fondos 16:9 se recortan con `background-position`
  hacia la derecha (donde vive la Y); el texto pasa a ocupar todo el ancho.
- `prefers-reduced-motion`: sin intro animada, sin blur-reveals, crossfades
  instantáneos.
- SEO: title "YNERA — Datos, IA y seguridad", meta description, OG con
  `og-image.png`, favicon = monograma Y (data URI SVG ya existente).
- Performance: imágenes de fondo como JPG optimizado (~200–400 KB c/u),
  `preload` solo del hero.

---

## 6. Assets generados (los hace el usuario en Gemini)

**5 imágenes, 16:9, la mayor resolución disponible.** Flujo: generar primero
`bg-hero`; para las otras 4, adjuntar el hero como referencia y pedir "misma
escultura, mismo estilo, nuevo mood".

**Style bible (pegar antes de cada prompt, siempre igual):**

```
Cinematic 3D render of a translucent frosted-glass sculpture shaped like the
letter "Y" — the recurring hero motif of a tech brand. Deep dark navy studio
background (hex #081826), seamless gradient fading to black at the edges. The
glass refracts two internal accent colors: warm coral #FF6B57 and teal #4B9B9B.
Soft volumetric studio lighting, floating dust particles catching the light,
subtle depth of field. Elegant, premium, editorial, minimalist. COMPOSITION:
the Y sculpture is weighted to the RIGHT side of the frame; the LEFT third is
empty, darker negative space (reserved for text overlay). 16:9 widescreen.
Photorealistic glass material, high detail. No text, no logos, no watermark.
```

**Variaciones (una por imagen):**

| Archivo | Prompt adicional |
|---|---|
| `bg-hero.jpg` | The Y stands complete and pristine, monumental and calm, a thin rim of coral light along one edge. Establishing shot. |
| `bg-datos.jpg` | The Y looks constructed from a fine wireframe data-lattice and softly glowing grid lines. Teal dominant. Structured, architectural, like infrastructure. |
| `bg-ia.jpg` | Inside the glass Y, luminous neural filaments and flowing light threads pulse and branch. Coral dominant. Intelligent, alive. |
| `bg-seguridad.jpg` | The Y is encased in a crystalline armored shell of sharp faceted planes. Cooler steel-white and teal light. Protective, impenetrable, a secure vault mood. |
| `bg-finale.jpg` | The Y resolves into perfect clarity, fully lit, a warm coral glow radiating outward into the dark. Hopeful, forward-looking. |

Hasta que existan: usar placeholders CSS (gradientes navy con glow coral/teal)
detrás del mismo sistema de capas, para no bloquear el desarrollo.

## 7. Assets existentes (vienen del repo/zip anterior)

- `ynera-logo.svg`, `ynera-logo-lockup.svg` — monograma y lockup.
- `cdi-preview.jpg`, `reservaya-preview.jpg` — screenshots de productos.
- `og-image.png` — para OG (revisar que siga vigente).
- Screenshot de **Aira**: pendiente (placeholder elegante mientras tanto).

## 8. Pendientes explícitos (no inventar)

- [ ] Datos de contacto reales (WhatsApp, email, LinkedIn, link de agenda) —
      placeholders marcados `TODO-CONTACTO`.
- [ ] Frase final del contacto (hay genérica provisoria).
- [ ] Screenshot + link de Aira.
- [ ] Las 5 imágenes de fondo (las genera el usuario en Gemini).
- [ ] Dominio definitivo (por ahora Railway).
