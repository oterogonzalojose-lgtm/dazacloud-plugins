---
name: nota-cliente
description: Deja en DazaCloud una pregunta o aclaración para el cliente sobre una tarea propia; la coordinadora la revisa y la publica en el ticket del cliente. Usar cuando el consultor dice "nota-cliente", "pregúntale al cliente…", "necesito que el cliente me confirme…", "deja una nota para el cliente", "no está claro el alcance de PROY-123" o similar.
---

# nota-cliente

Ayudas al consultor a dejar una pregunta o aclaración para el cliente. No va directo al cliente: la coordinadora la lee, la adapta si hace falta y la publica en el ticket. Por eso tiene que entenderse sola, sin contexto extra.

## Reglas que no se rompen

- **No se guarda nada sin el OK explícito del consultor** sobre el texto final.
- Solo sobre tareas propias y no aprobadas.
- La nota no promete fechas, horas, alcance ni viabilidad técnica, no culpa a nadie (ni al cliente ni al equipo) y no cuenta problemas internos. Si el consultor lo escribe así, propón una versión neutral.
- No inventes datos del ticket: si no sabes algo, pregúntale al consultor.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `dejar_nota`.

## Pasos

1. `leer_biblia("nota-cliente")`, `leer_biblia("general")` y `leer_biblia("inicio-tarea")` (qué hacer si la spec no está clara). Su criterio (tono, qué se le puede decir al cliente) manda sobre lo de abajo, salvo en "Reglas que no se rompen".
2. **Qué tarea.** La que nombró; si no nombró ninguna, la que está en curso. Si no hay en curso, pregunta entre sus tareas abiertas (una línea cada una). Si la tarea está pendiente de aprobación, se puede dejar la nota igual.
3. `ver_tarea(tarea_id)` para tener a mano la spec y los comentarios del cliente, y no preguntar algo que ya está respondido ahí. Si ya está respondido, díselo y cita dónde.
4. **Borrador.** Arma la nota con lo que dijo el consultor. Por defecto:

```
Contexto: <qué se está haciendo y dónde aparece la duda, en una o dos líneas>
Pregunta: <la duda concreta; si son varias, numeradas>
Opciones: <si hay alternativas claras, "A) … B) …"; si no, omitir>
Impacto: <qué queda frenado hasta tener la respuesta; "nada, se avanza con lo demás" si no frena>
```

   - Usa los términos del cliente y los API names tal cual (`Fecha_Vencimiento__c`).
   - Una nota por tema. Si mezcla temas distintos, propón separarlas.

   Muéstrala y pide OK o correcciones.
5. Con el OK: `dejar_nota(tarea_id, texto)`. Confirma: "✔ Nota guardada en PROY-123. La coordinadora la revisa y la publica en el ticket."
6. Si la duda frena el trabajo, ofrécele **pausar** la tarea como bloqueo y seguir con otra; si no la frena, recuérdale avanzar con lo que sí está claro.

Si el backend rechaza la nota (por ejemplo, la tarea ya fue aprobada), muestra el motivo tal cual y no reintentes.
