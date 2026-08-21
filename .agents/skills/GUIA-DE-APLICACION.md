# Guía de aplicación de las skills a este proyecto

Esta guía complementa el catálogo (`README.md` en esta misma carpeta): ahí está el "qué es cada skill", aquí está el **diagnóstico concreto del `index.html` actual** frente a las reglas de esas skills, y qué hacer con ese diagnóstico. No modifica el sitio — es el mapa para la próxima sesión de mejora.

## 1. Inventario de lo que ya existe

`index.html` (single-file, ~1300 líneas) ya tiene un sistema de efectos bastante maduro. Antes de añadir nada, hay que saber qué hay:

| Efecto | Dónde (selector / línea aprox.) | Mecanismo |
|---|---|---|
| Estrellas titilando | `.stars` + `::before/::after`, L66-118 | 3 capas de `radial-gradient` + `@keyframes twinkle`, duraciones 4.3–6.4s, siempre activo |
| Luna flotando + halo pulsante | `.moon`, `.moon::before`, L260-311, L346-370 | `@keyframes float` (traslada 38px) + `@keyframes pulse-glow` (opacity/scale), 8s loop |
| Botón CTA con glow pulsante | `.button-primary`, L230-251 | `@keyframes glow-pulse` infinito sobre `box-shadow`, se pausa en `:hover` |
| Reveal al hacer scroll | `.reveal` / `.reveal.visible`, L820-829 + JS L1241-1254 | `IntersectionObserver`, `opacity`+`translateY(24px)`, `.7s ease` |
| Sobre que se abre (carta) | `.envelope*`, L687-769 + JS L1259-1275 | `transform-style: preserve-3d` + `rotateX(180deg)` en `.envelope-flap`, `.9s cubic-bezier` |
| Panel de sorpresa | `.surprise-panel`, L776-790 + JS L1280-1289 | `grid-template-rows: 0fr → 1fr`, `.5s ease` |
| Pétalos al abrir la sorpresa | `.petal`, L831-848 + JS `createPetals()`, L1294-1307 | Elementos `position:fixed` generados por JS, `@keyframes fall`, valores aleatorios de deriva/duración |
| Hover en galería | `.gallery-item:hover`, L628-642 | `translateY(-4px)` + `scale(1.04)` en la imagen, `.3-.4s ease` |
| Hover en tarjetas "reason" | `.reason:hover`, L547-550 | `translateY(-3px)`, `.22s ease` |

Ya hay `prefers-reduced-motion` global (L918-933) que anula duraciones y muestra `.reveal` sin animar. Esto ya cumple el requisito no-negociable de `AGENTS.md` y de `emil-anim` (regla 36).

**Conclusión del inventario:** el sitio no está "pelado" de efectos — al contrario, tiene 8 sistemas de animación simultáneos posibles en el hero (3 twinkles + float + pulse-glow + glow-pulse = 6 loops activos a la vez sin que el usuario haga nada). El trabajo pendiente no es "agregar más efectos", es **auditar los que hay contra las skills descargadas y podar/afinar**, y luego añadir 1-2 momentos nuevos con intención, no cantidad.

## 2. Diagnóstico sección por sección

### Hero (`.hero`, L938-963)

- ⚠️ **Riesgo de cliché "hecho por IA":** un botón CTA con gradiente rosa→dorado que pulsa infinitamente (`glow-pulse`, L241-251) es casi el ejemplo de libro de lo que `frontend-design` describe como default genérico ("gradiente llamativo que aparece sin importar el sujeto"). La propia skill lo dice explícitamente: *"a veces menos es más, y la animación de más contribuye a que el diseño se sienta hecho por IA"*. Un pulso infinito en un botón es un patrón muy visto en landings SaaS.
  - **Acción sugerida:** quitar el loop infinito y dejar el glow solo como respuesta a interacción (hover/focus) siguiendo `emil-anim` regla 20 ("hover: instant on, 150ms off"). El botón sigue destacando por color/posición sin necesitar moverse solo.
- ⚠️ **Demasiados loops ambientales simultáneos.** 6 animaciones corriendo a la vez en el primer viewport compiten entre sí y ninguna se vuelve "el elemento firma" que pide `frontend-design` (*"deja que un solo elemento memorable"*). Candidato a firma: la luna (`float` + `pulse-glow`) ya es un buen candidato — el resto (estrellas titilando + botón pulsando) puede quedar más quieto para que la luna gane protagonismo.
  - **Acción sugerida:** conservar `float`/`pulse-glow` de la luna como firma. Bajar la opacidad del twinkle o reducirlo a 1-2 capas en vez de 3 si tras probarlo se siente cargado.
- ✅ El `float` de la luna (38px de desplazamiento, 8s) es ambiental, no de interacción — no está sujeto a los límites de "microinteracción" de `emil-anim`, está bien como está.

### Reveal on scroll (global, L820-829 + JS)

- ⚠️ **Duración algo larga:** `.7s ease` para un reveal está por encima del rango que `emil-anim` marca para "standard transitions" (200-350ms, regla 2) y roza el techo de "orquestaciones complejas" (400-600ms, regla 3). No es grave — es una elección de ritmo "cinematográfico lento" válida para un sitio romántico — pero conviene decidirlo a propósito, no dejarlo por default.
- ⚠️ **Sin stagger.** Cada `<section class="reveal">` entra como bloque único; dentro de una sección con varias tarjetas (`.reason`, `.gallery-item`) todo aparece al mismo tiempo. `emil-anim` (reglas 31-34) y `animate-css` (soporte `data-delay`) recomiendan escalonar 30-80ms entre elementos hermanos para que la entrada se sienta más viva sin ser más larga.
  - **Acción sugerida:** usar el patrón de `animate-css` (`data-delay` + el mismo `IntersectionObserver` que ya existe) para escalonar `.reason` y `.gallery-item` dentro de cada `.reveal`, en vez de crear un segundo sistema de observadores. El sitio ya tiene la infraestructura (un solo `IntersectionObserver` L1242-1249); solo falta usar `transition-delay: calc(var(--i) * 45ms)` con un `--i` puesto por nth-child o inline.
- ✅ La distancia de movimiento (`translateY(24px)`) cae bien dentro del rango "20-40px reveals grandes" de `emil-anim` (regla 19).

### Sobre / carta (`.envelope`, L687-769)

- Es el **momento de mayor peso emocional de la página** (abrir la carta) — el mejor candidato para aplicar `css-animation` o al menos las reglas de orquestación de `emil-anim` con más cuidado, porque aquí sí vale la pena "gastar" complejidad (regla de `frontend-design`: *"gasta tu atrevimiento en un solo lugar"*).
- ⚠️ `.9s cubic-bezier(.5, 0, .2, 1)` para el flap es más largo que el techo de 600ms que sugiere `emil-anim` para orquestaciones — aquí sí hay una razón real (es un payoff, no una microinteracción), pero vale la pena probarlo a 650-750ms y comparar: `emil-anim` advierte que pasado 1s deja de sentirse animación y empieza a sentirse "pantalla de carga".
- ✅ El uso de `preserve-3d` + `rotateX` para simular un sobre abriéndose es exactamente el tipo de "riesgo estético justificado" que pide `frontend-design`, no un efecto genérico de plantilla — vale la pena conservarlo y quizás **reforzarlo** en vez de recortarlo.
- 💡 **Oportunidad:** este es el lugar ideal para invocar la skill `css-animation` (fases 2-3 adaptadas, ver catálogo) y generar una micro-secuencia adicional dentro de la carta ya abierta — p. ej. una flor o un pétalo que se dibuja junto a la firma — en vez de repartir más efectos por el resto de la página.

### Sorpresa (`.surprise-panel` + pétalos, L776-848)

- ✅ Buen ejemplo de "animación con propósito": los pétalos solo aparecen tras una acción del usuario (`surpriseButton`), no de forma ambiental. Coincide con `emil-anim` regla "cuándo NO animar" (evita ambiental infinito para momentos que deberían sentirse especiales) y con el patrón de `frontend-design` de reservar el "momento wow" para un solo lugar.
- ⚠️ Los pétalos son `position: fixed`, full-viewport — en scroll largo esto puede solaparse con contenido si el usuario hace scroll mientras caen (7-9s de vida). No es un problema de estética-IA, es un detalle técnico a vigilar si se toca este bloque.

### Galería (`.gallery-item`, L611-642) y tarjetas "reason" (L537-576)

- ✅ Hover con `transform` únicamente (`translateY` + `scale`), sin animar `width/height/margin` — cumple al pie de la letra `emil-anim` regla 14-15.
- 💡 **Oportunidad concreta para `animate-css`:** aplicar el patrón `scroll-animate` + `IntersectionObserver` de esa skill a las imágenes de cada `gallery-grid`, con `data-animate="fadeInUp"` y `data-delay` escalonado por bloque temático (cine, llamadas, fotos de ella, de la mano, otros momentos) — hoy todas las imágenes de un bloque aparecen exactamente igual y al mismo tiempo que el resto de la sección `.memory-band`, perdiendo la oportunidad de que cada bloque se sienta como su propio "capítulo".

## 3. Tabla priorizada de acciones (de más a menos impacto)

| # | Acción | Skill que la guía | Sección de la skill a releer |
|---|---|---|---|
| 1 | Quitar el pulso infinito de `.button-primary` (glow-pulse), dejar el glow solo en hover/focus | `frontend-design` + `emil-anim` | "AI-generated design clusters" / regla 20 |
| 2 | Elegir un único elemento firma en el hero (recomendado: la luna) y aquietar el resto | `frontend-design` | "Restraint & self-critique" |
| 3 | Escalonar (`stagger`) la entrada de `.reason` y `.gallery-item` con `transition-delay` 30-80ms | `emil-anim` + `animate-css` | reglas 31-34 / "Delay para listas escalonadas" |
| 4 | Revisar duración del sobre (`.9s` → probar 650-750ms) sin perder el peso del momento | `emil-anim` | reglas 3-4 |
| 5 | (Opcional, ambicioso) Generar con `css-animation` una micro-escena adicional dentro de la carta ya abierta | `css-animation` | Fases 2-3 |
| 6 | Revisar `.reveal` global: decidir a propósito si 700ms es el ritmo deseado o bajarlo a ~400-500ms | `emil-anim` | reglas 2-3 |

Estas acciones se pueden pedir una por una; no hace falta abordarlas todas juntas.

## 4. Flujo de trabajo recomendado para la próxima sesión

1. **`romantic-web-polish` primero, siempre.** Antes de tocar cualquier efecto, confirmar que el cambio sigue sintiéndose "hecho para Sofía" y no una plantilla — es la skill con veto sobre las demás.
2. **`frontend-design` para decidir qué podar y qué reforzar.** Este proyecto ya tiene bastante ejecución; lo que falta es criterio de restricción, que es justamente el aporte de esta skill (identidad + un solo riesgo estético, no una lista de efectos).
3. **`emil-anim` como checklist de números** al escribir o ajustar cualquier `transition`/`@keyframes` (duración, easing, distancia) — es la referencia rápida para no "sobre-animar".
4. **`animate-css`** para lo puntual y ejecutable ya (stagger de galería, scroll-trigger de secciones nuevas) sin escribir el observer desde cero.
5. **`css-animation`** solo para el momento único y ambicioso (la carta), no para microinteracciones sueltas — es la más pesada de las cuatro y depende de Claude-in-Chrome para su flujo completo (ver limitación anotada en el catálogo).

Prompt de ejemplo para encadenar todo esto:

> Usa `romantic-web-polish` como filtro de tono y, apoyándote en `frontend-design` y `emil-anim`, aplica las acciones 1 y 2 de `.agents/skills/GUIA-DE-APLICACION.md` (quitar el pulso infinito del botón CTA y aquietar el hero para que la luna sea el elemento firma). Valida antes/después en 360px y 1440px.

## 5. Checklist de validación (combina las 5 skills)

Antes de dar por cerrado cualquier cambio de efectos en este sitio:

- [ ] ¿El efecto tiene una razón ligada a Sofía/el sitio, o es un default que pondrías en cualquier landing? (`frontend-design`, `romantic-web-polish`)
- [ ] ¿Hay más de 2-3 loops ambientales activos a la vez en el mismo viewport? Si sí, ¿por qué? (`frontend-design`)
- [ ] ¿La duración está en rango (150-250ms micro, 200-350ms estándar, 400-600ms orquestación, nunca >1s salvo justificación)? (`emil-anim` reglas 1-6)
- [ ] ¿Se anima solo `transform`/`opacity`? (`emil-anim` regla 14-15)
- [ ] ¿Las salidas son más rápidas que las entradas? (`emil-anim` regla 4)
- [ ] ¿El movimiento es pequeño (4-16px micro, 20-40px reveals)? (`emil-anim` regla 19)
- [ ] ¿`prefers-reduced-motion` sigue cubriendo el efecto nuevo? (ya hay un bloque global en L918-933; los efectos generados por JS como `createPetals` deben seguir respetando `prefersReducedMotion`)
- [ ] ¿Se probó en 360×800 y 1440×900 sin overflow horizontal? (`AGENTS.md`)
- [ ] ¿Sigue siendo HTML/CSS/JS estático sin build ni librerías pesadas nuevas? (`AGENTS.md`)

## 6. Nota sobre las skills descartadas

Si en algún momento el sitio deja de ser estático (por ejemplo, se migra a Next.js para añadir un backend de RSVP o galería dinámica), retomar `delphi-ai/animate-skill` o `199-biotechnologies/motion-dev-animations-skill` (mencionadas como descartadas en el catálogo) tendría sentido — hoy no aplican porque requieren build.
