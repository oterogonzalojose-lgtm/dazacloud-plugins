---
name: resumen-dia
description: Cierre del día de la coordinadora o de un líder de proyecto en Dazacloud (el líder ve solo sus empresas). Resume qué se envió a aprobación y qué se aprobó, los bloqueos que registraron los consultores al pausar, las tareas que superaron la estimación, las notas para el cliente sin publicar y las horas registradas por persona. Usar cuando la coordinadora o un líder dice "resumen-dia", "cómo fue el día", "resumen de hoy", "qué pasó ayer", "cerremos el día" o similar.
---

# resumen-dia

Le das a quien coordina (la coordinadora o el líder de proyecto de una empresa), en una pantalla, cómo terminó el día del equipo y qué necesita su atención. Solo lectura, salvo marcar notas como publicadas cuando lo confirma.

En este archivo, "quien coordina" es quien usa la skill: la coordinadora o un líder. Llámalo siempre por su nombre (sale de `quien_soy`).

## Reglas que no se rompen

- **Solo lectura.** No apruebes, no devuelvas, no reasignes ni cambies estimaciones desde aquí: para eso están `aprobaciones` e `inicio-dia`.
- **Una nota solo se marca publicada cuando quien coordina dice que ya la publicó** en Jira. Jira es solo lectura.
- No juzgues a nadie por sus horas: pocas horas registradas o una tarea excedida son datos para que quien coordina pregunte, no conclusiones.
- La estimación de la IA no aparece en este resumen, y **nunca se le muestra a un líder de proyecto**.
- Un líder ve solo lo de las empresas que lidera y no guarda nada en las biblias: una regla permanente se le pide a la coordinadora (ofrécele redactar el pedido).

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados (los nombres y tickets son de ejemplo).

## Qué necesitas

Servidor MCP `dazacloud-admin`: `quien_soy`, `leer_biblia`, `resumen_diario`, `marcar_nota_publicada`, `descartar_nota`.

## Pasos

0. `quien_soy()`. **Coordinadora** (`es_admin`): todo el equipo, como está escrito aquí. **Líder** (no admin, con empresas en `lidera`): el backend ya le devuelve solo lo de sus empresas y su equipo; el título lleva la empresa ("🌙 Cliente A: así terminó el lunes 28/09") y se lo llama por su nombre. **Ninguna de las dos** (el backend le niega el acceso): "Este resumen es para la coordinadora y los líderes de proyecto." y para.
1. `leer_biblia("resumen-dia")`, `leer_biblia("general")` y `leer_biblia("inicio-dia")` (equipo, carga esperada, criterio de bloqueos y prioridades). Mandan sobre lo de abajo, salvo en "Reglas que no se rompen".
2. **Qué día.** Hoy por defecto (el backend lo cuenta en hora de Colombia). Si nombra otro ("ayer", "el viernes", "el 22"), pásalo como `fecha` en formato AAAA-MM-DD.
3. `resumen_diario(fecha)`.
4. Preséntalo, omitiendo lo que esté vacío. Por defecto:

```
🌙 Así terminó el lunes 28/09

🔒 Frenado (1)
• PROY-101 · Carla espera las credenciales de la sandbox del cliente → ¿se las pedimos?
⚠️ Pasadas de lo estimado (1)
• PROY-118 · Bruno lleva 5 h 15 min de 4 h (sigue trabajando)
📝 Notas sin publicar (1)
• PROY-110 · Carla: «¿El descuento aplica también a renovaciones?»
⏳ Esperan tu aprobación: PROY-120 (Ana, 2 h 15 min)
✅ Aprobadas hoy: PROY-115 (Diego, 3 h; se cobran 2 h 30 min)

Horas de hoy: Ana 6 h 30 min · Bruno 7 h · Carla 4 h 15 min · Diego sin horas
```

   - **Lo frenado** primero: es lo único que puede trabar el día siguiente. Para cada uno, la acción posible en media frase ("¿se las pedimos?", "¿se la paso a otra persona mientras tanto?"), sin ejecutarla.
   - **Pasadas de lo estimado:** tiempo registrado contra la estimación de quien repartió y si sigue en curso.
   - **Horas de hoy:** tal cual las devuelve el backend, en horas y minutos. Con un líder, "el equipo" es el de sus empresas, no el de toda la biblia. Si alguien del equipo no aparece o no tiene horas, dilo sin suponer por qué (puede estar ausente; la biblia tiene las ausencias).
   - Si no hubo nada frenado ni pasado, arranca con "✅ Día tranquilo: nada frenado."

5. Cierra con los siguientes pasos que correspondan, en una línea cada uno:
   - Hay para aprobar → "¿Las aprobamos ahora? 1) Sí  2) Mañana"
   - Hay notas sin publicar → ofrece redactar el borrador de comentario para Jira de cada una (mismo criterio que el Paso 5 de `inicio-dia`: tono de Dazacloud, sin el nombre del consultor, sin promesas). Cuando diga que las pegó, `marcar_nota_publicada(nota_id)` por cada una que nombró. Si una nota ya no hace falta (el cliente ya respondió, está duplicada), ofrece cerrarla con `descartar_nota(nota_id, motivo)`, con el motivo que diga quien coordina.
   - Hay bloqueos o excedidas → se retoman en el próximo `inicio-dia`.
