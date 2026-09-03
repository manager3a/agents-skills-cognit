# Estándar de seguridad — Business Website (Cognit)

Aplica TODAS las siguientes medidas en el código generado durante la Fase 2 y Fase 3, sin excepción, independientemente del tipo de sitio o cliente. Este es el estándar que garantiza que un sitio de Cognit sea confiable desde el día uno, no un parche posterior.

## 1. Protección contra XSS (Cross-Site Scripting)

- NUNCA usar `innerHTML` para insertar datos del usuario o de fuentes externas. Usar siempre `textContent` o `createElement`.
- Escapar todos los caracteres especiales en inputs antes de mostrarlos: `<`, `>`, `"`, `'`, `&`.
- Si se usan templates, asegurarse de que el motor de plantillas escapa por defecto.

## 2. Manejo seguro de datos sensibles

- NUNCA hardcodear claves API, tokens, passwords o credenciales en el código frontend.
- NUNCA almacenar datos sensibles en `localStorage` o `sessionStorage` sin cifrado.
- Emails del cliente: no exponerlos en texto plano en el HTML. Usar formularios intermediarios o codificación `mailto:` con ofuscación.

## 3. Formularios — seguridad obligatoria

- Agregar validación en el lado del cliente (HTML5 + JS) para todos los campos.
- Incluir protección anti-bot: implementar reCAPTCHA v3 o hCaptcha en todo formulario de contacto.
- Indicar en comentarios de código que el backend debe implementar: rate limiting (máx. 5 envíos/IP/hora) y validación server-side.
- No procesar datos de formularios directamente desde el frontend sin sanitización.

## 4. Recursos y dependencias externas

- Cargar librerías solo desde CDNs confiables: `cdnjs.cloudflare.com`, `cdn.jsdelivr.net` o `unpkg.com`.
- Agregar atributo SRI (Subresource Integrity) a TODO script y CSS externo:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/[libreria]/[version]/[archivo].min.js" integrity="sha384-[HASH]" crossorigin="anonymous"></script>
```

- Usar únicamente las versiones más recientes estables de cada librería.
- Minimizar el número de dependencias externas.

## 5. Headers de seguridad (meta tags y comentarios)

Incluir en el `<head>` del HTML:

```html
<!-- SEGURIDAD: Configurar estos headers en el servidor/hosting -->
<!-- Content-Security-Policy: default-src 'self'; script-src 'self' https://cdnjs.cloudflare.com; style-src 'self' 'unsafe-inline' -->
<!-- X-Frame-Options: DENY -->
<!-- X-Content-Type-Options: nosniff -->
<!-- Referrer-Policy: strict-origin-when-cross-origin -->
<!-- Permissions-Policy: geolocation=(), microphone=(), camera=() -->
<meta http-equiv="X-Content-Type-Options" content="nosniff">
<meta name="referrer" content="strict-origin-when-cross-origin">
```

## 6. HTTPS y comunicaciones seguras

- Todos los recursos (imágenes, scripts, fuentes, iframes) deben cargarse por `https://`. NUNCA `http://`.
- Los formularios deben enviar datos a endpoints `https://`.
- Indicar en comentarios que el hosting debe tener SSL/TLS activo con renovación automática.

## 7. Iframes y contenido embebido

- Agregar atributo `sandbox` a todos los iframes con los permisos mínimos necesarios:

```html
<iframe src="..." sandbox="allow-scripts allow-same-origin" loading="lazy"></iframe>
```

- No embeber contenido de fuentes no verificadas.

## 8. Checklist de configuración para el hosting

Al final del HTML, incluir un bloque de comentarios con instrucciones para el administrador del servidor:

```html
<!--
+------------------------------------------+
|  CHECKLIST DE SEGURIDAD PARA EL HOSTING   |
+------------------------------------------+
[ ] Activar HTTPS con certificado SSL (Let's Encrypt)
[ ] Redirigir todo HTTP → HTTPS (301)
[ ] Activar HSTS: Strict-Transport-Security: max-age=31536000
[ ] Configurar headers de seguridad en el servidor (ver arriba)
[ ] Desactivar listado de directorios
[ ] Habilitar firewall de aplicaciones web (recomendado: Cloudflare free)
[ ] Configurar backups automáticos diarios
[ ] Mantener librerías y CMS actualizados
-->
```

## 9. Imágenes y archivos subidos (si aplica)

- Validar tipo MIME y extensión antes de procesar cualquier archivo subido.
- Nunca almacenar archivos subidos en el mismo directorio que el código.
- Limitar tamaño máximo de archivos subidos.

## 10. Privacidad y cumplimiento legal

- Incluir aviso de cookies si el sitio usa cookies o herramientas de analítica (Google Analytics, Meta Pixel, etc.).
- Banner de consentimiento de cookies compatible con GDPR/LGPD según la región del cliente.
- Enlace a Política de Privacidad en el footer (obligatorio si se recopilan datos personales).
- Enlace a Términos y Condiciones si el sitio ofrece servicios o productos.
