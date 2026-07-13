# 7pago — Guía para Claude

## Qué es este repositorio

Este repositorio no es una aplicación con build propio: es un almacén de
**snippets HTML/CSS autocontenidos** que se pegan tal cual en los bloques de
código embebido del CMS **Jouwweb** (dominio de assets `primary.jwwb.nl`) que
aloja la web de **7 Pago**, una carrera de montaña (Mendi Lasterketa) que se
celebra en Mallabia, Bizkaia, al pie del monte Oiz.

No hay `package.json`, ni framework, ni proceso de build: cada archivo
`.html` es un bloque independiente pensado para pegarse en una página del CMS.

- **Idioma del contenido**: euskera como idioma principal del sitio, con
  selector a español (`/es/...` vs `/eu/...`). Los nombres de marca
  (`7PAGO`, `7 Pago`) llevan `translate="no"`.
- **Idioma de la documentación/commits**: español.

## Estructura del repositorio

| Archivo | Contenido |
|---|---|
| `CLAUDE.md` | Esta guía. |
| `REGLAS-DISENO-VERCEL.md` | Checklist de accesibilidad/rendimiento/UX (sistema Geist de Vercel, traducido) usado como rúbrica de auditoría para cualquier snippet HTML de este repo. |
| `berri.html` | Bloque mínimo con los `<link>` de Google Fonts (Syne, DM Sans) para una página de noticias ("berri" = noticia en euskera). |

Otras ramas pueden añadir snippets de página adicionales (por ejemplo una
rama con `pagina-inicio.html`, el bloque completo de la portada: hero,
vídeo de YouTube embebido, tarjetas de información, galería de fotos y
panel de cierre). Antes de asumir que un archivo existe, revisa la rama en
la que estás trabajando — este repo crece por snippet/página, no todo vive
siempre en la rama por defecto.

## Convenciones de los snippets

- **Namespacing por prefijo**: todo selector CSS y `id` va prefijado con
  `p7-` (`#p7-etorkizuna-basa`, `.p7-txartel-izenburua`, `#p7-gora-botoia`…)
  porque el bloque se inyecta junto a otros bloques del CMS y **no hay**
  aislamiento real (ni Shadow DOM ni CSS Modules) — colisionar con estilos
  de otro bloque es el bug más fácil de introducir.
- **CSS inline en `<style>`** dentro del propio snippet, con variables CSS
  con nombres en euskera (`--bg-nagusia`, `--neon-ziana`, `--testu-argia`…)
  scopeadas al contenedor raíz del bloque.
- **Fuentes**: se cargan vía Google Fonts con `<link rel="preconnect">` +
  `<link rel="stylesheet">` al principio del snippet (nunca `@import`).
  Fuentes vistas hasta ahora: Syne, DM Sans, Space Grotesk — no hay una
  única familia fija todavía, revisa el snippet más reciente antes de
  introducir una nueva.
- **Imágenes** alojadas en `primary.jwwb.nl` (el propio CMS); siempre con
  `width`/`height` explícitos y `loading="lazy"` salvo above-the-fold.
- **Vídeo**: `<iframe>` de YouTube embed con `loading="lazy"`.
- **Botones/enlaces de acción** usan emoji decorativos con
  `aria-hidden="true"` junto al texto (`<span aria-hidden="true">🏁</span> Sailkapenak`).
- Enlaces de idioma (`ES`/`EU`) y "volver arriba" son patrones recurrentes;
  revisa si ya existe un bloque equivalente antes de duplicar lógica.

## Reglas de Diseño

Sigue siempre las reglas documentadas en [`REGLAS-DISENO-VERCEL.md`](./REGLAS-DISENO-VERCEL.md)
al generar o revisar cualquier snippet de este repositorio. Es la rúbrica de
auditoría, no una descripción del stack: los snippets reales son HTML/CSS
plano (no Next.js/Tailwind), pero deben cumplir igualmente sus criterios de
accesibilidad, foco, formularios, animación, tipografía, rendimiento e
i18n.

Resumen de las reglas más importantes:

- Espaciado basado en múltiplos de **4 px**; relleno estándar **24 px**
- Radio de borde **6 px** en tarjetas, inputs y botones
- Todo elemento interactivo debe tener estado `hover:` y `focus-visible:`
- HTML semántico siempre — nunca `<div onClick>`
- Respetar `prefers-reduced-motion` en todas las animaciones
- Solo animar `transform` y `opacity`
- Mensajes de error en voz activa con solución incluida

## Cómo ayudar con 7pago

Cuando el usuario pida cambios o mejoras a un snippet de 7pago:

1. **Auditar** el HTML/CSS contra `REGLAS-DISENO-VERCEL.md` antes de sugerir cambios.
2. **Priorizar** accesibilidad, rendimiento y consistencia visual con los otros snippets del repo.
3. **Mantener el namespacing `p7-`** en cualquier selector o `id` nuevo — nunca genéricos (`.card`, `#button`) que puedan colisionar en el CMS.
4. **Proponer** mejoras concretas con código listo para pegar en el bloque del CMS (el snippet completo, no un diff parcial — Jouwweb no entiende diffs).
5. **Señalar** anti-patrones detectados con su ubicación exacta (`archivo:línea`).
6. **Respetar el idioma existente** del snippet (euskera por defecto) al añadir texto nuevo; no traducir contenido que no se pidió traducir.

## Flujo de ramas

- Rama por defecto actual: `claude/vercel-design-rules-hlp77q`. No hay `main`/`master`.
- Cada tarea (nuevo snippet, revisión de accesibilidad, nueva documentación) se desarrolla en su propia rama `claude/<descripción-corta>-<hash>` y se abre como PR independiente — no se apila trabajo no relacionado en la misma rama.
- Antes de empezar, comprueba con `git remote show origin` cuál es la rama HEAD real, ya que puede cambiar entre tareas.

## Comandos útiles

No hay build ni tests — la única "prueba" es abrir el `.html` en un navegador
o pegarlo en un bloque de prueba del CMS y revisar visualmente.

```bash
# Previsualizar un snippet suelto en el navegador
python3 -m http.server 8000   # y abrir http://localhost:8000/berri.html

# Auditar un snippet contra las directrices de Vercel
# (leer REGLAS-DISENO-VERCEL.md y aplicar su checklist antes de cada PR)
```
