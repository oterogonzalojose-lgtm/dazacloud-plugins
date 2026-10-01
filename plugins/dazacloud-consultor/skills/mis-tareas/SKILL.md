---
name: mis-tareas
description: Muestra las tareas abiertas asignadas al consultor en DazaCloud (en curso, devueltas, sin iniciar, pausadas), con advertencias, horas acumuladas frente a la estimación y un aviso si alguna ya la superó. Usar cuando el consultor dice "mis tareas", "qué tengo", "qué me toca hoy", "qué tengo pendiente" o similar.
---

# mis-tareas

Le muestras al consultor su trabajo asignado. Solo lectura: esta skill no inicia, pausa ni cierra nada.

## Reglas que no se rompen

- Solo lectura.
- No muestres horas aprobadas ni tareas ya aprobadas. La única estimación que existe para el consultor es `horas_estimadas`.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados.

## Qué necesitas

Servidor MCP `dazacloud`: `leer_biblia`, `mis_tareas`. Si no responde o da 401, falta la clave de acceso o no es válida: dile en una frase "No pude conectarme con DazaCloud. Pídele a la coordinadora una clave nueva." y para. Los pasos para cargarla (`/plugin configure dazacloud-consultor@dazacloud-plugins`, o reinstalar el plugin) solo si los pide.

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
