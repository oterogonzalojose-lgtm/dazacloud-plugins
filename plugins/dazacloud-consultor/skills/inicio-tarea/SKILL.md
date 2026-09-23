---
name: inicio-tarea
description: Arranca o retoma una tarea asignada por la coordinadora en DazaCloud. Baja la spec copiada del ticket, las advertencias y, en retrabajos, el motivo del rechazo; y empieza a registrar el tiempo. Usar cuando el consultor dice "inicio-tarea", "arranco PROY-123", "empiezo con…", "retomo…", "vuelvo a la tarea" o similar.
---

# inicio-tarea

Preparás al consultor para trabajar en una tarea y empezás a registrar su tiempo. Los consultores **no tienen acceso al sistema de tickets del cliente**: todo lo que necesitan está en la spec que copió la coordinadora.

## Reglas que no se rompen

- El tiempo empieza a contar **solo** cuando llamás `iniciar_tarea`, y solo después de que el consultor confirme cuál tarea arranca.
- No inventes información del ticket: si la spec no dice algo, es un hueco para consultar con la coordinadora.

## Qué necesitás

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `ver_tarea`, `iniciar_tarea`. Si da 401, el token no está cargado o es inválido: que le pida a la coordinadora un token nuevo y reinstale el plugin (`claude plugin uninstall dazacloud-consultor@dazacloud-plugins` y `claude plugin install dazacloud-consultor@dazacloud-plugins`).

## Pasos

1. `leer_biblia("inicio-tarea")` y `leer_biblia("general")`. Su criterio (qué mostrar, checklist de arranque) manda sobre lo de abajo.
2. **Qué tarea.** Si el consultor nombró un ticket (por ejemplo "PROY-123"), buscalo en `mis_tareas` por `clave_ticket`. Si no nombró ninguno, mostrá las no iniciadas y pausadas (una línea cada una) y preguntá cuál. Si hay más de una tarea con esa clave, preguntá cuál.
3. Si la tarea está **pendiente de aprobación**, no se puede iniciar: decile que está esperando a la coordinadora. Si ya estaba **en curso**, decíselo y seguí al paso 5 sin volver a iniciar.
4. Antes de iniciar, anotá cuál tarea estaba **en curso** según el `mis_tareas` del paso 2 (si hay una distinta de la elegida). `iniciar_tarea(tarea_id)`. Si había otra en curso, el backend la pausa sola: llamá `mis_tareas(estado="pausada")`, buscala ahí y avisale que quedó pausada y con cuántas horas acumuladas.
5. `ver_tarea(tarea_id)` y presentá:

```
▶ PROY-123 · Generar PDF falla al abrir la cotización · <cliente>
  Timer corriendo · acumulado <horas registradas> h

⚠ Advertencias de la coordinadora
  <advertencia completa, tal cual>

Objetivo      <una línea>
Qué tocar     <objetos, campos, clases, LWC, flows>
Criterios de aceptación
  1. …
Huecos en la spec  <lo que no queda claro; "ninguno" si está completa>
```

   - **Retrabajo** (la advertencia dice "Devuelta:" o la spec tiene "Motivo del rechazo"): mostrá primero el motivo del rechazo, hallazgo por hallazgo, y después la spec original resumida.
   - Mantené los API names tal cual (`Fecha_Vencimiento__c`).

6. Ofrecé el checklist de arranque de la biblia (entorno, dependencias) y ayudá a arrancar: por ejemplo, un plan de pasos a partir de los criterios de aceptación.

Recordale en una línea que puede decir **pausar** si deja la tarea, y **fin-tarea** cuando termine.
