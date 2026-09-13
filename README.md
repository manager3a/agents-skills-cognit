# Agents & Skills — Cognit

Inventario oficial de agentes y skills de Claude que Cognit utiliza para estandarizar y acelerar sus procesos de agencia. Cada skill encapsula un proceso repetible del negocio, de modo que su ejecución sea consistente sin importar quién la invoque.

## Skills disponibles

### 🧩 `pd01-extract-brief`

Extrae automáticamente la información de un cliente desde el cuestionario de respuestas en Google Drive y la convierte en el brief de entrada estandarizado que necesita `pd01-business-website`.

- **Input:** nombre exacto del cliente/empresa tal como aparece en el cuestionario.
- **Output:** brief en formato Markdown con todos los campos normalizados, listo para pasar al siguiente skill.
- **Cuándo usarlo:** es el paso obligatorio previo a iniciar el desarrollo de un sitio web para un nuevo cliente.
- 📄 [`skills/pd01-extract-brief/SKILL.md`](skills/pd01-extract-brief/SKILL.md)

### 🖥️ `pd01-business-website`

Proceso estándar de Cognit para diseñar y construir *business websites* premium: sitios estáticos HTML/CSS/JS con arquitectura JAMstack, listos para desplegar en Vercel vía GitHub.

- **Input:** brief del cliente (generado por `pd01-extract-brief` o provisto directamente).
- **Output:** prototipo y, posteriormente, el sitio web completo, siguiendo las fases fijas de la agencia (prototipo → desarrollo completo → ajustes finales).
- **Cuándo usarlo:** siempre que se cree, diseñe o avance de fase un website premium/landing/sitio corporativo para un cliente de Cognit.
- 📄 [`skills/pd01-business-website/SKILL.md`](skills/pd01-business-website/SKILL.md)
- Referencias incluidas:
  - [`pd01-extract-brief.md`](skills/pd01-business-website/references/pd01-extract-brief.md) — cómo se conecta con el skill de extracción de brief.
  - [`react-stack.md`](skills/pd01-business-website/references/react-stack.md) — lineamientos de stack técnico.
  - [`security-standards.md`](skills/pd01-business-website/references/security-standards.md) — estándares de seguridad para el frontend.

### 🧩 `pd02-extract-brief`

Extrae automáticamente la información de un cliente desde el cuestionario de respuestas en Google Drive y la convierte en el brief de entrada estandarizado que necesita `pd02-tienda-online`.

- **Input:** nombre exacto del cliente/empresa tal como aparece en el cuestionario de Tienda Online.
- **Output:** brief en formato Markdown con todos los campos normalizados, listo para pasar al siguiente skill.
- **Cuándo usarlo:** es el paso obligatorio previo a iniciar el desarrollo de una tienda online para un nuevo cliente.
- 📄 [`skills/pd02-extract-brief/SKILL.md`](skills/pd02-extract-brief/SKILL.md)

### 🛒 `pd02-tienda-online`

Proceso estándar de Cognit para diseñar y construir *tiendas online* (e-commerce) para clientes de PYME: arquitectura JAMstack desplegada en Vercel, con un catálogo que el propio cliente puede actualizar sin depender de Cognit (vía automatización con n8n u otra herramienta, conectada a la fuente de datos que el cliente ya usa: hoja de cálculo, carpeta de archivos o base de datos).

- **Input:** brief del cliente (tier básica/profesional/avanzada y fuente de datos del catálogo).
- **Output:** prototipo y, posteriormente, la tienda online completa, siguiendo las fases fijas de la agencia (prototipo → desarrollo completo → ajustes finales).
- **Cuándo usarlo:** siempre que se cree, diseñe o avance de fase una tienda online/e-commerce para un cliente de Cognit.
- 📄 [`skills/pd02-tienda-online/SKILL.md`](skills/pd02-tienda-online/SKILL.md)
- Referencias incluidas:
  - [`arquitectura-tiers.md`](skills/pd02-tienda-online/references/arquitectura-tiers.md) — arquitectura técnica por tier (básica/profesional/avanzada).
  - [`extract-brief-base.md`](skills/pd02-tienda-online/references/extract-brief-base.md) — cómo se construye el brief de entrada.
  - [`security-ecommerce.md`](skills/pd02-tienda-online/references/security-ecommerce.md) — estándares de seguridad específicos de e-commerce.
  - [`security-standards.md`](skills/pd02-tienda-online/references/security-standards.md) — estándares de seguridad generales del frontend.

### 🎨 `ui-ux-pro-max`

Inteligencia de diseño UI/UX de terceros ([nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)) con una base de datos local consultable: 79 estilos visuales (50 activos), 192 paletas de color con perfiles de razonamiento, 74 combinaciones tipográficas, 119 guías de UX, 105 íconos curados, 17 presets de animación GSAP, 25 tipos de gráficos y 22 stacks tecnológicos (React, Next.js, Vue, Svelte, Astro, Flutter, SwiftUI, Laravel, entre otros).

- **Input:** una consulta en lenguaje natural sobre la necesidad de diseño (tipo de producto, estilo, componente o problema de UX), junto con el stack técnico del proyecto cuando aplique.
- **Output:** recomendaciones concretas y verificables — sistema de diseño completo (paleta, tipografía, layout, efectos), o guía puntual de accesibilidad, interacción, tipografía, color, animación, navegación o gráficos, con opción de persistir el sistema de diseño (`MASTER.md` + overrides por página) para reutilizarlo entre sesiones.
- **Cuándo usarlo:** al diseñar, construir o revisar cualquier interfaz — páginas nuevas, componentes, sistemas de diseño, accesibilidad, interacción, layout responsive, tipografía, color o visualización de datos — para cualquier proyecto de Cognit (no específico de un proceso PD01/PD02).
- 📄 [`skills/ui-ux-pro-max/SKILL.md`](skills/ui-ux-pro-max/SKILL.md)
- Incluye: `scripts/search.py` (motor de búsqueda local), `data/` (catálogos CSV/JSON), y `references/quick-reference.md` + `references/pro-rules.md` (guías detalladas y checklist de pre-entrega).

## Flujo de trabajo (Website Premium)

```
Cuestionario de cliente (Google Drive)
        │
        ▼
pd01-extract-brief  →  genera el brief estandarizado
        │
        ▼
pd01-business-website  →  prototipo → desarrollo completo → ajustes finales
        │
        ▼
Deploy en Vercel vía GitHub
```

## Flujo de trabajo (Tienda Online)

```
Cuestionario de cliente (Google Drive)
        │
        ▼
pd02-extract-brief  →  genera el brief estandarizado
        │
        ▼
pd02-tienda-online  →  prototipo → desarrollo completo → ajustes finales
        │
        ▼
Deploy en Vercel vía GitHub + catálogo administrable por el cliente
```

## Estructura del repositorio

```
skills/
├── pd01-extract-brief/
│   └── SKILL.md
├── pd01-business-website/
│   ├── SKILL.md
│   └── references/
│       ├── pd01-extract-brief.md
│       ├── react-stack.md
│       └── security-standards.md
├── pd02-extract-brief/
│   └── SKILL.md
├── pd02-tienda-online/
│   ├── SKILL.md
│   └── references/
│       ├── arquitectura-tiers.md
│       ├── extract-brief-base.md
│       ├── security-ecommerce.md
│       └── security-standards.md
└── ui-ux-pro-max/
    ├── SKILL.md
    ├── data/           # catálogos de estilos, paletas, tipografía, íconos, stacks, etc.
    ├── references/      # quick-reference.md, pro-rules.md
    └── scripts/         # search.py y motor de búsqueda local
```

## Convenciones

- Cada skill vive en `skills/<nombre-skill>/` con un `SKILL.md` como punto de entrada (metadata + instrucciones) y, opcionalmente, una carpeta `references/` con material de apoyo.
- Los nombres de skill siguen el prefijo del proceso al que pertenecen (`PD01` = línea de Business Website), facilitando agrupar futuras fases o variantes.
- Estos skills son el método fijo de la agencia: no varían de cliente a cliente, solo cambia el input (el brief).

## Contribuir

Nuevos skills o mejoras a los existentes deben desarrollarse en una rama descriptiva y mergearse a `main` una vez validados, manteniendo la estructura y convenciones descritas arriba.
