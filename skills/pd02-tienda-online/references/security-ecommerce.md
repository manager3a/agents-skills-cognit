# Seguridad específica de e-commerce — PD02 (Cognit)

Este archivo complementa `security-standards.md` (estándar general de Cognit). Aplica ambos en Fase 2 y Fase 3, sin excepción, independientemente del tier o cliente.

## 1. Manejo seguro del carrito y datos del pedido

- NUNCA almacenar precios, descuentos ni totales modificables por el usuario en `localStorage` — estos deben recalcularse siempre desde la fuente de datos (`fuente_datos_cliente` o la base de datos del sitio), nunca confiar en lo que el navegador tiene guardado.
- NUNCA hardcodear claves API de pasarelas de pago ni tokens de la automatización (GitHub API, Vercel Deploy Hook, o credenciales de la fuente de datos y de la base de datos del sitio) en el frontend. Deben vivir como variables de entorno del lado del servidor o del job de automatización, nunca expuestas en el cliente.
- No exponer emails ni datos de contacto del cliente en texto plano en el HTML.

## 2. Formulario de checkout — seguridad obligatoria

- Validación completa en el cliente (HTML5 + JS) para todos los campos: nombre, email, teléfono, dirección, ciudad y código postal.
- Incluir protección anti-bot: implementar hCaptcha o reCAPTCHA v3 en el formulario de checkout.
- Indicar en comentarios que el backend/automatización debe implementar: rate limiting, validación server-side y tokenización de datos de pago.
- Nunca procesar datos de tarjeta directamente desde el frontend — siempre mediante SDK oficial de la pasarela (ej. MercadoPago SDK, Stripe.js).

## 3. Checklist de seguridad de e-commerce (agregar al final del HTML, junto al checklist general)

```html
<!--
  ╔══════════════════════════════════════════════════════════╗
  ║   CHECKLIST DE SEGURIDAD ADICIONAL PARA E-COMMERCE       ║
  ╚══════════════════════════════════════════════════════════╝
  □ Integrar pasarela de pago SOLO mediante SDK oficial (nunca datos de tarjeta en frontend)
  □ Proteger tokens de automatización (GitHub API, Deploy Hook, fuente de datos, base de datos) como variables de entorno
  □ Habilitar rate limiting en endpoints de checkout y de sincronización (máx. 5 intentos/IP/hora)
  □ Configurar backups automáticos del repositorio y de fuente_datos_cliente
  □ Cumplir PCI-DSS si se procesan tarjetas directamente
-->
```

## 4. Privacidad y cumplimiento legal específico de e-commerce

Además de lo indicado en el estándar general:

- Enlace a Política de Envíos con tiempos estimados y zonas de cobertura.
- Enlace a Términos y Condiciones de Compra, incluyendo política de devoluciones y garantías (más específico que un T&C genérico de servicios).
- Aviso legal sobre el procesamiento de datos de pago por terceros (pasarela de pago).
- El checkout debe indicar claramente en el UI que la conexión es segura (ícono de candado, texto "Compra 100% segura").
