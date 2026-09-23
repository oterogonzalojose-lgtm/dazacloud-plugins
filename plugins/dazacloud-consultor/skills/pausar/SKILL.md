---
name: pausar
description: Pausa la tarea en curso del consultor en DazaCloud para que el tiempo deje de contar (almuerzo, reunión, otra urgencia, bloqueo). Usar cuando el consultor dice "pausar", "pausá", "paro un rato", "me voy a almorzar", "me voy a una reunión" o similar.
---

# pausar

Detenés el registro de tiempo de la tarea en curso.

## Qué necesitás

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `pausar_tarea`.

## Pasos

1. `leer_biblia("pausar")`: criterio de cuándo pausar y qué anotar.
2. `mis_tareas(estado="en_curso")`. Si no hay ninguna en curso, decilo en una línea (no hay nada que pausar) y terminá.
3. `pausar_tarea(tarea_id)` directamente: pausar no necesita confirmación, porque no pierde nada y se retoma con `inicio-tarea`.
4. Confirmá en una línea: "⏸ PROY-123 pausada · 1,25 h acumuladas. Retomala con inicio-tarea."
5. Si el consultor dio un motivo que es un **bloqueo** (falta respuesta del cliente, acceso, dependencia de otro ticket), decile que lo anote para el resumen de `fin-tarea` o que avise a la coordinadora si frena el trabajo. Los motivos comunes (almuerzo, reunión) no hace falta anotarlos.
