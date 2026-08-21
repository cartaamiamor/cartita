# Plan de implementación — Universo flotante de recuerdos

## Objetivo

Transformar la sección actual de fotografías en una experiencia visual animada donde **todas las imágenes `foto#.jpeg` estén flotando alrededor de la pantalla**, moviéndose continuamente de forma orgánica, impredecible y ligeramente caótica.

Cada fotografía debe mostrar debajo:

1. **Categoría**
2. **Descripción**

Las fotografías deben tener profundidad visual, cruzarse entre sí y moverse en distintas direcciones.

Además:

- todas deben tener oportunidades de pasar por el primer plano;
- ninguna debe quedar permanentemente escondida detrás;
- al hacer click/tap sobre una fotografía, esa fotografía debe trasladarse inmediatamente al frente;
- la foto seleccionada debe poder verse claramente;
- las demás siguen existiendo alrededor sin destruir la composición.

La experiencia buscada es:

> una constelación viva de recuerdos flotando alrededor del usuario.

No debe parecer:

> un grid moviéndose.

---

# 1. Skills que debes leer primero

Antes de modificar código, lee en este orden:

```text
AGENTS.md

GUIA-DE-APLICACION.md

.agents/skills/romantic-web-polish/SKILL.md

.agents/skills/frontend-design/SKILL.md

.agents/skills/emil-anim/SKILL.md

.agents/skills/animate-css/SKILL.md

.agents/skills/css-animation/SKILL.md
```

Jerarquía de decisión:

```text
romantic-web-polish
        ↓
frontend-design
        ↓
emil-anim
        ↓
css-animation
        ↓
animate-css
```

Para esta tarea `css-animation` tiene más relevancia que en una animación convencional porque estamos construyendo una composición espacial compleja.

Aun así:

**NO utilices librerías externas.**

Todo debe permanecer en:

```text
HTML
CSS
JavaScript vanilla
```

Compatible con GitHub Pages.

---

# 2. Alcance

La animación se aplica exclusivamente a imágenes cuyo archivo cumpla:

```regex
^foto\d+\.jpeg$
```

Ejemplos:

```text
foto1.jpeg
foto2.jpeg
foto6.jpeg
foto17.jpeg
foto21.jpeg
```

Pueden encontrarse dentro de distintas carpetas.

Por ejemplo:

```text
assets/cine/foto6.jpeg
assets/llamadas_meet/foto1.jpeg
assets/fotos_de_ella/foto8.jpeg
assets/agarrados_de_la_mano/foto11.jpeg
assets/otros_momentos/foto18.jpeg
assets/salida_cocina/foto21.jpeg
```

## Excluir

No incluir:

```text
foto_ella.jpeg
```

porque no cumple el patrón numérico.

---

# 3. Concepto visual

Imagínate muchas fotografías físicas suspendidas en una habitación sin gravedad.

Cada foto:

- deriva lentamente;
- cambia ligeramente de dirección;
- se acerca;
- se aleja;
- pasa parcialmente detrás de otras;
- después vuelve a acercarse;
- rota unos pocos grados;
- nunca sigue exactamente el mismo recorrido que otra.

La escena debe sentirse viva.

No deben recorrer círculos perfectos.

No deben moverse todas igual.

No deben moverse sincronizadas.

---

# 4. Movimiento deliberadamente irregular

Quiero movimiento **errático**, pero visualmente agradable.

Errático NO significa:

```text
teletransportarse
vibrar
saltar
cambiar violentamente de dirección
```

Significa:

```text
trayectorias no repetitivas
+
velocidades diferentes
+
direcciones diferentes
+
profundidades diferentes
+
pequeñas rotaciones diferentes
```

Piensa:

> hojas flotando suavemente con corrientes de aire impredecibles.

No:

> objetos rebotando en un salvapantallas de DVD.

---

# 5. Sistema espacial

Crear una zona de animación específica.

Conceptualmente:

```html
<section class="memory-universe">

    <div class="floating-photo">
        <img>
        <div class="photo-meta">
            <span class="photo-category"></span>
            <p class="photo-description"></p>
        </div>
    </div>

</section>
```

No es obligatorio utilizar exactamente estas clases.

La zona debe ocupar aproximadamente:

```css
width: 100%;
height: 100svh;
```

o una altura suficientemente grande para permitir el movimiento.

Debe sentirse como una escena.

---

# 6. NO utilizar el grid convencional durante esta experiencia

La estructura actual:

```css
.gallery-grid
```

no debe seguir controlando visualmente las posiciones de las fotografías durante la experiencia flotante.

Puede conservarse la información semántica original.

Pero visualmente la composición debe convertirse en:

```text
posición libre
+
profundidad
+
movimiento
```

---

# 7. Categoría

La categoría debe obtenerse automáticamente de la sección en la que actualmente se encuentra cada fotografía.

Ejemplo:

```text
🎬 Unas de nuestras salidas en el cine
```

produce:

```text
Cine
```

Otro ejemplo:

```text
🐶 Tú y Snoopy
```

produce:

```text
Tú y Snoopy
```

Otro:

```text
🤝 De la mano
```

produce:

```text
De la mano
```

No escribas manualmente una tabla duplicada de categorías si el HTML ya contiene esa información.

---

# 8. Descripción

Para cada fotografía:

### Prioridad 1

Si existe:

```html
.gallery-label
```

usa su contenido como descripción.

### Prioridad 2

Si no existe `.gallery-label`, usa el `alt` de la fotografía.

### Prioridad 3

Si el `alt` es demasiado genérico como:

```text
Momento
Cine
Meet
Cocina
```

puedes mostrar únicamente la categoría debajo de la fotografía.

NO inventes historias que no están presentes en el HTML.

---

# 9. Diseño de cada fotografía

Cada elemento flotante debe sentirse como una pequeña pieza de recuerdo.

Conceptualmente:

```text
┌────────────────────┐
│                    │
│     FOTOGRAFÍA     │
│                    │
└────────────────────┘

       Categoría

   descripción breve
```

No utilizar una tarjeta pesada de dashboard.

Evitar:

```text
borders enormes
glassmorphism exagerado
cards SaaS
botones innecesarios
```

La fotografía debe dominar.

---

# 10. Texto debajo

Debajo de cada fotografía debe aparecer:

### Categoría

Pequeña y claramente diferenciada.

Ejemplo:

```text
DE LA MANO
```

### Descripción

Debajo:

```text
Primer día de enamorados
23/07/2026
```

o la descripción existente.

El texto debe moverse junto con la fotografía.

Es decir:

```text
foto + metadata
```

forman un solo objeto flotante.

---

# 11. Profundidad

Cada fotografía debe tener una profundidad virtual.

Define conceptualmente:

```javascript
depth
```

con valores, por ejemplo:

```text
0 → fondo
1 → medio
2 → cerca
3 → primer plano
```

Pero es preferible utilizar un valor continuo:

```text
0.0 → 1.0
```

La profundidad debe afectar:

```text
scale
opacity
z-index
shadow
```

Ejemplo conceptual:

### Fondo

```text
scale: 0.65
opacity: 0.55
```

### Medio

```text
scale: 0.82
opacity: 0.8
```

### Frente

```text
scale: 1
opacity: 1
```

No hagas las del fondo ilegibles.

---

# 12. TODAS deben llegar al frente

Esta es una regla obligatoria.

No quiero que Gemini interprete:

> algunas fotos están delante y otras detrás.

Quiero un sistema donde:

> **cada fotografía tenga eventualmente su momento en el primer plano.**

Es decir:

```text
foto1
↓
en algún momento llega al frente

foto2
↓
en algún momento llega al frente

foto3
↓
en algún momento llega al frente

...

fotoN
↓
en algún momento llega al frente
```

Ninguna fotografía debe quedar permanentemente relegada al fondo.

---

# 13. Sistema de profundidad dinámica

Implementa una cola o scheduler de protagonismo.

Conceptualmente:

```text
foto A → frente

unos segundos después

foto B → frente

unos segundos después

foto C → frente

...
```

Mientras tanto todas continúan moviéndose.

No tienen que hacer una fila evidente.

El cambio debe sentirse natural.

---

# 14. Aleatoriedad controlada

NO utilizar simplemente:

```javascript
Math.random()
```

en cada frame.

Eso produciría jitter.

La aleatoriedad debe generar **destinos**, no movimiento frame a frame.

Ejemplo conceptual:

```text
posición actual
     ↓
generar nuevo destino aleatorio
     ↓
interpolar suavemente
     ↓
llegar
     ↓
generar otro destino
```

Así:

```text
random target
+
smooth interpolation
```

produce movimiento errático pero fluido.

---

# 15. Waypoints

Cada fotografía puede tener un sistema de waypoints:

```javascript
{
    x,
    y,
    rotation,
    scale,
    duration
}
```

Cuando termina un movimiento:

```text
generar siguiente waypoint
```

Los rangos deben ser limitados.

Por ejemplo:

```text
X:
5% → 85%

Y:
8% → 78%

rotación:
-6deg → +6deg
```

No utilices rotaciones grandes.

---

# 16. Evitar salida de pantalla

Las fotos NO deben perderse fuera de la pantalla.

Siempre conservar aproximadamente:

```text
80–90%
```

del objeto visible.

No permitir posiciones donde:

```text
solo se ve una esquina
```

salvo quizá durante una transición extremadamente breve.

---

# 17. Tamaño variable

No todas deben tener exactamente el mismo tamaño.

Las fotografías de fondo pueden ser más pequeñas.

Las fotografías cercanas pueden aumentar ligeramente.

Pero conserva sus aspect ratios originales.

No fuerces:

```css
aspect-ratio: 4 / 5;
```

a todas.

---

# 18. Velocidad

Las fotografías deben tener distintas velocidades.

Ejemplo conceptual:

```text
foto A → 9 segundos
foto B → 13 segundos
foto C → 7 segundos
foto D → 16 segundos
```

para alcanzar su siguiente waypoint.

Eso evita sincronización.

---

# 19. Ninguna sincronización

PROHIBIDO:

```text
todas van izquierda
todas paran
todas van arriba
todas escalan juntas
```

Cada una debe poseer:

```text
posición
velocidad
dirección
rotación
profundidad
```

independientes.

---

# 20. La fotografía protagonista

En cualquier instante puede existir:

```text
activePhoto
```

Cuando una foto se convierte naturalmente en protagonista:

```text
z-index → alto
scale → mayor
opacity → 1
```

Debe acercarse suavemente.

No aparecer instantáneamente.

---

# 21. Click en una fotografía

Al hacer:

```text
click
```

o:

```text
tap
```

sobre una foto:

esa fotografía debe convertirse inmediatamente en:

```text
activePhoto
```

---

# 22. Comportamiento al seleccionar

La foto elegida debe animarse hacia una posición de primer plano.

Idealmente:

```text
centro o ligeramente desplazada del centro
```

Debe:

```text
aumentar scale
aumentar z-index
aumentar claridad
reducir ligeramente movimiento
```

El usuario debe poder observarla claramente.

---

# 23. No utilizar un modal tradicional

No quiero:

```text
click
↓
modal blanco
↓
fondo bloqueado
```

Quiero que siga sintiéndose parte de la misma escena.

La fotografía simplemente:

> emerge desde la constelación y se acerca al usuario.

---

# 24. Estado seleccionado

Conceptualmente:

```css
.floating-photo.is-selected
```

debe tener:

```text
z-index muy alto
scale mayor
opacity 1
```

y una transformación que la lleve al frente.

---

# 25. Fotografías no seleccionadas

Cuando una fotografía sea seleccionada:

las demás pueden:

```text
alejarse ligeramente
+
reducir opacity un poco
+
seguir flotando lentamente
```

NO deben desaparecer.

Queremos mantener la sensación de universo alrededor.

---

# 26. Click otra fotografía

Si mientras una está activa el usuario pulsa otra:

```text
foto anterior
↓
vuelve suavemente al universo

foto nueva
↓
viene al frente
```

No requiere cerrar primero.

---

# 27. Cerrar selección

Permite retirar la selección mediante:

```text
click sobre el fondo
```

o:

```text
Escape
```

En móvil puede utilizarse:

```text
tap en el fondo
```

Entonces la fotografía regresa suavemente a su movimiento normal.

---

# 28. Interacción con drag

NO implementar drag inicialmente.

No añadirlo salvo que después de completar todo lo anterior exista una razón visual clara.

La primera versión debe utilizar:

```text
click/tap
```

exclusivamente.

---

# 29. Movimiento mientras una foto está seleccionada

La fotografía seleccionada puede:

```text
quedarse casi quieta
```

o tener solamente:

```text
un movimiento extremadamente pequeño
```

para que el usuario pueda verla.

Las demás continúan flotando.

---

# 30. Prioridad manual sobre automática

Si el usuario selecciona una foto:

```text
selección manual
>
scheduler automático
```

Mientras exista una fotografía seleccionada manualmente:

NO cambiar automáticamente de protagonista.

---

# 31. Reiniciar scheduler

Cuando el usuario cierre la fotografía:

espera un pequeño periodo antes de reactivar el ciclo automático.

No inmediatamente.

---

# 32. Colisiones visuales

No necesitas implementar física real.

Pero evita que todas las fotografías terminen amontonadas.

Al generar nuevos destinos, comprueba aproximadamente la distancia respecto a otras fotografías.

Puede utilizarse una heurística sencilla:

```text
si nuevo destino está demasiado cerca
↓
generar otro
```

No crear un motor de física.

---

# 33. Las superposiciones SON permitidas

No hay que evitar todas las colisiones.

Quiero que algunas fotografías se crucen.

Eso crea profundidad.

Pero evita:

```text
10 fotos completamente una encima de otra
```

---

# 34. Movimiento tridimensional simulado

Puedes utilizar:

```css
perspective
translate3d
scale
rotate
```

para generar sensación de profundidad.

No es necesario WebGL.

No utilizar Three.js.

---

# 35. NO usar canvas

Las fotografías tienen:

- textos;
- descripciones;
- click;
- accesibilidad.

Por eso mantenerlas como elementos DOM.

No dibujarlas dentro de `<canvas>`.

---

# 36. Rendimiento

Priorizar:

```css
transform
opacity
```

No animar constantemente:

```css
top
left
width
height
box-shadow
filter
```

Las posiciones animadas deben utilizar:

```css
transform: translate3d(...)
```

---

# 37. Arquitectura JavaScript

Mantén un estado por fotografía.

Conceptualmente:

```javascript
{
    element,
    x,
    y,
    targetX,
    targetY,
    rotation,
    targetRotation,
    depth,
    targetDepth,
    speed,
    selected
}
```

No necesitas utilizar exactamente esta estructura.

Pero quiero una arquitectura legible.

---

# 38. No crear CSS específico para cada foto

PROHIBIDO:

```css
.photo-1 {}
.photo-2 {}
.photo-3 {}
...
```

Las diferencias deben asignarse dinámicamente mediante:

```text
CSS custom properties
```

o JavaScript.

Ejemplo:

```css
--x
--y
--rotation
--depth
```

---

# 39. CSS custom properties recomendadas

Considera:

```css
--photo-x
--photo-y
--photo-rotation
--photo-scale
--photo-opacity
--photo-z
```

y utiliza esas propiedades desde:

```css
transform
```

si produce una arquitectura más limpia.

---

# 40. No hacer movimiento frame a frame con React

No existe React.

No añadir React.

No añadir npm.

No añadir paquetes.

---

# 41. `requestAnimationFrame`

Si realmente necesitas interpolación personalizada:

utiliza:

```javascript
requestAnimationFrame()
```

No:

```javascript
setInterval(..., 16)
```

Pero antes evalúa si puede resolverse simplemente mediante:

```text
CSS transitions
+
nuevos destinos generados desde JS
```

Esa opción será normalmente más simple.

---

# 42. Estrategia preferida de movimiento

Primera opción a intentar:

```text
JS genera waypoint
↓
asigna custom properties
↓
CSS transition mueve la fotografía
↓
transitionend
↓
JS genera siguiente waypoint
```

Esto evita un loop permanente de JavaScript.

---

# 43. Movimiento errático sin perder elegancia

Combina:

```text
duración variable
+
easing variable dentro de pocas opciones
+
waypoints diferentes
+
rotación mínima
+
profundidad cambiante
```

No combines:

```text
bounce
elastic
shake
wobble
flash
```

---

# 44. Easing

Utiliza curvas suaves.

Puedes reutilizar:

```css
--spring-smooth
--spring-snappy
```

del proyecto.

No abuses de:

```text
spring-bounce
```

---

# 45. Foto en primer plano automático

Cada cierto tiempo una fotografía diferente debe recibir:

```text
foreground priority
```

y migrar suavemente hacia una posición cercana al usuario.

No siempre exactamente al centro.

Ejemplos:

```text
centro
centro izquierda
centro derecha
```

con variaciones pequeñas.

---

# 46. Cola justa

No elegir permanentemente fotografías al azar.

De lo contrario algunas podrían:

```text
salir 5 veces al frente
```

antes de que otra aparezca una vez.

Utiliza una cola barajada.

Conceptualmente:

```javascript
shuffle(allPhotos)
```

y recorrerla completa.

Cuando termina:

```text
shuffle otra vez
```

Así garantizas:

> todas terminan pasando por delante.

---

# 47. Fotografía seleccionada manualmente

Cuando el usuario haga click:

retírala temporalmente de cualquier cambio automático de profundidad.

Tiene prioridad absoluta.

---

# 48. Metadata visible incluso en movimiento

La categoría y descripción deben permanecer debajo de la fotografía.

No hacer que aparezcan únicamente en hover.

Esto es especialmente importante para móvil.

---

# 49. Jerarquía tipográfica

Categoría:

```text
pequeña
uppercase opcional
letter-spacing ligero
rosa/lavanda/dorado suave
```

Descripción:

```text
más pequeña
blanco/crema
máximo 2–3 líneas
```

No permitir grandes bloques de texto flotando.

---

# 50. Descripciones largas

Cuando `.gallery-label` contenga demasiado texto:

limita visualmente la anchura.

No borrar contenido del HTML.

Puedes utilizar:

```css
max-width
line-height
```

y ajustar tamaño.

Evita textos diminutos.

---

# 51. Fondo

Conservar la identidad nocturna existente.

La sección debería sentirse integrada con:

```text
lavanda
rosa
azul noche
luna
dorado
```

No introducir un fondo blanco.

---

# 52. Visibilidad

Las fotografías necesitan suficiente contraste.

No permitir:

```text
foto oscura
+
fondo oscuro
+
sin separación
```

Puede utilizarse una sombra sutil.

---

# 53. Capa ambiental

Opcionalmente pueden existir elementos muy sutiles detrás:

```text
estrellas
partículas
brillos
```

pero solamente si reutilizas elementos existentes.

NO hagas otro espectáculo de partículas.

Las fotografías son las protagonistas.

---

# 54. Entrada a la sección

Al llegar por primera vez a la sección:

las fotografías pueden aparecer desde posiciones dispersas y empezar a moverse.

No tienen que aparecer todas simultáneamente.

Utiliza stagger pequeño.

Después:

```text
comienza el universo flotante
```

---

# 55. Primera composición

La posición inicial debería distribuir las fotografías por toda la escena.

Ejemplo:

```text
arriba izquierda
centro derecha
abajo
medio
bordes
```

No generar todas desde:

```text
center center
```

---

# 56. Densidad

Como existen muchas fotografías, debe garantizarse que la escena siga siendo comprensible.

Las del fondo pueden:

```text
reducirse
```

Las cercanas:

```text
crecer
```

Eso permite colocar más elementos sin que parezca un collage plano.

---

# 57. Responsive

Desktop y móvil NO necesitan utilizar exactamente el mismo movimiento.

## Desktop

Puede utilizarse todo el viewport.

## Mobile

Reducir:

```text
cantidad de desplazamiento
rotación
scale máximo
velocidad
```

Las fotos pueden ser algo más pequeñas para evitar saturación.

---

# 58. Pantallas pequeñas

En una pantalla tipo:

```text
390 × 844
```

las fotos seleccionadas deben caber completamente.

No permitir que el texto se salga por debajo del viewport.

---

# 59. Reduced motion

Si:

```text
prefers-reduced-motion: reduce
```

está activo:

DESACTIVAR completamente el movimiento continuo.

Mostrar una alternativa estable.

Por ejemplo:

```text
grid/masonry estático
```

o una distribución inmóvil.

Click para destacar una fotografía puede mantenerse con una transición mínima o instantánea.

---

# 60. No esconder contenido para reduced-motion

Todas las fotografías y sus textos deben seguir siendo accesibles.

---

# 61. Accesibilidad

Los objetos seleccionables necesitan una interacción accesible.

Si una fotografía funciona como control interactivo:

considera permitir:

```text
Enter
Space
```

además del click.

No rompas los `alt`.

---

# 62. Cursor

En desktop:

```css
cursor: pointer;
```

sobre fotografías seleccionables.

Puedes usar:

```text
grab
```

solamente si implementas drag.

Como no implementaremos drag inicialmente:

usa `pointer`.

---

# 63. Click vs movimiento

CRÍTICO:

Una fotografía moviéndose sigue necesitando ser fácil de pulsar.

No permitas velocidades tan altas que sea difícil hacer click.

Cuando:

```text
pointerenter
```

en desktop:

puedes reducir o pausar temporalmente su movimiento.

Esto mejora muchísimo la interacción.

---

# 64. Hover

En desktop:

```css
@media (hover:hover) and (pointer:fine)
```

puedes utilizar:

```text
pequeño scale
+
mayor claridad
```

No hacer hover agresivo.

---

# 65. Al pasar mouse

Una buena opción:

```text
pointer enters photo
↓
foto desacelera
↓
usuario puede clickear
```

Al salir:

```text
retoma su trayectoria
```

---

# 66. Evitar movimientos mareantes

Aunque el usuario pidió movimiento errático:

NO mover constantemente todo a grandes velocidades.

El caos debe ser espacial, no agresivo.

Objetivo:

```text
impredecible
pero suave
```

---

# 67. Ritmo visual

Combina aproximadamente:

```text
70% movimiento lento
20% movimiento medio
10% transición de protagonismo
```

No quiero:

```text
100% hiperactividad
```

---

# 68. Fotografías protagonistas

Cuando una foto entra al frente automáticamente:

puede mantenerse allí brevemente.

Después:

```text
vuelve suavemente al flujo
```

y otra toma protagonismo.

---

# 69. No bloquear scroll

El usuario debe poder continuar desplazándose por la página.

No secuestrar:

```text
wheel
touchmove
scroll
```

---

# 70. Posición sticky — evaluar

Puedes evaluar:

```css
position: sticky;
top: 0;
height: 100svh;
```

para que durante un tramo del scroll el universo ocupe la pantalla.

Pero SOLO impleméntalo si mejora claramente la experiencia.

No debes bloquear al usuario durante diez scrolls.

---

# 71. Opción recomendada

Recomiendo una sección aproximadamente:

```text
120–180vh
```

con una escena:

```text
position: sticky
height: 100svh
```

durante una parte razonable del recorrido.

Esto permite observar el universo de fotografías mientras se navega.

Pero mantenlo corto.

---

# 72. Click no debe afectar scroll

Seleccionar una fotografía:

NO debe cambiar la posición del documento.

No utilizar:

```javascript
scrollIntoView()
```

para seleccionarla.

Debe venir hacia el usuario.

---

# 73. Imágenes lazy

Mantener:

```html
loading="lazy"
```

cuando siga siendo apropiado.

Si el nuevo sistema necesita tener fotografías disponibles inmediatamente al entrar a la escena, evalúa cuidadosamente la distancia del lazy loading.

No desactives lazy loading globalmente sin motivo.

---

# 74. Precarga

Si necesitas evitar flashes al iniciar el universo:

puedes esperar a que cada imagen esté cargada antes de animarla.

Pero NO esperar a que absolutamente todas las imágenes del sitio carguen para mostrar la página.

---

# 75. Estados

Diseña como mínimo:

```text
floating
foreground
selected
paused
reduced-motion
```

Evita añadir estados innecesarios.

---

# 76. Clases conceptuales

Ejemplo:

```css
.floating-photo
.floating-photo.is-foreground
.floating-photo.is-selected
.floating-photo.is-paused
```

Puedes elegir mejores nombres.

---

# 77. Arquitectura automática

NO modificar manualmente 21 imágenes una por una.

Gemini debe:

1. localizar todas las fotos que cumplan `fotoN.jpeg`;
2. detectar su categoría;
3. detectar su descripción;
4. construir o enriquecer automáticamente el elemento flotante;
5. registrarlo en el sistema de animación.

---

# 78. No duplicar contenido

Si el texto ya existe como:

```html
.gallery-label
```

no crear otra copia fija independiente que quede duplicada en DOM sin propósito.

Reutiliza o reorganiza la estructura.

---

# 79. Mantener contenido original

No cambiar:

```text
descripciones
fechas
categorías
alt text
rutas
```

salvo corregir exclusivamente errores técnicos necesarios.

---

# 80. No tocar otras secciones

Mantener intactos:

```text
hero
mar
luna
estrellas
countdown
Sofía
cosas que adoro
carta
música
sorpresa
footer
```

La tarea está centrada en:

```text
Nuestros Momentos
```

---

# 81. No reescribir index.html completo

Realizar cambios localizados.

Al terminar:

```bash
git diff -- index.html
```

y revisar que no se haya reformateado todo el documento.

---

# 82. Fase 1 — Auditoría

Antes de implementar:

- encuentra `#momentos`;
- encuentra todas las `.gallery-grid`;
- encuentra todas las `.gallery-item`;
- encuentra todos los `fotoN.jpeg`;
- identifica `gallery-title`;
- identifica `.gallery-label`;
- revisa animaciones existentes;
- revisa IntersectionObserver existente.

---

# 83. Fase 2 — Modelo de datos

Construye automáticamente una colección conceptual:

```javascript
[
    {
        element,
        image,
        category,
        description
    }
]
```

No hardcodear valores que ya se encuentran en DOM.

---

# 84. Fase 3 — Escena

Transforma exclusivamente la presentación visual de `#momentos` en una escena flotante.

---

# 85. Fase 4 — Posiciones iniciales

Genera posiciones iniciales suficientemente separadas.

No usar puro random sin validación.

---

# 86. Fase 5 — Movimiento

Implementa waypoints dinámicos independientes.

---

# 87. Fase 6 — Profundidad

Asigna y anima:

```text
depth
scale
opacity
z-index
```

---

# 88. Fase 7 — Scheduler de primer plano

Crear una cola justa que garantice que todas las fotos lleguen al frente.

---

# 89. Fase 8 — Click/tap

Implementar selección manual y prioridad sobre scheduler.

---

# 90. Fase 9 — Metadata

Garantizar categoría y descripción debajo de cada fotografía.

---

# 91. Fase 10 — Responsive

Ajustar desktop y mobile independientemente.

---

# 92. Fase 11 — Reduced motion

Crear versión estática usable.

---

# 93. Fase 12 — Performance

Medir visualmente:

```text
scroll
CPU
fluidez
jank
```

No debería sentirse pesado.

---

# 94. Pruebas obligatorias

Probar al menos:

```text
1920×1080
1366×768
390×844
```

---

# 95. Prueba de justicia del scheduler

Dejar correr la animación suficiente tiempo.

Verificar que:

```text
todas las fotografías
```

pasan eventualmente por el primer plano.

No solamente las mismas cinco.

---

# 96. Prueba de interacción

Mientras varias fotos se mueven:

hacer click en una que esté:

```text
al fondo
```

Debe inmediatamente:

```text
venir al frente
```

sin saltar.

---

# 97. Prueba cambio de selección

```text
seleccionar A
↓
seleccionar B
```

Resultado:

```text
A vuelve
B viene al frente
```

suavemente.

---

# 98. Prueba de cerrar

```text
seleccionar A
↓
click en fondo
```

A debe regresar al universo.

---

# 99. Prueba Escape

En desktop:

```text
seleccionar foto
↓
Escape
```

debe cerrar selección.

---

# 100. Criterios de aceptación

- [ ] Todas las `fotoN.jpeg` participan.
- [ ] `foto_ella.jpeg` queda fuera.
- [ ] Todas las fotografías están distribuidas por la pantalla.
- [ ] Todas se mueven continuamente.
- [ ] Las trayectorias son distintas.
- [ ] El movimiento se siente errático pero fluido.
- [ ] No existe jitter.
- [ ] Las fotografías tienen profundidad.
- [ ] Se cruzan parcialmente.
- [ ] Todas eventualmente pasan por el frente.
- [ ] Existe un scheduler justo.
- [ ] Hacer click lleva inmediatamente una foto al frente.
- [ ] La selección manual pausa el cambio automático de protagonista.
- [ ] Click en otra cambia de fotografía.
- [ ] Click en fondo cierra.
- [ ] Escape cierra.
- [ ] Categoría siempre visible.
- [ ] Descripción siempre visible.
- [ ] La metadata viaja junto con la foto.
- [ ] Se conservan aspect ratios.
- [ ] Funciona en móvil.
- [ ] No existe falso hover móvil.
- [ ] `prefers-reduced-motion` funciona.
- [ ] No se agregó React.
- [ ] No se agregó npm.
- [ ] No se agregó Three.js.
- [ ] No se agregó Canvas.
- [ ] No se agregó GSAP.
- [ ] No se rompieron otras secciones.
- [ ] No hay errores en consola.
- [ ] El diff es localizado.

---

# Resultado emocional buscado

La sección no debe sentirse como:

> una galería de imágenes.

Debe sentirse como:

> entrar dentro de los recuerdos del primer mes.

Las fotografías están flotando alrededor.

Algunas están lejos.

Otras se acercan.

Se cruzan.

Se alejan.

Cada cierto tiempo una se convierte en protagonista.

Y cuando el usuario reconoce una fotografía y la toca:

> ese recuerdo sale de entre todos los demás y viene hacia él.

Ese es el efecto que quiero.

---

# Instrucción final para Gemini

No empieces escribiendo código inmediatamente.

Haz primero:

```text
READ
↓
AUDIT
↓
DESIGN
↓
IMPLEMENT
↓
TEST
↓
REFINE
```

Utiliza las skills proporcionadas como criterios reales de decisión.

No conviertas el concepto de “movimiento errático” en movimiento desagradable.

La regla principal es:

> caos en las trayectorias, suavidad en la percepción.

Y la regla de interacción más importante es:

> cualquier fotografía que el usuario toque debe ganar inmediatamente la profundidad y venir al primer plano.