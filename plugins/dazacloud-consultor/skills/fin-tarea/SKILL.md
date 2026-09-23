---
name: fin-tarea
description: Cierra una tarea en DazaCloud. Hace las preguntas guiadas, arma el resumen de lo hecho, propone las horas registradas para que el consultor las confirme o corrija, y la envía a aprobación de la coordinadora. Usar cuando el consultor dice "fin-tarea", "terminé", "cerré PROY-123", "listo con…" o similar.
---

# fin-tarea

Cerrás la tarea del consultor con un resumen claro y sus horas. La coordinadora lo va a leer para aprobar las horas que se imputan al cliente, y lo usa para redactar el comentario en el ticket.

## Reglas que no se rompen

- **Las horas las declara el consultor.** Proponés las registradas por el timer, pero el número final lo dice él.
- **No envíes nada sin que el consultor apruebe el resumen y las horas.** Una vez enviada, la tarea queda en manos de la coordinadora y ya no se puede editar.
- No inventes trabajo: el resumen se arma solo con lo que el consultor contó (y lo que haya hecho con vos en esta sesión).

## Qué necesitás

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `finalizar_tarea`.

## Pasos

1. `leer_biblia("fin-tarea")` y `leer_biblia("general")`: preguntas guiadas, formato del resumen y criterio de horas. Mandan sobre lo de abajo.
2. **Qué tarea.** La que está en curso. Si no hay ninguna en curso, preguntá entre las pausadas o no iniciadas (una línea cada una).
3. `ver_tarea(tarea_id)`: spec, criterios de aceptación, advertencias (motivo del rechazo, si es retrabajo) y horas registradas.
4. **Preguntas guiadas**, todas juntas y cortas. Por defecto:
   1. ¿Qué hiciste? (objetos, campos, clases, flows, configuraciones)
   2. ¿En qué entorno quedó y se desplegó?
   3. ¿Se cubren los criterios de aceptación? (listalos numerados para que responda por número)
   4. Solo retrabajos: ¿qué hiciste con cada hallazgo del rechazo?
   5. ¿Quedó algo pendiente o que tenga que validar el cliente?

   Si en esta misma sesión trabajaste con el consultor en la tarea, prellená las respuestas con lo que ya sabés y pedile que confirme. Si una respuesta es vaga ("lo arreglé"), repreguntá una vez.
5. **Horas.** Mostrá las registradas (timer) y preguntá si las confirma o cuántas declara. Si la diferencia es mayor a 1 h, pedí el motivo y sumalo al resumen. Múltiplos de 0,25 h, máximo 24.
6. **Borrador.** Armá el resumen con el formato de la biblia. Por defecto:

```
Qué se hizo: <1–3 líneas>
Cambios: <objetos / clases / flows>
Entorno: <dónde quedó>
Criterios de aceptación: <cubiertos / falta X porque …>
Hallazgos QA: <H2 resuelto: …>          (solo retrabajos)
Pendiente: <nada / …>
Horas: <declaradas> (timer: <registradas>; <motivo de la diferencia, si hay>)
```

   Mostralo y pedí OK o correcciones.
7. Con el OK: `finalizar_tarea(tarea_id, resumen, horas_declaradas)`. Confirmá: "✔ PROY-123 enviada a aprobación · 2,25 h declaradas."

Si el backend rechaza el cierre (por ejemplo, la tarea ya estaba cerrada), mostrá el motivo tal cual y no reintentes.
