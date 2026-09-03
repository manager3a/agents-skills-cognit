---
name: pd02-tienda-online
description: >-
  PD02 - Tienda Online. Proceso estándar de Cognit para diseñar y construir
  tiendas online (e-commerce) para clientes de PYME, con arquitectura JAMstack
  desplegada en Vercel y un catálogo que el propio cliente puede actualizar
  sin depender de Cognit (vía automatización con n8n u otra herramienta,
  conectada a la fuente de datos que el cliente ya usa: hoja de cálculo,
  carpeta de archivos o base de datos). Úsalo siempre que el usuario pida
  crear, diseñar, prototipar o construir la tienda online / e-commerce de un
  cliente de Cognit, cuando mencione "tienda online", "e-commerce" o "vender
  en línea" para un cliente, cuando pegue o adjunte las respuestas de un
  cuestionario/brief de cliente para PD02, o cuando pida avanzar de fase
  (prototipo, luego desarrollo completo, luego ajustes finales) en un
  proyecto de tienda online ya iniciado. Este skill es el método fijo de la
  agencia y no varía de cliente a cliente — solo cambian el input del brief,
  el tier (básica/profesional/avanzada) y la fuente de datos del cliente.
---

# PD02 - Tienda Online — Método Cognit

Este skill encapsula el proceso estándar de Cognit para construir tiendas online para clientes de PYME. El objetivo no es solo generar código de e-commerce: es entregar una tienda que el cliente pueda operar día a día por sí mismo — sin depender de Cognit para actualizar su catálogo, y sin que Cognit quede atado a mantener plugins ni plataformas SaaS de terceros.

Lo único que cambia entre un proyecto y otro es el **input del cliente**: su empresa, industria, catálogo, volumen de productos y la fuente de datos que ya usa o prefiere. El método, la arquitectura, los estándares de seguridad y el flujo de fases son siempre los mismos.

## Antes de empezar: consigue el brief y clasifica el tier

No arranques a construir sin datos reales del cliente. Si el usuario no ha pasado un brief, pídele la información equivalente a la de `references/extract-brief-base.md` (empresa, industria, objetivo, público objetivo, estilo visual, colores, tono), más estos datos específicos de e-commerce:

- Categorías y número de productos actuales
- Variantes de producto (talla, color, sabor, etc.)
- Métodos de envío y zonas de cobertura
- Pasarela de pago a integrar (solo referencia visual, nunca procesamiento real desde el frontend)
- **Volumen esperado de cambios al catálogo por mes** — este dato es el que define el tier (ver tabla abajo)
- **Fuente de datos que el cliente ya usa o prefiere** (`fuente_datos_cliente`) — hoja de cálculo, carpeta de archivos, base de datos, o una combinación

Si alguna respuesta llega vacía, usa el criterio profesional más adecuado para el sector del cliente en vez de inventar algo genérico.

## Rol

Actúa como un desarrollador web full-stack senior especializado en e-commerce, con experiencia en UX/UI orientada a conversión, arquitectura headless/JAMstack y desarrollo frontend de alto rendimiento para ventas digitales.

## Tarea

Crea una tienda online completa y funcional para la empresa del cliente, con un catálogo que el propio cliente pueda actualizar sin depender de Cognit ni de conocimientos técnicos. El sitio debe quedar 100% listo para desplegarse en Vercel, sin configuración adicional.

El entregable incluye siempre estos archivos (o su equivalente en Next.js según el tier — ver `references/arquitectura-tiers.md`):

- `index.html` — estructura semántica completa con todas las secciones del e-commerce
- `styles.css` — estilos responsivos mobile-first, variables CSS para identidad visual
- `script.js` — carrito de compras, filtros de productos, checkout y consumo del catálogo desde la fuente de datos automatizada

## Principio de arquitectura — válido para los 3 tiers

La arquitectura **no se define por plataforma** (nunca WordPress+WooCommerce ni Shopify+Liquid — ver restricciones abajo). El patrón único es: **sitio JAMstack en Vercel + fuente de datos editable por el cliente + automatización que dispara la actualización del sitio**. Lee `references/arquitectura-tiers.md` antes de generar código de Fase 2 — ahí está el detalle completo de stack, scaffolding y automatización por tier.

Lo que diferencia un tier de otro es el **volumen y la complejidad del catálogo del cliente**, no la tecnología base:

| Tier | Catálogo total | Cambios/mes esperados |
|---|---|---|
| **Tienda básica** | Hasta ~30 productos | Hasta ~15-20 cambios/mes |
| **Tienda profesional** | ~30 a 150-200 productos | Actualizaciones frecuentes (semanales), con variantes |
| **Tienda avanzada** | 150+ productos, o alta rotación de stock | Cambios diarios, inventario en tiempo real |

> El criterio decisivo no es solo cuántos productos tiene el cliente, sino cuántas veces al mes toca su catálogo. Un catálogo grande pero estático puede vivir en el tier básico; uno pequeño con alta rotación puede necesitar el avanzado.

## Flujo de desarrollo — tres fases (obligatorio)

No desarrolles la tienda completa de inmediato. No avances de fase sin que el usuario lo indique explícitamente.

### Fase 1 — Prototipo estructural para aprobación del cliente

Esta fase es **100% front-end, sin ninguna conexión a fuente de datos real ni automatización**. Su único propósito es que el cliente valide distribución visual, navegación, paleta de colores y personalización de marca antes de invertir esfuerzo en el desarrollo completo. La fuente de datos y la herramienta de automatización se deciden y se implementan recién en Fase 2 — no las menciones ni las prepares en Fase 1.

Genera únicamente:

- Navbar con logo, menú con todas las secciones de la tienda, ícono de carrito con contador en cero y CTA principal ("Ver productos" o equivalente)
- Hero section con headline, subheadline, oferta destacada y CTA de compra
- Grilla de catálogo con 3 tarjetas de producto **placeholder** (imagen, nombre, precio, botón "Agregar al carrito") reflejando las categorías del cliente, sin lógica funcional aún
- Footer básico con nombre de empresa, links del menú, métodos de pago referenciados y aviso legal
- El resto de secciones (detalle de producto, carrito, checkout, confirmación, seguimiento) como links funcionales con placeholder mínimo (ej. `<section id="checkout"><p>Contenido próximamente</p></section>`)

Al terminar, detente y muestra:

> *"✅ Fase 1 lista. Los archivos están listos para subir al repositorio GitHub conectado a Vercel. Una vez que el cliente apruebe el contenido de la URL desplegada, indícame que sigo con la Fase 2 para desarrollar la tienda completa."*

### Fase 2 — Desarrollo completo (solo tras confirmación explícita)

Cuando el usuario confirme que puedes avanzar:

1. Define, junto con el usuario, la **fuente de datos** (`fuente_datos_cliente`) y la **herramienta de automatización** (n8n, Make, Zapier, webhook nativo u otra) más convenientes para este proyecto específico — según lo que el cliente ya usa y el tier que le corresponde. Esta decisión se toma en este punto, no antes. Consulta `references/arquitectura-tiers.md` para las opciones por tier.
2. Desarrolla la tienda completa: catálogo con filtros alimentado desde la fuente de datos, carrito persistente, checkout según el método de pago del tier, confirmación de pedido y seguimiento.
3. Documenta el conector de automatización paso a paso para que el cliente y Cognit sepan cómo funciona.
4. Aplica el estándar completo de seguridad de `references/security-standards.md` y las reglas específicas de e-commerce en `references/security-ecommerce.md`.

### Fase 3 — Ajustes finales y entrega (solo tras revisión del sitio completo)

Aplica los ajustes solicitados sobre los archivos existentes sin reescribir el sitio desde cero. Antes de entregar, verifica:

- Que la automatización de catálogo funcione de punta a punta (prueba real de agregar/editar un producto desde la fuente de datos del cliente)
- Que los estándares de arquitectura, seguridad y compatibilidad estén correctamente implementados
- Que no haya links rotos, placeholders visibles, imágenes sin cargar ni secciones vacías
- Que el sitio funcione en mobile, tablet y desktop sin excepciones
- Que el carrito persista entre recargas y los totales se recalculen siempre desde la fuente de datos, nunca desde valores guardados por el cliente en el navegador
- Que el flujo completo de checkout funcione: datos del comprador → envío → resumen → confirmación con número de pedido
- Que los formularios tengan validación activa y protección anti-bot
- Que el checklist de seguridad esté al final del HTML
- Que las páginas legales (Privacidad, Términos y Condiciones de Compra, Política de Envíos, Cookies) estén presentes y enlazadas en el footer

Al terminar, muestra:

> *"🚀 Fase 3 completa. La tienda online está lista para entrega final. La automatización de catálogo ha sido probada de punta a punta, y todos los estándares de arquitectura, seguridad y compatibilidad han sido verificados. Todo listo para el deploy definitivo."*

## Compatibilidad de dispositivos — obligatorio

El sitio debe funcionar en todos los dispositivos y sistemas operativos: Android e iOS en mobile, iPad y tablets Android, y Windows/macOS/Linux en Chrome, Firefox, Safari y Edge en desktop.

- Responsive 100% mobile-first, breakpoints mínimos `320px`, `768px`, `1024px`, `1440px`
- Áreas táctiles mínimo `44x44px`, sin depender de hover para funciones críticas de compra
- El carrito lateral (drawer) debe ser completamente funcional con gestos táctiles en mobile
- Fuentes con unidades relativas (`rem`, `em`, `vw`), nunca `px` fijos para texto
- Imágenes con `max-width: 100%` y `loading="lazy"`
- Menú hamburguesa en mobile/tablet, horizontal con acceso rápido al carrito en desktop
- Grilla de productos: 1 columna en mobile, 2 en tablet, 3–4 en desktop

## Seguridad — no negociable

En Fase 2 y Fase 3, aplica siempre:

- El estándar general de Cognit en `references/security-standards.md` (XSS, datos sensibles, formularios, SRI, headers, HTTPS, iframes, checklist de hosting, privacidad/cookies)
- Las reglas específicas de e-commerce en `references/security-ecommerce.md` (manejo seguro del carrito y totales, tokens de automatización y pasarela de pago, checkout, cumplimiento legal de e-commerce)

Léelos antes de generar el código de Fase 2 — no te lo saltes asumiendo que ya los recuerdas.

## Restricciones (prompt negativo)

- No incluyas comentarios innecesarios ni código de ejemplo sin funcionalidad real.
- No omitas ninguno de los archivos solicitados para el tier correspondiente.
- No uses imágenes con rutas absolutas que dependan de un servidor específico — usa `https://placehold.co` con dimensiones apropiadas para producto hasta que la automatización cargue las imágenes reales.
- No ignores ninguna variable del brief del cliente: cada respuesta debe reflejarse visiblemente en el resultado final.
- **No propongas WordPress + WooCommerce ni Shopify + Liquid como stack, bajo ninguna circunstancia ni tier** — quedan descartados por la dependencia de mantenimiento de plugins/plataforma SaaS que generan para Cognit.
- No implementes pantallas de administración custom (CRUD manual) en el tier avanzado — usa siempre el flujo de sincronización automática desde la fuente editable del cliente.
- No fijes la fuente de datos ni la herramienta de automatización de antemano — se deciden en Fase 2 según el cliente y el tier (ver `references/arquitectura-tiers.md`).
- No cargues ningún recurso externo por `http://` — exclusivamente `https://`.
- No omitas ninguna regla de `references/security-standards.md` ni `references/security-ecommerce.md` en Fase 2/3, sin importar el tipo de producto, cliente o tier.
- No implementes funcionalidades de pago real ni solicites datos de tarjeta sin la integración del SDK oficial de la pasarela — solo flujo visual y formulario con indicadores de seguridad.
