# Ajuste de Proporciones para Imágenes

Para evitar que las imágenes de la galería se recorten al agrandarse o achicarse, debes asignarle a cada contenedor `<article class="gallery-item">` su proporción original exacta. 

De esta manera, el navegador sabrá exactamente qué forma tiene la foto y `object-fit: cover` no esconderá ninguna parte de la imagen.

**Instrucción general:** 
Ve a tu archivo `index.html`, busca cada etiqueta `<article class="gallery-item">` y agrégale el atributo `style="aspect-ratio: ANCHO / ALTO;"` según los valores que obtuve a continuación.

> [!WARNING]
> La imagen `assets/otros_momentos/foto12.jpeg` no fue encontrada en la carpeta. Verifica si el nombre está escrito correctamente o si falta subir el archivo.

---

### 🎬 Unas de nuestras salidas en el cine
```html
<!-- assets/cine/foto6.jpeg (Cuadrada) -->
<article class="gallery-item" style="aspect-ratio: 3072 / 3072;">
  <img src="assets/cine/foto6.jpeg" alt="Cine" loading="lazy" />
</article>

<!-- assets/cine/foto16.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 720 / 1183;">
  <img src="assets/cine/foto16.jpeg" alt="Cine" loading="lazy" />
</article>
```

### 💻 Nuestras llamadas
```html
<!-- assets/llamadas_meet/foto1.jpeg (Horizontal) -->
<article class="gallery-item" style="aspect-ratio: 1440 / 900;">
  <img src="assets/llamadas_meet/foto1.jpeg" alt="Meet" loading="lazy" />
</article>

<!-- assets/llamadas_meet/foto2.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 900 / 1440;">
  <img src="assets/llamadas_meet/foto2.jpeg" alt="Meet" loading="lazy" />
</article>

<!-- assets/llamadas_meet/foto3.jpeg (Horizontal) -->
<article class="gallery-item" style="aspect-ratio: 1440 / 900;">
  <img src="assets/llamadas_meet/foto3.jpeg" alt="Meet" loading="lazy" />
</article>
```

### ✨ Tú
```html
<!-- assets/fotos_de_ella/foto4.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/fotos_de_ella/foto4.jpeg" alt="Sofía" loading="lazy" />
</article>
```

### 🐶 Tú y Snoopy
```html
<!-- assets/fotos_de_ella/foto7.jpeg (Horizontal) -->
<article class="gallery-item" style="aspect-ratio: 1599 / 899;">
  <img src="assets/fotos_de_ella/foto7.jpeg" alt="Sofía y Snoopy" loading="lazy" />
</article>

<!-- assets/fotos_de_ella/foto8.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/fotos_de_ella/foto8.jpeg" alt="Sofía y Snoopy" loading="lazy" />
</article>
```

### 💪 En el gym
```html
<!-- assets/fotos_de_ella/foto10.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 1640 / 3072;">
  <img src="assets/fotos_de_ella/foto10.jpeg" alt="En el gym" loading="lazy" />
</article>
```

### 🤝 Caminando juntos
```html
<!-- assets/agarrados_de_la_mano/foto5.jpeg (Cuadrada) -->
<article class="gallery-item" style="aspect-ratio: 3072 / 3072;">
  <img src="assets/agarrados_de_la_mano/foto5.jpeg" alt="De la mano" loading="lazy" />
</article>

<!-- assets/agarrados_de_la_mano/foto9.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/agarrados_de_la_mano/foto9.jpeg" alt="Primer día de enamorados" loading="lazy" />
</article>

<!-- assets/agarrados_de_la_mano/foto11.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/agarrados_de_la_mano/foto11.jpeg" alt="Primer día que nos sentimos muy bien" loading="lazy" />
</article>
```

### 📷 Otros momentos bonitos
```html
<!-- assets/otros_momentos/foto12.jpeg -->
<!-- ⚠️ ¡ESTA FOTO NO FUE ENCONTRADA EN TU CARPETA! Verifica el nombre. -->

<!-- assets/otros_momentos/foto13.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 1200 / 1600;">
  <img src="assets/otros_momentos/foto13.jpeg" alt="Un gesto muy bonito" loading="lazy" />
</article>

<!-- assets/otros_momentos/foto14.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 1200 / 1600;">
  <img src="assets/otros_momentos/foto14.jpeg" alt="Momento" loading="lazy" />
</article>

<!-- assets/otros_momentos/foto15.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 720 / 1280;">
  <img src="assets/otros_momentos/foto15.jpeg" alt="Momento" loading="lazy" />
</article>

<!-- assets/otros_momentos/foto18.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/otros_momentos/foto18.jpeg" alt="Una salida bonita" loading="lazy" />
</article>

<!-- assets/otros_momentos/foto17.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/otros_momentos/foto17.jpeg" alt="La primera vez que le regalé flores" loading="lazy" />
</article>
```

### 🍳 Cocinando juntos
```html
<!-- assets/salida_cocina/foto19.jpeg (Cuadrada) -->
<article class="gallery-item" style="aspect-ratio: 3072 / 3072;">
  <img src="assets/salida_cocina/foto19.jpeg" alt="Cocina" loading="lazy" />
</article>

<!-- assets/salida_cocina/foto20.jpeg (Cuadrada) -->
<article class="gallery-item" style="aspect-ratio: 3072 / 3072;">
  <img src="assets/salida_cocina/foto20.jpeg" alt="Cocina" loading="lazy" />
</article>

<!-- assets/salida_cocina/foto21.jpeg (Vertical) -->
<article class="gallery-item" style="aspect-ratio: 899 / 1599;">
  <img src="assets/salida_cocina/foto21.jpeg" alt="Cocina" loading="lazy" />
</article>
```
