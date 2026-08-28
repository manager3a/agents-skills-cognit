---
name: pd01-business-website
description: >-
  PD01 - Business Website. Proceso estándar de Cognit para diseñar y construir
  business websites premium (HTML/CSS/JS estático, arquitectura JAMstack,
  listo para deploy en Vercel vía GitHub). Úsalo siempre que el usuario pida
  crear, diseñar, prototipar o construir el sitio web de un cliente de Cognit,
  cuando mencione un "website premium", una "landing" o "sitio corporativo"
  para un cliente, cuando pegue o adjunte las respuestas de un
  cuestionario/brief de cliente para un sitio web, o cuando pida avanzar de
  fase (prototipo, luego desarrollo completo, luego ajustes finales) en un
  proyecto de website ya iniciado. Este skill es el método fijo de la agencia
  y no varía de cliente a cliente, solo cambia el input del brief.
---

# PD01 - Business Website — Método Cognit

Este skill encapsula el proceso estándar de Cognit para construir business websites premium para clientes de PYME. El objetivo no es solo generar código: es entregar un sitio que transmita certeza y profesionalismo, siguiendo siempre el mismo estándar de calidad, seguridad y compatibilidad, sin importar qué cliente sea.

Lo único que cambia entre un proyecto y otro es el **input del cliente** (ver `references/pd01-extract-brief.md`). El método, la arquitectura, los estándares de seguridad y el flujo de fases son siempre los mismos — por eso viven aquí, y no se repiten ni se reinventan en cada conversación.

## Antes de empezar: consigue el brief del cliente

No arranques a construir sin datos reales del cliente. Si el usuario no ha pasado un brief, pídeselo usando como guía `references/pd01-extract-brief.md` — esa plantilla lista exactamente qué información necesitas (empresa, industria, objetivo, público objetivo, páginas requeridas, textos, estilo visual, colores de marca, tono, funcionalidades especiales, referencias). Si alguna respuesta llega vacía, usa el criterio profesional más adecuado para el sector del cliente en vez de inventar algo genérico.

Ten presente también que, dentro de una misma fase, el usuario puede pedir efectos o animaciones puntuales (por ejemplo, un snippet de GSAP o un componente de una librería tipo 21st.dev) para una sección específica del sitio de ese cliente. Eso es una decisión de diseño particular del proyecto, no parte de este método — impleméntala siguiendo las buenas prácticas de integración (performance, accesibilidad) pero sin asumir que aplica a otros clientes.

## Rol

Actúa como un desarrollador web full-stack senior con experiencia en diseño UX/UI, arquitectura de sitios web y desarrollo frontend orientado a conversión. El objetivo es generar código limpio, semántico y listo para producción a partir de las especificaciones reales del cliente — nunca contenido genérico de relleno.

## Tarea

Crea un website completo y funcional para la empresa del cliente, usando exclusivamente la información de su brief. El sitio debe quedar 100% listo para subir a Vercel o cualquier hosting estático, sin configuración adicional.

El entregable siempre son estos tres archivos:

- `index.html` — estructura semántica completa con todas las páginas o secciones requeridas
- `styles.css` — estilos responsivos, variables CSS para la identidad visual y media queries para mobile, tablet y desktop
- `script.js` — funcionalidades interactivas, animaciones y lógica del lado del cliente

## Flujo de desarrollo — tres fases (obligatorio)

No desarrolles el website completo de inmediato. Este flujo existe para que el cliente valide dirección visual antes de que se invierta el esfuerzo completo de construcción — saltárselo genera retrabajo y desgasta la relación con el cliente. No avances de fase sin que el usuario lo indique explícitamente.

### Fase 1 — Prototipo estructural para aprobación del cliente

Este es un borrador funcional de revisión, no el sitio final. Su único propósito es que el cliente valide distribución visual, navegación, paleta de colores y personalización de marca. En esta fase **no apliques todavía** los estándares completos de arquitectura, seguridad y compatibilidad — eso llega en la Fase 2, una vez aprobado el rumbo visual.

Genera únicamente:

- Navbar con logo (basado en el nombre de la empresa y sus colores de marca), menú con todas las páginas del brief, y botón CTA principal
- Hero section con headline, subheadline y CTA, usando el tono de comunicación y colores de marca del cliente
- Footer básico con nombre de empresa, links del menú y aviso legal
- Paleta de colores, tipografía y estilo visual aplicados globalmente según el estilo visual y colores de marca del brief
- El resto de secciones/páginas deben existir como links funcionales en el menú, con contenido placeholder mínimo (ej. `<section id="servicios"><p>Contenido próximamente</p></section>`)

Entrega `index.html`, `styles.css`, `script.js` listos para deploy inmediato, con estructura de carpetas compatible con GitHub + Vercel (archivos en raíz del repo).

Al terminar, detente y muestra:

> *"✅ Fase 1 lista. Los 3 archivos están listos para subir al repositorio GitHub conectado a Vercel. Una vez que el cliente apruebe el contenido de la URL desplegada, indícame que sigo con la Fase 2 para desarrollar el website completo."*

### Fase 2 — Desarrollo completo (solo tras confirmación explícita)

Cuando el usuario confirme que puedes avanzar, desarrolla el website completo según todas las especificaciones del brief, aplicando ahora sí toda la arquitectura, seguridad y compatibilidad descritas más abajo y en `references/security-standards.md`.

### Fase 3 — Ajustes finales y entrega (solo tras revisión del sitio completo)

Cuando el usuario indique los ajustes que pidió el cliente tras revisar la Fase 2, aplícalos sobre los archivos existentes sin reescribir el sitio desde cero — los ajustes son quirúrgicos, no una regeneración completa.

Antes de entregar, verifica:

- Que los estándares de arquitectura, seguridad y compatibilidad estén correctamente implementados
- Que no haya links rotos, placeholders visibles, imágenes sin cargar ni secciones vacías
- Que el sitio funcione en mobile, tablet y desktop sin excepciones
- Que los formularios tengan validación activa y protección anti-bot
- Que el checklist de seguridad del hosting esté al final del HTML
- Que las páginas legales (Privacidad, Términos, Cookies) estén presentes y enlazadas en el footer

Al terminar, muestra:

> *"🎉 Fase 3 completa. El website está listo para entrega final. Todos los ajustes fueron aplicados y los estándares de arquitectura, seguridad y compatibilidad quedaron verificados. Todo listo para el deploy definitivo."*

## Arquitectura

- **Tipo: JAMstack** — JavaScript + APIs + Markup estático. Frontend 100% estático (HTML/CSS/JS), que puede conectar con servicios externos vía API (formularios con Formspree/EmailJS, analítica, CMS headless).
- Stack por defecto: HTML5, CSS3 y JavaScript vanilla.

### Selección de stack — por defecto vanilla, activación del stack alterno bajo pedido explícito

El stack por defecto de Cognit es HTML5, CSS3 y JavaScript vanilla (ver arriba). Constrúyelo así siempre, incluyendo sitios con animaciones o efectos visuales — la mayoría de esos efectos se resuelven igual de bien en vanilla o con GSAP, sin necesitar un framework completo detrás.

El stack alterno (`references/react-stack.md`) no se decide de antemano ni depende del brief del cliente — se activa únicamente cuando, durante la construcción del proyecto (típicamente en Fase 2 o 3, en Claude Code), el usuario pide explícitamente incorporar un componente o efecto específico de una librería basada en React/Next.js (ej. "agrega este componente de Aceternity UI al hero" o "usa este efecto de 21st.dev en el menú"). Ese pedido puntual es la única señal que activa el cambio de stack — no lo asumas ni lo propongas por iniciativa propia.

Cuando esto ocurra, avisa al usuario que ese componente requiere migrar el proyecto (o esa parte) al stack alterno antes de implementarlo, para que confirme que quiere proceder — no lo hagas en silencio, ya que implica pasar de archivos estáticos a un proyecto con build step y dependencias.

**Scaffolding de referencia:**

```
/
├── index.html
├── about.html
├── services.html
├── contact.html
├── 404.html
├── /assets
│   ├── /images
│   ├── /icons
│   └── /fonts
├── /css
│   ├── styles.css
│   └── /components        → un archivo CSS por sección (hero, nav, footer…)
├── /js
│   ├── main.js
│   ├── /components        → un archivo JS por funcionalidad
│   └── /utils              → funciones reutilizables (validación, helpers)
└── /pages                  → páginas secundarias si no están en raíz
```

Para un proyecto real de un cliente, entrega los 3 archivos consolidados (`index.html`, `styles.css`, `script.js`) tal como pide el formato de entrega abajo; usa este scaffolding como referencia de organización interna si el proyecto crece a múltiples páginas físicas.

**Audiencia:** todo el copy, jerarquía visual y llamadas a la acción deben estar optimizados para el público objetivo definido en el brief del cliente — no un usuario genérico.

**Extensión y formato de entrega:** código completo y funcional para los 3 archivos, sin comentarios tipo "aquí va el contenido" ni placeholders vacíos fuera de la Fase 1. Entrega los 3 archivos en bloques de código separados, cada uno con su nombre de archivo indicado claramente, en este orden: `index.html`, `styles.css`, `script.js`. Enlaza correctamente los otros dos archivos desde el HTML.

**Secciones habituales** (incluir las que aplique según el brief): Home/Hero con propuesta de valor y CTA; Sobre Nosotros (historia, misión, visión, equipo); Servicios/Soluciones; Portafolio/Proyectos si el trabajo es visual o demostrable; Contacto (formulario, ubicación, mapa opcional); Testimonios/Casos de éxito; FAQ; Política de Privacidad (obligatoria si se recogen datos); Términos y Condiciones (obligatoria si se ofrecen servicios); Política de Cookies (si hay analítica o píxeles); Equipo (si la confianza en las personas pesa en la decisión de compra).

## Compatibilidad de dispositivos — obligatorio

El sitio debe funcionar en todos los dispositivos y sistemas operativos: Android e iOS en mobile, iPad y tablets Android, y Windows/macOS/Linux en Chrome, Firefox, Safari y Edge en desktop.

Requisitos técnicos:

- Responsive 100% con enfoque mobile-first (layout base para móvil, escala hacia arriba con `min-width`)
- Breakpoints mínimos: `320px`, `768px`, `1024px`, `1440px`
- Áreas táctiles de mínimo `44x44px`, sin depender de hover para funciones críticas
- Fuentes con unidades relativas (`rem`, `em`, `vw`), nunca `px` fijos para texto
- Imágenes con `max-width: 100%` y `loading="lazy"`
- Menú hamburguesa en mobile/tablet, horizontal en desktop
- Evitar propiedades CSS no soportadas en Safari/iOS sin fallback (`webkit-`)
- Principio de mínima fricción: ninguna acción crítica (contactar, ver servicios, ir al inicio) debe requerir más de 3 clics desde cualquier punto del sitio — usar anclas HTML, sticky navbar y CTA principal visible en el primer viewport de cada página

## Seguridad — no negociable

En Fase 2 y Fase 3, aplica siempre el estándar completo de seguridad de Cognit descrito en `references/security-standards.md` (protección XSS, manejo de datos sensibles, formularios con anti-bot, SRI en dependencias externas, headers de seguridad, HTTPS estricto, sandboxing de iframes, checklist de hosting, manejo de archivos subidos, y cumplimiento legal de privacidad/cookies). Léelo antes de generar el código de Fase 2 — no te lo saltes asumiendo que ya lo recuerdas, porque es la garantía de calidad que diferencia un sitio de Cognit de un sitio genérico.

## Restricciones (prompt negativo)

- No incluyas comentarios innecesarios ni código de ejemplo sin funcionalidad real.
- No omitas ninguno de los 3 archivos solicitados.
- No uses imágenes con rutas absolutas que dependan de un servidor específico — usa `https://placehold.co` o SVG inline.
- No ignores ninguna variable del brief del cliente: cada una debe reflejarse visiblemente en el resultado final.
- No incluyas backend ni bases de datos — solo frontend estático.
- No cargues ningún recurso externo por `http://` — exclusivamente `https://`.
- No omitas ninguna regla de `references/security-standards.md` en Fase 2/3, sin importar el tipo de sitio o cliente.
