---
name: fin-tarea
description: Cierra una tarea en DazaCloud. Hace las preguntas guiadas, arma el resumen interno de lo hecho, propone las horas registradas para que el consultor las confirme o corrija, pide justificar las desviaciones frente al reloj y a la estimación, pregunta si hubo corrección de un error propio, y la envía a aprobación de quien coordina el proyecto (la coordinadora o el líder de ese cliente). Usar cuando el consultor dice "fin-tarea", "terminé", "cerré PROY-123", "listo con…" o similar.
---

# fin-tarea

Cierras la tarea del consultor con un resumen claro y sus horas. El resumen es **interno**: quien coordina el proyecto (la coordinadora o el líder de ese cliente) lo lee para aprobar las horas sin tener que llamar al consultor, y a partir de él redacta lo que ve el cliente.

## Reglas que no se rompen

- **Las horas las declara el consultor.** Propones las registradas por el reloj, pero el número final lo dice él.
- **No envíes nada sin que el consultor apruebe el resumen y las horas.** Una vez enviada, la tarea queda en manos de quien coordina el proyecto y ya no se puede editar.
- No inventes trabajo, pruebas ni evidencia: el resumen se arma solo con lo que el consultor contó (y lo que haya hecho contigo en esta sesión). Si falta algo, queda escrito que falta.
- Superar la estimación no es un error: se pide la causa, no se juzga.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `finalizar_tarea`.

## Pasos

1. `leer_biblia("fin-tarea")` y `leer_biblia("general")`: preguntas guiadas, formato del resumen, umbrales y qué no se factura. Mandan sobre lo de abajo, salvo en "Reglas que no se rompen".
2. **Qué tarea.** La que está en curso. Si no hay ninguna en curso, pregunta entre las pausadas o no iniciadas (una línea cada una).
3. `ver_tarea(tarea_id)`: spec, criterios de aceptación, advertencias (motivo del rechazo, si es retrabajo), `horas_estimadas` y `horas_registradas`.
4. **Qué hizo.** Lo que hay que saber está en las preguntas guiadas de la biblia (por defecto: qué se pidió y qué hizo; qué modificó exactamente —objetos, campos, clases, flows—; qué probó y con qué resultado; si se cumplió el objetivo y qué criterios de aceptación cubre; en retrabajos, qué hizo con cada hallazgo del QA y si el rechazo vino de un error propio; pendientes; evidencia; bloqueos; tiempo corrigiendo un error propio). **No las hagas todas juntas.** Arranca con una sola pregunta abierta:

   > "Cuéntame con tus palabras qué hiciste, qué probaste y cómo quedó. Si quedó algo pendiente, hubo algún bloqueo o parte del tiempo fue corregir un error tuyo, dilo también."

   Con esa respuesta completa todo lo que puedas y repregunta **de a una** solo lo que falte o haya quedado vago (como mucho tres repreguntas), por ejemplo: "¿Qué probaste exactamente?" o "De estos criterios, ¿cuáles quedaron? 1) … 2) … 3) …". Si en esta misma sesión trabajaste con el consultor en la tarea, arma lo que ya sabes y pídele solo que confirme. Si algo sigue sin saberse después de las repreguntas, queda escrito que falta.

5. **Tiempo.** Una pregunta, con el reloj como primera opción:

   > "El reloj marcó 35 min. ¿Cuánto le dedicaste?  1) 35 min   2) Otro (dime cuánto)"

   El consultor habla en horas y minutos; tú pasas a decimales solo al llamar a la herramienta (múltiplos de 0,25 h, máximo 24). Umbrales por defecto (la biblia manda):
   - **Declarado vs. reloj:** toda diferencia se justifica con una actividad concreta, su motivo y su resultado ("¿Qué hiciste en esos 25 min fuera del reloj?"). Si es mayor a 1 h, pide además una referencia donde se pueda validar (ticket, subtarea, evidencia).
   - **Declarado vs. estimación** (`horas_estimadas`, si la hay): si la supera en **más de 30 min y más del 20 %**, o en **más de 2 h**, pide que explique la causa y el trabajo adicional. Por ejemplo, con 4 h estimadas: 4 h 30 min no pide nada (30 min justos); 5 h sí (1 h y 25 %).
   - **Error propio:** si dijo que hubo, anota cuánto en el resumen. No lo descuentes de lo declarado: quien coordina el proyecto decide al aprobar qué parte no se cobra.
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
Error propio: <no / sí, unos 30 min: …>
Horas: <declaradas> (reloj: <registradas>; estimación: <estimadas>; <justificación de las diferencias>)
```

   El resumen se guarda con ese formato (lo lee quien coordina el proyecto), con los tiempos en horas y minutos. Muéstralo así y pregunta:

   > "¿Lo envío a aprobación?  1) Sí, envíalo   2) Quiero corregir algo"
7. Con el OK: `finalizar_tarea(tarea_id, resumen, horas_declaradas)`. Confirma: "✅ Enviada. PROY-123 quedó esperando aprobación (2 h 15 min)."

Si el backend rechaza el cierre (por ejemplo, la tarea ya estaba cerrada), explica en una frase qué pasó y no reintentes.
