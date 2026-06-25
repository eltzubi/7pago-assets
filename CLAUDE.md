# 7pago — Guía para Claude

Este repositorio contiene los assets y directrices de diseño del proyecto **7pago**.

## Reglas de Diseño

Sigue siempre las reglas documentadas en [`REGLAS-DISENO-VERCEL.md`](./REGLAS-DISENO-VERCEL.md) al generar o revisar cualquier código de interfaz de usuario para 7pago.

Resumen de las reglas más importantes:

- Usar el sistema de diseño **Geist** de Vercel (tipografía, colores, espaciado)
- Espaciado basado en múltiplos de **4 px**; relleno estándar **24 px**
- Radio de borde **6 px** en tarjetas, inputs y botones
- Color primario `#171717`, borde `#ebebeb`
- Fuente principal: **Geist Sans**; código: **Geist Mono**
- Todo elemento interactivo debe tener estado `hover:` y `focus-visible:`
- HTML semántico siempre — nunca `<div onClick>`
- Respetar `prefers-reduced-motion` en todas las animaciones
- Solo animar `transform` y `opacity`
- Mensajes de error en voz activa con solución incluida

## Cómo ayudar con 7pago

Cuando el usuario pida mejoras a la web de 7pago:

1. **Auditar** el código UI contra `REGLAS-DISENO-VERCEL.md` antes de sugerir cambios
2. **Priorizar** accesibilidad, rendimiento y consistencia visual
3. **Proponer** mejoras concretas con código listo para usar
4. **Señalar** anti-patrones detectados con su ubicación exacta (`archivo:línea`)

## Stack Tecnológico Esperado

- **Framework**: Next.js (App Router)
- **Estilos**: Tailwind CSS
- **Fuentes**: Geist (`next/font/google` o paquete `geist`)
- **Idioma**: Español (México)

## Comandos Útiles

```bash
# Instalar Geist
npm install geist

# Auditar con las directrices de Vercel
# (revisar REGLAS-DISENO-VERCEL.md antes de cada PR)
```
