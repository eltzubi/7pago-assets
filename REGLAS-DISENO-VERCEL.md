# Reglas de Diseño de Vercel

Directrices oficiales para construir interfaces web accesibles, de alto rendimiento y con la estética de Vercel.

> Fuente: [vercel-labs/web-interface-guidelines](https://github.com/vercel-labs/web-interface-guidelines)

---

## Accesibilidad

- Botones con solo ícono: requieren `aria-label`
- Controles de formulario: deben tener `<label>` o `aria-label`
- Elementos interactivos: deben incluir manejadores de teclado (`onKeyDown` / `onKeyUp`)
- Usar `<button>` para acciones y `<a>` / `<Link>` para navegación — nunca `<div onClick>`
- Imágenes: deben tener texto alternativo `alt` (o `alt=""` si son decorativas)
- Íconos decorativos: añadir `aria-hidden="true"`
- Actualizaciones asíncronas (toasts, validaciones): usar `aria-live="polite"`
- Preferir HTML semántico sobre ARIA cuando sea posible
- Encabezados jerárquicos `<h1>` – `<h6>` con enlace de salto al contenido principal
- Los anclajes de encabezado necesitan `scroll-margin-top`

---

## Estados de Foco

- El foco visible es obligatorio: usar `focus-visible:ring-*` o equivalente
- Nunca eliminar los contornos sin ofrecer un reemplazo visual
- Usar `:focus-visible` en lugar de `:focus`
- Controles compuestos: agrupar con `:focus-within`

---

## Formularios

- Los campos de entrada necesitan `autocomplete` y un atributo `name` significativo
- Usar los atributos `type` correctos (`email`, `tel`, `url`, `number`) e `inputmode`
- No bloquear el pegado de texto con `onPaste` + `preventDefault`
- Etiquetas clicables mediante `htmlFor` o envolviendo el control
- Desactivar el corrector ortográfico en emails, códigos y nombres de usuario: `spellCheck={false}`
- Checkboxes y radios: un solo objetivo de clic, sin zonas muertas
- El botón de envío permanece activo hasta que comienza la solicitud; mostrar spinner durante la petición
- Errores en línea junto a los campos; enfocar el primer error al enviar
- El texto de marcador (`placeholder`) termina con `…` mostrando un patrón de ejemplo
- Campos que no sean de autenticación: `autocomplete="off"` para evitar interferencia del gestor de contraseñas
- Advertir antes de navegar si hay cambios sin guardar

---

## Animación

- Respetar `prefers-reduced-motion`
- Animar únicamente `transform` / `opacity`
- Evitar `transition: all` — listar las propiedades explícitamente
- Establecer el `transform-origin` correcto
- SVG: transformaciones en `<g>` con `transform-box: fill-box; transform-origin: center`
- Las animaciones deben poder interrumpirse

---

## Tipografía

- Puntos suspensivos: usar `…` no `...`
- Comillas tipográficas `"` `"` en lugar de comillas rectas `"`
- Espacios de no separación: `10&nbsp;MB`, `⌘&nbsp;K`, nombres de marcas
- Estados de carga: `"Cargando…"`, `"Guardando…"`
- Columnas numéricas: `font-variant-numeric: tabular-nums`
- Encabezados: `text-wrap: balance` o `text-pretty`

---

## Manejo de Contenido

- Contenido largo: usar `truncate`, `line-clamp-*` o `break-words`
- Hijos de flex: usar `min-w-0` para truncar texto correctamente
- Gestionar los estados vacíos — nunca dejar una pantalla en blanco sin contexto
- Anticipar entradas de usuario cortas, promedio y muy largas

---

## Imágenes

- `width` y `height` explícitos en cada imagen (evita CLS — Cumulative Layout Shift)
- Imágenes fuera del viewport inicial: `loading="lazy"`
- Imágenes críticas sobre el pliegue: `priority` o `fetchpriority="high"`

---

## Rendimiento

- Listas largas (>50 elementos): se requiere virtualización
- No leer el layout del DOM durante el render
- Agrupar operaciones del DOM en lote
- Preferir inputs no controlados cuando sea posible
- Usar `<link rel="preconnect">` para CDNs y dominios de terceros
- Fuentes críticas: `<link rel="preload">` con `font-display: swap`

---

## Navegación y Estado

- La URL debe reflejar el estado (filtros, pestañas, paginación, paneles)
- Los enlaces deben usar `<a>` / `<Link>` para conservar el comportamiento nativo del navegador
- UI con estado: vincular mediante parámetros de consulta (`query params`)
- Acciones destructivas: mostrar modal de confirmación o ventana de deshacer

---

## Toque e Interacción

- Usar `touch-action: manipulation`
- Configurar `-webkit-tap-highlight-color` de forma intencional
- Modales y cajones laterales: `overscroll-behavior: contain`
- Durante el arrastre: desactivar la selección de texto, usar `inert`
- `autoFocus` con moderación — solo en escritorio

---

## Áreas Seguras y Layout

- Diseños de borde a borde: usar `env(safe-area-inset-*)`
- Prevenir barras de desplazamiento no deseadas
- Preferir flexbox/grid sobre medición con JavaScript

---

## Modo Oscuro y Temas

- Aplicar `color-scheme: dark` en `<html>`
- `<meta name="theme-color">` debe coincidir con el color de fondo
- Elementos `<select>` nativos: especificar `background-color` y `color` explícitamente

---

## Localización (i18n)

- Fechas y horas: usar `Intl.DateTimeFormat`
- Números y divisas: usar `Intl.NumberFormat`
- Detección de idioma mediante `Accept-Language` / `navigator.languages`
- Nombres de marcas e identificadores: `translate="no"`

---

## Seguridad en Hidratación (SSR)

- Los inputs con `value` necesitan `onChange` (o usar `defaultValue`)
- Proteger el renderizado de fechas/horas contra desajustes de hidratación
- Minimizar el uso de `suppressHydrationWarning`

---

## Estados Hover e Interactivos

- Botones y enlaces deben tener estados `hover:`
- Los estados interactivos deben aumentar el contraste visual

---

## Redacción y Contenido (Copywriting)

- Preferir voz activa
- Title Case para encabezados y botones
- Numerales para conteos
- Etiquetas de botones específicas y orientadas a la acción
- Los mensajes de error deben incluir la solución, no solo el problema
- Perspectiva en segunda persona
- Usar `&` en lugar de "y" cuando el espacio sea limitado
- Usar lenguaje positivo por defecto, incluso en mensajes de error

---

## Anti-patrones — Siempre Evitar

| Anti-patrón | Motivo |
|---|---|
| `user-scalable=no` o `maximum-scale=1` | Impide el zoom de accesibilidad |
| `onPaste` + `preventDefault` | Bloquea flujos legítimos del usuario |
| `transition: all` | Causa animaciones involuntarias y pérdida de rendimiento |
| `outline-none` sin reemplazo | Elimina el foco visible |
| `onClick` en navegación sin `<a>` | Rompe comportamiento nativo del navegador |
| `<div>` / `<span>` con manejadores de clic | Rompe accesibilidad por teclado |
| Imágenes sin dimensiones | Provoca cambios de layout (CLS) |
| Arrays grandes sin virtualización | Degrada el rendimiento |
| Inputs de formulario sin etiqueta | Inaccesible para lectores de pantalla |
| Botones de ícono sin `aria-label` | Inaccesible |
| Formatos de fecha/número codificados de forma fija | Rompe la localización |
| `autoFocus` sin justificación | Interrumpe el flujo del usuario |

---

## Sistema de Diseño Geist — Tokens Visuales

### Colores

| Token | Valor | Uso |
|---|---|---|
| Primary | `#171717` | Texto principal, botones primarios |
| Secondary | `#4d4d4d` | Texto secundario |
| Tertiary | `#006bff` | Énfasis, enfoque, éxito |
| Neutral | `#f2f2f2` | Fondos de superficie |
| Background | `#ffffff` / `#fafafa` | Fondos de página |
| Border | `#ebebeb` | Bordes de tarjetas, inputs |
| Error | Rojo | Errores y estados destructivos |
| Warning | Ámbar | Advertencias |
| Success | Verde | Confirmaciones |

### Tipografía

| Elemento | Fuente | Observación |
|---|---|---|
| Cuerpo de texto | Geist Sans | Geométrica, cálida, legible |
| Bloques de código | Geist Mono | Para todos los encabezados de código |
| Tamaño de encabezado | Display | Espacio entre letras negativo (–2.4 px a –2.88 px) |

### Espaciado

- Sistema basado en múltiplos de **4 px**
- Relleno interno estándar: **24 px**
- Separación entre secciones: **32 px**

### Elevación y Sombras

- Filosofía **border-first**: los elementos estáticos se definen con un borde de 1 px (`#ebebeb`)
- `box-shadow` reservada para estados interactivos (hover) y elementos sobre el plano principal (popovers, modales)

### Bordes

- Radio estándar: **6 px** — aplicado a tarjetas, inputs y botones

---

*Estas reglas deben consultarse antes de iniciar cualquier trabajo de UI en proyectos que utilicen el sistema de diseño de Vercel.*
