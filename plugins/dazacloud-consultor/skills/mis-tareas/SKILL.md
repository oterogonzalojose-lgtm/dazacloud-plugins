---
name: mis-tareas
description: Muestra las tareas abiertas asignadas al consultor en DazaCloud (en curso, devueltas, sin iniciar, pausadas), con advertencias, horas acumuladas frente a la estimación y un aviso si alguna ya la superó; y, si lo pide, su historial de tareas aprobadas con lo que hizo cada día y el texto para Jira. Usar cuando el consultor dice "mis tareas", "qué tengo", "qué me toca hoy", "qué tengo pendiente", "qué hice en PROY-123", "mi historial", "lo que hice la semana pasada" o similar.
---

# mis-tareas

Le muestras al consultor su trabajo asignado. Solo lectura: esta skill no inicia, pausa ni cierra nada.

## Reglas que no se rompen

- Solo lectura.
- **Nunca muestres horas aprobadas** (las que fija quien aprueba): una tarea aprobada figura como "Aprobada", sin la cantidad. Las tareas aprobadas solo aparecen en el historial, cuando el consultor lo pide. La única estimación que existe para el consultor es `horas_estimadas`.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados.

**Días de la semana:** calcúlalos siempre a partir de la fecha (año incluido), nunca de memoria ni por cercanía con otra fecha. Si no puedes calcularlo con seguridad, pon solo la fecha ("11/10"). Ninguna biblia cambia esto.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`, `mi_historial`. Si no responde o da 401, falta la clave de acceso o no es válida: dile en una frase "No pude conectarme con DazaCloud. Pídele a la coordinadora una clave nueva." y para. Los pasos para cargarla (`/plugin configure dazacloud-consultor@dazacloud-plugins`, o reinstalar el plugin) solo si los pide.

## Pasos

1. `leer_biblia("mis-tareas")` y `leer_biblia("general")`. Su criterio de qué mostrar, orden y formato manda sobre lo de abajo, salvo en "Reglas que no se rompen".
2. `mis_tareas` (sin filtro: abiertas y pendientes de aprobación).
3. Preséntalo agrupado. Por defecto (el de la biblia): primero la tarea en curso; después devueltas y urgentes (advertencias con ⚠); después el resto por fecha de asignación.

```
▶️ Trabajando ahora
   PROY-123 · PDF de la cotización — 1 h 15 min de 4 h

⚠️ Devueltas o urgentes
   PROY-118 · Validaciones en Cuenta — devuelta: falta la migración de datos
   PROY-125 · Error al guardar la oportunidad — prioridad alta, sin empezar (2 h)

📋 Por hacer
   PROY-121 · Reglas de descuento — sin empezar (3 h)
   PROY-110 · Integración de pagos — en pausa, 3 h 30 min de 3 h ⚠️ te pasaste de lo estimado

⏳ Esperando aprobación: PROY-097

¿Con cuál sigues? Dime, por ejemplo, «empiezo PROY-125».
```

- **Devueltas:** son las pausadas cuya advertencia empieza con "Devuelta:". Muestra el motivo al lado.
- Las advertencias de quien asignó la tarea se muestran siempre, al lado de la tarea (recortadas a una línea; completas en `inicio-tarea`).
- **Tiempo:** "<registradas> de <estimadas>" en horas y minutos si hay estimación; si no, "llevas <registradas>". Son el acumulado de cada tarea (el backend no separa por día): no muestres totales diarios ni horas de inicio.
- Si `excede_estimacion` es true, marca "⚠️ te pasaste de lo estimado" y recuérdale al final, en una línea, que lo informe a quien coordina el proyecto antes de seguir con esa tarea.
- Las pendientes de aprobación van en una sola línea con sus claves, sin horas.
- Si hay tareas asignadas hace más de 5 días hábiles que todavía no se iniciaron, recuérdalo en una línea al final.

Cierra con una sola línea sobre qué sigue, en palabras: "dime «empiezo PROY-125»" para arrancar o retomar, o "dime «terminé»" si la que está en curso ya está lista. No nombres las skills.

## Historial: lo que ya se aprobó

Cuando el consultor pregunta por trabajo ya aprobado ("¿qué hice en PROY-118?", "mi historial", "lo de la semana pasada", "necesito lo que puse en la tarea para pasarlo a Jira"):

1. `mi_historial(dias, ticket)`: por defecto los últimos 30 días; con `ticket` si nombró uno. Trae, por tarea: su cierre, lo que marcó su reloj cada día, el texto de cada día que fue a la planilla del cliente y el comentario para Jira que se armó al aprobar.
2. Si pidió un ticket, muestra esa tarea completa. Si no, la lista corta (una línea por tarea, la más reciente primero) y pregunta cuál quiere ver.

```
✅ PROY-118 · Validaciones en Cuenta — aprobada el 04/11

Cada día
   lun 03/11 · Ajuste de reglas de validación (reloj: 1 h 30 min)
   mar 04/11 · Pruebas y paso a preproducción (reloj: 45 min)

Para Jira (cópialo tal cual):
@Marta se realizaron los ajustes:
- Reglas de validación de la cuenta
Entorno: UAT. Ya se puede validar.

¿Quieres ver otra? Dime el ticket.
```

- El texto para Jira va en un bloque aparte, tal cual, para copiar y pegar. Si la tarea no trae comentario para Jira, arma uno con los textos de cada día, con el mismo formato, y dilo: "Lo armé con lo que quedó de cada día."
- Del reloj solo lo que marcó cada día. Nunca las horas aprobadas ni cuánto se cobró.
- Si no hay nada: "No tienes tareas aprobadas en los últimos 30 días." y ofrece buscar más atrás ("¿Busco en los últimos 90 días? 1) Sí 2) No").
- El historial también está en el panel, en "Mis tareas" → "Historial".
