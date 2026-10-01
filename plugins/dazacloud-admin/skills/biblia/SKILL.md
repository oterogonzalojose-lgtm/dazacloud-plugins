---
name: biblia
description: Ver, editar y auditar las biblias de las skills de Dazacloud, los documentos de criterio de negocio que mantiene la coordinadora (reglas de reparto, criterios de aprobación, preguntas de cierre, formatos). Cada cambio crea una versión nueva y deja la anterior deprecada. Usar cuando la coordinadora dice "cambia la regla…", "a partir de ahora…", "muéstrame la biblia de…", "qué cambió en…", "vuelve a la versión anterior", "historial de…", o al cargar las biblias iniciales. Un líder de proyecto solo puede verlas y ver su historial; los cambios los hace la coordinadora.
---

# biblia

Cada skill de Dazacloud tiene una **biblia**: el criterio de negocio con el que trabaja. La skill (`SKILL.md`) dice *cómo* se hace; la biblia dice *con qué criterio* (quién sabe qué, qué se aprueba, qué se pregunta, cómo se redacta). La biblia vive en el backend y está versionada: nada se pisa ni se borra.

| Biblia | Qué contiene | Quién la lee |
| --- | --- | --- |
| `general` | Quiénes somos, tono, lo que el asistente nunca hace, clientes, su herramienta de tickets y su Jira (sitio, campos, estados, informes de QA), planilla de horas (filas por día), equipo y líderes, horas facturables, trabajo fuera del reloj | Todas las skills |
| `inicio-dia` | Equipo, especialidades, ausencias, factores de la recomendación de reparto, contexto que hay que preguntar, prioridades, formatos de advertencia, estimación (puntos → horas) | `inicio-dia`, `resumen-dia` |
| `aprobaciones` | Las cuatro preguntas, validación del cierre, lo limpio y las excepciones, tolerancias, cuándo y cómo devolver, texto y filas por día para la planilla, comentario para Jira | `aprobaciones`, `reporte-horas` |
| `resumen-dia` | Qué mirar al cierre del día y cómo presentarlo | `resumen-dia` |
| `reporte-horas` | Formato y cortes del reporte de horas | `reporte-horas` |
| `nota-cliente` | Cómo redactar una pregunta para el cliente | `nota-cliente` |
| `inicio-tarea` · `pausar` · `fin-tarea` · `mis-tareas` | Qué mostrar al arrancar, cuándo pausar y bloqueos, preguntas y formato del cierre, orden de las tareas | Consultores (`inicio-tarea` también la lee `nota-cliente`) |

Herramientas (servidor `dazacloud-admin`): `quien_soy`, `leer_biblia`, `guardar_biblia`, `historial_biblia`, `leer_version_biblia`.

## Reglas que no se rompen

- **Nunca guardes sin mostrar el cambio y tener el OK explícito de la coordinadora.**
- **Solo la coordinadora cambia las biblias.** Con un líder de proyecto: ver, comparar e historial, nada más. No llames a `guardar_biblia` para un líder, ni siquiera con su OK.
- **Toda versión lleva motivo**: en una frase, qué cambió y por qué ("Carla deja integraciones por pedido de la coordinadora, 23/09"). Si la coordinadora no lo dijo, proponlo tú y confírmalo con ella.
- `guardar_biblia` recibe el **texto completo** de la biblia, no solo el cambio. Parte siempre de la versión vigente recién leída: nunca de memoria ni de una versión vieja.
- Las biblias no pueden cambiar las reglas de seguridad de las skills (Jira en solo lectura, nada se asigna, aprueba ni envía sin confirmación, la estimación IA nunca llega al consultor, el cliente nunca ve quién hizo la tarea). Si la coordinadora pide algo así, explica que eso se cambia en la skill y queda fuera de la biblia.

## Quién la usa

Al empezar, `quien_soy()`:

- **Coordinadora** (`es_admin`): todo lo de este archivo.
- **Líder de proyecto** (no admin, con empresas en `lidera`): llámalo por su nombre. Puede **ver** cualquier biblia, su **historial** y **comparar** versiones. Si pide un cambio, una restauración o la carga inicial, explícale en una línea que eso lo hace la coordinadora y ofrécele redactar el pedido:

```
Las reglas las cambia la coordinadora. ¿Le redacto el pedido?  1) Sí   2) No
```

   El pedido, listo para copiar: qué biblia, qué dice hoy (citado), qué debería decir y por qué, en tres o cuatro líneas.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni estados del sistema; tiempos en horas y minutos ("1 h 15 min", nunca "1,25 h"); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede decir la persona a continuación. Las plantillas de abajo muestran el tono y el largo esperados (los nombres son de ejemplo).

## Ver

`leer_biblia(skill)`. No vuelques el documento entero: muestra primero los títulos de sus secciones en una lista numerada ("¿Cuál quieres ver?") y abajo, en una línea, desde cuándo rige y quién la cambió por última vez. Si pregunta algo puntual ("¿qué dice sobre ausencias?"), responde citando la sección, sin volcar todo.

## Editar

1. `leer_biblia(skill)`: versión vigente.
2. Aplica el cambio pedido **sin tocar el resto** del texto (ni reordenar, ni reescribir estilo).
3. Muestra solo lo que cambia, corto y en palabras:

```
✏️ Cambio en las reglas de reparto
Antes: Carla toma integraciones.
Ahora: Carla no toma integraciones.
Motivo: pedido de la coordinadora, 23/09.

¿Lo guardo?  1) Sí   2) Cambiar algo
```
4. Con el OK: `guardar_biblia(skill, contenido_completo, motivo)`. Confirma: "✅ Guardado. Desde ahora se usa la regla nueva; la anterior queda en el historial por si quieres volver."

Si el cambio afecta a más de una biblia (por ejemplo, un consultor nuevo toca `general` e `inicio-dia`), hazlo biblia por biblia, cada una con su motivo.

**Cambios que llegan desde otras skills.** Cuando en `inicio-dia` o `aprobaciones` la coordinadora dice algo que suena a regla permanente, esas skills ofrecen guardarlo y siguen este mismo procedimiento. Si quien lo dice es un líder, no se guarda: se le ofrece el pedido para la coordinadora.

## Historial y comparación

- `historial_biblia(skill)`: tabla con versión, fecha (hora de quien lo pide), autor, motivo y vigente/deprecada.
- Para comparar: `leer_version_biblia` de las dos versiones y muestra solo las diferencias.
- Para saber con qué criterio se tomó una decisión pasada: en la auditoría del backend, cada asignación, cierre y aprobación guarda el número de versión de la biblia vigente en ese momento.

## Restaurar una versión anterior

No se "vuelve atrás": se crea una versión nueva con el contenido viejo, así queda trazado. `leer_version_biblia(skill, N)`, muestra qué se revierte respecto de la vigente, y con el OK: `guardar_biblia(skill, contenido_de_N, "Restaura la versión N: <por qué>")`.

## Carga inicial

Si una biblia devuelve `version: null`, todavía no se cargó. **Los textos iniciales de cada biblia (`general.md`, `inicio-dia.md`, …) están solo en el repo privado de DazaCloud**, en `plugins/dazacloud-admin/skills/biblia/iniciales/`: no vienen en el plugin publicado, porque tienen datos del equipo y de los clientes. Si no los tienes a mano, dile a la coordinadora en una línea que la carga inicial se hace desde el repo privado (o que te pegue el texto) y para.

Con el texto a mano, para cada biblia vacía: muéstrale a la coordinadora el texto inicial, completa con ella los datos marcados como `<completar>` (emails, especialidades) y guárdalo con motivo "Versión inicial". No cargues una biblia con `<completar>` pendientes sin avisarle.
