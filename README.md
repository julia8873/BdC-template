---
type: Document
title: LLM Wiki + OKF — Despliegue Básico
description: Estructura inicial lista para usar. Descomprime, ajusta DOMAIN en AGENTS.md y empieza a ingerir.
tags: [despliegue, llm-wiki, okf, quickstart]
timestamp: 2026-06-17T17:21:00Z
---

# LLM Wiki + OKF — Despliegue Básico

Bundle listo para usar. Contiene la estructura mínima viable del sistema
basado en el patrón de Andrej Karpathy y estandarizado con OKF v0.1.

## Estructura

```
llmwiki-deploy-bundle/
├── AGENTS.md              ← Schema — AJUSTA LA SECCIÓN DOMAIN
├── raw/                   ← Pon aquí tus documentos fuente (PDF, MD, TXT)
├── okf/                   ← Bundle OKF v0.1 (el LLM escribe aquí)
│   ├── index.md           ← Índice maestro (type: Index)
│   ├── log.md             ← Historial append-only (type: Log)
│   ├── concepts/
│   ├── entities/
│   ├── sources/
│   └── playbooks/
```

## Primeros pasos

1. **Descomprime** el bundle en tu directorio de trabajo
2. **Edita `AGENTS.md`** → cambia la sección `DOMAIN` por tu caso de uso
3. **Abre Obsidian** → `Open folder as vault` → selecciona la carpeta `okf/`
4. **Primer ingest**: pon un fichero en `raw/` y ejecuta el prompt INGEST con tu agente

## Prompt INGEST (copia y pega en tu agente)

```
Lee AGENTS.md para entender las convenciones de este wiki.

Ejecuta la operación INGEST sobre el fichero: raw/<tu-fichero>

Sigue exactamente los 8 pasos definidos en AGENTS.md.
Al finalizar, muéstrame:
1. Lista de ficheros nuevos creados en okf/
2. Lista de ficheros existentes modificados
3. Contradicciones detectadas (si las hay)
4. 3 preguntas de seguimiento que te sugiere este material
```

## LLM Wiki Assistant

Plugin nativo para Obsidian que integra un chatbot de Inteligencia Artificial capaz de estructurar, consultar y auditar de forma autónoma una base de conocimiento persistente. El plugin implementa el patrón LLM Wiki de Andrej Karpathy bajo el estándar de metadatos OKF v0.1 (Open Knowledge Format) de Google Cloud.

https://github.com/ceprud/LLM_Wiki_Assistant

## Integración con tu stack

| Herramienta            | Rol                  | Configuración                                |
| ---------------------- | -------------------- | -------------------------------------------- |
| **Obsidian**           | Visor del wiki       | Vault → `/`                                  |
| **LLM Wiki Assistant** | Plugin para Obsidian | Instalar como plugin comunitario en el vault |
