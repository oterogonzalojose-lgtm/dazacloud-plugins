---
name: pausar
description: Pausa la tarea en curso del consultor en DazaCloud para que el tiempo deje de contar (almuerzo, reunión interna, bloqueo, otra urgencia), registrando el motivo y si es un bloqueo. Usar cuando el consultor dice "pausar", "pausa", "paro un rato", "me voy a almorzar", "me voy a una reunión", "estoy bloqueado" o similar.
---

# pausar

Detienes el registro de tiempo de la tarea en curso y dejas anotado por qué.

## Reglas que no se rompen

- Solo se pausa la tarea **en curso** del propio consultor.
- No inventes el motivo: si el consultor no lo dijo, pregúntalo; si no quiere darlo, pausa sin motivo.
- La coordinadora se entera de los bloqueos con el resumen diario: no le prometas al consultor que ella ya lo sabe ni que va a hacer algo.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `pausar_tarea`.

## Pasos

1. `leer_biblia("pausar")` y `leer_biblia("general")`: cuándo se pausa, cuándo no, y qué es un bloqueo. Mandan sobre lo de abajo, salvo en "Reglas que no se rompen".
2. `mis_tareas(estado="en_curso")`. Si no hay ninguna en curso, dilo en una línea (no hay nada que pausar) y termina.
3. **Motivo.** Si el consultor ya lo dijo ("me voy a almorzar", "espero respuesta del cliente"), úsalo. Si no, pregúntalo en una línea: "¿Por qué pausas? (almuerzo, reunión interna, bloqueo: cliente / acceso / dependencia, otro)". No preguntes nada más.
4. **¿Corresponde pausar?** Según la biblia. Por defecto:
   - **Corregir un error propio: no se pausa.** El reloj sigue corriendo y se cuenta en el cierre (`fin-tarea` pregunta cuánto fue); la coordinadora decide al aprobar qué parte no se factura. Díselo en una línea y no pauses.
   - Si el motivo es trabajo real del ticket (analizar, desarrollar, probar, documentar, una reunión **con el cliente** sobre ese ticket), dile en una línea que eso cuenta como tiempo de la tarea y pregunta si igual quiere pausar. Pausa solo si confirma.
5. **¿Es un bloqueo?** Falta respuesta del cliente, falta un acceso, depende de otro ticket o de otra persona → `bloqueo=true`. Almuerzo, descanso, reunión interna, otra urgencia → `bloqueo=false`.
6. `pausar_tarea(tarea_id, motivo, bloqueo)`, con el motivo en pocas palabras y concreto ("Esperando credenciales de la sandbox del cliente", no "bloqueado").
7. Confirma en una línea: "⏸ PROY-123 pausada · 1,25 h acumuladas. Retómala con inicio-tarea."
   - Si fue un bloqueo, agrega: "Quedó registrado como bloqueo; la coordinadora lo ve en el resumen del día." Si lo que falta es una respuesta del cliente, ofrécele dejar la pregunta con **nota-cliente**.
