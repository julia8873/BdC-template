---
type: Schema
title: Wiki Schema — AGENTS.md
description: Instrucciones para el agente LLM que gestiona este wiki OKF v0.1.
tags: [schema, agents, instrucciones, okf, llm-wiki]
timestamp: 2026-06-17T17:21:00Z
---

# Wiki Schema — AGENTS.md

## DOMAIN

Este wiki cubre: **Inserta aquí el ámbito de conocimiento de la ByD**

## ESTRUCTURA DE DIRECTORIOS

- raw/ → Fuentes originales — SOLO LECTURA
- okf/concepts/ → Conceptos, tecnologías, definiciones (type: Concept)
- okf/entities/ → Herramientas, personas, productos (type: Entity)
- okf/sources/ → Resúmenes de documentos ingestados (type: Source)
- okf/playbooks/ → Guías de procesos paso a paso (type: Playbook)
- okf/index.md → Índice maestro del bundle (type: Index)
- okf/log.md → Historial append-only (type: Log)

## FRONTMATTER OKF OBLIGATORIO (v0.1)

Todo fichero creado en okf/ debe incluir este frontmatter completo:

```
---
type: <Concept|Entity|Source|Playbook|Metric|Constraint|Index|Log>
title: <Título legible por humanos>
description: <Una línea que resume el concepto — máximo 160 caracteres>
resource: <URI de la fuente de origen — opcional si no existe>
tags: [tag1, tag2-multi-palabra]
claims: [afirmacion1, afirmacion2] # Opcional: lista de afirmaciones de hecho clave para LINT optimizado
timestamp: <ISO 8601 — ej. 2026-06-17T17:00:00Z>
---
```

## OPERACIÓN: INGEST

Al recibir: "INGEST raw/nombre-fichero"

1. Lee el documento completo sin truncar
2. Extrae conceptos clave, entidades y afirmaciones principales
3. Crea okf/sources/nombre.md (type: Source) con resumen estructurado
4. Crea o actualiza ficheros en okf/concepts/ y okf/entities/ afectados
5. Añade cross-links relativos entre todas las páginas relacionadas mediante `[[nombre]]`
6. Marca con ⚠️ cualquier afirmación que contradiga contenido existente
7. Añade al final (append) y documenta un resumen en okf/log.md: ## [YYYY-MM-DD] ingest | nombre-fichero

## OPERACIÓN: QUERY

Al recibir una pregunta:

1. Lee okf/index.md para identificar las páginas más relevantes
2. Lee esas páginas completas
3. Sintetiza la respuesta citando con links relativos funcionales y operativos `[[nombre]]`
4. Al final del mensaje añade las Páginas Consultadas obligatoriamente en este formato exacto:
   #### Páginas Consultadas
   - [[ruta/fichero1.md]]
   - [[ruta/fichero2.md]]
5. Si la síntesis es valiosa, ofrece archivarla como nueva página

## OPERACIÓN: LINT

Al recibir: "LINT"

1. Lista páginas sin ningún inbound link (huérfanas)
2. Detecta afirmaciones contradictorias entre páginas — añade nuevos ⚠️
3. Identifica 5 conceptos mencionados repetidamente sin página propia
4. Sugiere 3 gaps de conocimiento relevantes para el dominio
5. Verifica que okf/index.md tenga entrada para cada fichero en okf/
6. Informa a través del chat de los 5 puntos anteriores
7. Añade al final (append) y documenta un resumen del lint en okf/log.md: ## [YYYY-MM-DD] lint | health-check

## OPERACIÓN: RESOLVE (Resolución de Contradicciones).

Tu misión es detectar y solventar las discrepancias marcadas con [WARN] o ⚠️ que aún no figuren como "SOLUCIONADAS" en el archivo `okf/log.md`.

### INSTRUCTIONS & SPECIFICATIONS

1. Consulta Humana (Prioridad Absoluta)
   - Pregunta directamente al usuario si dispone de información clara para solventar la discrepancia.
   - ¡CRÍTICO!: Detén el proceso y NO realices ningún cambio en los archivos de la wiki hasta recibir la respuesta explícita del usuario.

2. Rastreo y Verificación (En defecto de respuesta humana)
   - Si el usuario no aporta la solución, rastrea el origen del conflicto en la ruta `okf/sources/`.
   - Contrasta la información buscando y verificando en fuentes de autoridad oficiales de la Universidad de Granada (UGR).

3. Consolidación y Aplicación de Cambios
   - Histórico/Evolución: Si la discrepancia se debe a una evolución del concepto en el tiempo, sustituye el [WARN] o ⚠️ por una aclaración bajo la etiqueta [NOTE].
   - Corrección: Si detectas un dato erróneo o duplicado, corrige el fichero correspondiente de la wiki y elimina por completo el [WARN] o ⚠️.

4. Registro en Log (`okf/log.md`)
   - Añade al final (append) del archivo `okf/log.md` una nueva entrada con únicamente la nueva entrada y usando exactamente el siguiente formato de cabecera: ## [YYYY-MM-DD] resolve | Título descriptivo
   - El cuerpo del registro debe aclarar de forma explícita si la contradicción se ha resuelto o si está en espera de resolución.

### QUALITY CRITERIA

- Si el usuario ha solventado la contradicción: Se dará por SOLUCIONADA. Debes consolidar todas las entradas de `log.md` referentes a esa contradicción específica en una sola entrada final, donde se expliquen detalladamente los cambios realizados.
- Si el sistema sigue en espera de confirmación humana: El estado del log permanecerá estrictamente como "Pendiente de validación humana".
- Rigor técnico: No asumas datos ni automatices correcciones sin contrastar. Si las fuentes de la UGR no son concluyentes, el estado debe ser de espera.

### RESPONSE FORMAT

- Primera interacción: Presenta la discrepancia encontrada de forma clara y directa, y lanza la consulta al usuario usando enlaces operativos y funcionales a los archivos de **Ubicación** referenciados. Pausa cualquier acción de escritura en los ficheros hasta su feedback.
- Output general: Cuando documentes o respondas, utiliza bloques de Markdown limpios, estructurados, profesionales y sin texto de relleno.

## OPERACIÓN: APPLY

Al recibir: "APPLY" o al solicitar aplicar cambios del hilo

1. Analiza el historial de la conversación para identificar todos los archivos y cambios propuestos.
2. Genera la versión definitiva y completa de cada archivo que se propuso crear o modificar.
3. Actualiza obligatoriamente okf/log.md añadiendo al final (append) una nueva entrada con únicamente la nueva entrada con el formato exacto: ## [YYYY-MM-DD] apply | changes
4. El cuerpo del registro de log debe resumir brevemente los archivos y cambios que estás aplicando en esta operación.
5. Devuelve todo archivo creado o modificado.

## CONVENCIONES

- Links siempre relativos: [[Ruta de la nota|Título de la Nota]]
- Máximo 500 palabras por página — crea páginas nuevas si el contenido crece
- Nunca borrar ni modificar ficheros en raw/
- Idioma: español, salvo términos técnicos establecidos
- Nombres de fichero: minúsculas, guiones, sin acentos ni espacios
- Title en frontmatter: en lugar de ":" usa " — "
- Archivo log.md: No sobreescribas, siempre haz un APPEND al final del contenido de log.md. Nunca reescribas todo el archivo log.md.
- Archivo index.md: No intentes actualizar ni devolver okf/index.md. La indexación de nuevos archivos se realiza de forma automática por el plugin local.
