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

## Estructura del repositorio

```
skills/
├── pd01-extract-brief/
│   └── SKILL.md
└── pd01-business-website/
    ├── SKILL.md
    └── references/
        ├── pd01-extract-brief.md
        ├── react-stack.md
        └── security-standards.md
```

## Convenciones

- Cada skill vive en `skills/<nombre-skill>/` con un `SKILL.md` como punto de entrada (metadata + instrucciones) y, opcionalmente, una carpeta `references/` con material de apoyo.
- Los nombres de skill siguen el prefijo del proceso al que pertenecen (`PD01` = línea de Business Website), facilitando agrupar futuras fases o variantes.
- Estos skills son el método fijo de la agencia: no varían de cliente a cliente, solo cambia el input (el brief).

## Contribuir

Nuevos skills o mejoras a los existentes deben desarrollarse en una rama descriptiva y mergearse a `main` una vez validados, manteniendo la estructura y convenciones descritas arriba.
