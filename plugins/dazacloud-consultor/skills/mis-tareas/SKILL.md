---
name: mis-tareas
description: Muestra las tareas abiertas que la coordinadora le asignó al consultor en DazaCloud (en curso, devueltas, sin iniciar, pausadas), con advertencias, horas acumuladas frente a la estimación y un aviso si alguna ya la superó. Usar cuando el consultor dice "mis tareas", "qué tengo", "qué me toca hoy", "qué tengo pendiente" o similar.
---

# mis-tareas

Le muestras al consultor su trabajo asignado. Solo lectura: esta skill no inicia, pausa ni cierra nada.

## Reglas que no se rompen

- Solo lectura.
- No muestres horas aprobadas ni tareas ya aprobadas. La única estimación que existe para el consultor es `horas_estimadas`.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`. Si no responde o da 401, el token no está cargado o es inválido: dile que le pida a la coordinadora un token nuevo y reinstale el plugin (`claude plugin uninstall dazacloud-consultor@dazacloud-plugins` y `claude plugin install dazacloud-consultor@dazacloud-plugins`), y para.

## Pasos

1. `leer_biblia("mis-tareas")` y `leer_biblia("general")`. Su criterio de qué mostrar, orden y formato manda sobre lo de abajo, salvo en "Reglas que no se rompen".
2. `mis_tareas` (sin filtro: abiertas y pendientes de aprobación).
3. Preséntalo agrupado. Por defecto (el de la biblia): primero la tarea en curso; después devueltas y urgentes (advertencias con ⚠); después el resto por fecha de asignación.

```
EN CURSO
  PROY-123  LWC para generar PDF de la cotización · 1,25 h de 4 h
DEVUELTAS Y URGENTES (2)
  PROY-118  Validaciones en Cuenta__c · 2 h de 3 h — Devuelta: falta migración de datos (H2)
  ⚠ PROY-125  Error al guardar la oportunidad · sin iniciar · est. 2 h — Prioridad: Alta
POR HACER (2)
  PROY-121  Reglas de descuento en Oportunidad · sin iniciar · asignada mar 22/09
  PROY-110  Integración de pagos · pausada · 3,5 h de 3 h ⚠ superó la estimación
ESPERANDO APROBACIÓN: PROY-097
```

- **Devueltas:** son las pausadas cuya advertencia empieza con "Devuelta:". Muestra el motivo al lado.
- Las advertencias de la coordinadora se muestran siempre, al lado de la tarea (recortadas a una línea; completas en `inicio-tarea`).
- **Horas:** "<registradas> h de <estimadas> h" si hay estimación; si no, "<registradas> h acumuladas". Son el acumulado de cada tarea (el backend no separa por día): no muestres totales diarios ni horas de inicio.
- Si `excede_estimacion` es true, marca "⚠ superó la estimación" y recuérdale al final, en una línea, que lo informe a la coordinadora antes de seguir con esa tarea.
- Las pendientes de aprobación van en una sola línea con sus claves, sin horas.
- Si hay tareas asignadas hace más de 5 días hábiles que todavía no se iniciaron, recuérdalo en una línea al final.

Cierra ofreciendo el siguiente paso natural: `inicio-tarea` para arrancar o retomar una, o `fin-tarea` si la que está en curso ya terminó.
