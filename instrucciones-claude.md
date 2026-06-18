# Instrucciones para Claude — CiTattoo

## Contexto del proyecto
SPA móvil en un solo archivo: `citattoo-mobile.html`. Sin framework, sin backend.
Toda la lógica, CSS y HTML están en ese archivo. Los assets están en `assets/fotos/`.

## Reglas de eficiencia

### Leer código
- Usar **Grep** con patrón exacto antes de Read. Solo hacer Read si Grep no es suficiente.
- Cuando necesito un bloque concreto, Grep con `-n` para saber la línea y Read con `offset`+`limit` ajustados — nunca leer el archivo entero.
- No releer después de editar. Edit confirma el cambio; confiar en eso.

### Editar
- Un solo Edit por bloque contiguo. Si hay varios cambios independientes en distintas zonas, hacer los Edits en paralelo en el mismo mensaje.
- No hacer Edit de verificación después del cambio. Si el string no se encuentra, Grep primero.
- Para CSS y HTML del mismo componente, cambiarlos en el mismo Edit si son adyacentes.

### Git y GitHub Pages
- Siempre encadenar en un solo Bash: `git add` → `git commit` → `git push` → `gh api pages/builds`.
- No verificar el estado del build con sleep loops. Lanzar el build y confirmar al usuario — él verifica en el navegador.
- Commit messages: una línea de título + co-author. Sin cuerpo largo.

### Respuestas al usuario
- No resumir lo que se acaba de hacer si el diff es obvio.
- No listar todos los cambios CSS uno a uno al final — con una frase basta.
- Si el usuario dice "perfecto" o aprueba sin comentarios, pasar al siguiente tema sin recap.

### Patrones que desperdician tokens
- ❌ Leer 100 líneas para encontrar 3
- ❌ Verificar `gh api pages` antes y después del build
- ❌ Hacer Grep + Read + Grep de nuevo para lo mismo
- ❌ Explicar el plan completo antes de ejecutar cuando la tarea es clara
- ❌ Resumir cambios que el usuario ya ve en el navegador

## Stack técnico
- CSS: variables `--text`, `--text2`, `--text3`, `--bg`, `--white`, `--border`, `--border2`, `--accent`
- Iconos: SVG symbols con `<use href="#ic-*">` — todos definidos en el bloque SVG defs al inicio del body
- Pantallas: `.screen` + `.screen.on`, navegación con `showScreen()` / `go()`
- Datos: objeto `PROS` con datos de cada tatuador, array `MASONRY_POSTS`
- GitHub Pages: rama `main`, raíz `/`, URL `https://roblesgg.github.io/CiTattoo/`

## Patrones de diseño establecidos
- Blur de fondo en área de info: `filter:blur(48px);transform:scale(1.5)` + overlay `rgba(255,255,255,.28)`
- Frosted glass sobre foto: `background:rgba(0,0,0,.38);backdrop-filter:blur(14px);border-radius:12px`
- Avatares circulares superpuestos entre banner e info: `position:absolute;bottom:-Xpx` donde X = mitad del avatar
- Texto blanco sobre banner: scrim `rgba(0,0,0,.18)` + pill frosted glass en el contenido
- Sin scrollbars: `scrollbar-width:none` + `*::-webkit-scrollbar{display:none}` ya global
