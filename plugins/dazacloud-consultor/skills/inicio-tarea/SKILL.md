---
name: inicio-tarea
description: Arranca o retoma una tarea asignada por la coordinadora en DazaCloud. Muestra las advertencias, la estimación, los últimos comentarios del cliente, la spec copiada del ticket y, en retrabajos, el motivo del rechazo; avisa si la tarea ya superó la estimación, y empieza a registrar el tiempo. Usar cuando el consultor dice "inicio-tarea", "arranco PROY-123", "empiezo con…", "retomo…", "vuelvo a la tarea" o similar.
---

# inicio-tarea

Preparas al consultor para trabajar en una tarea y empiezas a registrar su tiempo. La mayoría de los consultores **no tiene acceso al sistema de tickets del cliente**: todo lo que necesitan está en la spec y en los comentarios del cliente que copió la coordinadora.

## Reglas que no se rompen

- El tiempo empieza a contar **solo** cuando llamas `iniciar_tarea`, y solo después de que el consultor confirme cuál tarea arranca.
- No inventes información del ticket: si la spec no dice algo, es un hueco para consultar (con `nota-cliente`).
- La estimación que ves es la de la coordinadora (`horas_estimadas`). No hay otra: no supongas ni menciones ninguna otra estimación.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `iniciar_tarea`. Si no responde o da 401, falta la clave de acceso o no es válida: dile en una frase "No pude conectarme con DazaCloud. Pídele a la coordinadora una clave nueva." y para. Los pasos para cargarla (`/plugin configure dazacloud-consultor@dazacloud-plugins`, o reinstalar el plugin) solo si los pide.

## Pasos

1. `leer_biblia("inicio-tarea")` y `leer_biblia("general")`. Su criterio (qué mostrar y en qué orden, checklist de arranque, tono) manda sobre lo de abajo, salvo en "Reglas que no se rompen". Si están vacías, sigue este archivo.
2. **Qué tarea.** Si el consultor nombró un ticket (por ejemplo "PROY-123"), búscalo en `mis_tareas` por `clave_ticket`. Si no nombró ninguno, muestra las no iniciadas y pausadas (una línea cada una) y pregunta cuál. Si hay más de una tarea con esa clave, pregunta cuál.
3. Si la tarea está **pendiente de aprobación**, no se puede iniciar: dile que está esperando a la coordinadora. Si ya estaba **en curso**, díselo y sigue al paso 5 sin volver a iniciar.
4. Antes de iniciar, anota cuál tarea estaba **en curso** según el `mis_tareas` del paso 2 (si hay una distinta de la elegida). `iniciar_tarea(tarea_id)`. Si había otra en curso, el backend la pausa sola: llama `mis_tareas(estado="pausada")`, búscala ahí y avísale en una línea: "⏸️ Dejé en pausa PROY-118 (llevabas 2 h)."
5. `ver_tarea(tarea_id)` y presenta, por defecto en este orden:

```
▶️ Arrancó el reloj · PROY-123 Generar PDF de la cotización
   Llevas 1 h 15 min de 4 h estimadas.

⚠️ Ojo, de la coordinadora: <advertencia completa, tal cual>

🎯 Qué lograr: <una línea>
🧭 Incluye: … · No incluye: …
✔️ Está listo cuando:
   1. …
   2. …
💬 Dijo el cliente: <comentarios_cliente, resumidos; se omite si no hay>
❓ No está claro: <huecos de la spec; se omite si no hay>

¿Quieres ver el contexto completo o armamos un plan de pasos?  1) Contexto   2) Plan   3) Así está bien
```

   - El **contexto** (cómo funciona hoy, quién lo usa) y la spec completa van en la opción 1, no en la tarjeta.
   - **Estimación.** Si `horas_estimadas` viene vacía, la segunda línea dice solo "Llevas <horas_registradas>" (en horas y minutos). Si viene, recuérdale en una línea que si ve que va a necesitar más horas, lo informe a la coordinadora **antes** de continuar.
   - **Ya excede la estimación** (`excede_estimacion = true`): justo debajo del título, resaltado: "⚠️ Ya te pasaste de lo estimado (<registradas> de <estimadas>). Avísale a la coordinadora antes de seguir." No lo repitas más abajo.
   - **Retrabajo** (la advertencia dice "Devuelta:" o la spec tiene "Motivo del rechazo"): muestra primero el motivo del rechazo, hallazgo por hallazgo (bloqueantes y mayores primero), y después la spec original resumida.
   - Mantén los API names tal cual (`Fecha_Vencimiento__c`).
   - Si falta objetivo, alcance o criterios de aceptación, dilo en "No está claro": no los completes por tu cuenta.

6. Ofrece el checklist de arranque de la biblia (impacto y dependencias, información disponible, entorno) y ayuda a arrancar: por ejemplo, un plan de pasos a partir de los criterios de aceptación.

Cierra con una línea en palabras, sin nombres de skills: "Cuando pares, dime «pausa»; si tienes una duda para el cliente, «nota»; y al terminar, «terminé»."
