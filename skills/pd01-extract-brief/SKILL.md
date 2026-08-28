---
name: "pd01-extract-brief"
description: "Este skill extrae automáticamente la información de un cliente desde el cuestionario de Google Drive y la convierte en el brief de entrada que necesita el skill `pd01-business-website`."
---

# PD01 – Extract Client Brief

Este skill extrae automáticamente la información de un cliente desde el cuestionario de Google Drive y la convierte en el brief de entrada que necesita el skill `pd01-business-website`.

## Cuándo usar este skill

Invócalo cuando tengas una nueva respuesta de cliente en el cuestionario y quieras iniciar el desarrollo de su sitio web. Es el paso previo obligatorio antes de arrancar `pd01-business-website`.

## Archivo fuente

- **Google Drive File ID:** `1u516Qzwd3ZAd-nNnpQeEEHBrQduXnHK_ax4Xy_BwWTo`
- **Nombre:** Respuestas Cuestionario para la creación de Busines Website
- **Columna identificadora:** `3. Nombre de tu empresa / proyecto`

## Cómo se invoca

El usuario provee el nombre exacto de la empresa tal como aparece en el cuestionario:

> `/pd01-extract-brief Cognit`

Si el usuario no especifica el nombre, pregúntaselo antes de continuar:

> "¿Para qué empresa quieres generar el brief? Escribe el nombre exacto tal como aparece en el cuestionario."

## Proceso — ejecuta estos pasos en orden

### Paso 1 — Leer el archivo de Drive

Usa la herramienta `mcp__Google_Drive__read_file_content` con el File ID indicado arriba para obtener el contenido actualizado del cuestionario.

### Paso 2 — Localizar la fila del cliente

Busca en la columna **"3. Nombre de tu empresa / proyecto"** la fila cuyo valor coincida (sin distinguir mayúsculas/minúsculas) con el nombre que proporcionó el usuario.

- Si hay más de una fila con ese nombre, usa la **más reciente** (mayor Timestamp).
- Si no hay ninguna coincidencia, responde:
  > "No encontré una respuesta con el nombre '[nombre]' en el cuestionario. Verifica que esté escrito exactamente como aparece en la columna 'Nombre de tu empresa / proyecto'."

### Paso 3 — Mapear columnas al brief

Extrae los siguientes campos de la fila encontrada y mapéalos así:

WhatsApp corporativo: [número de WhatsApp del negocio]
Dirección física de tu empresa / proyecto: [dirección, o "no aplica" si es 100% digital]
Nombre de tu empresa / proyecto: [nombre]
¿Cuál es tu industria?: [industria/sector]
¿Cuál es el objetivo principal de tu sitio web?: [ej. generar leads, vender directo, dar credibilidad institucional, agendar citas, otro]
Si elegiste "Otro", describe el objetivo de tu sitio web: [descripción libre]
¿A quién va dirigido tu sitio web? (Público objetivo): [perfil demográfico, rol, intención de quien visita]
¿Qué problema resuelves y cuál es tu principal diferencial?: [descripción]
¿Qué acción quieres que haga el visitante de tu sitio web? (Puedes elegir varias): [ej. escribir por WhatsApp, llenar formulario, llamar, comprar, agendar cita, otra]
Si elegiste "Otra", describe qué otra acción quieres que el visitante haga: [descripción libre]
¿Qué páginas o secciones necesitas en tu sitio web? (Puedes elegir varias): [ej. Inicio, Sobre Nosotros, Servicios, Portafolio, Contacto, FAQ, otra]
Si elegiste "Otra", describe qué otra sección quieres en tu sitio web: [descripción libre]
¿Tienes textos listos? (Los textos son la descripción de la información de lo que haces o vendes): [sí/no — pegar textos o indicar "no tiene, generar copy"]
¿Tienes imágenes/fotos/videos listos?: [sí/no — enlaces o descripción de recursos visuales; si no hay, se usarán placeholders]
Si lo tienes, pega aquí un link de un sitio web de referencia o inspiración que te guste: [URL o "no aplica"]
¿Qué NO quieres que tenga tu sitio web?: [restricciones explícitas del cliente]
¿Qué estilo visual quieres proyectar? (Puedes elegir varios que tengan relación entre sí): [ej. minimalista, corporativo serio, moderno/tech, cálido/artesanal, otro]
Si elegiste "Otro" como estilo visual, descríbelo aquí: [descripción libre]
¿Tienes logo y manual de marca?: [sí/no/parcial]
Si indicaste que no cuentas con un logo ni un manual de marca, ¿te gustaría que te pongamos en contacto con un diseñador gráfico aliado que te ayude a crearlos?: [sí/no]
Colores preferidos que expresen tu marca (Código HEX o descripción): [paleta]
Colores prohibidos (Los que NO quieres): [paleta a evitar]
¿Qué tono de comunicación o sensación debe transmitir tu sitio web? (Puedes elegir varios): [ej. cercano y directo, formal y técnico, aspiracional]
¿Qué funciones necesitas? (Puedes elegir varias): [ej. calculadora de cotización, chat, integración con calendario, multi-idioma, otra]
Si elegiste "Otra", indica cuál: [descripción libre]
¿En qué idiomas te gustaría ofrecer tu sitio web?: [idioma(s)]
Si elegiste "Otro" idioma, indica cuál: [descripción libre]
¿Necesitas que tu web sea administrable y editable por ti?: [sí/no]
¿Ya tienes dominio?: [sí/no — indicar cuál si aplica]
¿Ya tienes hosting?: [sí/no — indicar cuál si aplica]
Si ya lo tienes, pega el link del perfil de redes sociales que quieras vincular (Ej: LinkedIn, TikTok, Facebook, etc): [URL(s)]

### Paso 4 — Generar el brief en formato MD

Produce el brief con **exactamente** este formato, sin alterar los encabezados:

```
WhatsApp corporativo: [número de WhatsApp del negocio]
Dirección física de tu empresa / proyecto: [dirección, o "no aplica" si es 100% digital]
Nombre de tu empresa / proyecto: [nombre]
¿Cuál es tu industria?: [industria/sector]
¿Cuál es el objetivo principal de tu sitio web?: [ej. generar leads, vender directo, dar credibilidad institucional, agendar citas, otro]
Si elegiste "Otro", describe el objetivo de tu sitio web: [descripción libre]
¿A quién va dirigido tu sitio web? (Público objetivo): [perfil demográfico, rol, intención de quien visita]
¿Qué problema resuelves y cuál es tu principal diferencial?: [descripción]
¿Qué acción quieres que haga el visitante de tu sitio web? (Puedes elegir varias): [ej. escribir por WhatsApp, llenar formulario, llamar, comprar, agendar cita, otra]
Si elegiste "Otra", describe qué otra acción quieres que el visitante haga: [descripción libre]
¿Qué páginas o secciones necesitas en tu sitio web? (Puedes elegir varias): [ej. Inicio, Sobre Nosotros, Servicios, Portafolio, Contacto, FAQ, otra]
Si elegiste "Otra", describe qué otra sección quieres en tu sitio web: [descripción libre]
¿Tienes textos listos? (Los textos son la descripción de la información de lo que haces o vendes): [sí/no — pegar textos o indicar "no tiene, generar copy"]
¿Tienes imágenes/fotos/videos listos?: [sí/no — enlaces o descripción de recursos visuales; si no hay, se usarán placeholders]
Si lo tienes, pega aquí un link de un sitio web de referencia o inspiración que te guste: [URL o "no aplica"]
¿Qué NO quieres que tenga tu sitio web?: [restricciones explícitas del cliente]
¿Qué estilo visual quieres proyectar? (Puedes elegir varios que tengan relación entre sí): [ej. minimalista, corporativo serio, moderno/tech, cálido/artesanal, otro]
Si elegiste "Otro" como estilo visual, descríbelo aquí: [descripción libre]
¿Tienes logo y manual de marca?: [sí/no/parcial]
Si indicaste que no cuentas con un logo ni un manual de marca, ¿te gustaría que te pongamos en contacto con un diseñador gráfico aliado que te ayude a crearlos?: [sí/no]
Colores preferidos que expresen tu marca (Código HEX o descripción): [paleta]
Colores prohibidos (Los que NO quieres): [paleta a evitar]
¿Qué tono de comunicación o sensación debe transmitir tu sitio web? (Puedes elegir varios): [ej. cercano y directo, formal y técnico, aspiracional]
¿Qué funciones necesitas? (Puedes elegir varias): [ej. calculadora de cotización, chat, integración con calendario, multi-idioma, otra]
Si elegiste "Otra", indica cuál: [descripción libre]
¿En qué idiomas te gustaría ofrecer tu sitio web?: [idioma(s)]
Si elegiste "Otro" idioma, indica cuál: [descripción libre]
¿Necesitas que tu web sea administrable y editable por ti?: [sí/no]
¿Ya tienes dominio?: [sí/no — indicar cuál si aplica]
¿Ya tienes hosting?: [sí/no — indicar cuál si aplica]
Si ya lo tienes, pega el link del perfil de redes sociales que quieras vincular (Ej: LinkedIn, TikTok, Facebook, etc): [URL(s)]
```

Si un campo está vacío o no aplica en esa respuesta, escribe `No especificado`.

### Paso 5 — Presentar el brief y la acción siguiente

Muestra el brief generado y a continuación indica:

> "? Brief listo para **[Nombre empresa]**. Puedes iniciar el desarrollo del sitio invocando el skill `pd01-business-website` y pegando este brief como contexto del cliente."

## Notas adicionales

- No modifiques ni enriquezcas los valores extraídos del cuestionario — transcríbelos tal como llegaron.
- Si un valor es ambiguo o incompleto (ej. el cliente escribió "no sé" en el estilo visual), inclúyelo tal cual; es el skill `pd01-business-website` quien decide cómo manejarlo.
- Este skill **no** inicia el desarrollo del sitio — solo prepara y entrega el insumo.