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

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `iniciar_tarea`. Si da 401, el token no está cargado o es inválido: que le pida a la coordinadora un token nuevo y reinstale el plugin (`claude plugin uninstall dazacloud-consultor@dazacloud-plugins` y `claude plugin install dazacloud-consultor@dazacloud-plugins`).

## Pasos

1. `leer_biblia("inicio-tarea")` y `leer_biblia("general")`. Su criterio (qué mostrar y en qué orden, checklist de arranque, tono) manda sobre lo de abajo, salvo en "Reglas que no se rompen". Si están vacías, sigue este archivo.
2. **Qué tarea.** Si el consultor nombró un ticket (por ejemplo "PROY-123"), búscalo en `mis_tareas` por `clave_ticket`. Si no nombró ninguno, muestra las no iniciadas y pausadas (una línea cada una) y pregunta cuál. Si hay más de una tarea con esa clave, pregunta cuál.
3. Si la tarea está **pendiente de aprobación**, no se puede iniciar: dile que está esperando a la coordinadora. Si ya estaba **en curso**, díselo y sigue al paso 5 sin volver a iniciar.
4. Antes de iniciar, anota cuál tarea estaba **en curso** según el `mis_tareas` del paso 2 (si hay una distinta de la elegida). `iniciar_tarea(tarea_id)`. Si había otra en curso, el backend la pausa sola: llama `mis_tareas(estado="pausada")`, búscala ahí y avísale que quedó pausada y con cuántas horas acumuladas.
5. `ver_tarea(tarea_id)` y presenta, por defecto en este orden:

```
▶ PROY-123 · Generar PDF falla al abrir la cotización · <cliente>
  Reloj corriendo · llevas 1,25 h de 4 h estimadas

⚠ Advertencias de la coordinadora
  <advertencia completa, tal cual>

Objetivo       <una línea>
Contexto       <cómo funciona hoy y quién lo usa>
Alcance        Incluye: … · No incluye: …
Criterios de aceptación
  1. …
Comentarios del cliente
  <comentarios_cliente, tal cual; "ninguno" si viene vacío>
Huecos en la spec  <lo que no queda claro; "ninguno" si está completa>
```

   - **Estimación.** Si `horas_estimadas` viene vacía, la segunda línea dice solo "llevas <horas_registradas> h". Si viene, recuérdale en una línea que si ve que va a necesitar más horas, lo informe a la coordinadora **antes** de continuar.
   - **Ya excede la estimación** (`excede_estimacion = true`): justo debajo del título, resaltado: "⚠ Ya superaste la estimación (<registradas> h de <estimadas> h). Infórmalo a la coordinadora antes de continuar." No lo repitas más abajo.
   - **Retrabajo** (la advertencia dice "Devuelta:" o la spec tiene "Motivo del rechazo"): muestra primero el motivo del rechazo, hallazgo por hallazgo (bloqueantes y mayores primero), y después la spec original resumida.
   - Mantén los API names tal cual (`Fecha_Vencimiento__c`).
   - Si falta objetivo, alcance o criterios de aceptación, dilo en "Huecos en la spec": no los completes por tu cuenta.

6. Ofrece el checklist de arranque de la biblia (impacto y dependencias, información disponible, entorno) y ayuda a arrancar: por ejemplo, un plan de pasos a partir de los criterios de aceptación.

Cierra con una línea: puede decir **pausar** si deja la tarea, **nota-cliente** si necesita preguntarle algo al cliente, y **fin-tarea** cuando termine.
