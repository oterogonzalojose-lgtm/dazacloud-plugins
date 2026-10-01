---
name: reporte-horas
description: Reporte de horas aprobadas de Dazacloud para un rango de fechas, con totales por consultor, por cliente y cruzados, horas facturables y no facturables, y el link para bajarlo en Excel desde el dashboard. Lo usa la coordinadora o un líder de proyecto (solo sus empresas). Usar cuando la coordinadora o un líder dice "reporte-horas", "cuántas horas hicimos este mes", "horas de Ana en septiembre", "horas facturables de Cliente A", "reporte para facturar" o similar.
---

# reporte-horas

Le armas a quien coordina (la coordinadora o el líder de proyecto de una empresa) el reporte de horas **aprobadas** en un período: cuánto se trabajó, cuánto se factura y a quién. Solo lectura.

En este archivo, "quien coordina" es quien usa la skill: la coordinadora o un líder. Llámalo siempre por su nombre (sale de `quien_soy`).

## Reglas que no se rompen

- **Solo lectura.** No se aprueba ni se corrige nada desde aquí; si ve un error en una tarea aprobada, dile que no se puede editar desde esta skill.
- **Este reporte es interno.** Lleva nombres de consultores y resúmenes internos: no redactes con él nada para el cliente con esos datos. Si pide una versión para el cliente, arma solo fecha, ticket, texto para el cliente y horas facturables, sin consultor.
- No compares a los consultores entre sí ni saques conclusiones de rendimiento: las horas son datos.
- La estimación de la IA no aparece en el reporte, y **nunca se le muestra a un líder de proyecto**.
- Un líder ve solo las empresas que lidera y no guarda nada en las biblias: una regla permanente se le pide a la coordinadora (ofrécele redactar el pedido).

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados (los nombres son de ejemplo).

## Qué necesitas

Servidor MCP `dazacloud-admin`: `quien_soy`, `leer_biblia`, `reporte_horas`.

## Pasos

0. `quien_soy()`. **Coordinadora** (`es_admin`): todas las empresas, como está escrito aquí. **Líder** (no admin, con empresas en `lidera`): solo esas empresas (el backend ya filtra); se lo llama por su nombre. Si pide otra empresa, dile en una línea que solo ve las que lidera y ofrécele las suyas. **Ninguna de las dos** (el backend le niega el acceso): "El reporte de horas es para la coordinadora y los líderes de proyecto." y para.
1. `leer_biblia("reporte-horas")`, `leer_biblia("general")` y `leer_biblia("aprobaciones")` (qué es facturable y cómo se nombra cada cliente). Mandan sobre lo de abajo, salvo en "Reglas que no se rompen".
2. **Rango y filtros.** Por defecto, el mes en curso hasta hoy (hora de Colombia). Interpreta lo que diga quien coordina: "septiembre" → 01/09 al 30/09; "la semana pasada" → lunes a domingo anteriores; "este mes" → del 1 a hoy. Filtros opcionales: cliente (nombre como está en el backend) y consultor (email; búscalo en la biblia si te da el nombre). Si el rango es ambiguo, pregunta en una línea.
3. `reporte_horas(desde, hasta, cliente, consultor_email)`, con fechas AAAA-MM-DD, ambas inclusive.
4. Preséntalo. Es el único lugar con horas en decimales (con coma), porque es para facturar. Arranca con dos líneas en palabras y después las tablas:

```
📊 Septiembre (del 01/09 al 25/09)
Se aprobaron 118,5 h en 42 tareas: se cobran 112 h y 6,5 h no se cobran.

Por persona
  Persona     Tareas   Aprobadas   Se cobran   No se cobran
  Ana             15        45,0        43,0            2,0
  Bruno           12        38,5        34,0            4,5

Por cliente
  Cliente       Tareas   Aprobadas   Se cobran   No se cobran
  Cliente A         42       118,5       112,0            6,5

¿Quieres el detalle tarea por tarea o el Excel?  1) Detalle   2) Excel   3) Así está bien
```

   - La tabla cruzada persona × cliente, solo si hay más de un cliente.
   - Si no hay filas, dilo en una línea con el rango usado y termina.
   - Las no facturables se muestran siempre (aunque sean 0): es lo que se decidió no cobrar al aprobar.
   - **Detalle** (`filas`) solo si lo pide o si hay menos de 10 filas: fecha de aprobación, ticket, título, consultor, horas finales y facturables, y el texto para el cliente. El resumen interno, solo si lo pide.

5. **Excel.** Dale el link del dashboard, https://web-production-52afc.up.railway.app/dashboard/#reporte, y en una línea qué hacer: "Elige las mismas fechas y toca **Descargar Excel**." Un líder entra al panel con su email y el código que le llega por correo, y ahí ve solo sus empresas. (No le ofrezcas el enlace directo al CSV: pide una clave y es más incómodo.)
