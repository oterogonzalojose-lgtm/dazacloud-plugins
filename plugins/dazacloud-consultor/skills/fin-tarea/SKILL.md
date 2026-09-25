---
name: fin-tarea
description: Cierra una tarea en DazaCloud. Hace las preguntas guiadas, arma el resumen interno de lo hecho, propone las horas registradas para que el consultor las confirme o corrija, pide justificar las desviaciones frente al reloj y a la estimación, pregunta si hubo corrección de un error propio, y la envía a aprobación de la coordinadora. Usar cuando el consultor dice "fin-tarea", "terminé", "cerré PROY-123", "listo con…" o similar.
---

# fin-tarea

Cierras la tarea del consultor con un resumen claro y sus horas. El resumen es **interno**: la coordinadora lo lee para aprobar las horas sin tener que llamar al consultor, y a partir de él redacta lo que ve el cliente.

## Reglas que no se rompen

- **Las horas las declara el consultor.** Propones las registradas por el reloj, pero el número final lo dice él.
- **No envíes nada sin que el consultor apruebe el resumen y las horas.** Una vez enviada, la tarea queda en manos de la coordinadora y ya no se puede editar.
- No inventes trabajo, pruebas ni evidencia: el resumen se arma solo con lo que el consultor contó (y lo que haya hecho contigo en esta sesión). Si falta algo, queda escrito que falta.
- Superar la estimación no es un error: se pide la causa, no se juzga.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `finalizar_tarea`.

## Pasos

1. `leer_biblia("fin-tarea")` y `leer_biblia("general")`: preguntas guiadas, formato del resumen, umbrales y qué no se factura. Mandan sobre lo de abajo, salvo en "Reglas que no se rompen".
2. **Qué tarea.** La que está en curso. Si no hay ninguna en curso, pregunta entre las pausadas o no iniciadas (una línea cada una).
3. `ver_tarea(tarea_id)`: spec, criterios de aceptación, advertencias (motivo del rechazo, si es retrabajo), `horas_estimadas` y `horas_registradas`.
4. **Preguntas guiadas**, todas juntas y cortas. Por defecto (las de la biblia):
   1. ¿Qué se pidió y qué hiciste?
   2. ¿Qué modificaste o desarrollaste exactamente? (objetos, campos, clases, flows, configuraciones)
   3. ¿Qué probaste y con qué resultado? (escenarios)
   4. ¿Se cumplió el objetivo? Cumplido, parcial o no completado; si no, qué falta y por qué. (Lista los criterios de aceptación numerados para que responda por número.)
   5. Solo retrabajos: ¿qué hiciste con cada hallazgo del QA (por número)? ¿El rechazo vino de un error propio?
   6. ¿Quedó algo pendiente o que tenga que validar el cliente?
   7. ¿Hay evidencia? (enlace a la sandbox, al registro, a capturas compartidas)
   8. ¿Hubo bloqueos durante la tarea?
   9. ¿Parte del tiempo fue corregir un error propio o rehacer algo mal implementado? ¿Cuánto, más o menos?

   Si en esta misma sesión trabajaste con el consultor en la tarea, prellena las respuestas con lo que ya sabes y pídele que confirme. Si una respuesta es vaga ("lo arreglé", "quedó funcionando"), repregunta una vez: qué problema había, qué se ajustó exactamente, qué se probó y con qué resultado.
5. **Horas.** Muestra las registradas (reloj) y pregunta si las confirma o cuántas declara. Múltiplos de 0,25 h, máximo 24. Umbrales por defecto (la biblia manda):
   - **Declaradas vs. reloj:** toda diferencia se justifica con una actividad concreta, su motivo y su resultado. Si es mayor a 1 h, pide además una referencia donde se pueda validar (ticket, subtarea, evidencia).
   - **Declaradas vs. estimación** (`horas_estimadas`, si la hay): si la superan en **más de 30 min y más del 20 %**, o en **más de 2 h**, pide que explique la causa y el trabajo adicional. Por ejemplo, con 4 h estimadas: 4,5 h no pide nada (30 min justos); 5 h sí (1 h y 25 %).
   - **Error propio:** si dijo que hubo, anota cuánto en el resumen. No lo descuentes de las declaradas: la coordinadora decide al aprobar qué parte no se factura.
6. **Borrador.** Arma el resumen con el formato de la biblia. Por defecto:

```
Resumen: <qué se solicitó y qué se hizo>
Solución: <qué se modificó o desarrolló: objetos / campos / clases / flows>
Pruebas: <qué se validó y resultado>
Resultado: ✅ Cumplido | Parcial | No completado
Hallazgos QA: <H6 resuelto: …>   (solo retrabajos)
Pendientes: <nada / …>
Evidencia: <enlaces / no hay>
Bloqueos: <ninguno / …>
Error propio: <no / sí, ~0,5 h: …>
Horas: <declaradas> (reloj: <registradas>; estimación: <estimadas>; <justificación de las diferencias>)
```

   Muéstralo y pide OK o correcciones.
7. Con el OK: `finalizar_tarea(tarea_id, resumen, horas_declaradas)`. Confirma: "✔ PROY-123 enviada a aprobación · 2,25 h declaradas."

Si el backend rechaza el cierre (por ejemplo, la tarea ya estaba cerrada), muestra el motivo tal cual y no reintentes.
