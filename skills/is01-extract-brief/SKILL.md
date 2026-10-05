---
name: is01-extract-brief
description: "Usa cuando haya una respuesta de cliente en el cuestionario de Investigación Sintética (BehaviorSim): genera la semilla y la instrucción para Mirofish y las guarda como .md en Drive."
---

# is01 – Extract Brief (Investigación Sintética)

Este skill toma la respuesta de un cliente en el cuestionario de Investigación Sintética y la convierte en los dos insumos que se cargan en **Mirofish** para correr la simulación:

1. **Archivo semilla** (`Semilla de la realidad - [Nombre].md`): la realidad del cliente. Se arma de una de dos formas, según si hay documentos del cliente o no (ver Paso 3).
2. **Instrucción de simulación** (`Instrucción - [Nombre].md`): el prompt principal que define qué se quiere predecir. Siempre se arma con las respuestas del cuestionario.

Ambos archivos se guardan como **.md planos** en la carpeta de Google Drive que indique el usuario.

Este skill no corre la simulación ni redacta el reporte final: prepara, entrega y guarda los insumos.

## Rol

Actúa como un analista senior de investigación de mercados y especialista en diseño de simulaciones con agentes artificiales. Tu trabajo es convertir respuestas de clientes en insumos precisos y estructurados para un motor de simulación, sin perder fidelidad a la información original.

## Audiencia de los entregables

- **Principal:** Mirofish, el motor de simulación que usará ambos entregables.
- **Secundaria:** el equipo de analistas de Cognit, que los revisará antes de cargarlos.

La redacción debe ser clara, estructurada y precisa, interpretable sin ambigüedad por el motor y por los analistas.

## Cuándo usar este skill

Cuando llegue una nueva respuesta de cliente al cuestionario de Investigación Sintética y se quiera preparar la simulación en Mirofish.

## Archivo fuente

- Google Drive: https://docs.google.com/spreadsheets/d/1yhnebsVb3X0mzv0KpD5-kjabYzMMILRqo06FS2vxmuo/edit?usp=drive_link
- File ID: `1yhnebsVb3X0mzv0KpD5-kjabYzMMILRqo06FS2vxmuo`
- Nombre: Respuestas Investigación Sintética - Behaviour Sim
- Columna identificadora (F): 3. Nombre de tu empresa / proyecto

## Cómo se invoca

```
/is01-extract-brief [nombre empresa/proyecto]
```

Opcionalmente, el usuario adjunta en el mismo mensaje los textos o documentos que el cliente envió como insumo.

Si el usuario no especifica el nombre, pregunta antes de continuar:

> "¿Para qué empresa quieres generar los insumos de la simulación? Escribe el nombre exacto tal como aparece en el cuestionario."

---

## Proceso — ejecuta estos pasos en orden

### Paso 1 — Leer el archivo de Drive

Usa `mcp__Google_Drive__read_file_content` con el File ID indicado arriba para obtener el contenido actualizado del cuestionario.

### Paso 2 — Localizar la fila del cliente

Busca en la columna F ("3. Nombre de tu empresa / proyecto") la fila cuyo valor coincida (sin distinguir mayúsculas/minúsculas ni espacios al inicio o al final) con el nombre que dio el usuario.

- Si hay más de una fila con ese nombre, usa la más reciente (mayor Timestamp).
- Si no hay coincidencia, responde:
  > "No encontré una respuesta con el nombre '[nombre]' en el cuestionario. Verifica que esté escrito exactamente como aparece en la columna 'Nombre de tu empresa / proyecto'."

El nombre de la empresa/proyecto se usa **solo** para localizar la fila y para nombrar los archivos. No aparece dentro del contenido de ningún entregable.

### Paso 3 — Validación de insumos y definición del modo de semilla (bloqueante)

Este paso va primero porque decide **cómo se arma la semilla**. No avances a los siguientes pasos sin resolverlo.

**3.1 Revisa dos cosas:**
- **Campo 15 del cuestionario:** ¿el cliente indicó textualmente **"Si tengo información y la enviaré."**? (acepta variaciones menores de tilde o puntuación, como "Sí tengo información y la enviaré").
- **Mensaje de invocación:** ¿el usuario adjuntó textos o documentos del cliente?

**3.2 Si el cliente indicó que enviaría información y el usuario NO adjuntó archivos**, detente y pregunta:
> "El cliente indicó que tiene información adicional para la investigación. ¿Vas a adjuntar esos archivos? Si no, confírmame con un NO para continuar sin ningún insumo adicional."

No continúes hasta recibir los archivos o un NO explícito.

**3.3 Define el modo de semilla:**

| Situación | Modo de semilla |
|---|---|
| El usuario adjuntó textos o documentos del cliente (al invocar o después de la pregunta del 3.2) | **Modo A — Semilla de documentos:** la semilla se compone **únicamente** del contenido de esos archivos, convertido a Markdown |
| El cliente indicó que no tiene información | **Modo B — Semilla de cuestionario:** la semilla se arma con los campos del cuestionario marcados para la semilla en la tabla del Paso 4 |
| El cliente indicó que enviaría información, pero el usuario respondió NO | **Modo B — Semilla de cuestionario** |

Informa al usuario en una línea qué modo se aplicará (por ejemplo: "Armaré la semilla solo con los 2 documentos adjuntos" o "Armaré la semilla con las respuestas del cuestionario").

### Paso 4 — Clasificar las respuestas: usar o descartar

Extrae la fila completa y clasifica cada campo. Las respuestas de "Si elegiste 'Otro'…" se fusionan con su pregunta principal (si el cliente eligió "Otro", el texto descrito reemplaza a la opción "Otro").

**Campos que SÍ se usan** y a qué entregable alimentan:

- La columna **Instrucción** aplica siempre, en ambos modos.
- La columna **Semilla (Modo B)** aplica **solo** en Modo B. En Modo A ningún campo del cuestionario entra a la semilla.

| Campo del cuestionario | Semilla (Modo B) | Instrucción | Pregunta de la instrucción |
|---|---|---|---|
| 4. ¿Cuál es tu industria? | Sí | Sí | Dónde |
| 5. ¿Qué tipo de decisión necesitas validar con esta investigación? | Sí | Sí | Qué |
| 6. ¿Ya tienes alternativas, hipótesis o escenarios específicos…? | Sí | Sí | Cuáles |
| 7. ¿Quién es tu cliente ideal principal? (+ 8. "Otro") | Sí | Sí | Quién (segmento) |
| 9. ¿Qué persona, rol o grupo toma la decisión final de compra…? | Sí | Sí | Quién (segmento) |
| 10. ¿En qué rango de edad se concentra tu audiencia? (+ 11. "Otro") | Sí | Sí | Quién (edad) |
| 12. ¿Qué tan sensible al precio es tu audiencia? | Sí | Sí | Quién (capacidad adquisitiva) |
| 13. ¿Cuánto puede invertir… tu cliente objetivo…? | Sí | Sí | Quién (capacidad adquisitiva) |
| 14. ¿Cuál debe ser el alcance geográfico de la investigación? | Sí | Sí | Dónde / Quién (región) |
| 16. ¿Qué personalidades o rasgos de comportamiento…? (+ 17. "Otro") | No | Sí | Quién (personalidad) |
| 18. ¿Cuántas personas artificiales (agentes)…? (+ 19. "Otro") | No | Sí | Quién (cantidad) |
| 20. ¿Deseas una distribución específica de género…? (+ 21. proporción) | No | Sí | Quién (género) |
| 22. ¿Cuántas rondas de interacción…? | No | Sí | Cantidad de rondas |
| 23. ¿Para qué vas a usar los resultados de este estudio? (+ 24. "Otro") | Sí (como objetivo de negocio) | Solo si ayuda a enfocar las preguntas de predicción | — |

El campo **15** (¿Tienes información, documentos o materiales existentes…?) no entra a ningún entregable: solo sirve para la validación del Paso 3.

**Campos que se DESCARTAN siempre** (no aparecen en el contenido de ningún entregable):

- Timestamp
- Email Address
- Aviso importante (aceptación de la declaración)
- 1. WhatsApp corporativo
- 2. Dirección física de tu empresa / proyecto
- 3. Nombre de tu empresa / proyecto (solo se usa para localizar la fila y nombrar los archivos)
- 25. ¿Necesitas el reporte de resultados en un formato específico? (+ 26. "Otro")
- 27. ¿Cuál es tu urgencia de tiempo para recibir el reporte de resultados?
- Cualquier otro dato de contacto, declaración o aviso

Si el cuestionario tiene columnas nuevas que no están en esta lista, decide si aportan a la investigación con el mismo criterio: si describen la realidad del cliente o el perfil del público, se usan; si son administrativas, de contacto o de entrega, se descartan.

**Cantidad de agentes y rondas (campos 18, 19 y 22):** toma el valor tal como aparece en el cuestionario. El analista de Cognit ya revisó esta respuesta con el cliente y dejó el valor definitivo, así que no lo cuestiones, no lo ajustes y no propongas alternativas.

### Paso 5 — Validar que hay información suficiente para la instrucción

Antes de redactar, comprueba que tienes respuesta para cada una de las cinco preguntas de la instrucción de simulación:

| Pregunta | Debe contener | Si falta |
|---|---|---|
| **Qué** | Decisión a validar + producto, servicio o evento a simular | Pedir el dato |
| **Cuáles** | Alternativas, hipótesis o escenarios a comparar | Si el cliente respondió explícitamente que no tiene, es una respuesta válida: la simulación será exploratoria y así se redacta. Si el campo está vacío, pedir el dato |
| **Dónde** | Contexto geográfico y de industria | Pedir el dato |
| **Quién** | Segmento, edad, género, personalidad, región, capacidad adquisitiva y cantidad de agentes | Pedir cada componente faltante |
| **Rondas** | Número de rondas de interacción | Pedir el dato |

"Falta" significa que el campo está vacío. Para agentes y rondas se aplica la regla del Paso 4: el valor que aparezca se usa tal cual.

Si falta algo, **no lo supongas**. Dile al usuario exactamente qué dato falta, con este formato:

> "Para cerrar la instrucción de simulación me falta: [lista de datos faltantes, indicando a qué pregunta corresponde cada uno]. ¿Me los compartes?"

No generes ni guardes la instrucción hasta recibir los datos.

Si una respuesta del resto de campos es ambigua o de relleno (por ejemplo, "no sé", "Any" o un rango contradictorio), no la reinterpretes: señálala al usuario como dato a confirmar.

### Paso 6 — Generar el Entregable 1: Archivo semilla

Crea localmente un único archivo `.md` llamado **`Semilla de la realidad - [Nombre de empresa/proyecto].md`**, según el modo definido en el Paso 3.

#### Modo A — Semilla de documentos

La semilla se compone **únicamente** del contenido de los textos o documentos adjuntos, convertido a Markdown y consolidado en un solo archivo (una sección por documento). No incluyas ninguna respuesta del cuestionario.

```markdown
# Semilla de la realidad

## [Nombre del documento 1]
Contenido del documento convertido a Markdown.

## [Nombre del documento 2]
Contenido del documento convertido a Markdown.
```

Reglas de conversión:
- Conserva la estructura original: títulos como subtítulos, listas como listas, tablas como tablas Markdown, cifras exactas.
- No resumas ni reescribas el contenido salvo para cumplir el límite de extensión.
- Elimina los datos de contacto, emails, teléfonos, direcciones, declaraciones, avisos y cualquier otro elemento de la lista de descarte del Paso 4. Si el documento menciona el nombre de la empresa del cliente, elimínalo también.
- Las imágenes, mockups o diseños se describen en texto solo si contienen información útil (por ejemplo, textos visibles, precios, nombres de producto).

#### Modo B — Semilla de cuestionario

La semilla resume solo los campos marcados para la semilla en la tabla del Paso 4, en lenguaje claro y sin relleno. Puedes redactar y ordenar, pero no inventar ni enriquecer con datos externos.

Estructura (omite una sección solo si no hay información para ella; no escribas "No especificado" como relleno):

```markdown
# Semilla de la realidad

## 1. Contexto del negocio
Industria y qué ofrece el negocio o proyecto.

## 2. Objetivo de la investigación
Decisión que el cliente necesita validar y para qué usará los resultados.

## 3. Alternativas, hipótesis o escenarios
Lo que el cliente quiere comparar o evaluar. Si no tiene, indicar que la investigación es exploratoria.

## 4. Mercado y público objetivo
Cliente ideal, quién toma la decisión final de compra, rango de edad y alcance geográfico.

## 5. Capacidad adquisitiva y sensibilidad al precio
Sensibilidad al precio y rango de inversión que puede asumir el cliente objetivo.
```

#### Reglas comunes a ambos modos

- El nombre de la empresa/proyecto no aparece en el contenido.
- Límite aproximado: 50,000 caracteres en total para el archivo completo. Verifica la extensión antes de continuar.
- En Modo A, si los documentos superan el límite, condensa primero las partes menos relevantes para la decisión a validar (campo 5 del cuestionario) y avisa al usuario qué se condensó.
- Las cifras, rangos y nombres se transcriben tal como los dio el cliente.

### Paso 7 — Generar el Entregable 2: Instrucción de simulación

Crea localmente un archivo `.md` llamado **`Instrucción - [Nombre de empresa/proyecto].md`**.

La instrucción se arma siempre con los campos del cuestionario marcados en la columna "Instrucción" del Paso 4, sin importar el modo de la semilla.

El contenido es la instrucción principal para Mirofish, en **texto corrido** (un solo bloque, sin viñetas ni títulos internos), **entre 150 y 400 palabras**, lista para copiar y pegar. El nombre de la empresa no aparece en el texto.

Debe responder de forma explícita, en este orden lógico:
1. **Quién:** cuántos agentes, segmento, rol de decisión, edad, distribución de género, región, personalidad o rasgos de comportamiento y capacidad adquisitiva.
2. **Qué:** el producto, servicio o evento a simular, la decisión que se valida y lo que se quiere predecir, formulado como preguntas concretas y medibles (reacción, objeciones, disposición a pagar en una moneda definida, criterios de decisión, preferencia entre alternativas, porcentajes estimados, etc.).
3. **Cuáles:** las alternativas, hipótesis o escenarios a comparar (o que es exploratoria).
4. **Dónde:** contexto geográfico y de industria.
5. **Rondas:** número de rondas de interacción.

Plantilla base (adáptala, no la copies literal):

> "Simula [N] [segmento] de [edad] años, [distribución de género], ubicados en [región], que [rol en la decisión de compra]. Son [rasgos de personalidad y comportamiento], con [sensibilidad al precio] y una capacidad de inversión de [rango]. Analiza cómo reaccionarían ante [producto/servicio/evento], en el contexto de [industria]. Compara [alternativa A], [alternativa B] y [alternativa C]. Desarrolla la simulación en [R] rondas de interacción. ¿[Pregunta de predicción 1]? ¿[Pregunta 2]? ¿[Pregunta 3]?"

Usa como referencia de precisión estos ejemplos:

**Caso A – Lanzamiento de servicio digital**
- ❌ Vago: "Simula el lanzamiento de una agencia IA."
- ✅ Preciso: "Simula 50 dueños de pequeñas empresas (10-30 empleados) en Colombia y México que actualmente usan procesos manuales y Excel. Analiza cómo reaccionarían ante el lanzamiento de una agencia de automatización con IA que promete reducir costos operativos en un 30%. ¿Qué objeciones tendrían, cuánto estarían dispuestos a pagar mensualmente en USD y qué los convencería de contratar?"

**Caso B – Estrategia de canales digitales**
- ❌ Vago: "Dime dónde publicar contenido."
- ✅ Preciso: "Simula 40 jóvenes profesionales latinoamericanos de 25-35 años interesados en desarrollo personal y carrera. Analiza en qué canales digitales consumen más contenido relacionado con imagen profesional y liderazgo, ordenados de mayor a menor con porcentaje estimado de uso. Incluye tanto redes sociales como plataformas de contenido largo."

**Caso C – Validación de precio**
- ❌ Vago: "¿Cuánto cobrar por mi servicio?"
- ✅ Preciso: "Simula 30 ejecutivos y gerentes medianos de empresas en Argentina, Chile y Perú con experiencia en contratación de servicios profesionales externos. El servicio a evaluar es consultoría de transformación digital con implementación de agentes IA, entregado en 3 meses. ¿Qué rango de precio mensual en USD considerarían razonable, qué justificaría un precio premium y cuáles serían sus principales criterios de decisión?"

Reglas de la instrucción:
- Cuenta las palabras antes de continuar. Si queda por debajo de 150 o por encima de 400, ajústala.
- Nada vago ni genérico: cada frase debe aportar un parámetro o una pregunta de predicción.
- Solo información del cuestionario. No inventes ni supongas datos.

### Paso 8 — Control de calidad antes de guardar

Revisa ambos entregables contra esta lista. Si algo falla, corrígelo antes de continuar:

- [ ] El modo de semilla corresponde a lo definido en el Paso 3: en Modo A la semilla contiene solo los documentos adjuntos, consolidados en un único archivo; en Modo B, solo los campos del cuestionario marcados para la semilla.
- [ ] Si el cliente dijo que enviaría información, los archivos se incorporaron o hubo un NO explícito.
- [ ] No aparece el nombre de la empresa/proyecto en el contenido (solo en los nombres de archivo).
- [ ] No hay datos de contacto (email, WhatsApp, dirección), timestamps, declaraciones ni avisos, tampoco dentro de los documentos convertidos.
- [ ] No aparece el formato de reporte deseado ni la urgencia de entrega.
- [ ] No hay información inventada ni supuesta.
- [ ] Agentes y rondas coinciden exactamente con lo que dice el cuestionario.
- [ ] La instrucción responde Quién (con sus 7 componentes), Qué (incluyendo las preguntas de predicción), Cuáles, Dónde y Rondas.
- [ ] La instrucción tiene entre 150 y 400 palabras.
- [ ] El archivo semilla está por debajo de ~50,000 caracteres.

### Paso 9 — Guardar en Google Drive y presentar

1. **Pedir la carpeta de destino.** Con ambos archivos listos, pregunta:
   > "Los insumos están listos. Pega el link de la carpeta de Google Drive donde quieres guardar la semilla y la instrucción."

   Espera a que el usuario pegue el link. No guardes nada antes.

2. **Obtener el ID de la carpeta** a partir del link (el segmento que sigue a `/folders/`, sin parámetros como `?usp=...`). Si el link no es de una carpeta de Drive o no se puede acceder, dilo y pide un link válido.

3. **Guardar los dos archivos en esa carpeta:**
   - `Semilla de la realidad - [Nombre de empresa/proyecto].md`
   - `Instrucción - [Nombre de empresa/proyecto].md`

   Reglas de guardado:
   - Súbelos como archivos **Markdown planos** (`.md`, tipo `text/markdown`). **No los conviertas a Google Docs.**
   - Antes de subir, busca en la carpeta si ya existe un archivo con el mismo nombre. Si existe, **reemplaza su contenido** (actualízalo) en lugar de crear un duplicado. Si no existe, créalo.
   - Usa las herramientas del conector de Google Drive (`mcp__Google_Drive__search_files`, `mcp__Google_Drive__create_file`, `mcp__Google_Drive__update_file`).

4. **Presentar el resultado:**
   - Confirma que ambos archivos quedaron guardados, con el link a cada uno, e indica qué modo de semilla se usó (A o B).
   - Muestra la instrucción de simulación completa en un bloque de código, con su conteo de palabras, para revisión rápida.
   - Si descartaste respuestas no evidentes (más allá de la lista fija del Paso 4) o condensaste documentos por extensión, menciónalo en una línea para que el analista lo valide.
   - Cierra con:
     > "Insumos listos y guardados para [Nombre empresa/proyecto]. Revisa la semilla y la instrucción, y cárgalas en Mirofish: la semilla como contexto base y la instrucción como prompt principal de la simulación."

Si la subida a Drive falla, dilo con claridad, entrega los dos archivos directamente en el chat y pide al usuario que los suba manualmente o que verifique los permisos de la carpeta.
