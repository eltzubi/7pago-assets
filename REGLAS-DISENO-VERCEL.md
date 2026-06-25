# Directrices de Interfaz Web

Revisa estos archivos para verificar su cumplimiento: $ARGUMENTS

Lee los archivos y contrasta con las reglas a continuación. El resultado debe ser conciso pero completo — sacrifica gramática por brevedad. Alta señal, bajo ruido.

## Reglas

### Accesibilidad

- Los botones con solo ícono necesitan `aria-label`
- Los controles de formulario necesitan `<label>` o `aria-label`
- Los elementos interactivos necesitan manejadores de teclado (`onKeyDown` / `onKeyUp`)
- Usar `<button>` para acciones, `<a>` / `<Link>` para navegación (nunca `<div onClick>`)
- Las imágenes necesitan `alt` (o `alt=""` si son decorativas)
- Los íconos decorativos necesitan `aria-hidden="true"`
- Las actualizaciones asíncronas (toasts, validaciones) necesitan `aria-live="polite"`
- Usar HTML semántico (`<button>`, `<a>`, `<label>`, `<table>`) antes de ARIA
- Encabezados jerárquicos `<h1>` – `<h6>`; incluir enlace de salto al contenido principal
- `scroll-margin-top` en los anclajes de encabezado

### Estados de Foco

- Los elementos interactivos necesitan foco visible: `focus-visible:ring-*` o equivalente
- Nunca usar `outline-none` / `outline: none` sin un reemplazo de foco
- Usar `:focus-visible` sobre `:focus` (evitar anillo de foco al hacer clic)
- Agrupar foco con `:focus-within` para controles compuestos

### Formularios

- Los inputs necesitan `autocomplete` y un `name` significativo
- Usar el `type` correcto (`email`, `tel`, `url`, `number`) e `inputmode`
- Nunca bloquear el pegado (`onPaste` + `preventDefault`)
- Etiquetas clicables (`htmlFor` o envolviendo el control)
- Desactivar spellcheck en emails, códigos y nombres de usuario (`spellCheck={false}`)
- Checkboxes/radios: la etiqueta y el control comparten un único objetivo de clic (sin zonas muertas)
- El botón de envío permanece activo hasta que comienza la solicitud; spinner durante la petición
- Errores en línea junto a los campos; enfocar el primer error al enviar
- Los placeholders terminan con `…` y muestran un patrón de ejemplo
- `autocomplete="off"` en campos que no sean de autenticación para evitar activación del gestor de contraseñas
- Advertir antes de navegar con cambios sin guardar (evento `beforeunload` o guardia del router)

### Animación

- Respetar `prefers-reduced-motion` (proporcionar variante reducida o desactivar)
- Animar solo `transform` / `opacity` (propiedades compatibles con el compositor)
- Nunca usar `transition: all` — listar las propiedades explícitamente
- Establecer el `transform-origin` correcto
- SVG: transformaciones en el contenedor `<g>` con `transform-box: fill-box; transform-origin: center`
- Las animaciones deben ser interrumpibles — responder a la entrada del usuario durante la animación

### Tipografía

- Usar `…` no `...`
- Comillas tipográficas `"` `"` en lugar de comillas rectas `"`
- Espacios de no separación: `10&nbsp;MB`, `⌘&nbsp;K`, nombres de marcas
- Los estados de carga terminan con `…`: `"Cargando…"`, `"Guardando…"`
- `font-variant-numeric: tabular-nums` para columnas/comparaciones numéricas
- Usar `text-wrap: balance` o `text-pretty` en encabezados (evita líneas cortas huérfanas)

### Manejo de Contenido

- Los contenedores de texto deben manejar contenido largo: `truncate`, `line-clamp-*` o `break-words`
- Los hijos de flex necesitan `min-w-0` para permitir el truncado de texto
- Gestionar estados vacíos — no renderizar UI rota para strings/arrays vacíos
- Contenido generado por el usuario: anticipar entradas cortas, promedio y muy largas

### Imágenes

- `<img>` necesita `width` y `height` explícitos (evita CLS)
- Imágenes fuera del viewport inicial: `loading="lazy"`
- Imágenes críticas sobre el pliegue: `priority` o `fetchpriority="high"`

### Rendimiento

- Listas grandes (>50 elementos): virtualizar (`virtua`, `content-visibility: auto`)
- Sin lecturas de layout en el render (`getBoundingClientRect`, `offsetHeight`, `offsetWidth`, `scrollTop`)
- Agrupar lecturas/escrituras del DOM; evitar intercalarlas
- Preferir inputs no controlados; los inputs controlados deben ser baratos por cada pulsación de tecla
- Agregar `<link rel="preconnect">` para dominios de CDN y assets
- Fuentes críticas: `<link rel="preload" as="font">` con `font-display: swap`

### Navegación y Estado

- La URL refleja el estado — filtros, pestañas, paginación, paneles expandidos en query params
- Los enlaces usan `<a>` / `<Link>` (soporte para Cmd/Ctrl+clic, clic con botón central)
- Vincular profundamente toda la UI con estado (si usa `useState`, considerar sincronización con URL mediante `nuqs` u similar)
- Las acciones destructivas necesitan modal de confirmación o ventana de deshacer — nunca inmediatas

### Toque e Interacción

- `touch-action: manipulation` (evita el retraso por zoom con doble toque)
- `-webkit-tap-highlight-color` configurado de forma intencional
- `overscroll-behavior: contain` en modales, cajones y hojas laterales
- Durante el arrastre: desactivar la selección de texto, usar `inert` en los elementos arrastrados
- `autoFocus` con moderación — solo en escritorio, en un único input primario; evitar en móvil

### Áreas Seguras y Layout

- Los layouts de borde a borde necesitan `env(safe-area-inset-*)` para muescas
- Evitar barras de desplazamiento no deseadas: `overflow-x-hidden` en contenedores, corregir desbordamiento de contenido
- Flexbox/grid sobre medición con JavaScript para el layout

### Modo Oscuro y Temas

- `color-scheme: dark` en `<html>` para temas oscuros (corrige barra de desplazamiento e inputs)
- `<meta name="theme-color">` debe coincidir con el fondo de la página
- `<select>` nativo: `background-color` y `color` explícitos (modo oscuro en Windows)

### Localización (i18n)

- Fechas/horas: usar `Intl.DateTimeFormat` — no formatos codificados de forma fija
- Números/divisas: usar `Intl.NumberFormat` — no formatos codificados de forma fija
- Detectar idioma mediante `Accept-Language` / `navigator.languages`, no por IP
- Nombres de marcas, tokens de código, identificadores: envolver con `translate="no"` para evitar traducción automática incorrecta

### Seguridad en Hidratación

- Los inputs con `value` necesitan `onChange` (o usar `defaultValue` para no controlados)
- Renderizado de fechas/horas: proteger contra desajuste de hidratación (servidor vs cliente)
- `suppressHydrationWarning` solo donde sea verdaderamente necesario

### Estados Hover e Interactivos

- Los botones y enlaces necesitan estado `hover:` (retroalimentación visual)
- Los estados interactivos aumentan el contraste: hover/active/focus más prominentes que el estado normal

### Redacción y Contenido (Copywriting)

- Voz activa: "Instala el CLI" no "El CLI será instalado"
- Title Case para encabezados y botones (estilo Chicago)
- Numerales para conteos: "8 despliegues" no "ocho"
- Etiquetas de botón específicas: "Guardar API Key" no "Continuar"
- Los mensajes de error incluyen la solución o el siguiente paso, no solo el problema
- Segunda persona; evitar la primera persona
- `&` en lugar de "y" donde el espacio sea limitado

### Anti-patrones (señalar siempre)

- `user-scalable=no` o `maximum-scale=1` desactivando el zoom
- `onPaste` con `preventDefault`
- `transition: all`
- `outline-none` sin reemplazo de `focus-visible`
- `onClick` inline en navegación sin `<a>`
- `<div>` o `<span>` con manejadores de clic (deben ser `<button>`)
- Imágenes sin dimensiones
- Arrays grandes con `.map()` sin virtualización
- Inputs de formulario sin etiquetas
- Botones de ícono sin `aria-label`
- Formatos de fecha/número codificados de forma fija (usar `Intl.*`)
- `autoFocus` sin justificación clara

## Formato de Salida

Agrupar por archivo. Usar formato `archivo:línea` (clicable en VS Code). Hallazgos concisos.

```text
## src/Button.tsx

src/Button.tsx:42 - botón de ícono sin aria-label
src/Button.tsx:18 - input sin label
src/Button.tsx:55 - animación sin prefers-reduced-motion
src/Button.tsx:67 - transition: all → listar propiedades

## src/Modal.tsx

src/Modal.tsx:12 - falta overscroll-behavior: contain
src/Modal.tsx:34 - "..." → "…"

## src/Card.tsx

✓ correcto
```

Indicar el problema y su ubicación. Omitir explicación a menos que la solución no sea obvia. Sin preámbulos.
