# Catálogo de skills disponibles

> Para el diagnóstico concreto del `index.html` actual frente a estas skills (qué podar, qué reforzar, prioridades) ver [`GUIA-DE-APLICACION.md`](./GUIA-DE-APLICACION.md).

Skills en formato `SKILL.md` (frontmatter YAML + instrucciones Markdown) usables por Codex o Claude Code sobre este repo. Se invocan pidiéndole al agente que "use la skill X" y opcionalmente pegando la ruta del archivo.

Todas las skills nuevas fueron descargadas el **2026-08-20** desde repos públicos de GitHub (ver cabecera "Origen" al inicio de cada `SKILL.md`). Este sitio es estático, sin build, para GitHub Pages (ver `AGENTS.md`) — cualquier técnica sugerida por estas skills debe adaptarse a HTML/CSS/JS puro, respetar `prefers-reduced-motion` y no requerir npm.

## Orden de uso recomendado

1. **`romantic-web-polish`** decide siempre el tono: es la skill "maestra" de este repo, específica para Sofía. Fija qué evitar (landing gen érica, bento grids, glassmorphism excesivo, IP de Disney) y qué contenido conservar.
2. **`frontend-design`** ayuda a elegir la dirección estética *antes* de tocar código (paleta, tipografía, layout, "elemento firma") y evitar los defaults típicos de diseño hecho por IA.
3. **`emil-anim`** da las reglas de "buen gusto" para cualquier microinteracción (duración, easing, distancias) — úsala como checklist al escribir CSS de animación a mano.
4. **`animate-css`** y **`css-animation`** son las que implementan efectos concretos — la primera para microinteracciones/entradas puntuales con una librería ya lista, la segunda para una animación autocontenida más elaborada (p. ej. una escena tipo "la carta se abre").

---

### `romantic-web-polish`
- **Ruta:** `.agents/skills/romantic-web-polish/SKILL.md`
- **Origen:** skill local ya existente en este proyecto (no descargada de GitHub).
- **Qué hace:** fija la dirección visual y de contenido del sitio (cuento nocturno, luna, flores, tonos lavanda/rosa/dorado) y qué evitar.
- **Cuándo usarla:** siempre que se pida embellecer, rediseñar o revisar el sitio. Es la skill que debe tener la última palabra sobre gusto y tono.
- **Prompt de ejemplo:**
  > Usa la skill `romantic-web-polish`. Revisa el sitio completo y mejora [lo que corresponda] sin convertirlo en una landing genérica.

### `frontend-design`
- **Ruta:** `.agents/skills/frontend-design/SKILL.md`
- **Fuente:** [`anthropics/skills`](https://github.com/anthropics/skills/blob/main/skills/frontend-design/SKILL.md) (oficial de Anthropic).
- **Qué hace:** guía para tomar decisiones de diseño deliberadas (paleta de 4-6 colores nombrados, tipografía en 2+ roles, layout, un "elemento firma") en vez de defaults genéricos. Nombra explícitamente los 3 clichés más comunes del "diseño hecho por IA" (fondo crema + serif + acento terracota; fondo casi negro + un acento verde/rojo ácido; layout tipo periódico con hairlines) para evitarlos salvo que el brief los pida.
  - También advierte: *"a veces menos es más, y la animación de más contribuye a que el diseño se sienta hecho por IA"* — es decir, esta skill modera a las de animación, no solo las complementa.
- **Cuándo usarla:** antes de rediseñar una sección entera, para fijar token system (color/tipografía/layout) coherente con la identidad de Sofía definida en `romantic-web-polish`.
- **Requiere build/framework:** no, es agnóstica.
- **Prompt de ejemplo:**
  > Usa la skill `frontend-design` para proponer un token system (paleta, tipografía, layout, elemento firma) para la sección hero, coherente con la identidad de Sofía descrita en `romantic-web-polish` y `AGENTS.md`.

### `emil-anim`
- **Ruta:** `.agents/skills/emil-anim/SKILL.md`
- **Fuente:** gist de [`corysimmons`](https://gist.github.com/corysimmons/1e2f64603ae234602f92dafe2b549ea9), basado en los principios de Emil Kowalski / [animations.dev](https://animations.dev).
- **Qué hace:** 40 reglas concretas de timing, easing y distancias para que la animación se sienta "invisible" (natural) y no decorativa: microinteracciones 150-250ms, `ease-out` en entradas / `ease-in` en salidas, mover solo 4-16px en microinteracciones, animar solo `transform`/`opacity`, siempre respetar `prefers-reduced-motion`, etc. Incluye snippets CSS puros listos para copiar.
- **Cuándo usarla:** como referencia rápida al escribir o revisar cualquier `transition`/`@keyframes` a mano en `index.html`, para no pasarse de frenada con los efectos.
- **Requiere build/framework:** no (los ejemplos CSS son puros; también incluye equivalentes en Framer Motion que no aplican aquí).
- **Prompt de ejemplo:**
  > Revisa las animaciones actuales de `index.html` contra las reglas de la skill `emil-anim` y ajusta duración/easing/distancia donde se pasen de lo recomendado.

### `animate-css`
- **Ruta:** `.agents/skills/animate-css/SKILL.md` (+ asset embebido `.agents/skills/animate-css/assets/animate.min.css`, Animate.css v4.1.1)
- **Fuente:** [`msrbuilds/animate-css-skill`](https://github.com/msrbuilds/animate-css-skill).
- **Qué hace:** añade animaciones de entrada/salida/atención (`fadeInUp`, `zoomIn`, `bounceIn`, etc.) a HTML plano, extrayendo del CSS embebido *solo* los `@keyframes` necesarios (no hay que cargar la librería entera ni un CDN). Incluye patrón listo de scroll-trigger con `IntersectionObserver`, que respeta `prefers-reduced-motion` en JS y CSS, y soporte RTL (no aplica a este sitio en español/LTR).
- **Cuándo usarla:** para animar entradas de tarjetas de fotos, secciones al hacer scroll, o dar énfasis puntual a un botón/CTA — es la opción más rápida de implementar en HTML estático sin dependencias externas.
- **Requiere build/framework:** no. Sección "HTML (vanilla)" del `SKILL.md` es la aplicable aquí; ignorar las secciones de React/WordPress.
- **Licencia del CSS embebido:** Hippocratic License (Animate.css) — distinta del MIT del propio skill; revisar antes de redistribuir el `.css` fuera de este repo.
- **Prompt de ejemplo:**
  > Usa la skill `animate-css` (sección HTML vanilla) para que las tarjetas de fotos aparezcan con scroll-trigger y `fadeInUp`, respetando `prefers-reduced-motion`.

### `css-animation`
- **Ruta:** `.agents/skills/css-animation/SKILL.md`
- **Fuente:** [`neonwatty/css-animation-skill`](https://github.com/neonwatty/css-animation-skill) (MIT).
- **Qué hace:** genera un HTML/CSS autocontenido (~30KB, sin dependencias salvo Google Fonts) para una animación más elaborada tipo "walkthrough" (antes → acción → después, o carrusel de escenas), con posicionamiento trigonométrico para layouts circulares y timing cuidado.
- **Cuándo usarla:** para un efecto centrado y elaborado (p. ej. una escena de "la carta se abre y aparecen flores", o un carrusel de momentos), no para microinteracciones sueltas.
- **Limitación en este proyecto:** el flujo original (Fases 1 y 4) asume una app en vivo para investigar vía Claude-in-Chrome y un servidor Python local para el ciclo de revisión — no aplican directamente a un sitio estático sin "app" que inspeccionar. Usar solo las Fases 2-3 (Interview + Generate) adaptadas: describir a mano el lenguaje visual (colores/tipografía ya definidos en `romantic-web-polish`) en vez de extraerlo de una app viva, y revisar el resultado abriendo el HTML directamente en el navegador.
- **Requiere build/framework:** no, salida 100% HTML/CSS/JS estático.
- **Prompt de ejemplo:**
  > Usa la skill `css-animation` (solo Fases 2-3) para generar una animación autocontenida tipo "la carta se abre", con la paleta lavanda/rosa/dorado de `romantic-web-polish`, y guárdala como `assets/animacion-carta.html` para embeber con un `<iframe>`.

## Skills descartadas (no descargadas)

Encontradas en la misma investigación pero descartadas porque requieren React/Next.js o build con npm, lo que viola la restricción de `AGENTS.md` de mantener el sitio como HTML/CSS/JS estático sin build:

- `delphi-ai/animate-skill` (basada en el curso de Emil Kowalski para React)
- `199-biotechnologies/motion-dev-animations-skill` (Motion.dev / sucesor de Framer Motion)
- `freshtechbro/claudedesignskills` (colección orientada a 3D/React)

Quedan como referencia por si el sitio migra algún día a un stack con build.
