# Checkpoint 1 · Agente de Triaje de Soporte EXPORED (n8n)

Pre-entregable del curso de Coderhouse: **Construcción del Agente Base y Motor de Razonamiento**.

**Archivo entregable:** [`checkpoint1_guillermo_fidelio.json`](./checkpoint1_guillermo_fidelio.json)

## Arquitectura del flujo

```
[Telegram Trigger] → [AI Agent (Tools Agent) + System Prompt] → [Gmail: Log de Observabilidad] → [Telegram: respuesta al expositor]
                          ├─ Chat Model: OpenAI Chat Model
                          └─ Tool: Google Sheets "Registrar consulta de expositor"
```

| Componente | Implementación |
|---|---|
| Disparador | Telegram Trigger ("EXPOREDY asistente"), que recibe los mensajes desestructurados de los expositores mediante webhook |
| Cerebro | AI Agent en modo Tools Agent |
| Modelo | OpenAI Chat Model (nodo nativo) |
| Guardrail | Max Iterations = 10 |
| System Prompt | Modular: Rol y Ámbito → Objetivo Operativo → Reglas de Uso de Herramientas → Protocolo de Escalamiento / Guardrails → Estilo |
| Herramienta | Google Sheets Tool conectada lateralmente al agente (no secuencial), con descripción semántica extensa de cuándo activarla y cuándo no |
| Observabilidad | Gmail envía el mensaje original, la respuesta del agente y el Execution Log (intermediate steps) |

## Rol del agente

**Agente de Triaje de Soporte para expositores de EXPORED.** Interpreta mensajes desestructurados, identifica el problema principal, clasifica la categoría (Stand, Logística, Servicios Técnicos, Administración, Información General) y la prioridad (Alta, Media, Baja), y registra la consulta en Google Sheets para el equipo humano.

**Acciones que el agente NO puede hacer:** prometer tiempos de resolución, modificar reservas, otorgar reembolsos o descuentos, hacer excepciones al reglamento, inventar datos, borrar registros, pedir datos bancarios, dar asesoramiento legal ni comprometer decisiones comerciales.

## Cómo importarlo

1. En n8n: **Import from File** y elegir el `.json`.
2. Configurar las credenciales: Telegram, OpenAI, Google Sheets y Gmail.
3. Reemplazar los placeholders:
   - Nodo **Registrar consulta de expositor**: `TU_ID_DE_PLANILLA` por la URL de la planilla. La planilla debe tener una pestaña **Consultas** con estas columnas: `Fecha | Contacto | Empresa | Medio de contacto | Categoria | Prioridad | Resumen | Estado | Proxima accion | Notas`.
   - Nodo **Log de Observabilidad (Gmail)**: `TU_CORREO@ejemplo.com` por el correo de supervisión.

> Por privacidad, en este repositorio se reemplazaron el correo de destino y la URL de la planilla por placeholders.

## Validación

El flujo se probó con mensajes reales por Telegram. Las ejecuciones terminaron con éxito: el agente activó la herramienta de Google Sheets cuando correspondía, registró la consulta y envió el log de observabilidad por Gmail.

## Roadmap del proyecto integrador

M2 Multi-agente · M3 Memoria (Airtable por Session_ID) · M4 Integraciones OAuth2 · M5 RAG · M6 Voz · … · M11 Proyecto Final.
