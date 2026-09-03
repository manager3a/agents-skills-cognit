# Arquitectura por tier — PD02 Tienda Online

Detalle completo de stack, fuente de datos, automatización y scaffolding para cada tier. Consulta este archivo en Fase 2, cuando ya sabes qué tier corresponde al cliente (ver tabla de volumen en `SKILL.md`).

## Fuente de datos — variable abierta según el cuestionario

`fuente_datos_cliente` puede ser cualquiera de estas categorías, o una combinación (ej. datos en una hoja de cálculo + fotos en una carpeta de archivos). No la fijes de antemano por tier — pregúntale al cliente qué ya usa.

| Categoría | Ejemplos | Cuándo tiene sentido |
|---|---|---|
| Hoja de cálculo / no-code | Google Sheets, Excel Online (Microsoft 365), Airtable, Notion | El cliente ya lleva su inventario así; cero curva de aprendizaje |
| Carpeta de archivos | Google Drive, OneDrive, Dropbox | Para fotos/documentos, normalmente combinada con una hoja de cálculo para precios/stock |
| Base de datos real | Supabase, Postgres, MySQL, Baserow | Cuando el volumen o la frecuencia de cambios es alta (tier avanzado); el cliente casi nunca la edita directo, sino a través de la hoja de cálculo que sincroniza hacia ella |

El nodo de automatización (n8n u otra herramienta) es quien se adapta a la fuente elegida, no al revés — el frontend del sitio siempre recibe el mismo formato de datos sin importar de dónde vinieron.

## Qué queda eliminado del stack, en todos los tiers

- **WordPress + WooCommerce** (tema PHP, panel wp-admin, ecosistema de plugins, hosting con base de datos) — descartado por la carga de mantenimiento y seguridad que implica para Cognit.
- **Shopify + Liquid** — descartado por la dependencia de una suscripción SaaS mensual, temas Liquid propietarios y el app store de Shopify, que generan soporte recurrente no deseado.
- **Pantallas de administración custom (CRUD manual)** en el tier avanzado — se reemplazan por el flujo de sincronización automática desde la fuente editable del cliente.

## Tienda básica

- **Frontend:** Sitio estático (HTML/CSS/JS o Astro) desplegado en Vercel
- **Fuente de datos:** `fuente_datos_cliente` — típicamente una hoja de cálculo (Sheets, Excel Online) para el catálogo (nombre, precio, categoría, stock, badge), y opcionalmente una carpeta de archivos para fotos
- **Automatización (a definir en Fase 2):** un conector detecta cambios en la fuente elegida y actualiza el sitio automáticamente. Puede resolverse con n8n, Make, Zapier u otra herramienta — la elección depende del proyecto. El resultado siempre es el mismo: el cliente edita su fuente de datos, el sitio se actualiza solo
- **Checkout:** Botón o link de pago (Mercado Pago, PSE) — sin carrito complejo ni backend de pagos

**Scaffolding:**
```
[nombre-proyecto]/
├── index.html
├── styles.css
├── script.js
├── data/
│   └── products.json          ← se alimenta desde fuente_datos_cliente (el conector se define e implementa en Fase 2)
└── assets/
    └── images/                ← sincronizadas desde la fuente de archivos del cliente
```
> El conector de automatización no forma parte del código del sitio: vive fuera del repositorio y solo escribe en `products.json`. El frontend nunca depende de qué herramienta ni qué fuente de datos se eligió.

## Tienda profesional

- **Frontend:** Next.js (SSG/ISR) desplegado en Vercel, mismo patrón de repositorio
- **Fuente de datos:** `fuente_datos_cliente` — típicamente una fuente con más estructura (Airtable, Notion, Excel Online avanzado) para catálogo con variantes (talla, color), más una carpeta de archivos para imágenes
- **Automatización (a definir en Fase 2):** conector (n8n, webhook nativo de la fuente elegida, Make o Zapier) que detecta el cambio y dispara la actualización del sitio
- **Checkout:** Checkout embebido de Stripe o Mercado Pago — sin necesidad de backend propio de pagos

**Scaffolding:**
```
[nombre-proyecto]/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── products/
│   │   └── [slug]/page.tsx
│   ├── cart/page.tsx
│   └── checkout/page.tsx
├── lib/
│   ├── data-source/
│   │   └── client.ts           ← lee el catálogo ya normalizado, sin importar si viene de Airtable, Sheets u otra fuente
│   └── stripe/
│       └── client.ts
├── components/
│   ├── ProductCard.tsx
│   ├── ProductGrid.tsx
│   └── CartDrawer.tsx
├── public/
│   └── images/
└── next.config.ts
```

## Tienda avanzada

- **Frontend:** Next.js + Tailwind CSS desplegado en Vercel
- **Fuente de datos:** una base de datos real (Supabase, Postgres u otra) alimentada por el mismo flujo de automatización desde `fuente_datos_cliente`, de forma que el cliente nunca edita la base de datos ni el backend directamente
- **Automatización (a definir en Fase 2):** job o conector (n8n u otra herramienta) que sincroniza la fuente editable del cliente con la base de datos en tiempo casi real, sin necesidad de rebuild completo del sitio para cada cambio
- **Checkout:** Stripe con backend propio (webhooks, órdenes, inventario en tiempo real)

**Scaffolding:**
```
[nombre-proyecto]/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── (shop)/
│   │   ├── products/[slug]/page.tsx
│   │   ├── cart/page.tsx
│   │   └── checkout/
│   │       ├── page.tsx
│   │       └── success/page.tsx
│   └── api/
│       ├── orders/route.ts
│       └── webhooks/stripe/route.ts
├── lib/
│   ├── supabase/
│   │   ├── client.ts
│   │   └── queries.ts
│   └── stripe/
│       └── client.ts
├── components/
│   ├── ui/
│   ├── layout/
│   └── shop/
├── hooks/
│   ├── useCart.ts
│   └── useProducts.ts
├── public/
│   └── images/
├── tailwind.config.ts
└── next.config.ts
```
> Igual que en los otros tiers, el conector que sincroniza la fuente del cliente con la base de datos vive fuera del repositorio del sitio (como un flujo de n8n u otra herramienta). El sitio solo lee de su base de datos — nunca necesita saber cómo llegaron los datos ahí.

## Lógica de negocio obligatoria del catálogo (todos los tiers)

- Los datos del catálogo (`id`, `nombre`, `categoria`, `precio`, `precioOriginal`, `stock`, `imagen`, `descripcion`, `variantes`, `badge`) provienen siempre de `fuente_datos_cliente` a través del conector de automatización definido en Fase 2, nunca hardcodeados manualmente por Cognit tras la entrega.
- Carrito persistente usando `localStorage` (sin datos sensibles): guardar y restaurar ítems entre recargas de página.
- Cálculo automático de subtotal, descuentos y costo de envío, siempre recalculado desde la fuente de datos, nunca desde valores guardados por el cliente en el navegador.
- Generador de número de pedido único al confirmar checkout (formato: `#ORD-YYYYMMDD-XXXX`).
- Validación completa del formulario de checkout en el frontend antes de procesar.
