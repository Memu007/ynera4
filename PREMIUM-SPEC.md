# YNERA — Spec premium (v2 de la landing)

> Complementa a `BRAND.md` (que sigue mandando en identidad, copy y paleta).
> Este documento define la MECÁNICA premium de la página, destilada de 4
> prompts de referencia que aportó el usuario. Reemplaza al `index.html`
> vanilla v1. Si algo contradice a BRAND.md en identidad, manda BRAND.md;
> en técnica/stack, manda este documento.

## Stack

- **Vite + React 18 + TypeScript + Tailwind CSS + Framer Motion** (`motion/react`).
- `lucide-react` para íconos.
- Fuentes: Playfair Display (700 + italic) y Inter (400/500/600) — Google Fonts.
- Deploy: Railway (build estático de Vite servido por cualquier static server).
- Nada de librerías extra de UI. Los componentes se escriben a mano según abajo.

## Assets de video (YA EN `assets/`, listos para usar)

Aportados por el usuario (generados con Gemini/Veo), marca de agua removida
con delogo, 1280×720 H.264 24fps 10s, faststart, sin audio:

| Archivo | Contenido | Uso |
|---|---|---|
| `assets/video-hero.mp4` (1.4 MB) | La Y 3D girando 360° sobre negro | **Hero con `ScrubVideo`** — el scrub del mouse rota el logo |
| `assets/video-datos.mp4` (3.0 MB) | La Y con splash líquido azul/violeta | Sección **Datos** (`FadingVideo`) |
| `assets/video-ia.mp4` (2.9 MB) | Chispas/partículas blancas formando la Y | Sección **IA** (`FadingVideo`) |
| `assets/video-seguridad.mp4` (3.8 MB) | La Y glow sobre malla hexagonal tech | Sección **Seguridad** (`FadingVideo`) |

- Los videos de los prompts de referencia (cloudfront `user_38xz...`,
  motionsites.ai) son de terceros: **NO usarlos en producción**.
- Fallback si un video no carga: gradiente CSS de la paleta, nunca un video
  ajeno.

## Sistema visual (hereda BRAND.md, con estos agregados)

### Liquid-glass (chrome de UI)

Dos variantes, CSS exacto:

```css
.liquid-glass {
  background: rgba(255,255,255,0.01);
  background-blend-mode: luminosity;
  backdrop-filter: blur(4px); -webkit-backdrop-filter: blur(4px);
  border: none;
  box-shadow: inset 0 1px 1px rgba(255,255,255,0.1);
  position: relative; overflow: hidden;
}
.liquid-glass::before {
  content: ""; position: absolute; inset: 0; border-radius: inherit;
  padding: 1.4px;
  background: linear-gradient(180deg,
    rgba(255,255,255,0.45) 0%, rgba(255,255,255,0.15) 20%,
    rgba(255,255,255,0) 40%, rgba(255,255,255,0) 60%,
    rgba(255,255,255,0.15) 80%, rgba(255,255,255,0.45) 100%);
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor; mask-composite: exclude;
  pointer-events: none;
}
/* .liquid-glass-strong: igual pero blur(50px), sombra externa suave,
   y stops 0.5/0.2/0/0/0.2/0.5 */
```

Uso: nav pill, chips, tarjetas de stats y productos, CTA primario
(`liquid-glass-strong`). El coral de marca queda para el CTA sólido y los
`<em>` — el glass no lo reemplaza, conviven.

### Tipografía a escala de viewport

Titulares de sección grandes con `clamp()`/vw (referencia: hasta ~10-12vw en
secciones de quiebre). Playfair 700; itálica coral en la palabra clave, como
BRAND.md.

## Componentes reutilizables (mecánica exacta)

1. **`ScrubVideo`** — video fullscreen que se scrubea con el mouse (desktop):
   `mousemove` en window; `delta = x - prevX`;
   `target += (delta / innerWidth) * 0.8 * duration`; clamp [0, duration];
   seek con `video.currentTime` y handler `seeked` que encola el próximo seek
   (evita flooding). En `< 1024px`: sin scrub, autoplay loop normal.
   `muted playsInline preload="auto"`.

2. **`FadingVideo`** — loop con crossfade manual por rAF (sin transiciones CSS):
   `FADE_MS = 500`, `FADE_OUT_LEAD = 0.55s`. `fadeTo(target)` lee la opacidad
   actual del style (resume desde donde está) y cancela el rAF anterior.
   `loadeddata` → play + fadeTo(1); `timeupdate` → si falta ≤0.55s, fadeTo(0);
   `ended` → reset a 0, play, fadeTo(1). Atributo `loop` APAGADO.

3. **`BlurText`** — titular palabra por palabra: cada palabra es un
   `motion.span` con keyframes
   `{blur(10px), op 0, y 50} → {blur(5px), op .5, y -5} → {blur(0), op 1, y 0}`,
   duración 0.7, stagger 100ms/palabra, `IntersectionObserver` al 10%.
   Padre en flex-wrap; `marginRight: 0.28em` por palabra.

4. **`Typewriter`** — hook `useTypewriter(text, speed=38, startDelay=600)`
   → `{displayed, done}`. Cursor: barra de 2px con `blink 1s step-end
   infinite`, desaparece al terminar.

5. **`StackCards`** — apilado sticky con escala (productos): cada card
   `sticky top-24` dentro de un contenedor `h-[85vh]`;
   `targetScale = 1 - (total - 1 - i) * 0.03`; offset `top: i * 28px`;
   `useScroll` + `useTransform`.

6. **`AnimatedText`** — párrafo revelado carácter por carácter con scroll:
   `useScroll` sobre el propio párrafo, offset `['start 0.8', 'end 0.2']`;
   cada carácter opacidad 0.2 → 1 según progreso (placeholder invisible +
   span absoluto animado).

7. **`Magnet`** — hover magnético (CTA final y logo): translate3d hacia el
   cursor / strength 3, activo a 150px del borde; in 0.3s ease-out,
   out 0.6s ease-in-out; `willChange: transform`.

8. **`FadeIn`** — wrapper `whileInView` `{once: true}`, y 30, dur 0.7,
   ease `[0.25, 0.1, 0.25, 1]`, con `delay` por prop (stagger de secciones).

## Estructura de la página (mantiene los 2 actos de BRAND.md)

### Acto 1 — la historia

1. **Hero** — `ScrubVideo` fullscreen (video del usuario; hasta entonces
   gradiente). Nav liquid-glass (pill central Datos·IA·Seguridad·Sistemas +
   CTA "Hablemos" sólido coral). Titular "Tu problema tiene un *sistema.*"
   con `BlurText`. Subtítulo con `Typewriter`:
   "Escuchamos lo que te frena. Diseñamos la solución. La dejamos corriendo
   en producción." Dos stat-cards liquid-glass: "3 — sistemas propios en
   producción" y "BA — estudio de Buenos Aires".
2. **Datos / IA / Seguridad** — tres secciones full-height con `FadingVideo`
   de fondo (o gradiente fallback), titular grande Playfair + `FadeIn`
   escalonado. Copy de BRAND.md sección 5.
3. **Statement** — sección puente con `AnimatedText` carácter por carácter:
   "No vendemos horas ni reportes. Construimos sistemas que quedan
   funcionando cuando nos vamos." (fondo sólido navy, sin video).

### Acto 2 — el aterrizaje (fondo sólido `--navy-ui`)

4. **Productos** — titular "No es teoría. Está *corriendo.*" +
   `StackCards` con Aira / CDI / ReservaYá (mismos datos, links y TODOs de
   BRAND.md — Aira sigue "Próximamente", sin link inventado).
5. **Contacto** — patrón de pills multi-select:
   - Pregunta: "¿Qué te está frenando?" — pills: `Datos` `IA` `Seguridad`
     `Otra cosa` (multi-select, check con spring stiffness 300 damping 20;
     activa: fondo coral texto navy; inactiva: liquid-glass).
   - `AnimatePresence` debajo: vacío → "Elegí arriba para empezar."
     (50% opacidad, itálica); con selección → banner liquid-glass
     "Listo para hablar de: [selección]" + campos nombre/email + botón
     `Magnet` "Hablemos →" coral.
   - Sigue siendo front-only hasta tener datos reales (`TODO-CONTACTO`
     de BRAND.md vigente).
6. **Footer** — monograma + YNERA + "Estudio de Buenos Aires · © 2026".

## Reglas

- `prefers-reduced-motion`: sin scrub (video pausado en frame 0 o gradiente),
  sin typewriter (texto completo), sin stagger; todo visible directo.
- Mobile: videos autoplay loop (sin scrub), stack cards pasan a lista,
  tipografía fluida con clamp().
- Performance: videos comprimidos (H.264, ≤3-4 MB c/u idealmente),
  `preload="auto"` solo el hero, `loading="lazy"` en imágenes.
- Accesibilidad: focus visible en pills y links, contraste AA sobre video
  (scrim si hace falta), `aria-live` en el banner del contacto.
- El copy, la paleta y las reglas de marca NO se renegocian acá: BRAND.md.
