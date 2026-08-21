# Plan de implementación — Animaciones premium para `foto#.jpeg`

## 0. Misión

Trabaja como un **Senior Frontend Motion Designer especializado en interfaces emocionales y cinematográficas**, pero manteniendo este proyecto como:

- HTML puro.
- CSS puro.
- JavaScript vanilla.
- Compatible con GitHub Pages.
- Sin npm.
- Sin frameworks.
- Sin proceso de build.
- Sin dependencias innecesarias.

Tu tarea consiste en **mejorar notablemente la calidad visual y la sensación de las animaciones asociadas a las fotografías cuyo nombre siga este patrón:**

```text
foto1.jpeg
foto2.jpeg
foto3.jpeg
...
fotoN.jpeg
```

Es decir:

```regex
(?:^|/)foto\d+\.jpeg$
```

Estas fotografías pueden encontrarse dentro de distintas carpetas de `assets/`.

### MUY IMPORTANTE

`foto_ella.jpeg` **NO entra en esta tarea**, porque no cumple el patrón numérico.

No modifiques sus animaciones salvo que sea estrictamente necesario para evitar una regresión.

---

# 1. Regla principal para Gemini

Piensa en esta explicación como si fueras un niño:

> Si mamá te pide ordenar un juguete, no necesitas cambiar todos los muebles de la habitación.

Eso es exactamente lo que debes hacer aquí.

No conviertas una mejora de animaciones de fotografías en:

- un rediseño completo;
- una reconstrucción de la página;
- una reescritura del HTML;
- una nueva galería;
- un carrusel innecesario;
- un lightbox gigante;
- una landing page diferente;
- una demostración de todas las animaciones que conoces.

## Regla anti-sobreingeniería

Antes de modificar algo pregúntate:

> ¿Este cambio hace que las fotografías se sientan más bonitas, naturales y especiales?

Si la respuesta es **no**, no lo hagas.

---

# 2. Antes de escribir código

Lee obligatoriamente, en este orden:

1. `AGENTS.md`
2. `GUIA-DE-APLICACION.md`
3. `.agents/skills/romantic-web-polish/SKILL.md`
4. `.agents/skills/frontend-design/SKILL.md`
5. `.agents/skills/emil-anim/SKILL.md`
6. `.agents/skills/animate-css/SKILL.md`
7. `.agents/skills/css-animation/SKILL.md`

No te bases únicamente en el resumen de las skills.

**Abre y lee los `SKILL.md` reales antes de implementar.**

---

# 3. Jerarquía de autoridad

Cuando dos skills sugieran cosas distintas, seguir esta prioridad:

```text
romantic-web-polish
        ↓
frontend-design
        ↓
emil-anim
        ↓
animate-css
        ↓
css-animation
```

### Interpretación

`romantic-web-polish` decide:

- qué se siente apropiado;
- qué encaja con Sofía;
- qué resulta romántico;
- qué debe evitarse.

`frontend-design` decide:

- coherencia visual;
- composición;
- jerarquía;
- evitar estética genérica de IA.

`emil-anim` decide:

- timing;
- easing;
- desplazamiento;
- naturalidad;
- respuesta de las interacciones.

`animate-css` puede proporcionar determinados keyframes si realmente aportan algo.

`css-animation` queda reservada para animaciones narrativas complejas.

### IMPORTANTE

No uses `css-animation` simplemente porque está disponible.

Para una galería fotográfica normal probablemente sería excesivo.

---

# 4. Primero audita la implementación actual

Antes de tocar código:

1. Busca todas las imágenes `<img>`.
2. Obtén su atributo `src`.
3. Identifica cuáles terminan en:

```regex
/foto\d+\.jpeg$
```

4. Localiza su `.gallery-item`.
5. Localiza todos los estilos que afectan a:

```css
.gallery-grid
.gallery-item
.gallery-item img
.gallery-item:hover
.gallery-item:hover img
.gallery-label
.reveal
.reveal.visible
```

6. Localiza todos los `IntersectionObserver`.
7. Identifica exactamente cuándo empiezan actualmente las animaciones.

Documenta mentalmente la arquitectura antes de modificarla.

---

# 5. Problema actual que debes resolver

Actualmente existe una animación de aparición general basada aproximadamente en:

```text
.reveal → IntersectionObserver → .visible → gallery-item
```

El problema conceptual es que el disparador pertenece principalmente al **contenedor/sección**, no necesariamente a cada fotografía individual.

Eso significa que una sección grande puede comenzar su animación cuando apenas entra en el viewport.

Las fotografías situadas mucho más abajo pueden:

1. recibir la animación;
2. terminarla;
3. permanecer fuera del viewport;
4. aparecer ya estáticas cuando el usuario finalmente llega a ellas.

## Objetivo

Las fotografías `fotoN.jpeg` deben recibir su animación **cuando ellas mismas estén próximas a entrar en el viewport**.

No cuando entra toda la sección.

---

# 6. Arquitectura deseada

Crear una lógica específica para las fotografías.

No dependas de que `.memory-band` o `.reveal` controle todas las imágenes a la vez.

Una arquitectura apropiada sería conceptualmente:

```text
fotoN.jpeg
    ↓
.gallery-item correspondiente
    ↓
clase de estado inicial
    ↓
IntersectionObserver individual
    ↓
clase .is-visible
    ↓
animación de entrada
```

Puedes elegir los nombres finales de las clases, pero deben ser:

- claros;
- específicos;
- reutilizables;
- no destructivos.

Ejemplo conceptual:

```text
.photo-motion
.photo-motion.is-visible
```

No es obligatorio utilizar exactamente esos nombres.

---

# 7. NO dupliques observadores innecesariamente

Debe existir **un único `IntersectionObserver` compartido** para las fotografías animadas.

NO crear:

```text
21 fotografías
=
21 IntersectionObserver diferentes
```

Debe ser:

```text
1 observer
↓
N gallery items observados
```

Una vez mostrada una fotografía:

```javascript
observer.unobserve(element);
```

para evitar trabajo innecesario.

---

# 8. Timing de aparición

Usa `emil-anim` como autoridad.

No quiero animaciones exageradamente lentas del tipo:

```text
1.2s
1.5s
2s
```

para una tarjeta normal.

La animación debe sentirse:

- elegante;
- rápida;
- natural;
- suave;
- casi invisible.

## Base recomendada para explorar

No copies estos números ciegamente. Contrástalos con `emil-anim`.

Aproximadamente:

```text
opacity: 0 → 1
translateY: 8px–16px → 0
scale: 0.985–0.995 → 1
```

Evita desplazamientos exagerados.

No hagas:

```text
translateY(80px)
translateX(100px)
rotate(20deg)
scale(.7)
```

La fotografía no debe parecer que está entrando a un PowerPoint.

---

# 9. Easing

Las fotografías entrando deben utilizar principalmente un easing tipo **ease-out**, preferiblemente siguiendo las curvas ya existentes en el proyecto cuando sean apropiadas.

Revisa:

```css
--spring-smooth
--spring-snappy
--spring-bounce
```

No introduzcas diez easings nuevos.

Utiliza una curva coherente.

La sensación deseada es:

```text
entra rápido
↓
desacelera suavemente
↓
queda estable
```

No:

```text
rebota
↓
rebota otra vez
↓
se pasa
↓
vuelve
```

---

# 10. Stagger inteligente

Quiero stagger, pero muy sutil.

Cuando varias fotografías de una misma fila aparezcan juntas:

```text
foto A → aparece
40–70 ms
foto B → aparece
40–70 ms
foto C → aparece
```

No utilices delays enormes.

### PROHIBIDO

No hacer:

```text
foto1 → 0 ms
foto2 → 300 ms
foto3 → 600 ms
foto4 → 900 ms
```

Eso obliga al usuario a esperar.

El stagger debe percibirse casi subconscientemente.

---

# 11. El stagger debe depender de las fotos visibles

No uses una regla global como:

```css
:nth-child(n+5) {
  animation-delay: 290ms;
}
```

si eso significa que una fotografía puede esperar simplemente por ser el elemento número 15 de una sección enorme.

La animación debe responder al contexto del viewport.

Prioriza:

- intersección individual;
- pequeños delays locales;
- cero esperas perceptibles.

---

# 12. Mejorar el hover

Actualmente el hover debe revisarse cuidadosamente.

La interacción debería sentirse como si la fotografía tuviera una pequeña profundidad física.

Experimenta con algo del orden de:

```text
card:
translateY pequeño

image:
scale muy pequeño

shadow:
aumenta ligeramente
```

Pero sigue estrictamente `emil-anim`.

## Muy importante

No debe existir una entrada brusca causada por:

```css
transition-duration: 0ms;
```

si eso provoca que la fotografía salte instantáneamente al estado hover.

Quiero que tanto:

```text
estado normal → hover
```

como:

```text
hover → estado normal
```

se sientan deliberados.

---

# 13. Hover solo donde existe hover real

Utiliza cuando corresponda:

```css
@media (hover: hover) and (pointer: fine)
```

para los efectos específicamente diseñados para mouse.

No asumas que un celular tiene hover.

---

# 14. Comportamiento táctil

En móvil no inventes un falso hover pegajoso.

Las fotografías deben sentirse bien mediante:

- entrada al hacer scroll;
- respuesta `:active` muy sutil si corresponde;
- buena estabilidad;
- cero desplazamientos accidentales.

No añadas gestos complicados.

---

# 15. Transformaciones permitidas

Prioriza animar exclusivamente:

```text
transform
opacity
```

Cuando sea posible.

Evita animar propiedades costosas como:

```text
top
left
width
height
filter blur enorme
box-shadow continuamente
```

Un cambio puntual de `box-shadow` en hover puede ser aceptable.

No hagas animaciones continuas de sombras.

---

# 16. Profundidad fotográfica

Las imágenes pueden obtener una sensación ligeramente más premium utilizando una combinación extremadamente sutil de:

```text
container movement
+
inner image scale
```

Ejemplo conceptual:

```text
gallery-item:
translateY(-2px)

img:
scale(1.015–1.025)
```

No conviertas esto en un efecto de zoom evidente.

El usuario debe pensar:

> qué bonito se siente

y no:

> mira, la foto está haciendo zoom.

---

# 17. No todas las animaciones tienen que ser distintas

NO hagas esto:

```text
foto1 → fade left
foto2 → bounce
foto3 → rotate
foto4 → zoom
foto5 → flip
foto6 → elastic
```

Eso destruye la coherencia.

La galería debe compartir un lenguaje de movimiento.

Puedes introducir como máximo pequeñas variaciones relacionadas con:

- posición;
- orientación;
- secuencia visual.

Pero el sistema debe sentirse como **una misma familia**.

---

# 18. Dirección creativa

Recuerda el tono del proyecto:

```text
noche
luna
recuerdos
flores
lavanda
rosa
dorado
cuento romántico
```

Las fotografías son recuerdos.

Por lo tanto, su movimiento debería recordar más a:

> recuerdos apareciendo suavemente

que a:

> tarjetas de un dashboard SaaS.

---

# 19. Efecto opcional de revelado

Puedes evaluar una entrada con:

```text
opacity
+
translate
+
scale mínimo
```

o, únicamente si queda mejor:

```text
clip-path muy ligero
```

Pero NO utilices `clip-path` simplemente por demostrar habilidad.

Primero implementa la versión sencilla.

Solo conserva una versión más compleja si visualmente mejora claramente el resultado.

---

# 20. Nada de animaciones infinitas en las fotos

Después de aparecer:

```text
la fotografía queda quieta
```

No agregar:

- floating infinito;
- breathing infinito;
- zoom infinito;
- tilt automático;
- wobble;
- pulse permanente.

El movimiento continuo ya existe en la ambientación del sitio.

Las fotografías deben aportar calma.

---

# 21. Labels

Las fotografías que tienen `.gallery-label` deben conservar completamente:

- su texto;
- posición;
- legibilidad;
- contraste.

Puedes hacer que el label acompañe sutilmente la aparición de la fotografía.

Pero NO:

- ocultarlo detrás de la imagen;
- hacerlo ilegible;
- moverlo de manera exagerada;
- borrar textos existentes.

---

# 22. Imágenes verticales y horizontales

El proyecto contiene:

- fotografías cuadradas;
- fotografías verticales;
- fotografías horizontales.

NO asumas que todas son `4/5`.

Respeta los `aspect-ratio` existentes salvo que detectes un problema real.

No alteres el cropping global únicamente para uniformar la cuadrícula.

La diversidad de proporciones forma parte de los recuerdos.

---

# 23. `loading="lazy"`

Conserva:

```html
loading="lazy"
```

en las imágenes existentes.

No fuerces la carga simultánea de todas las fotografías.

---

# 24. `prefers-reduced-motion`

Esta parte es OBLIGATORIA.

Si:

```css
@media (prefers-reduced-motion: reduce)
```

está activo:

- no desplazar fotos;
- no hacer zoom de entrada;
- no utilizar stagger;
- no ejecutar animaciones innecesarias;
- mostrar inmediatamente el contenido.

La página debe seguir siendo completamente funcional.

También respeta la comprobación JavaScript existente mediante:

```javascript
window.matchMedia('(prefers-reduced-motion: reduce)')
```

No dupliques lógica innecesariamente.

---

# 25. Progressive enhancement

Si `IntersectionObserver` no está disponible:

```text
todas las fotografías deben mostrarse inmediatamente
```

Nunca dejes fotografías con:

```css
opacity: 0;
```

permanentemente por un fallo JavaScript.

---

# 26. No tocar estas áreas

Salvo que sea imprescindible para evitar un bug, NO modificar:

- hero;
- escena del mar;
- luna;
- estrellas;
- olas;
- audio;
- pantalla inicial;
- countdown;
- carta;
- sorpresa;
- pétalos;
- textos;
- fechas;
- contenido sentimental;
- rutas de imágenes;
- estructura general del sitio.

La misión es:

> mejorar las animaciones de `fotoN.jpeg`

No:

> hacer otra página.

---

# 27. No introducir librerías externas

No añadir:

- GSAP;
- Framer Motion;
- Motion.dev;
- React;
- Vue;
- npm packages;
- CDN de librerías de animación.

Si utilizas algo de `animate-css`, sigue las instrucciones de la skill y extrae únicamente el keyframe estrictamente necesario.

Pero intenta primero resolverlo mediante CSS propio.

---

# 28. Fase de implementación 1 — selector de fotos

Implementa una selección robusta de imágenes.

La lógica debe identificar solamente imágenes cuyo basename cumpla:

```regex
^foto\d+\.jpeg$
```

No dependas de una lista manual:

```javascript
foto1
foto2
foto3
...
```

porque pueden añadirse más fotos posteriormente.

La solución debe funcionar automáticamente con:

```text
foto22.jpeg
foto23.jpeg
foto24.jpeg
```

si se añaden en el futuro.

---

# 29. Fase de implementación 2 — marcar contenedores

Para cada imagen válida:

```text
img
↓
closest('.gallery-item')
```

Asocia el comportamiento de movimiento al contenedor.

No cambies manualmente el HTML de 20 fotografías si puede hacerse de manera limpia y segura.

Prioriza una implementación mantenible.

---

# 30. Fase de implementación 3 — IntersectionObserver

Crear un observer específico para estos contenedores.

Punto inicial sugerido para evaluar:

```text
threshold bajo/moderado
+
rootMargin ligeramente positivo hacia abajo
```

La animación debería comenzar **un poco antes** de que la fotografía quede completamente visible.

No debe:

- empezar cuando está muy lejos;
- empezar demasiado tarde;
- esperar hasta que el 100% de la fotografía esté visible.

Ajusta visualmente el valor.

---

# 31. Fase de implementación 4 — entrada visual

Implementar un estado inicial elegante.

Conceptualmente:

```css
opacity: 0;
transform:
  translate3d(0, pequeño-desplazamiento, 0)
  scale(valor-muy-cercano-a-1);
```

Estado visible:

```css
opacity: 1;
transform:
  translate3d(0, 0, 0)
  scale(1);
```

Revisa tiempos y easing contra `emil-anim`.

---

# 32. Fase de implementación 5 — interacción

Después de la entrada:

### Desktop

Añadir un hover refinado:

```text
container sube ligeramente
+
imagen aumenta mínimamente de escala
+
shadow obtiene un poco más de profundidad
```

### Mobile

Mantenerlo simple.

Como máximo:

```text
:active → scale muy pequeño
```

si mejora la sensación táctil.

---

# 33. Fase de implementación 6 — limpieza del sistema viejo

Una vez que el sistema específico de fotografías funcione:

Revisa si esta lógica antigua:

```css
.reveal.visible .gallery-item
```

sigue siendo necesaria.

Si entra en conflicto con la animación nueva, elimínala o restríngela.

### MUY IMPORTANTE

No elimines el sistema `.reveal` general.

Otras partes de la página lo utilizan.

Solo evita que controle también las fotografías si ya existe un sistema específico mejor.

---

# 34. Evitar transform conflicts

Comprueba especialmente que:

```text
animación de entrada
+
hover de gallery-item
+
active
```

no intenten escribir simultáneamente valores incompatibles de `transform`.

No quiero:

```text
entrada usa transform
hover reemplaza transform
→ salto visual
```

Diseña los estados conscientemente.

---

# 35. Revisar compositor y rendimiento

Las fotografías pueden ser grandes.

Optimiza el movimiento para que el navegador pueda ejecutarlo de forma fluida.

Prioriza:

```text
transform
opacity
```

No abuses de:

```css
will-change: transform;
```

en todas las fotografías permanentemente.

Si utilizas `will-change`, hazlo de manera justificada.

---

# 36. No uses JavaScript para animar frame a frame

PROHIBIDO crear:

```javascript
setInterval(...)
```

para controlar posiciones de las fotografías.

PROHIBIDO crear loops manuales de animación.

JavaScript debe encargarse principalmente de:

```text
detectar visibilidad
↓
cambiar clases
```

CSS debe encargarse del movimiento.

---

# 37. Accesibilidad

No sacrifiques accesibilidad por animación.

Conservar:

- `alt`;
- semántica actual;
- `loading="lazy"`;
- `prefers-reduced-motion`.

No conviertas los `<article>` en botones si no realizan ninguna acción.

No añadas `tabindex` innecesarios.

---

# 38. Revisión visual obligatoria

Después de implementar, abre la página y revisa al menos:

## Desktop

- viewport ancho;
- scroll lento;
- scroll rápido;
- hover en varias fotos;
- salida del hover;
- varias filas consecutivas.

## Mobile

Simula aproximadamente:

```text
390 × 844
```

y revisa:

- scroll;
- carga lazy;
- entrada de fotografías;
- ausencia de falso hover;
- rendimiento;
- labels.

---

# 39. Prueba con `prefers-reduced-motion`

En DevTools activa:

```text
prefers-reduced-motion: reduce
```

Comprueba que:

- ninguna foto queda invisible;
- ningún elemento queda desplazado;
- la galería funciona normalmente.

---

# 40. Prueba de scroll rápido

Haz scroll rápidamente desde arriba hasta la galería.

Después desplázate lentamente.

Todas las fotografías deben aparecer correctamente.

No debe existir ninguna foto que quede permanentemente:

```css
opacity: 0;
```

---

# 41. Prueba de vuelta hacia arriba

Después de mostrar todas las fotos:

1. baja;
2. vuelve arriba;
3. vuelve a bajar.

Las fotografías NO necesitan repetir su animación.

Una entrada por sesión es suficiente.

Esto evita una página demasiado inquieta.

---

# 42. Prueba de fotografías nuevas

Simula mentalmente que mañana se añade:

```html
<img src="assets/otros_momentos/foto22.jpeg">
```

La arquitectura debe incluirla automáticamente sin escribir CSS o JS específico para `foto22`.

---

# 43. Control de calidad visual

Antes de terminar pregúntate:

### ¿La animación llama más la atención que la fotografía?

Si sí:

> reduce la animación.

### ¿La transición tarda tanto que hay que esperarla?

Si sí:

> acórtala.

### ¿El hover parece un efecto de videojuego?

Si sí:

> simplifícalo.

### ¿La fotografía parece tener un poco más de profundidad y delicadeza?

Si sí:

> probablemente vas en la dirección correcta.

---

# 44. Restricción de cambios

No hagas un rewrite completo de `index.html`.

Realiza cambios localizados.

Antes de finalizar revisa:

```bash
git diff -- index.html
```

El diff debería mostrar principalmente:

- estilos de galería;
- estilos de animación;
- pequeña lógica JS relacionada con las fotografías.

Si aparecen cientos de cambios no relacionados:

**DETENTE y reduce el alcance.**

---

# 45. No reformatear todo el archivo

No ejecutes una operación que reformatee las más de 2000 líneas del HTML.

No cambies:

```text
indentación
comillas
saltos de línea
orden general
```

de código que no estés modificando.

Necesito un diff pequeño y auditable.

---

# 46. Comentarios

Añade únicamente comentarios útiles.

Ejemplo aceptable:

```javascript
// Reveal individual gallery photos as they approach the viewport.
```

No añadir comentarios obvios a cada línea.

---

# 47. Criterios de aceptación

La tarea está terminada solamente si se cumplen TODOS:

- [ ] Solo las imágenes `fotoN.jpeg` reciben el nuevo sistema.
- [ ] `foto_ella.jpeg` no se modifica accidentalmente.
- [ ] Cada fotografía aparece cuando ella misma se aproxima al viewport.
- [ ] Las fotografías inferiores no se animan prematuramente.
- [ ] El movimiento de entrada es sutil.
- [ ] El hover ya no entra bruscamente.
- [ ] El hover funciona solo en dispositivos apropiados.
- [ ] Mobile funciona correctamente.
- [ ] `prefers-reduced-motion` funciona.
- [ ] No hay librerías externas nuevas.
- [ ] No hay npm.
- [ ] No hay frameworks.
- [ ] No se rompió `loading="lazy"`.
- [ ] Los labels siguen siendo legibles.
- [ ] Los aspect ratios se conservan.
- [ ] No cambió el contenido de la página.
- [ ] No cambió la escena principal.
- [ ] No cambió la música.
- [ ] No cambió la carta.
- [ ] No aparecen errores en consola.
- [ ] El diff es pequeño y entendible.

---

# 48. Resultado que espero

No busco “más animación”.

Busco:

> **mejor movimiento.**

Las fotografías deberían sentirse como recuerdos que van apareciendo suavemente mientras Sofía recorre la página.

La animación debe acompañar el contenido emocional, no competir con él.

---

# 49. Procedimiento de ejecución

Sigue exactamente esta secuencia:

```text
1. Leer AGENTS.md
2. Leer GUIA-DE-APLICACION.md
3. Leer romantic-web-polish
4. Leer frontend-design
5. Leer emil-anim
6. Leer animate-css
7. Leer css-animation
8. Auditar index.html
9. Identificar fotoN.jpeg
10. Analizar animaciones actuales
11. Diseñar solución mínima
12. Implementar IntersectionObserver individual
13. Implementar entrada CSS
14. Refinar hover
15. Implementar reduced-motion
16. Probar desktop
17. Probar mobile
18. Revisar consola
19. Revisar git diff
20. Corregir únicamente regresiones
```

No saltes directamente al punto 12.

---

# 50. Entrega final

Una vez terminada la implementación, responde con:

## Cambios realizados

Explica brevemente qué cambiaste.

## Motivo

Explica por qué el nuevo movimiento es mejor que el anterior.

## Skills utilizadas

Indica exactamente qué reglas aplicaste de:

- `romantic-web-polish`
- `frontend-design`
- `emil-anim`
- `animate-css`, si la utilizaste
- `css-animation`, si la utilizaste

No afirmes haber usado una skill si no abriste su `SKILL.md`.

## Archivos modificados

Enuméralos.

## Verificación

Indica:

- desktop probado;
- mobile probado;
- `prefers-reduced-motion` probado;
- errores de consola;
- resultado de la revisión del diff.

## Decisiones que deliberadamente NO tomaste

Explica qué efectos descartaste por ser excesivos.

---

# Regla final

**No intentes impresionarme con la cantidad de animaciones.**

Impresióname con que la página se sienta más bonita sin que sea obvio por qué.

Si tienes que elegir entre:

```text
más efectos
```

y:

```text
mejor timing
```

elige siempre:

```text
mejor timing
```