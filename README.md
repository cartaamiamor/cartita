# Primer mes con Sofía 💗

Sitio estático listo para GitHub Pages.

## Personalizar

Edita `index.html`. Busca los comentarios `PERSONALIZA` y cambia la carta por tus propias palabras.

### Fotos opcionales
Guarda tus imágenes en `assets/` con estos nombres:

- `sofia.jpg` — foto principal
- `cine.jpg` — recuerdo del cine
- `crepes.jpg` — una foto relacionada con crepes/salida
- `luna.jpg` — una foto de la luna o de ustedes de noche

Si esas imágenes no existen, el sitio mantiene placeholders y no se rompe.

## Publicar gratis en GitHub Pages

1. Crea un repositorio en GitHub, por ejemplo `primer-mes-sofia`.
2. Sube todo el contenido de esta carpeta a la raíz del repositorio.
3. En GitHub abre **Settings → Pages**.
4. Elige publicar desde la rama principal (`main`) y la carpeta raíz (`/ root`) si esa opción aparece en tu configuración.
5. Guarda. GitHub mostrará la URL del sitio cuando la publicación esté activa.

## Usar con Codex

El repo incluye:

- `AGENTS.md`: reglas permanentes para que Codex no destruya la estética ni la compatibilidad con GitHub Pages.
- `.agents/skills/romantic-web-polish/SKILL.md`: skill local específica para pulir esta página.
- `.agents/skills/README.md`: catálogo de todas las skills disponibles (incluye 4 skills de diseño/animación descargadas de GitHub para lograr efectos bonitos sin caer en la estética típica "hecha por IA"), con qué hace cada una y cómo invocarla.
- `.agents/skills/GUIA-DE-APLICACION.md`: guía profunda que audita el `index.html` actual sección por sección contra esas skills (qué efecto quitar, cuál reforzar, prioridades concretas).

Prompt recomendado para Codex:

> Usa la skill `romantic-web-polish`. Revisa el sitio completo, mejora la dirección visual y la experiencia móvil sin convertirlo en una landing page genérica. Mantén GitHub Pages, HTML/CSS/JS estático y conserva el contenido personalizado de Sofía. Valida visualmente antes de terminar.

También puedes usar el plugin oficial **Build Web Apps** de OpenAI en Codex, especialmente sus skills `frontend-app-builder` y `frontend-testing-debugging`.
