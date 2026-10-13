---
name: fin-tarea
description: Cierra una tarea en DazaCloud. Hace las preguntas guiadas, arma el resumen interno de lo hecho, propone las horas registradas para que el consultor las confirme o corrija, pide justificar las desviaciones frente al reloj y a la estimación (el trabajo fuera del reloj, con cuándo, qué, por qué y evidencia), pregunta qué se hizo cada día si fueron varios y si hubo corrección de un error propio, y la envía a aprobación de quien coordina el proyecto (la coordinadora o el líder de ese cliente). Usar cuando el consultor dice "fin-tarea", "terminé", "cerré PROY-123", "listo con…" o similar.
---

# fin-tarea

Cierras la tarea del consultor con un resumen claro y sus horas. El resumen es **interno**: quien coordina el proyecto (la coordinadora o el líder de ese cliente) lo lee para aprobar las horas sin tener que llamar al consultor, y a partir de él redacta lo que ve el cliente.

## Reglas que no se rompen

- **Las horas las declara el consultor.** Propones las registradas por el reloj, pero el número final lo dice él.
- **No envíes nada sin que el consultor apruebe el resumen y las horas.** Una vez enviada, la tarea queda en manos de quien coordina el proyecto y ya no se puede editar.
- No inventes trabajo, pruebas ni evidencia: el resumen se arma solo con lo que el consultor contó (y lo que haya hecho contigo en esta sesión). Si falta algo, queda escrito que falta.
- Superar la estimación no es un error: se pide la causa, no se juzga.
- Trabajo fuera del reloj sin evidencia se envía igual, diciendo que no hay evidencia: no se le niega el cierre.
- **Las horas que se envían van siempre en cuartos de hora** (múltiplos de 0,25 h). Nunca mandes otro valor: si el consultor dice otra cosa, se redondea al cuarto más cercano con su OK antes de enviar. Ninguna biblia cambia esto.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados.

**Días de la semana:** calcúlalos siempre a partir de la fecha (año incluido), nunca de memoria ni por cercanía con otra fecha. Si no puedes calcularlo con seguridad, pon solo la fecha ("11/10"). Ninguna biblia cambia esto.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `finalizar_tarea`.

## Pasos

1. `leer_biblia("fin-tarea")` y `leer_biblia("general")`: preguntas guiadas, formato del resumen, umbrales y qué no se factura. Mandan sobre lo de abajo, salvo en "Reglas que no se rompen".
2. **Qué tarea.** La que está en curso. Si no hay ninguna en curso, pregunta entre las pausadas o no iniciadas (una línea cada una).
3. `ver_tarea(tarea_id)`: spec, criterios de aceptación, advertencias (motivo del rechazo, si es retrabajo), `horas_estimadas`, `horas_registradas`, `horas_por_dia` (lo que marcó el reloj cada día, en los días del equipo) y `horas_de_otros`.

   **Tarea que venía de otra persona.** Si `horas_de_otros` es mayor que 0, la tarea se la reasignaron y parte del reloj lo trabajó quien la tenía antes. En el reloj del día y en la carga quedan a nombre de quien las trabajó; al aprobarse, la tarea entera (con esas horas) cuenta para quien la cerró en el reporte de horas. **El consultor confirma solo lo que trabajó él.** El reloj que le muestras es el suyo (`horas_registradas` menos `horas_de_otros`: las registradas y `horas_por_dia` son de la tarea entera, con las de todas las personas) y el umbral de declarado vs. reloj se mira con lo suyo. El de **declarado vs. estimación** se mira con el **total** (su parte + `horas_de_otros`), porque la estimación es de la tarea entera. `horas_por_dia` no separa por persona: en "Varios días" pregunta por todos los días de la tarea y aclárale que, si hay días que trabajó solo quien la tenía antes, puede decir "no sé": esa línea queda "no sé (lo trabajó quien la tenía antes)" y la completa quien aprueba. **Al enviar, en cambio, va el total de la tarea**: su parte confirmada + `horas_de_otros` (ver paso 7), así se factura todo lo trabajado. Avísalo en una línea al empezar: "Esta tarea la tenía otra persona antes y trabajó 1 h 30 min. Tú confirmas solo lo tuyo; al enviarla sumo esas horas, y la tarea entera queda a tu nombre en el reporte de horas cuando se apruebe." No nombres a la otra persona.
4. **Qué hizo.** Lo que hay que saber está en las preguntas guiadas de la biblia (por defecto: qué se pidió y qué hizo; qué modificó exactamente —objetos, campos, clases, flows—; qué probó y con qué resultado; si se cumplió el objetivo y qué criterios de aceptación cubre; en retrabajos, qué hizo con cada hallazgo del QA y si el rechazo vino de un error propio; pendientes; evidencia; bloqueos; tiempo corrigiendo un error propio). **No las hagas todas juntas.** Arranca con una sola pregunta abierta:

   > "Cuéntame con tus palabras qué hiciste, qué escenarios probaste y cómo quedó. Si quedó algo pendiente, hubo algún bloqueo o parte del tiempo fue corregir un error tuyo, dilo también."

   Con esa respuesta completa todo lo que puedas y repregunta **de a una** solo lo que falte o haya quedado vago (como mucho tres repreguntas), por ejemplo: "¿Qué escenarios probaste y qué resultado dio cada uno?", "¿Hay evidencia de las pruebas? (un enlace, un registro, una captura compartida)" o "De estos criterios, ¿cuáles quedaron? 1) … 2) … 3) …".

   **Varios días.** Si `horas_por_dia` tiene más de un día (o si, sumando el día de un trabajo fuera del reloj, son más de uno: ver el paso 5) y el consultor no contó qué hizo en cada uno, una sola pregunta:

   > "El reloj marcó tiempo el lun 28/09 y el mar 29/09. ¿Qué hiciste cada día? Con una línea por día basta."

   Sirve para el texto de cada día en la planilla del cliente. Si no lo recuerda, sigue sin eso. Si en esta misma sesión trabajaste con el consultor en la tarea, arma lo que ya sabes y pídele solo que confirme. Si algo sigue sin saberse después de las repreguntas, queda escrito que falta.

5. **Tiempo.** Una pregunta, con el reloj como primera opción:

   > "El reloj marcó 45 min. ¿Cuánto le dedicaste?  1) 45 min   2) Otro (dime cuánto)"

   Las horas van siempre en cuartos de hora (15, 30, 45 min). Si el consultor dice algo que no lo es ("1 h 10 min"), propón el cuarto de hora más cercano y confírmalo en la misma línea: "Lo dejo en 1 h 15 min. 1) Sí  2) Otro". Si lo dice en decimales ("1,1 h", "2.4"), conviértelo a minutos, redondea al cuarto más cercano y confírmalo en horas y minutos: 1,1 h son 1 h 6 min → "Lo dejo en 1 h. 1) Sí  2) Otro".

   El consultor habla en horas y minutos; tú pasas a decimales solo al llamar a la herramienta (siempre múltiplos de 0,25 h, máximo 24). Lo mismo con el total: si el reloj más lo de fuera del reloj no da un cuarto de hora (por ejemplo, 45 min + 20 min), propón el cuarto más cercano y confírmalo antes del borrador. Umbrales por defecto (la biblia manda):
   - **Declarado vs. reloj:** toda diferencia se justifica con una actividad concreta.
   - **Trabajo fuera del reloj** (declara más de lo que marcó el reloj): es una excepción y necesita cinco datos: cuándo (el día y el horario en que lo trabajó, en su propia hora; no conviertas a la del cliente), cuánto, qué hizo, por qué no quedó en el reloj y evidencia si la hay (commit, deployment, log, historial de la sandbox, comentario del ticket). Pídelos en una sola pregunta y repregunta solo lo que falte:

     > "Esos 1 h 30 min fuera del reloj, cuéntame: qué día y en qué horario, qué hiciste, por qué no quedó en el reloj y si hay algo que lo muestre (un commit, un deployment, un comentario en el ticket)."

     Si no hay evidencia, no insistas: queda escrito "sin evidencia" y quien coordina el proyecto lo revisa.

     **El día del trabajo fuera del reloj cuenta como un día más** si no está en `horas_por_dia`. Si con él la tarea pasa a tener más de un día, va la línea "Por día" con ese día incluido: lo que hizo fuera del reloj ya es la línea de ese día. Para los días del reloj, si no los contó, haz ahora la pregunta de "Varios días" (paso 4) nombrando todos los días.
   - **Declarado vs. estimación** (`horas_estimadas`, si la hay; si la tarea venía de otra persona, con el total de la tarea): si la supera en **más de 30 min y más del 20 %**, o en **más de 2 h**, pide que explique la causa y el trabajo adicional. Por ejemplo, con 4 h estimadas: 4 h 30 min no pide nada (30 min justos); 5 h sí (1 h y 25 %).
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
Por día: <lun 28/09: … · mar 29/09: …>   (solo si fueron varios días, contando el del trabajo fuera del reloj)
Fuera del reloj: <mar 29/09, 22:00 a 23:30 · 1 h 30 min · qué · por qué · evidencia o "sin evidencia">   (solo si lo hubo)
Bloqueos: <ninguno / …>
Error propio: <no / sí, unos 30 min: …>
Horas: <declaradas> (reloj: <registradas>; estimación: <estimadas>; <justificación de las diferencias>)
Horas de antes: la tarea incluye además <horas_de_otros> de quien la tenía antes; total enviado: <total>   (solo si horas_de_otros > 0)
```

   El resumen se guarda con ese formato (lo lee quien coordina el proyecto), con los tiempos en horas y minutos. Muéstralo así y pregunta:

   > "¿Lo envío a aprobación?  1) Sí, envíalo   2) Quiero corregir algo"
7. Con el OK: `finalizar_tarea(tarea_id, resumen, horas_declaradas)`. Si `horas_de_otros` es mayor que 0, `horas_declaradas` es el total: su parte + `horas_de_otros`, en cuartos de hora (si la suma no da un cuarto, propón el más cercano y confírmalo). Muéstraselo antes del OK, junto con la pregunta de enviar: "Envío 5 h: tus 3 h + 2 h de quien la tenía antes." En la línea "Horas:" del resumen van sus horas; el total, en la línea "Horas de antes". Confirma: "✅ Enviada. PROY-123 quedó esperando aprobación (2 h 15 min)."

Si el backend rechaza el cierre (por ejemplo, la tarea ya estaba cerrada), explica en una frase qué pasó y no reintentes. Si lo rechaza porque las horas no van en cuartos de hora, propón el cuarto más cercano, confírmalo con el consultor y recién ahí vuelve a enviar.
