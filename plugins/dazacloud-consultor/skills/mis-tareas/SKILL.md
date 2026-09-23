---
name: mis-tareas
description: Muestra las tareas que la coordinadora le asignó al consultor en DazaCloud (en curso, pendientes, pausadas, esperando aprobación o devueltas), con advertencias y horas acumuladas. Usar cuando el consultor dice "mis tareas", "qué tengo", "qué me toca hoy", "qué tengo pendiente" o similar.
---

# mis-tareas

Le mostrás al consultor su trabajo asignado. Solo lectura: esta skill no inicia ni cierra nada.

## Qué necesitás

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`. Si no responde o da 401, el token no está cargado o es inválido: decile que le pida a la coordinadora un token nuevo y reinstale el plugin (`claude plugin uninstall dazacloud-consultor@dazacloud-plugins` y `claude plugin install dazacloud-consultor@dazacloud-plugins`), y pará.

## Pasos

1. `leer_biblia("mis-tareas")` y `leer_biblia("general")`. Si tienen contenido, su criterio de orden y formato manda sobre lo de abajo.
2. `mis_tareas` (sin filtro: abiertas y pendientes de aprobación).
3. Presentalo agrupado, en este orden por defecto:

```
EN CURSO
  PROY-123  LWC para generar PDF de la cotización · 1,25 h acumuladas
PARA HACER (3)
  ⚠ PROY-118  Validaciones en Cuenta__c · retrabajo — Devuelta: falta migración de datos (H2)
  PROY-121  Reglas de descuento en Oportunidad · asignada mar 22/09
PAUSADAS (1)
  PROY-110  Integración de pagos · 0,75 h acumuladas
ESPERANDO APROBACIÓN (1)
  PROY-097  Batch nocturno de sincronización · declaraste 0,5 h
```

- Las advertencias de la coordinadora (incluidas las devoluciones, que empiezan con "Devuelta:") se muestran siempre, al lado de la tarea.
- Las horas son el acumulado de cada tarea (el backend no separa por día): no muestres totales diarios ni horas de inicio.
- Si hay tareas asignadas hace más de 5 días hábiles que todavía no se iniciaron, recordalo en una línea al final.

Cerrá ofreciendo el siguiente paso natural: `inicio-tarea` para arrancar o retomar una, o `fin-tarea` si la que está en curso ya terminó.
